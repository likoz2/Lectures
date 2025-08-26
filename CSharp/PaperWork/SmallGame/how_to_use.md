# Goal
The goal of this article is to help you get started with the Lecture_SmallGame example.
# Sections
- Setup
- Abstract
- Structure
- Keyboard input
- Interface
- How it works
- Creating custom behavior

- Summary
# Prerequisites
- Requisite
    - setup_guide.md
    - Windows OS

The Lecture_SmallGame uses Windows console as an interface. Other OS terminals might not work. Built-in consoles of IDEs can glitch.
# Setup
To set up the project, please refer to the `setup_guide.md`.\
If you have the project running, let's continue by explaining how things work.
# Abstract
The Lecture_SmallGame example is my custom made Unity-like game engine which uses Windows console as an interface. In this article I will show you how to use my engine and what will differ when you switch to Unity.
# Structure
## Parts
- `Engine.cs` is capturing inputs and handling `GameObjects`.
- `Writer.cs` is rendering `Renderers`.
- `GameObject` is in game object that you can create.
- `Component` is defining behavior of the `GameObject` which holds it.
## Which parts you care about
You will create instances of `GameObjects` and assign `Components` to them.\
You may create a `class` which inherits `GameObject` and defines your custom behaviors. You will then instantiate this class.
## What changes from Unity
In Unity, your scripts are inheriting MonoBehavior, which is a `Component`. In my engine you are supposed to inherit `GameObject` instead.
# Keyboard input
Console can only register an input key that is pressed. Method such as "key held" or "key released" cannot be achieved.\
Any mouse inputs like moving, clicking or scrolling are also not possible to register with a console.

What can be achieved is a key combo such as "CTRL + A". The combo can be registered with all special keys in combination with all normal keys.
## What changes from Unity
In Unity you can register all "key press", "key hold", "key release" and all mouse inputs.
# Interface
Console can only display characters. However, it is possible to set the character or its background to an RGB value.
## What changes from Unity
Unity can draw anything on the screen as it uses a window and draws pixels.
# How it works
In `Program.cs` we can find the method `Main(string[] args)` in which we define the behavior. We can keep programming as we are used to from normal C#. The only difference is how to display things. We can create a `GameObject` and give him a `Renderer` `Component`.
```csharp
GameObject textObject = Engine.Instantiate<GameObject>();
TextRenderer renderer = textObject.AddComponent<TextRenderer>();
```
This creates a `GameObject` which has a `TextRenderer` `Component`. The `TextRenderer` inherits from `Renderer` which means it will automatically draw the text.

Lets give it some text to display then.
```csharp
renderer.Text = "This is a test line.";
```
When we give it a new text, the `TextRenderer` automatically redraws the new text into the console.

That is not much fun, is it? Let's add some colors.
```csharp
Color color1 = new Color(120, 10, 200);
Color color2 = new Color(220, 110, 50);
renderer.ColorTopLeft = color1;
renderer.ColorTopRight = color2;
```
When we give it new colors, it will automatically redraws the text with the colors into the console.

This is how it all looks together.
```csharp
Color color1 = new Color(120, 10, 200);
Color color2 = new Color(220, 110, 50);

GameObject textObject = Engine.Instantiate<GameObject>();
TextRenderer renderer = textObject.AddComponent<TextRenderer>();

renderer.Text = "This is a test line.";

renderer.ColorTopLeft = color1;
renderer.ColorTopRight = color2;
```
This will write a colorful text.

To see what is happening we can delay the code execution like so.
```csharp
Color color1 = new Color(120, 10, 200);
Color color2 = new Color(220, 110, 50);

GameObject textObject = Engine.Instantiate<GameObject>();
TextRenderer renderer = textObject.AddComponent<TextRenderer>();
Task.Delay(1000).Wait();
renderer.Text = "This is a test line.\nThis is a test line.";
Task.Delay(1000).Wait();
renderer.ColorTopLeft = color1;
Task.Delay(1000).Wait();
renderer.ColorTopRight = color2;
```
# Creating a custom behavior
Now let's say we want to create a Player that moves. Lets give him a `TextRenderer` to show many Coins he picked up. Coin is a stationary `GameObject` that is spawned by CoinSpawner. CoinSpawner is a `GameObject` with a custom behavior as well. Let's see how to create all of them.

First, let's take a look at the CoinSpawner.\
The method `Update` is called every frame. It tries to spawn a Coin with a 10% chance each frame, which means 3 coins a second. (By default, the game runs on 30 FPS.)\
The Spawn method is our custom method. It randomly selects a position of the Coin and instantiates it. The `Name` property is to mimic Unity, it is not visible anywhere since there is no inspector or hierarchy viewer as in Unity.
```csharp
public class CoinSpawner : GameObject
{
    public override void Update()
    {
        if (Rand.Value < 0.1f)
            Spawn();
    }

    private void Spawn()
    {
        Vector pos = new(Rand.Next(Writer.Width - 3), Rand.Next(Writer.Height - 3));
        Coin coin = Engine.Instantiate<Coin>(pos);
        coin.Transform.Name = "Coin";
    }
}
```
Let's continue with the Coin itself.\
The `SpriteRenderer` allows us to draw a multiline `Sprite`.\
We give it a `BoxCollider` which is a `Component` that can be hit by a `RigidBody` `Component`. More in Player section below.\
The `BoxCollider` must know it's `Size`, which in our example, is the size of the `Sprite`.
```csharp
public class Coin : GameObject
{
    public Coin()
    {
        SpriteRenderer renderer = AddComponent<SpriteRenderer>();
        renderer.Sprite = new Sprite([" __", "/Co\\", "\\in/", " ˇˇ"]);
        BoxCollider collider = AddComponent<BoxCollider>();
        collider.Size = new Vector(renderer.Sprite.Width, renderer.Sprite.Height);
    }
}
```
The last but not least is the Player.\
This might seem overwhelming so let's tackle it down part by part.\

First we add `Components` in the constructor.\
`RigidBody` which allows us to check collision and therefore hit a `Collider`. When adding a `RigidBody` `Component`, the `BoxCollider` `Component` is added automatically.\
As the `BoxCollider` is added automatically, we have to get it to assign its `Size`.\
Then we add a `SpriteRenderer` `Component` with a `Sprite`.\
The `TextRenderer` added at the end is for the text above the Player's head. To move it above the Player, we use the `LocalPosition`. The `Layer` property specifies the order in which the `Renderers` are rendered. 0 means it will always be on the top.

The next method is `OnKeyPressed`. This method is called by the Engine when user presses a key. Thanks to this, we can move the Player around.

The last method is `OnTriggerEnter`. This method is called by the Engine when a `RigidBody` has collided with other `Collider`.\
When the Player hits a `Collider` which is a Coin, we can pick up the Coin by increasing the amount of coins we have collected so far `_coins` and Destroying the `GameObject` of the Coin. Then we update the text to correctly display the amount of coins.
```csharp
internal class Player : GameObject
{
    public readonly TextRenderer _coinText;
    private readonly RigidBody _rb;
    private int _coins = 0;

    public Player()
    {
        _rb = AddComponent<RigidBody>();
        BoxCollider collider = GetComponent<BoxCollider>()!;
        collider.Size = new Vector(3, 3);

        SpriteRenderer renderer = AddComponent<SpriteRenderer>();
        renderer.Sprite = new Sprite([" o ", "\\|/", "/ \\"]);

        _coinText = AddComponent<TextRenderer>();
        _coinText.LocalPosition = new Vector(0, -2);
        _coinText.Layer = 0;
        _coinText.ColorTopLeft = Color.Yellow;
        _coinText.ColorTopRight = Color.Red;
    }

    public override void OnKeyPressed(ConsoleKeyInfo cki)
    {
        if (cki.Key == ConsoleKey.W)
        {
            _rb.Move(Vector.Up);
        }
        else if (cki.Key == ConsoleKey.S)
        {
            _rb.Move(Vector.Down);
        }
        else if (cki.Key == ConsoleKey.A)
        {
            _rb.Move(Vector.Left);
        }
        else if (cki.Key == ConsoleKey.D)
        {
            _rb.Move(Vector.Right);
        }
    }

    public override void OnTriggerEnter(Collider other)
    {
        if (other.GameObject is Coin coin)
        {
            _coins++;
            Engine.Destroy(coin);
            _coinText.Text = "C: " + _coins;
        }
    }
}
```
The Program.cs Main method will then look like this.
```csharp
static void Main(string[] args)
{
    // Without this the Console could print "?" instead of some characters
    Console.OutputEncoding = System.Text.Encoding.UTF8;

    // Let's clear the console first before we try to draw any GameObject
    Console.ResetColor();
    Console.BackgroundColor = ConsoleColor.DarkGray;
    Console.Clear();

    // Remember that doing "Player player = new Player()" will not work as the Engine would not know about the GameObject
    // Always use Engine.Instantiate<T>()
    Player player = Engine.Instantiate<Player>(new(Writer.Width / 2, Writer.Height / 2)); // Instantiate the Player
    player.Transform.Name = "Player"; // Not necessary

    // Instantiate the CoinSpawner
    CoinSpawner spawner = Engine.Instantiate<CoinSpawner>();

    Engine.Wait(); // This will make the game never end, unless the Engine is stopped (by pressing ESCAPE)
}
```
## Hint
The `RigidBody` is used when we want the object to be the one that collides.\
Note that you can move any object even without the `RigidBody` `Component` using "Transform.Position += new Vector(1,1);".\
Giving every object the `RigidBody` `Component` might cause lag because it would have to calculate many many many more collisions. Only add the `RigidBody` to the "main" objects.