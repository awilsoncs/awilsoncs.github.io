---
tags: csharp fundamentals coding tutorials
---

# Read-only Views Using Generics in C\#

This blog post is a brief tutorial on using generics to create read-only views, and more broadly, working with substitution and generics.

I wanted to create a read-only view for the gamestate in [Untitled Roguelike Framework](https://github.com/awilsoncs/Untitled-Roguelike-Framework) so that I could expose the gamestate to the client side without allowing the client to directly manipulate the state. URF uses an event-driven model, but forcing the client to maintain a local view ends up needlessly duplicating code and work.

## SOLID Principles in Action

This concept will demonstrate two important [SOLID principles](https://en.wikipedia.org/wiki/SOLID). Namely, we will apply the [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle) by allowing a method to use a subtype of the declared signature, and we will apply the [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle) by separating the view interface from the controller interface. Furthermore, this will illuminate a pattern found through the C# standard library in which generics inherit from [IReadOnlyCollection](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.ireadonlycollection-1?view=net-7.0).

## A Simple Example

First, let's take a look at substituting generics with a simple example. In this example, we want to define the generic concept of a `Zoo` and an `Animal`. Then, we define a `Kennel` to be a `Zoo` of `Dogs`, which are `Animals`.

```csharp
public interface Animal {}

// Note the out keyword in the generic parameter. Without this, the compiler will reject our later usage.
public interface Zoo<out TAnimal> where TAnimal : Animal {
  TAnimal GetAnimal();
}

public class Dog : Animal {
  public void Bark() {}
}

public class Kennel : Zoo<Dog> {
  Dog GetAnimal() {
    return new Dog();
  }
}

public class ZooTest {
  public void Test() {
    Zoo<Animal> someZoo = new Kennel();

    // WILL NOT COMPILE
    someZoo.GetAnimal().Bark() // Zoo is not aware that this is a dog

    Kennel kennel = new Kennel();
    kennel.GetAnimal().Bark() // Yes! Will compile!
  }
}
```

Note that this doesn't work without using generics (or creating overloads that explicitly override the interface). To avoid confusion, I won't write out a failing example that attempts to use pure inheritance- you can imagine that `Zoo.GetAnimal` returns an `Animal` instead of a generic type. As far as I can tell, there's no _theoretical_ reason this couldn't work. It's just a decision by the maintainers of C#.

## A (More) Real Example

Alright, so let's see this in the game state example. It's essentially the same code, just more code-sounding.

```csharp
// Only exposes query methods
public interface IReadOnlyEntity {
  int Health { get; }
}

// Only exposes query methods
public interface IReadOnlyGameState<out TEntity> where TEntity : IReadOnlyEntity {
  IReadOnlyCollection<TEntity> GetEntities();
}

// Adds command methods. This could also be another interface.
public class Entity : IReadOnlyEntity {
  public int Health { get; set; }
}

// Provide command methods for the backend to use. Here, we lock in the type of TEntity.
public class IGameState : IReadOnlyGameState<Entity> {
  void PutEntity(Entity entity);
  void UpdateEntity(Entity entity);
  void DeleteEntity(Entity entity);
}

// GameState can implement GetEntities with type locked in above, or any type that is derived from that one.
public class GameState : IGameState {
  // The entities here will be manipulable, even though the original declaration only required IReadOnlyEntity.
  IReadOnlyCollection<Entity> GetEntities() {}
  void PutEntity(Entity entity) {}
  void UpdateEntity(Entity entity) {}
  void DeleteEntity(Entity entity) {}
}

// using GameStateView = IReadOnlyGameState<IReadOnlyEntity>
// using EntitiesView = IReadOnlyCollection<IReadOnlyEntity>
public class Client {
  void Update(GameStateView state) {
    // entities will be a read-only view of GetEntities.
    // bear in mind that entities could still be cast to a manipulable object.
    EntitiesView entities = state.GetEntities();

    int health = entities.First().Health; // we can query the health

    // WILL NOT COMPILE
    state.GetEntities().First().Health = 100; // NO! We cannot set the health!
  }
}

// Here, we see that the Server (which is aware of the "full" interface) can still manipulate the entities.
public class Server {
  void Update(IGameState state) {
    // the server has full access to the game state.
    state.PutEntity(new Entity());

    int health = state.GetEntities().First().Health; // we can query the health
    state.GetEntities().First().Health = 100; // we can also set the health
  }
}
```

You can apply this pattern recursively to build up a read-only hierarchy. For instance, if `Entities` have `Components`, you may apply a `IReadOnlyComponent` view as well. Unfortunately, this gets pretty wordy, so you'll want to lean on `var` or type aliases liberally, in my opinion.

## Covariance and Substitution

According to the [the C# docs](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/)

> In C#, covariance ... enable[s] implicit reference conversion for ... generic type arguments.

But uh...what does that mean? In other words, covariance allows a specific type to _implicitly_ apply to a generic type as long as its type parameters are children of the original type. For example, here

```csharp
Zoo<Animal> someZoo = new Kennel();
```

`Kennel` is parameterized with `Dog`, not `Animal`. Without covariance, Kennel would have to return a `Zoo<Animal>` (or some type that explicitly derives from it) in order for this to compile.

```csharp
Zoo<Animal> someZoo = ...new Zoo<Dog>()...; // does not compile without covariance

class WildZoo : Zoo<Animal> {}

Zoo<Animal> anotherZoo = new WildZoo() // works fine
```

## The `out` Keyword

Covariance is "turned on" by using the `out` keyword on a generic parameter. **This is easy to forget if you're not comfortable in C#.** When you begin using generics, **write this down and post it in plain sight.** Remembering this feature at the right time could save you hours of confusion.

## Conclusion

In this post, we've taken a look at creating safer views by applying generic substitution, along with a pair [SOLID principles](https://en.wikipedia.org/wiki/SOLID), the [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle), and the [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle) We've also seen how `IReadOnlyCollection` works behind the scenes.

Did I get anything wrong? Let me know [@AaronWilson@mastodon.gamedev.place](https://mastodon.gamedev.place/@AaronWilson)!
