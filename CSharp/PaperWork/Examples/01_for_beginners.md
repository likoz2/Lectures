# Examples for beginners
Every example builds on something shown previously hoping for cumulative learning.\
Create new project using `vs code` or `visual studio`.\
In case you haven't downloaded any IDE yet, you can use online compilers. 
- [programiz.com](https://www.programiz.com/csharp-programming/online-compiler/)
- [dotnetfiddle.net](https://dotnetfiddle.net/)

Note that the online compilers won't necessary have the latest C# version or will not have full functionality.

## Lets get started
### Basics
Each program when executed starts at Main function. It executes all lines of code from top to bottom. Once it reaches the `}` which belongs to the `Main()` the application ends.\
We can stop the application from ending immediately by writing `Console.ReadLine()` which will be expecting an input from the user. The input is send when user hits `Enter`.\
After the input is sent, the app reaches the `}` and ends.

For now, the `args` are expected to be empty and can be ignored. I will show how args work later.

What are we even doing?\
We are creating a `console application`. That is a type of app which works in/with the console.\
Therefore we can interact with the `Console`.


The 2 main interactions are writing and reading.\
The `static` `class` `Console` has methods `WriteLine` and `Write` which displays text in the console.\
`\n` is a character for end of line. `\r\n` is used on Windows. I terms of Windows console `\n` is just fine.
```csharp
public static void Main(string[] args) {
    Console.WriteLine("Hello user!");

    Console.Write("Hello");
    Console.Write(" user!\n");

    Console.ReadLine(); // stop the console from closing immediately
}
```
The `static` `class` `Console` has a method `ReadLine` which reads text from the console.
```csharp
public static void Main(string[] args) {
    Console.WriteLine("Hello user!");
    Console.WriteLine("What's your name?");

    string name = Console.ReadLine();
    Console.WriteLine("Nice to meet you " + name + "!");

    Console.ReadLine(); // stop the console from closing immediately
}
```
### Input variations
Now that we know how to interact with user, lets see what can we do with it.

There are many variable types we can get from the user.\
To get a variable from a `string` we use the method `Parse` from the variable type we want to parse into.
```csharp
public static void Main(string[] args) {
    string input = "2";

    int number = int.Parse(input);
    Console.WriteLine(number);

    Console.ReadLine(); // stop the console from closing immediately
}
```
Similar to above, we can choose any variable type. It is usual in C# for classes to have the `Parse` method implemented.\
This gives us as the programmer a way to crete a variable from a `string`.\
```csharp
static void Main(string[] args) {
    string input = "2";

    int number = int.Parse(input);
    long longNumber = long.Parse(input);
    float floatNumber = float.Parse(input);
    double doubleNumber = double.Parse(input);
    decimal decimalNumber = decimal.Parse(input);

    Console.WriteLine(number);
    Console.WriteLine(longNumber);
    Console.WriteLine(floatNumber);
    Console.WriteLine(doubleNumber);
    Console.WriteLine(decimalNumber);

    Console.ReadLine(); // stop the console from closing immediately
}
```
### String manipulation
Other manipulation we can do with the `string` is splitting it into `char`s.
```csharp
public static void Main(string[] args) {
    string input = "18";

    char[] chars = input.ToCharArray();
    foreach (char c in chars) {
        Console.WriteLine(c);
    }

    Console.ReadLine(); // stop the console from closing immediately
}
```
We can also split the `string`. Splitting by `" "` means we split it into words.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    string[] arr = input.Split(" ");
    foreach (string s in arr) {
        Console.WriteLine(s);
    }

    Console.ReadLine(); // stop the console from closing immediately
}
```
StringSplitOption can be added to specify special behavior.
- RemoveEmptyEntries
  - Omit array elements that contain an empty string from the result.
- TrimEntries
  - Trim white-space characters from each substring in the result.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    string[] arr = input.Split(" ", StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in arr) {
        Console.WriteLine(s);
    }

    Console.ReadLine(); // stop the console from closing immediately
}
```
To use both options do this.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    string[] arr = input.Split(" ", StringSplitOptions.RemoveEmptyEntries | StringSplitOptions.TrimEntries);
    foreach (string s in arr) {
        Console.WriteLine(s);
    }

    Console.ReadLine(); // stop the console from closing immediately
}
```
We can also measure the length of the `string`.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    int length = input.Length;
    Console.WriteLine("The length is: " + length);

    Console.ReadLine(); // stop the console from closing immediately
}
```
We can also get certain `char` from the `string`.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    Console.WriteLine("The first char is: " + input[0]);

    Console.ReadLine(); // stop the console from closing immediately
}
```
We can now try to count certain `char`s in the `string`.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    int count = 0;
    for (int i = 0; i < input.Length; i++) {
        if (input[i] == 'i') {
            count++;
        }
    }
    Console.WriteLine("The letter \"i\" was present " + count + "x times!"); 

    Console.ReadLine(); // stop the console from closing immediately
}
```
With that we can try to count multiple `char`s in the `string`.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    int count = 0;
    for (int i = 0; i < input.Length; i++) {
        if (input[i] == 'i' || input[i] == 's') {
            count++;
        }
    }
    Console.WriteLine("The letter \"i\" or \"s\" were present " + count + "x times!"); 

    Console.ReadLine(); // stop the console from closing immediately
}
```
We can try to count `char`s in order. Notice the `- 1` in the `.Length`. This is here so we don't go out of bounds. Try to remove the `- 1` in your code and see what happens.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";

    int count = 0;
    for (int i = 0; i < input.Length - 1; i++) {
        if (input[i] == 'i' && input[i+1] == 's') {
            count++;
        }
    }
    Console.WriteLine("The word \"is\" was present " + count + "x times!"); 

    Console.ReadLine(); // stop the console from closing immediately
}
```
To expand this to any word, we need to make the method more generic.\
We will utilize a method called `Substring`. This method gets a `string` of a specified `Length` instead of just one `char`.\
Instead of `- 1` as before, when trying to look for 2 `char`s, we now have to subtract `word.Length - 1`, as the `word` can be any `Length`.
```csharp
public static void Main(string[] args) {
    string input = "This is a long input.";
    string word = "is";

    int count = 0;
    for (int i = 0; i < input.Length - (word.Length-1); i++) {
        if (input.Substring(i, word.Length) == word) {
            count++;
        }
    }
    Console.WriteLine("The word \"is\" was present " + count + "x times!");

    Console.ReadLine(); // stop the console from closing immediately
}
```
If you are confused by the `(word.Length-1)`, try to run this. You will understand.
```csharp
public static void Main(string[] args) {
    string input = "is";
    string word = "is";

    int count = 0;
    //for (int i = 0; i < input.Length - (word.Length-1); i++) {
    for (int i = 0; i < input.Length - word.Length; i++) {
        Console.WriteLine(input.Substring(i, word.Length));
        if (input.Substring(i, word.Length) == word) {
            count++;
        }
    }
    Console.WriteLine("The word \"is\" was present " + count + "x times!");

    Console.ReadLine(); // stop the console from closing immediately
}
```
### Feeling bored?
Feeling bored part will sometimes jumps in to entertain you. :)\
Try to play with this. You don't need to understand it! Just run it and see what happens. :)\
For more about [Random](https://learn.microsoft.com/en-us/dotnet/api/system.random.-ctor?view=net-9.0&redirectedfrom=MSDN#System_Random__ctor).
```csharp
public static void Main(string[] args) {
    string text = "Very long text for you to play with.";
    Random rnd = new Random();
    ConsoleColor[] colors = Enum.GetValues<ConsoleColor>();

    string[] arr = text.Split();
    Console.BackgroundColor = ConsoleColor.Green;
    Console.WriteLine(text);
    Console.ResetColor();
    foreach (string s in arr)
    {
        Console.ForegroundColor = colors[rnd.Next(0, colors.Length)];
        Console.WriteLine(s);
    }
    Console.ResetColor();

    //Console.ReadLine();
    //Console.Clear();

    Console.ReadLine(); // stop the console from closing immediately
}
```
Can you print each `char` with a different `color`?\
Can you create a rainbow effect, where the `color`s are repeating in order?\
Can you draw a checker board?\
Can you draw a chess board? (If you want to feel extra, there are unicode characters for the pieces. `♙♘♗♖♕♔♚♛♜♝♞♟`)\
HINT: If you are using windows cmd to run your code, probably the case if you are using visual studio, instead of vs code,\
    you can set the console's font to something that is 10x20 pixels so you can create an actual square instead of a rectangle. :)
### A Simple Game
Now that we know how to interact with user, lets play a game.
```csharp
public static void Main(string[] args) {
            Console.WriteLine("Hello user!");
            Console.WriteLine("What's your name?");

            string name = Console.ReadLine(); // We can store the value for later
            Console.WriteLine("Nice to meet you " + name + "!");

            // Note the two ways to write out a string

            Console.WriteLine($"Do you want to play a game {name}?");
            while (Console.ReadLine() != "yes") { // Or we can just one-time check the value and never use it again // In this case we don't need a variable
                Console.WriteLine("Incorrect answer!");
            }

            Console.WriteLine("Yeah! Lets play a game!");

            Random rnd = new Random(); // Random should always be initialized once in the whole program
            while (true) {
                int randomNumber = rnd.Next(1, 11);
                Console.WriteLine("Guess a number between 1-10!");

                string input = Console.ReadLine();
                if (input == "exit")
                    break;

                if (input is null || input == string.Empty) // This is a standard way to check whether the string is Empty or not
                    continue;

                if (int.TryParse(input, out int guessedNumber)) { // Can you see the advantages of using TryParse instead of Parse?
                    if (guessedNumber == randomNumber) {
                        Console.WriteLine("Wow you guessed it right!");
                    } else {
                        Console.WriteLine("Bad luck.");
                    }
                } else {
                    Console.WriteLine("That is not even a number!?");
                }
            }

            Console.WriteLine("App finished.");
            Console.ReadLine(); // stop the console from closing immediately
}
```
This game was an example on how you can create a simple program that is actually a game. Now you can create basically anything you can imagine.\
There are many possibilities with the console so don't rush to `Unity` yet. (Or any other Game Engine)\
How creative can you be with the console and with what you have learned so far?\
For example, with this knowledge, I created a game that was 2 stickmen in the console throwing fireballs at each other until one of them died. Can you do the same?