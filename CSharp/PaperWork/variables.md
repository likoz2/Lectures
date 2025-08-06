# Goal
The goal of this article is to explain `Variables` in C#.
# Sections
- What is a `Variable`
- `Stack` and `Heap`
- When `Variable` can be used
- Access modifiers and `Property`
# Prerequisites
- Requisite
  - introduction.md

This article is meant to be read right after introduction.
# What is a `Variable`
Variable holds a value. The value can be stored in the RAM in two different places. In Stack or Heap. The variable in the code then behaves differently based on where it is stored.

Variable can be one of two types:
- `field` (name should be in camelCase)
- `Property` (name should be in PascalCase)
# `Stack` and `Heap`
The variable `Type` can be either a "Simple" type or a "Reference".

## Value and Reference (pointer)
Simple type is stored directly in the `Stack`. This results in the following behavior.
```csharp
int a = 2;
int b = a;
a = 3 + 2;
Console.WriteLine(a); // 5
Console.WriteLine(b); // 2
```
Reference types have only the reference (pointer) stored in the `Stack`. Their data are stored in `Heap`. This results in the following behavior.
```csharp
public class Enemy {
    private int hp;

    public int Hp => hp;

    public Enemy(int hp) {
        this.hp = hp;
    }

    public void Damage(int damage) {
        hp -= damage;
    }
}
```
```csharp
Enemy a = new Enemy(10);
Enemy b = a;
Console.WriteLine(a.Hp); // 10
Console.WriteLine(b.Hp); // 10

a.Damage(2);
Console.WriteLine(a.Hp); // 8
Console.WriteLine(b.Hp); // 8
```
As you can see, the value on `Stack` is always passed. (To clarify and maybe confuse and contradict the last sentence: Always is used what is there. If a class contains a reference type, it is technically on `Heap` therefore it is passed from the `Heap` but still it passes the reference.)
- If it is the value itself, the value is passed (Simple type).
- If it is the reference, the reference is passed (Reference). This results in multiple `Variables` pointing to the same `Heap` section.

To clarify lets create an example where we overcome the issue with multiple `Variables` pointing to the same address.
```csharp
public class Enemy {
    private int hp;

    public int Hp => hp;

    public Enemy(int hp) {
        this.hp = hp;
    }

    public void Damage(int damage) {
        hp -= damage;
    }

    public Enemy Clone() { // This creates a new instance (New section on Heap and therefore a new pointer to that value)
        return new Enemy(hp);
    }
}
```
```csharp
Enemy a = new Enemy(10);
Enemy b = a.Clone();
Console.WriteLine(a.Hp); // 10
Console.WriteLine(b.Hp); // 10

a.Damage(2);
Console.WriteLine(a.Hp); // 8
Console.WriteLine(b.Hp); // 10
// Technically a and b are different instances, each having its own part in the Heap where hp is stored and its own pointer on Stack pointing on the part on Heap.
```
## Equality
With simple types.
```csharp
int a = 2;
int b = a;
Console.WriteLine(a == b); // true
```
```csharp
int a = 2;
int b = 2;
Console.WriteLine(a == b); // true
```
With references.
```csharp
Enemy a = new Enemy(10);
Enemy b = a;
Console.WriteLine(a == b); // true
```
```csharp
Enemy a = new Enemy(10);
Enemy b = a.Clone();
Console.WriteLine(a == b); // false
```
```csharp
Enemy a = new Enemy(10);
Enemy b = new Enemy(10);
Console.WriteLine(a == b); // false
```
The `==` by default compares what is in the variable. So if it is a reference, it compares the pointer address and doesn't look for the values inside the `Class`. The `==` when used with reference types only compares the address it is pointing to so it only equals when both point to the same value.\
To check the variables instead we can override the Equals method with our own implementation.
```csharp
public class Enemy {
    private int hp;

    public override bool Equals(object obj) {
        return obj is Enemy otherEnemy && this.hp == otherEnemy.hp;
    }
}
```
Called like following.
```csharp
Enemy a = new Enemy(10);
Enemy b = a;
Console.WriteLine(a.Equals(b)); // true
```
```csharp
Enemy a = new Enemy(10);
Enemy b = a.Clone();
Console.WriteLine(a.Equals(b)); // true
```
```csharp
Enemy a = new Enemy(10);
Enemy b = new Enemy(10);
Console.WriteLine(a.Equals(b)); // true
```
In real scenario we would probably not want to say that two enemies are the same if they have the same hp, rather we should use its name or ID... So this only serves as a simple example.
## `Struct`
`Struct` behaves like a bit like a primitive type.

It can be created in the exact same way as `Class` but with the `struct` keyword.
```csharp
public struct Vector {
    int x;
    int y;
    
    public int X { get => x; set => x = value; } // Created for later use in examples.
    public int Y { get => y; set => y = value; } // This is nothing special for struct. See "Property" section for more.

    public Vector(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Values works like following.
```csharp
Vector a = new Vector(10, 15);
Vector b = a;
a.X += 10;
Console.WriteLine(a.X + " " + a.Y); // 20 15
Console.WriteLine(b.X + " " + b.Y); // 10 15
Console.WriteLine(a.Equals(b)); // false
```
The equality works like following.
```csharp
Vector a = new Vector(10, 15);
Vector b = new Vector(10, 15);
Console.WriteLine(a.Equals(b)); // true
```
```csharp
Vector a = new Vector(10, 15);
Vector b = new Vector(20, 25);
Console.WriteLine(a.Equals(b)); // false
```
```csharp
Vector a = new Vector(10, 15);
Vector b = a;
Console.WriteLine(a.Equals(b)); // true
```
You can notice we don't use `==` thats because it is not defined in struct. However we can define it ourselves.
```csharp
public struct Vector {
    int x;
    int y;
    
    public int X { get => x; set => x = value; } // Created for later use in examples.
    public int Y { get => y; set => y = value; } // This is nothing special for struct. See "Property" section for more.

    public Vector(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public static bool operator ==(Vector a, Vector b) {
        return a.x == b.x && a.y == b.y;
    }

    public static bool operator !=(Vector a, Vector b) {
        return !(a == b);
    }
}
```
Now we can use it like this.
```csharp
Console.WriteLine(a == b);
```
# Access modifiers and `Property`
## `private`
No other `Class` can access this `Variable`.\
Default, if not stated otherwise.
## `public`
Every `Class`, which can access the `Class` in which the `Variable` is created, can also access the `Variable`. (Simply said, every `Class` can access.)
## `internal`
Acts like `public` but denies access to `Classes` from other dlls. Usually used in .dll (libraries) or APIs.
## `protected`
Only this `Class` and `Classes` which inherits from this `Class` can access the `Variable`.

## Which to use
For `fields` only `private` should be used.\
For `Property` only `public`, `internal` and `protected` should be used.

The correct usage of these is as follows.
```csharp
public struct Vector {
    float x;
    float y;
    float z;

    bool isDirty = true;
    float magnitude;

    public float X { // Public getter and setter, the "get" and "set" are always public if not stated otherwise.
        get => x;
        set {
            x = value;
            isDirty = true;
        }
    }
    public float Y {
        get => y;
        set {
            y = value;
            isDirty = true;
        }
    }
    public float Z {
        get => z;
        set {
            z = value;
            isDirty = true;
        }
    }

    public float Magnitude {
        get {
            if (isDirty)
                Recalculate(); // The Magnitude is only recalculated when it is needed and the values have changed from last time.

            return magnitude;
        }
    }

    public Vector(int x, int y) {
        this.x = x;
        this.y = y;
    }

    private void Recalculate() { // In this Method will recalculate all the values we will add in the future.
        magnitude = CalculateMagnitude();

        isDirty = false;
    }

    public float CalculateMagnitude() {
        return (float)Math.Sqrt(x * x + y * y + z * z);
    }
}
```
Since C# 13 we can do the following. We are no longer required to write out the `field` (`int x;`).\
So the `field` can be replaced with actual "field" keyword.
```csharp
public struct Vector {
    public float X {
        get;
        set {
            field = value;
            isDirty = true;
        }
    }
    public float Y {
        get;
        set {
            field = value;
            isDirty = true;
        }
    }
    public float Z {
        get;
        set {
            field = value;
            isDirty = true;
        }
    }

    public Vector(int x, int y) {
        X = x;
        Y = y;
    }
}
```
Quick hint, to see your C# version type "#error version" anywhere in your code and hover your mouse over it.

# Summarization
So thats all to the `Variables`.