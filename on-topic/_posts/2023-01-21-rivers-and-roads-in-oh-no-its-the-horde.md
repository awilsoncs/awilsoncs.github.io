---
tags: python tcod roguelike coding tutorials
title: "Rivers and Roads in Oh No! It's THE HORDE!"
---

## Introduction

This post will look at the rivers and road generation techniques for [_Oh No! It's THE HORDE!_](https://jazzbox.itch.io/oh-no-its-the-horde). We'll look at the design goals of these features before showing how I implemented them.

![Oh No! It's THE HORDE Gameplay](https://img.itch.zone/aW1nLzg4NjI2NTMucG5n/original/f1emZj.png)

## Design Goals

Let's look at the goals and principles that guided the creation of the map generation in HordeRL.

I knew I wanted a road network to connect the houses. In *The Horde (1994)*, Chauncey moves more quickly on road tiles. This is an important strategic aspect, as it forces the player to decide between taking a longer, paved route vs a shorter unpaved one. The winding roads also bring a cozy aesthetic and charm to the town.

Rivers are not an element in the original game (as far as I know). However, water and ponds can spawn and be created. These tiles slow Chauncey and the Hordelings down, and drown Adolescent Hordelings. Water serves as a natural barrier and obstacle that players must navigate around and through, adding an additional layer of challenge and strategy to the game. Water also adds visual interest to the map and helps to break up the terrain, making the map feel more varied and interesting.

I knew I wanted the drama of the river flowing from one side of the map to the other. However, I initially designed water to completely block passage, and if we allow disconnected segments of the map, we risk entities being stuck on one side or the other. While I eventually redesigned water to deal damage rather than block movement, the initial goal informed world-building design.

The solution to this problem was to fix the river's path from the top to bottom of the map and then to add a main road from the left to right. Since mathematically these two must cross, it's easy to imagine that we're guaranteed that the entire map is accessible. However, we'll see that this is different in practice.

## Generating the River

We generate fake terrain height to build the river using a Simplex noise map. While this terrain never appears in-game, it allows for the river to flow naturally through the terrain, following the contours of the "land". `CostMappers` are full component objects since different hordelings may want different behaviors. As you will see, the `PlaceRiver` component creates its own cost mapper rather than looking for one on the entity.


```python

# simplex_cost_mapper.py

class SimplexCostMapper(CostMapper):
    """Component for generating a random Simplex noise map."""

    def get_cost_map(self, scene):
        # We use tcod's Simplex implementation to generate a noise map.
        noise = tcod.noise.Noise(dimensions=2, algorithm=tcod.noise.Algorithm.SIMPLEX, octaves=3)
        cost = noise[tcod.noise.grid(shape=(settings.MAP_WIDTH, settings.MAP_HEIGHT), scale=0.5, origin=(0, 0))]
        
        # Note that Numpy matrices are transposed from the gameplay map.
        return cost.astype(np.uint16).transpose()
```

After calculating the height map, we can apply `tcod` pathfinding to generate a windy route across the map. The `tcod` docs aren't explicit about the pathfinding algorithm used, but Djikstra works here if coding from scratch. The general idea is that we want to find the "cheapest" path from the start to the end, using the height map as our costs.


```python

# pathfinder.py

class Pathfinder(Component):
    """Component for calculating a pathfrom point A to B given a cost map."""

    def get_path(self, cost_map, start, end, diagonal=3) -> List[tuple[int, int]]:
        # set up the Pathfinder
        graph = tcod.path.SimpleGraph(cost=cost_map, cardinal=2, diagonal=diagonal)
        pf = tcod.path.Pathfinder(graph)
        pf.add_root(start)
        
        # run the calculation
        path = pf.path_to(end).tolist()
        return path
```

Next, we can see how these two objects work together in the `PlaceRiver` component. This component is a BuildWorldListener, which means that when a BuildWorldEvent occurs, the event system will call `on_build_world` and the `PlaceRiver` will construct the river.


```python

# place_river.py

class PlaceRiver(BuildWorldListener):
    """Component for placing the river during worldbuilding."""

    # Invoked by the event system
    def on_build_world(self, scene):
        self._log_info(f"placing river")

        # Components can be casually created and discarded if it suits the purpose.
        cost = SimplexCostMapper().get_cost_map(scene)
        
        # We leave some space at the borders so that the map feels more framed.
        start = (random.randint(2, settings.MAP_WIDTH-3), 0)
        end = (random.randint(2, settings.MAP_WIDTH-3), settings.MAP_HEIGHT-1)
        
        # Here we pass in the costs to get a river path.
        river = Pathfinder().get_path(cost, start, end, diagonal=0)
        
        # Place the water tiles. Each biome has a "river_rapids" value that
        # determines how fast the water animation works. This lends some tone to
        # each biome.
        params = scene.cm.get_one(WorldParameters, entity=core.get_id("world"))
        for x, y in river:
            self._log_debug(f"placing water ({x}, {y})")
            scene.cm.add(*make_water(x, y, rapidness=params.river_rapids)[1])

```

### Very Windey Rivers

This approach works to create a windy river that begins at the top and ends at the bottom. Given some noise maps, the river can bump the edge of the map. When water was to be impassable, this created disconnected islands that could trap Hordelings. Ultimately, this led to revising the water system to deal damage rather than block passage, which better corresponds to the *The Horde*.


## Generate the Roads

We generate the road network in three steps:

1. Place the town center.
2. Connect each House to the road network.
3. Connect the edges of the map to the road network.

### Placing the Town Center

The Town Center is the player's starting location during raids. It's nearly situated at the [centroid](https://en.wikipedia.org/wiki/Centroid) of the triangle formed by the initial three homes, which makes the first year a little easier on the player.

```python

# place_roads.py

def get_town_center(house_coords, scene):

    avg_x = int(sum(c.x for c in house_coords) / len(house_coords))
    avg_y = int(sum(c.y for c in house_coords) / len(house_coords))
    all_coords = set(scene.cm.get(Coordinates, project=lambda c: (c.x, c.y)))
    
    # Jitter the location until we find a safe place to drop the town square.
    # This is not hot code, or else such randomness would be strongly frowned
    # upon in favor of a geometric solution. For this case, it's quick and
    # dirty.
    while not get_3_by_3_square(avg_x, avg_y).isdisjoint(all_coords):
        avg_x += random.randint(-2, 2)
        avg_y += random.randint(-2, 2)
    return avg_x, avg_y

```

We draw a 3x3 square of roads at this location, and tag the middle one with the `TownCenter` component. This component serves as a waypoint when we need to teleport the player here. The nine road tiles form the initial component of the road network graph.

### Connecting the Houses to the Road Network

Connecting the houses is similar to drawing the river. In this stage, we need to do a few things for each home:

1. Get a cost map.
2. Find the best road tile to connect to, given the cost map.
3. Draw the path.

```python

# roads.py

def connect_point_to_road_network(scene, start: Tuple[int, int], trim_start: int = 0):
    # Find all roads in the scene and assign them a target value (100)
    road_coords = scene.cm.get(RoadMarker, project=lambda rm: (rm.entity, 100))
    
    # Get the cost map.
    cost_map = RoadCostMapper().get_cost_map(scene)
    
    # Leverage the AI target determination system to find the best road tile.
    best_entity: int = get_new_target(scene, cost_map, start, road_coords)
    
    # After choosing the best road tile to connect to, find its position and
    # draw a road to that point.
    best_point: Tuple[int, int] = scene.cm.get_one(Coordinates, entity=best_entity).position
    _draw_road(scene, start, best_point, cost_map, trim_start=trim_start)
```

#### Getting a Road Cost Map


Here's the cost mapper for the `PlaceRoads` step. Notable, it depends on the scene for cost determination. This object prioritizes avoiding all entities other than Water, which it lightly suggests to avoid. As such, this mapper tends to create bridges where they make sense.

```python

# road_cost_mapper.py

class RoadCostMapper(CostMapper):
    def get_cost_map(self, scene):
        size = (settings.MAP_WIDTH, settings.MAP_HEIGHT)
        cost = np.ones(size, dtype=np.uint16, order='F')
        for coord in scene.cm.get(Coordinates):
            if scene.cm.get_one(Attributes, entity=coord.entity):
                # Given where we are in worldgen, this is a House Wall. We should definitely avoid passing through a house at all costs.
                cost[coord.x, coord.y] = 10000
            elif scene.cm.get_one(WaterTag, entity=coord.entity):
                # It's fine to cross water, if the alternative path is at least twice as long.
                cost[coord.x, coord.y] += 2
            else:
                # We should generally avoid previously existing entities.
                cost[coord.x, coord.y] += 1000
        return cost
```

> Editorial note: I'm hesitant to edit this for the sake of the post, but there appears to be a bug such that the cost map is transposed from the game map. AW 1/21/2023

#### Find the Best Road Tile

The target selection function is more mathy, so bear with me. This function takes the cost map from above, the starting position, and the value of all possible targets (as object, int tuples).

In order to understand `tcod.path.dijkstra2d`, it's best to read the [docs](https://python-tcod.readthedocs.io/en/latest/tcod/path.html#tcod.path.dijkstra2d). In short, this function takes an array of maximum values of dtype with (usually) a single cell set to zero. The function updates `out` with the cost to reach all reachable cells (and leaves other cells at maximum).

Given the cost to reach all possible targets, the target evaluator compares the cost to reach by the value of the target (in this case, all values are equal). `get_new_target` strongly prefers closer targets. It then returns the best candidate.

```python

# targeting.py

def get_new_target(scene, cost_map, start, entity_values) -> int:
    dist = tcod.path.maxarray((settings.MAP_WIDTH, settings.MAP_HEIGHT), dtype=np.int32)
    dist[start[0], start[1]] = 0
    tcod.path.dijkstra2d(dist, cost_map, 2, 3, out=dist)

    # find the cost of all the possible targets
    best = (None, 0)
    for entity, value in entity_values:
        target_coords = scene.cm.get_one(Coordinates, entity=entity)
        cost_to_reach = float(dist[target_coords.x, target_coords.y]) ** 2
        if cost_to_reach == 0:
            cost_to_reach = 1
        value = float(value) / cost_to_reach
        if value > best[1]:
            best = (entity, value)

    return best[0]
    
```

#### Draw the Road

Now that we have our start and end point for this road segment, we can draw the road.


```python

# roads.py

def _draw_road(scene, start: Tuple[int, int], end: Tuple[int, int], cost_map, trim_start: int=0):
    for node in _road_between(cost_map, start, end, trim_start=trim_start):
    
        # Base case: check to see if we've reached a road. If we have, we're
        # connected to the network and can stop.
        coords = scene.cm.get(Coordinates, query=lambda c: scene.cm.get_one(RoadMarker, entity=c.entity))
        if coords and coords[0].x == node[0] and coords[0].y == node[1]:
            break
            
        # Otherwise, create one!
        other_coords = scene.cm.get(Coordinates, query=lambda c: list(c.position) == node, project=lambda c: c.entity)
        
        # We pave over everything, including water. However, if we find water,
        # we make a bridge tile instead of a road tile. They differ only by
        # appearance.
        is_water = False
        for other in other_coords:
            is_water = scene.cm.get_one(WaterTag, entity=other)
            scene.cm.delete(other)
        if is_water:
            scene.cm.add(*make_bridge(node[0], node[1])[1])
        else:
            scene.cm.add(*make_road(node[0], node[1])[1])
```


We build up an increasingly complex road network by repeating this process for each house. Notably, this process happens everytime we add a house, including at the start of the new year.

### Placing the Highway

The final step of building the initial road network is to connect the existing network to the edge of the map. This ensures that the road network crosses the river, even if all houses are on one side of it.

This step is very simple after considering the above. We choose a point on the left and connect it to the network, then a point on the right and do the same.

```python

def draw_road_across_map(self, scene):
        self._log_info(f"placing highway")
        start = (0, random.randint(2, settings.MAP_WIDTH-3))
        connect_point_to_road_network(scene, start)
        end = (settings.MAP_WIDTH-1, random.randint(2, settings.MAP_HEIGHT-3))
        connect_point_to_road_network(scene, end)
        
```

## Gameplay Mechanics and Design Outcomes

This discussion wouldn't be complete without describing the "how" in front of the "why". Road, Bridge, and Water tiles have unique effects on entities that affect gameplay.

**Road** and **Bridge** tiles grant the main character (and not hordelings) **Haste** when stepped into. Haste reduces the cost of the entity's next move by half. So, while traveling on roads, the player moves twice as fast. This allows the player to move between houses much easier, but occasionally forces a decision between a paved and unpaved route.

**River** tiles deal one damage when stepped into. They also grant the status effect **Swamped** to all entities. Swamped forces the player to spend a turn to remove it before they can move. Water is an important strategic consideration. Each hordeling has a different relationship to water- Juggernauts will wade straight through it, slowing and harming them. Young hordelings attempt to go around but occasionally drown in it. These pathing behaviors mean that a player can leverage their knowledge to create chokepoints and other defensive strategies. However, water freezes in Winter, which can undermine such strategies.

Sometimes the main character may be forced to take a short, unpaved path across water to reach a peasant in time.

## Conclusion

In this post, we've looked at design goals, implementation, and the mechanics behind the rivers and roads feature of [_Oh No! It's THE HORDE!_](https://jazzbox.itch.io/oh-no-its-the-horde). I hope this post inspires and educates you to add some organic character to your game worlds by using random noise and pathfinding algorithms.

As always, if you have comments, reach out to me on [Mastodon](https://mastodon.gamedev.place/@AaronWilson)!

