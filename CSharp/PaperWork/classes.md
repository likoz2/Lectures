# Goal
The goal of this article is to explain `Classes` in C#.
# Sections
- What is a `Class`
- When `Class` can be used
- `static` `Class`
- `abstract` and `sealed`
- Access modifiers
- `Interface`
- `Struct`
- `Record`
- Summary
# Prerequisites
- Requisite
  - introduction.md
  - variables.md
  - methods.md

This article is meant to be read after methods.md.
# What is a `Class`
`Class` is a way to organize you `Variables` or `Methods` and pack them together.

It is always a reference type. Its value is null until instance is explicitly created.
```csharp
public class Player {
    string name;

    public Player(string name){
        this.name = name;
    }
}
```
Called like following.
```csharp
Player player; // null
Console.WriteLine(player);
player = new Player("John"); // The value is an address pointing to Heap where the data are stored.  player.name == John
Console.WriteLine(player);
```
# When `Class` can be used
## Tidy the code
Following code would be messy real quick.
```csharp
string[] playerNames = new string[4];
int[] playerHp = new int[4];
```
So lets create a class.
```csharp
public class Player {
    string name;
    int hp;

    public Player(string name){
        this.name = name;
        this.hp = 100;
    }
}
```
```csharp
Player[] players = new Player[4];
```
## `inheritance`
Implemented once, inherited multiple times.
```csharp
public class Damageable {
    protected int hp;

    public void Damage(int damage) {
        hp -= damage;

        if (hp <= 0)
            Die();
    }

    protected void Die() {
        Console.WriteLine("I died. :(");
    }
}
```
```csharp
public class Player : Damageable {
    string name;

    public Player(string name) {
        this.name = name;
        this.hp = 100;
    }
}
```
```csharp
public class Enemy : Damageable {
    string name;

    public Enemy(string name) {
        this.name = name;
        this.hp = 150;
    }
}
```
```csharp
Player player = new Player("John");
Enemy enemy = new Enemy("Zombie");
player.Damage(10);
enemy.Damage(10);
```
Another advantage apart from less code repetition is we can put these `classes` into one array or a list.
```csharp
Player player = new Player("John");
Enemy enemy = new Enemy("Zombie");
Damageable[] arr = [player, enemy];
List<Damageable> list = [player, enemy];
```
## Generic
You have noticed that the "List" above is just another `class` but it can also specify a `class` it works with. Lets see how to create that. I will try my best to mimic how real "List" works.
```csharp
internal class MyList<T> : IEnumerable<T> {
    public int Capacity { get; private set; }
    public int Count => index;

    private T[] array;
    private int index = 0;

    // Indexer makes it possible to access the list like MyList<T>[index]. Similar to how an array or a list can be accessed.
    // We just define what happens when we try to "get" and "set" the data.
    public T this[int index] {
        get {
            if (index < 0 || index >= Count) {
                throw new ArgumentOutOfRangeException(nameof(index), "Index is out of range.");
            }
            return array[index];
        }
        set {
            if (index < 0 || index >= Count) {
                throw new ArgumentOutOfRangeException(nameof(index), "Index is out of range.");
            }
            array[index] = value;
        }
    }

    // I am here showing how you can call a ctor from other ctor.
    // In this case, when user doesn't specify the "expected" size of the array, we specify it to be 8, but still providing empty ctor.
    public MyList() : this(8) {

    }

    public MyList(int capacity) {
        Capacity = capacity;
        array = new T[Capacity];
    }

    public void Add(T item) {
        if (index >= array.Length) {
            ExpandArray();
        }

        array[index++] = item;
    }

    public bool Remove(T item) {
        int at = IndexOf(item);
        if (at == -1)
            return false;

        return RemoveAt(at);
    }

    // The parameter "at" should probably be named "index" but I didn't want to confuse between "this.index" and "index", so I used "index" and "at" respectively just to make this example more clear.
    public bool RemoveAt(int at) {
        if (at < 0 || at >= index)
            return false;
        
        if (at + 1 < array.Length)
            Array.Copy(array, at + 1, array, at, Capacity - at-1);

        index--;
        return true;
    }

    public int IndexOf(T item) {
        for (int i = 0; i < Count; i++) {
            if (EqualityComparer<T>.Default.Equals(array[i], item)) { // We cannot just "T == T", we must use the default comparer
                return i;
            }
        }

        return -1;
    }

    private void ExpandArray() {
        int newCapacity = Capacity * 2;
        T[] newArray = new T[newCapacity];

        Array.Copy(array, newArray, Capacity);
        Capacity = newCapacity;
        array = newArray;
    }

    // A Method required by IEnumerable<T>
    public IEnumerator<T> GetEnumerator() {
        return new Enumerator(this);
    }

    // A Method required by IEnumerable<T>
    IEnumerator IEnumerable.GetEnumerator() {
        return GetEnumerator();
    }

    // A Nested struct inside the class, used only for this class to enumerate over the class itself.
    // Is private as there is no need for anyone else then the class itself to access this in the "IEnumerator<T> GetEnumerator()" method above.
    private struct Enumerator : IEnumerator<T>, IEnumerator, IDisposable {
        // A Property required by IEnumerator<T>
        public T Current => list[index];

        // A Property required by IEnumerator<T>
        object IEnumerator.Current => Current;

        MyList<T> list;
        private int index = -1;

        public Enumerator(MyList<T> list) {
            this.list = list;
        }

        // A Method required by IEnumerator<T>
        public void Dispose() {
            index = -1;
            list = null;
        }

        // A Method required by IEnumerator<T>
        public bool MoveNext() {
            index++;
            return index < list.Count;
        }

        // A Method required by IEnumerator<T>
        public void Reset() {
            index = -1;
        }
    }
}
```
The above then can be used like following.
```csharp
MyList<int> list = new MyList<int>(); // we can specify any Type we want

list.Add(5); // Add works like expected
list.Add(6);
list.Add(7);

Console.WriteLine("List:");
foreach (int item in list) { // Thanks to the IEnumerable<T> interface we implemented, we can use it in foreach
    Console.WriteLine(item);
}

bool removed = list.Remove(7); // Removes first value with that value, as expected
Console.WriteLine("\nRemove " + (removed ? "Successful" : "Failed"));

bool removedAt = list.RemoveAt(5); // Removes value on that index if possible, as expected
Console.WriteLine("\nRemove At " + (removedAt ? "Successful" : "Failed"));

Console.WriteLine("\nList:");
foreach (int item in list) {
    Console.WriteLine(item);
}
```
So our original example would look like following.
```csharp
MyList<Damageable> damageables = [new Player("John")]; // The list can be also initialized with []
damageables[0].Damage(10); // Thanks to the implemented indexer we can use the index to retrieve data as usual

foreach (Damageable item in damageables) {
    item.Damage(10);
}
```
Note that the indexer isn't required to be indexed with `int`. The most likely other index type would be some `enum`.\
For simplicity I used `string` to show that the following can be done.
```csharp
public T this[string indexS] {
    get {
        int index = int.Parse(indexS);
        if (index < 0 || index >= Count) {
            throw new ArgumentOutOfRangeException(nameof(index), "Index is out of range.");
        }
        return array[index];
    }
    set {
        int index = int.Parse(indexS);
        if (index < 0 || index >= Count) {
            throw new ArgumentOutOfRangeException(nameof(index), "Index is out of range.");
        }
        array[index] = value;
    }
}
```
Called like following.
```csharp
MyList<int> list = new MyList<int>();

list.Add(5);
list.Add(6);
list.Add(7);

Console.WriteLine("Value: " + list["1"]);
```
Quick tip:\
If you inherit the interface like so.
```csharp
public class MyNewList<T> : IEnumerable<T>
```
The "IEnumerable<T>" will be underlined saying you don't implement all the interface's members.\
You can right click it (or "CTRL" + ".") and select "Implement interface". That will generate a "template" you can fill in.\
This automatically works with every interface.\
You will end up with the following. You can notice that your only task is to fill what throws an exception.
```csharp
public class MyNewList<T> : IEnumerable<T> {
    public IEnumerator<T> GetEnumerator() {
        throw new NotImplementedException();
    }

    IEnumerator IEnumerable.GetEnumerator() {
        return GetEnumerator();
    }
}
```
For extra readers:\
If you wonder how the `foreach` works, I have shown it using a `while` loop. This is most likely what is happening in the `foreach`.\
As you can see it is a very nice shortcut.
```csharp
MyList<int> list = new MyList<int>();

list.Add(5);
list.Add(6);
list.Add(7);

Console.WriteLine("foreach:");
foreach (int item in list) {
    Console.WriteLine(item);
}

Console.WriteLine("\nwhile:");
IEnumerator<int> enumerator = list.GetEnumerator();
enumerator.Reset();
int item2;
while (enumerator.MoveNext()) { // The MoveNext() here is the reason why the index needs to be reset to -1.
    item2 = enumerator.Current;
    Console.WriteLine(item2);
}
enumerator.Dispose();
```
# `static` `Class`
A `static` `Class` is a `Class` of which instance cannot be made. It is only used when expecting only one `Class` "instance".\
The `static` `Class` cannot be inherited. (There is no reason for it to be inheritable.)

For example the `Class` "Console" is `static`. There is only one console at a time.\
Also the `Class` "Math" is `static`.
```csharp
// We CANNOT do something like
//Console console = new Console();
//console.WriteLine();
// We can only do
Console.WriteLine();

// Math class
Math.Abs(-2);
Math.Sin(Math.PI*2d);
Math.FloorToInt(2.4d);
// Quick note:
// The Math class uses "double" everywhere.
// We can use "Math.Abs(-2);" with the -2 being "int" because C# can implicitly convert "int" to "double"
```
# `abstract` and `sealed`
Similar to `static` `Class`, it is not possible to create an instance of an `abstract` `Class`. But the difference is that `abstract` is meant to be inherited. The inheriting `Class`, when not marked as `abstract` as well, can be made instance of.
```csharp
public abstract class Weapon {
    public abstract bool TryShoot();
}

public class MachineGun : Weapon {
    int ammo;

    public override bool TryShoot() {
        if (ammo <= 0)
            return false;

        Shoot();
        ammo--;
        return true;
    }
}

public class Spear : Weapon {
    bool thrown = false;

    public override bool TryShoot() {
        if (thrown)
            return false;

        Throw();
        thrown = true;
        return true;
    }

    private void OnPickUp() {
        thrown = false;
    }
}
```
Called like following.
```csharp
List<Weapon> weapons = new List<Weapon>();
weapons.Add(new MachineGun());
weapons.Add(new Spear());

foreach (Weapon weapon in weapons) {
    weapon.TryShoot();
}
```
The `sealed` `Class` is a `Class` that cannot be inherited.\
Learn more on [why to use `sealed` `Class`](https://stackoverflow.com/questions/7777611/when-and-why-would-you-seal-a-class)
# Access modifiers
## `private`
Only `Classes` in which this `Class` is nested can access it.\
Default, if not stated otherwise.
## `public`
Every `Class` can access this `Class`.
## `internal`
Acts like `public` but denies access to `Classes` from other dlls. Usually used in .dll (libraries) or APIs.
## `protected`
Only this `Class` and `Classes` which inherits from this `Class` can access this `Class`.
# Interface
An `Interface` defines a contract. Any `Class`, `Record` or `Struct` that implements that contract must provide an implementation of the members defined in the `Interface`. An `Interface` may define a default implementation for members. It may also define a `static` members in order to provide a single implementation for common functionality.
# Struct
A `Struct` is a value type that can encapsulate data and related functionality.

Is is declared the same as a `Class`, but the "struct" keyword is used instead of "class" keyword like following.
```csharp
class VectorClass {
    float x, y, z;
}

struct VectorStruct {
    float x, y, z;
}
```
It is used when we want to do some calculations or value assignment where we are expecting to "copy" the **values**, not the **pointer**.
For example when working with `class` we can face the following issue.
```csharp
public class Vector {
    public float X, Y, Z;
}

public class Player {
    public Vector Position;

    public Player() {
        Position = new Vector();
    }
}

public class Ball {
    public Vector Position;

    public Ball(Vector position) { // Creates a ball at a given position
        Position = position;
    }
}
```
We want to create a ball at the player's position and move it forward.
```csharp
Player player = new Player();
Console.WriteLine("Player init pos X: " + player.Position.X);

Ball ball = new Ball(player.Position); // This will copy the pointer to the player's position.
Console.WriteLine("Ball init pos X: " + ball.Position.X);

ball.Position.X += 5; // Moving the ball on the X axis. This will also move the player.

Console.WriteLine("Player pos X: " + player.Position.X);
Console.WriteLine("Ball pos X: " + ball.Position.X);
```
What actually happens is that the player moves as well because the "Position" is the player's position as the pointer was copied to the Ball.

To fix this, we would need to "clone" the values while creating a new Vector.\
Added `ctors` so you can just copy-paste it to try it out.
```csharp
public class Vector {
    public float X, Y, Z;

    public Vector() { }

    public Vector(float x, float y, float z) {
        X = x;
        Y = y;
        Z = z;
    }

    public Vector Clone(){
        return new Vector(X, Y, Z);
    }
}
```
Now we can call it like following.
```csharp
Player player = new Player();
Console.WriteLine("Player init pos X: " + player.Position.X);

Ball ball = new Ball(player.Position.Clone()); // This will create a new instance of the vector in the "Clone()".
Console.WriteLine("Ball init pos X: " + ball.Position.X);

ball.Position.X += 5; // This will no longer move the player.

Console.WriteLine("Player pos X: " + player.Position.X);
Console.WriteLine("Ball pos X: " + ball.Position.X);
```
And because we would use the "Vector" a lot, we would need to say ".Clone()" everywhere, which would get annoying fast.

That is what the struct does for us.
```csharp
public struct Vector {
    public float X, Y, Z;
}

public class Player {
    public Vector Position;

    public Player() {
        // Because Vector is a struct, we don't actually need to create an instance of it.
        // Struct creates a default value on its own.
        Position = new Vector();
    }
}

public class Ball {
    public Vector Position;

    public Ball(Vector position) { // Creates a ball at a given position
        Position = position;
    }
}
```
Now it is called the same as originally (without the ".Clone()"), but thanks to the "struct" it behaves differently.
```csharp
Player player = new Player();
Console.WriteLine("Player init pos X: " + player.Position.X);

Ball ball = new Ball(player.Position); // This will copy the values of the player's position
Console.WriteLine("Ball init pos X: " + ball.Position.X);

ball.Position.X += 5; // This will not move the player.

Console.WriteLine("Player pos X: " + player.Position.X);
Console.WriteLine("Ball pos X: " + ball.Position.X);
```
# Record
`Record` is used to define a reference type that provides built-in functionality for encapsulating data.
```csharp
public record Person(string FirstName, string LastName);
```
The `Record` can also define `Record struct`
```csharp
public record struct DataMeasurement(DateTime TakenAt, double Measurement);
```
Personally, there isn't much more to say about this. It is used to encapsulate data.\
You can use `Class` for that. If your `Class` doesn't provide any other functionality apart from encapsulate the data, it should be marked `Record` for readability. It will not change its behavior, just others reading or using your code will understand that there is no functionality and it's purpose is just the data encapsulation.

Learn more about [`Record`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record).
# Summary
`Class` -> Reference type
- Can be inherited
- Can inherit a `Class` (max one inherited `Class`)
- Can inherit an `Interfaces` (any amount of `Interfaces`)
- Can be marked `Record` instead of `Class` when used only for data encapsulation.
- `Static`
  - Cannot be made an instance of
  - Used as a "library" or rather "section of functions"
- `Abstract`
  - Meant to be inherited, joining common functionality
- `Sealed`
  - **Cannot** be inherited

`Struct` -> Primitive/Value type
- **Cannot** be inherited
- **Cannot** inherit a `Class`
- Can inherit an `Interfaces` (any amount of `Interfaces`)

`Interface`
- Meant to be inherited, joining common functionality

Access modifiers
- Always use `private`, unless someone else need to access it
- Allow access only to who really needs it (For simplicity only `public` can be used but it is not correct)