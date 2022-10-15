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
3. Facade


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

The problem is: IHunter interface just expects ILion. In this case, the adpter patter comes to play. We can make `DogAdapter` so that `IHunter` can hunt `Dog`.

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

### Facade Design Pattern
Facade class takes several libraries and execute necessary methods to achieve goal.
![Facade Class Diagram](images/FacadeClassDiagram.png)

There are several operations are made when we start and shut-down computers. We can make libraries for computer start and computer shutdown.

```C#
class ComputerStart
{
    public void GetElectricShock()
    {
        Console.Write("Ouch!");
    }

    public void MakeSound()
    {
        Console.Write("Beep Beep!");
    }

    public void ShowLoadingScreen()
    {
        Console.Write("Loading..!");
    }

    public void Bam()
    {
        Console.Write("Bam!");
    }
}

class ComputerShutDown
{
    public void CloseEverything()
    {
        Console.Write("Bup bup bup buzzz!");
    }

    public void Sooth()
    {
        Console.Write("Zzzzz");
    }
}

class ComputerStartShutDownFacade
{
    private readonly ComputerStart _computerStart;
    private readonly ComputerShutDown _computerShutDown;

    public ComputerStartShutDownFacade(ComputerStart computerStart,
                                       ComputerShutDown computerShutDown)
    {
        _computerStart = computerStart;
        _computerShutDown = computerShutDown;
    }

    public void TurnOn()
    {
        _computerStart.GetElectricShock();
        _computerStart.MakeSound();
        _computerStart.ShowLoadingScreen();
        _computerStart.Bam();
    }

    public void TurnOff()
    {
        _computerShutDown.CloseEverything();
        _computerShutDown.PullCurrent();
        _computerShutDown.Sooth();
    }
}
```

Now to use the facade

```C#
var _omputerStartShutDownFacade = new ComputerStartShutDownFacade(new ComputerStart(),
                                                                  new ComputerShutDown());
_omputerStartShutDownFacade.TurnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
Console.WriteLine();
_omputerStartShutDownFacade.TurnOff();  // Bup bup buzzz! Haah! Zzzzz
Console.ReadLine();
```

## Behavioral Design Pattern

Several behavioral dsign patterns are:
1. Command
2. Obseraver

### Behavioral Design Pattern
`Client` delegates `Command` to `Invoker`. `Invoker` then gives it to `Receiver`.

```C#
//Receiver
public class Bulb
{
    public void TurnOn()
    {
        Console.WriteLine("Bulb has been lit");
    }

    public void TurnOff()
    {
        Console.WriteLine("Darkness");
    }
}

//Command
public interface ICommand
{
    void Execute();
}

public class TurnOn : ICommand
{
    void Execute() => Console.WriteLine("Light!");
}

public class TurnOff : ICommand
{
    void Execute() => Console.WriteLine("Darkness!");
}

//Invoker
class RemoteControl
{
    public void Submit(ICommand command)
    {
        command.Execute();
    }
}
```

This is how we can use Command Design Pattern:
```C#
var bulb = new Bulb();

var turnOn = new TurnOn(bulb);
var turnOff = new TurnOff(bulb);

var remoteControl = new RemoteControl();
remoteControl.Submit(turnOn);
remoteControl.Submit(turnOff);
```