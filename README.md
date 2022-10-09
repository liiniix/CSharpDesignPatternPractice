> This is my design patterns practice from [C# Design Pattern for Humans](https://github.com/anupavanm/csharp-design-patterns-for-humans)

# Design Patterns
There are three types of design patterns:
1. Creational
2. Structural
3. Behabioral


## Structural Design Pattern
Several structural design patterns are:
1. Adapter
2. Bridge

### Adapter Design Pattern
We have a game where hunters hunt lions.

```C#
interface ILion
{
    void Roar();
}

class AfricanLion : ILion
{
    void Roar()
    {
        
    }
}

class AsianLion : ILion
{
    void Roar()
    {

    }
}
```

Hunter interface expects ILion interface to hunt.

```C#
interface Ihunter
{
    void Hunt(ILion lion)
    {

    }
}
```

Lets's say we want to introduce dogs for hunter to hunt.

```C#
class IDog
{
    void Bark()
    {

    }
}
```

The problem is: IHunter interface just expects ILion. In this case, the adpter patter comes to play. We can make `DogAdapter` so that `IHunter` can hunt `IDog`.

```C#
class DogAdapter : ILion
{
    private Dog _dog;

    public DogAdapter(Dog dog)
    {
        _dog = dog;
    }

    vod Roar()
    {
        _dog.Bark();
    }
}
```