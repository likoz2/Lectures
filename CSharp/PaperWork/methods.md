# Goal
The goal of this article is to explain `Methods` in C#.
# Sections
- What is a Method
- How Method looks
- When Method can be used
- Static / Non-Static
- Access modifiers
- Constructor
- Extension
- Summary
# Prerequisites
- Requisite
  - introduction.md
- Helpful
  - variables.md

This article is meant to be read after introduction. If you have read the variables.md as well, it can help you understand some parts better. It is advised to read both first.
# Explanation
## What is a Method
`Method` is a "piece" of code that can be "called" from other part of the code. `Method` can return a value, update a value or return nothing. `Method` can be passed some parameters.\
`Method's` name should be in PascalCase.
## How Method looks
Before explaining how method can be used, first lets see how some basic method can look like.
```csharp
public static void SayHello() {
    Console.WriteLine("Hello!");
}
```
Called like following.
```csharp
SayHello();
```
That is an example which creates `public` (Can be seen by every class.) `static` (Is accessed without an instance of a `Class`.) `Method` named "SayHello". The `Method` has no return value or parameters.\
In it's code body only one line of code is executed.\
This `Method` prints "Hello!" into the console and returns no value.

### Note
Note that `Methods` can have thousands of parameters, each can be any `Type` you please. I am just always showing none or one for simplicity.

## When Method can be used
`Method` can be used for multiple reasons.\
The unwritten rule is that there should not be a code longer than 5 lines. Also `Methods` shouldn't be longer then 5 lines. If your code has more lines of code in one `Method` you are probably doing something wrong.\
Note that more complex algorithms are often longer without other calls for the purpose of fast code execution. The code is then less readable but faster to execute, such `Methods` are often commented heavily.

### Repetition
If we just write all of the code in the `Main` without using other methods we will notice repetition. Methods, mostly with parameters, can prevent code repetition. We can then `call` the method from a different part of the code with different or same parameters to achieve the expected behavior. In the following example `Method` "Add" is used to show the most common repetition which is a math operation for more complex variable types.
```csharp
Vector vec1 = new Vector(1,2);
Vector vec2 = new Vector(3,4);
Vector vec3 = Vector.Add(vec1, vec2);
```
The code above can be used instead of the following.
```csharp
Vector vec1 = new Vector(1,2);
Vector vec2 = new Vector(3,4);
Vector vec3 = new Vector(vec1.x + vec2.x, vec1.y + vec2.y);
```
Note that operators can be overridden in C#. (They behave like a method, but are called with the operator instead of the `()`.)
```csharp
Vector vec1 = new Vector(1,2);
Vector vec2 = new Vector(3,4);
Vector vec3 = vec1 + vec2;
```

### Naming
`Method` can also be used to give a part of code a name. For example the Sort algorithm we created earlier. The algorithm itself can be hard to understand, but because it is called "Sort" and accepts parameter "int[] arr" we can expect it to sort an int array without fully understanding each line of the code in the `Method`.\
This is very descriptive for itself so no comments `//` are necessary to be used.\
For such reason, the method must have a "correct" name. It is necessary to call things the way they are doing. If the name of anything is not descriptive enough or is confusing it is a mistake and should be corrected.\
This exemplary `Method` can also be called from many places, so it also fulfils the 'repetition' reason.
```csharp
public static Sort(int[] arr) {
    for (int i = 0; i < arr.Length - 1; i++) {
        for (int j = 0; j < arr.Length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int swap = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = swap;
            }
        }
    }
}
```
The name it was given is simple and understandable. You might be thinking whether "SortIntArray" ins't a better name. Simple answer is no, because of the parameter. Lets see a quick example for this question.

When using a parameter, two `Method`s can have the same name. They differ in the type of parameter passed.
```csharp
public static void Write(int value) {
    Console.Write(value);
}

public static void Write(double value) {
    Console.Write(value);
}
```
When not using a parameter but instead returning a value the compiler would not know which one he should call therefore it is necessary to make a difference in the name, usually like shown.
```csharp
public static int ReadInt() {
    return int.Parse(Console.ReadLine());
}

public static double ReadDouble() {
    return double.Parse(Console.ReadLine());
}
```
If you don't like this style, which is preferred, there is other way.
```csharp
public static void Read(out int value) {
    value = int.Parse(Console.ReadLine());
}

public static void Read(out double value) {
    value = double.Parse(Console.ReadLine());
}
```
The difference will be in the call. The latter way should not be used as it is less readable in the call.
```csharp
int value1 = ReadInt();
Read(int value2);
```
If you spent a few seconds looking at the call, you might have asked another question: "Isn't "ReadIntFromConsole" a better name?", and yes, it most likely is. But it depends on the class in which the `Method` is created.
```csharp
public class MyConsole {
    public static int ReadInt() { // Calling like MyConsole.ReadInt is obviously reading it from the console.
        return int.Parse(Console.ReadLine());
    }
}
```
```csharp
public class MyFile {
    public static int ReadInt() { // Calling like MyFile.ReadInt is obviously reading it from a file.
        return int.Parse(File.ReadLine("file.txt"));
    }
}
```
```csharp
public class MyLib {
    public static int ReadIntFromConsole() { // There it is not explained by the class name from where it is read, so we give it a name.
        return int.Parse(Console.ReadLine());
    }
}
```
### Tidying up the code
In the "Naming" part we touched this reason as well.

There we can see how methods make more sense when tidied into a class. It is just one line of code so it might seem redundant.
```csharp
public class MyConsole {
    public static int ReadInt() {
        return int.Parse(Console.ReadLine());
    }
}
```
```csharp
public class MyFile {
    public static int ReadInt() {
        return int.Parse(File.ReadLine("file.txt"));
    }
}
```
But when the method is created with some checks which is very likely to happen it then looks like the following and makes much more sense to use to prevent a code repetition.
```csharp
public class MyConsole {
    public static int ReadInt() {
        string s = Console.ReadLine();
        if (!int.TryParse(s, out int ret)) {
            Console.WriteLine("Read value was not an int.");
            return int.MinValue;
        }

        return ret;
    }
}
```
```csharp
public class MyFile {
    public static int ReadInt() {
        string fileName = "file.txt";
        if (!File.Exists(fileName)) {
            Console.WriteLine("File not found.");
            return int.MinValue;
        }

        if (!int.TryParse(s, out int ret)) {
            File.WriteLine("Read value was not an int.");
            return int.MinValue;
        }

        return ret;
    }
}
```
#### For extra readers
The method mentioned above should probably look like this for maximum clearance.
```csharp
public class MyConsole {
    public static bool TryReadInt([NotNullWhen(true)] out int? value) {
        string s = Console.ReadLine();
        if (!int.TryParse(s, out int ret)) {
            Console.WriteLine("Read value was not an int.");
            value = null;
            return false;
        }

        value = ret;
        return true;
    }
}
```
### Summarization
The `Method` should be used because of all of the reasons. More simply to put this: `Method` should not violate any of the reasons and should always help with the code clarity.
# Static / Non-Static
Static means that we do not need to create an instance of the class. It is mostly used for the `Methods` which operate only on the parameters they are passed, and don't use any of the classes variables.
```csharp
public class MyConsole {
    public static SayHello() {
        Console.WriteLine("Hello!");
    }
}
```
Non-static is used in `Classes` we expect to create an instance of.
```csharp
public class Player {
    int hp = 100;

    public void Heal(int value) {
        hp += value;
    }
}
```
Called like following.
```csharp
Console.SayHello(); // There is only one console where it could get printed on so the Console class is static

Player player = new Player(); // Multiple players can be present in the game so we need to create an instance for each player.
player.Heal(10); // We call the method on the instance we please.
```
# Access modifiers
## `private`
No other `Class` can access this `Method`.\
Default, if not stated otherwise.
## `public`
Every `Class`, which can access the `Class` in which the `Method` is created, can also access the `Method`. (Simply said, every `Class` can access.)
## `internal`
Acts like `public` but denies access to `Classes` from other dlls. Usually used in .dll (libraries) or APIs.
## `protected`
Only this `Class` and `Classes` which inherits from this `Class` can access the `Method`.

## Which to use
If you are just starting, use `public` everywhere.

The correct usage of `private` is the following example. It tries to hide the `Method` "Shoot" which reduces the ammo and does the shooting logic (hidden for simplicity).\
If it was `public` then by a mistake when called, the "Weapon" would be able to shoot without any ammo in the magazine. This solution allows us to hide the "Shoot" behind "TryShoot" which only allows the "Weapon" to shoot when there is enough ammo in the magazine.
```csharp
public class Weapon {
  private int ammo = 30;

  public bool TryShoot() {
    if (ammo <= 0)
      return false;

    Shoot();
    return true;
  }

  private void Shoot() { // This method is private, so only Weapon can call it. That means it only gets called from TryShoot and only if there is enough ammo.
    ammo--;
  }
}
```
Called like following.
```csharp
public class Player {
    Weapon weapon = null;

    public void PlayerPickUpWeapon(Weapon weapon) {
        this.weapon = weapon;
    }

    public void OnClick(){
        if (weapon == null)
            return false;

        //weapon.Shoot(); // Cannot be called. Will throw a Compile Error -> The code will not compile. Therefore I have commented it out.
        weapon.TryShoot(); // Only this method can be accessed (called).
    }
}
```
# Constructor
Constructor (ctor) is a `Method` used to initialize `Class` instance. It is defined by an access modifier, `Class` name and the parameters.\
In previous code examples the `ctor` was hidden for simplicity.

Remember that when no `ctor` is defined, C# automatically creates an empty `ctor`. `ctor` created in this way is not visible in the code. This creates a common issue where when defining a `ctor` with parameters later on, the automatically created default `ctor` is "removed" by C#, which leads to Compile errors in all of the code which used the empty `ctor`. Simple way to solve it -> add the empty `ctor` manually.

Tip: In many IDEs typing ' "ctor" Tab Tab' creates a `ctor` for you.
```csharp
public class Player {
    private string name;

    public Player() { // empty ctor
        this.name = "Player";
    }

    public Player(string name) { // ctor
        this.name = name;
    }
}
```
Called like following.
```csharp
Player player1 = new Player();
Player player2 = new Player("John");
```
# Extension
If a `Class` is already created, we can "add" a `Method` to it without having access to the original source code. For the simplest example I can think of, we can add "Sq" (Square) and "Add" `Method` to `int`. 
```csharp
public static class IntExtension {
    public static int Sq(this int number)
    {
        return number * number;
    }

    public static int Add(this int number, int otherNumber)
    {
        return number + otherNumber;
    }
}
```
Called like following.
```csharp
int a = 2;
int b = a.Sq();
int c = a.Add(b);
```
This has some limitations. The `Method` must always be `static`. For non-static methods inheritance should be used. Some `Classes` are defined as `sealed` which makes them unable to be inherited. In such scenario "wrapper" should be used.
Another limitation is that operators cannot be implemented using Extensions.

Wrapper is an implementation "design" which "has" (contains) the `Classes` instance and/or implements all the `Properties` and `Methods` as the original `Class` as well as providing a new functionality.
# Summary
The more you will code the better you will use the `Methods`. OOP is the main thing in C#. Design how to use `Classes` and `Methods` correctly is basically most of the C# knowledge.