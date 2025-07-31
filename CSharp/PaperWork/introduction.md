# Basics
- Programming is about automation
  - We don't want to do something -> we tell the computer how to do it automatically
- The code in c# starts its execution in the Main method
  - From there the execution continues down line by line
  - There should only be one command on each line
    - Command is what ends with `;`
- Program is an algorithm
  - Step by step instruction how to create something
- We need 3 things to do this
  - Data types -> storing a variable
  - Flow control -> if something happens, do this, otherwise don't do this.
  - Repetition -> do this 5 times... or do it until something happens

### Recipe
```
We need: (Data types: how much of each ingredient we need)
- 1 liter of water
- 1 packet of sugar

We do: (Flow control: control whether we need to boil it)
if (water is cold){
  heat up the water
}

We do: (repetition: repeat until the water is sweet)
while (water is not sweet){
  put sugar into the water
}

We have: (return/output)
a sweet water
```

### So in code we do something like this:
This is advanced and stands only for a reference on how would it look like in an actual code. Please refer to this later as you go thru all the introduction lectures.
```csharp
public static void Main(string args[]){ // Main() is the method that is called when program.exe starts
  float waterAmount = 1f;
  SugarPacket sugarPacket = new SugarPacket(); // We create new sugar packet out of nowhere
  
  Water sweetWater = Cook(waterAmount, sugarPacket); // We call a method which is defined bellow
}

// We expect parametr saying how much water we want and a sugar as an ingredient.
// We return the sweetened Water.
public Water Cook(float waterAmount, SugarPacket sugarPacket){ 
  Water water = new Water(waterAmount);

  if (water.IsCold){ // If water is cold, heat it up
    water.HeatUp();
  }

  while (!water.IsSweet && !sugarPacket.IsEmpty){ // if water is not sweet AND the sugar packet has some sugar left, sweaten the water
    water.PutABitOfSugar(sugarPacket);
  }

  return water; // after the cooking, return the water which we have sweetend
}
```
Of course in a real world the universe already said what does it mean when water.IsCold or what water.HeatUp() does. In the program we need to implement that as ourselves.
So:
```csharp
// a class that contains information about the water and the function to heat it up or to sweeten it
public class Water {
  private float temp; // in kelvin
  private float amountOfWater;
  private float gramsOfSugar;

  public bool IsCold => temp < 273.15f + 25f; // return true if temp is below 25 °C
  public bool IsSweet => (gramsOfSugar / (amountOfWater + gramsOfSugar)) > 0.1f; // return true if there is more than 10% of sugar.
  // Assume 1 liter of water (1000 milliliters) == 1 kilogram (1000 grams)

  public Water(float amount) {
    this.amountOfWater = amount;
    this.temp = 273.15f + 22f; // a room temperature
  }

  public void HeatUp() {
    temp = 273.15f + 100f; // set to boil temperature
  }

  public void PutABitOfSugar(SugarPacket sugarPacket) { // to put a bit of sugar, we need a sugar packet to take it from
    float aBit = 10f;
    aBit = sugarPacket.TakeSugar(aBit); // we try to take 10 grams, if there is only lets say 5 grams left, we only take 5 grams
    this.gramsOfSugar += aBit; // if so, here we can only add 5 grams, otherwise we add 10 grams
  }
}
```
```csharp
// a class that contains information about the sugar packet and the function to take sugar from it
public class SugarPacket {
  float gramsOfSugar;

  public bool IsEmpty => gramsOfSugar == 0f;
  
  public SugarPacket() {
    gramsOfSugar = 1000f;
  }

  public float TakeSugar(float amountToTake) {
    if (amountToTake > this.gramsOfSugar) { // if we want to take more sugar then is left, we just take what is left
      amountToTake = this.gramsOfSugar;
    }
    
    this.gramsOfSugar -= amountToTake; // we subtract the value we are taking, the gramsOfSugar will be 0 if we took what is left
    return this.amountToTake; // tell the caller how much we took
  }
}
```
So now you know what can be done using some basic c# features. Now lets break them down one by one.

After each "chapter" you can come back here and see how much more you understand what is going on in the above. :)
\
\
\
\
PLEASE READ THIS:\
Before we continue, please note the naming convention of c#.

Naming convention is the "Rule" on how to name things. Violating naming convention causes worse readability of your code.\
The naming convention of c# is as follows:
- camelCase -> start with lower case letter and every word inside starts with UPPER case letter
- PascalCase -> start with UPPER case letter and every word inside starts with UPPER case letter
- snake_case -> lower cased with `_` between all words (this is not used in c#)
```csharp
private int someValue = 0; // camelCase for private variables (variable should never be public)
public int SomeProperty {get; set;} = 1; // PascalCase for public properties (property should never be private)
int SomeMethod() {} // PascalCase for all methods
class SomeClass {} // PascalCase for all classes
interface IComparable {} // PascalCase with I as first for interfaces // usually ends with "able" because of the usage of interfaces
enum OrderStatus {Pending, Fulfilled, Error} // Singular PascalCase for enum name, PascalCase for enum values
```

# Variables
- [Microsoft Docs](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)
- are used to store a value

C# is a strongly typed language which means that it is required to say which type the variable is when the variable is created\
Creating a variable consists of 2 steps
  - declaration
  - initialization
```csharp
int a; // declaration -> I declare a variable "a" to be the type of "int"
a = 2; // initialization -> I initialize a variable "a" with a value of "2"
int b = 3; // combined, most common to use
```
How to access a variables
```csharp
int a = 2;
int b = 3;
// notice we only say "int" when we "create a new variable" of that type
int c = a + b; // 5
```
Modification of a variable
```csharp
a = c - 2; // 3
a = a + 1; // 4
```
The name cannot start with a number, however it can contain a number
```csharp
int a1 = 0;
int a2a = 24;
```
There cannot be 2 variables Names or basically any "Name" which are the same
```csharp
int a = 1;
float a = 1.5f; // This will throw a Compile Error as "a" already exists in the current context
```
Quick +1 or -1
```csharp
int d = 2;
d++; // 3 // this is the same as d = d + 1;
d--; // 2 // this is the same as d = d - 1;
```
For advanced readers
```csharp
int a = 0;

// There is a difference between
a++;
a--;
// and
++a;
--a;

// Which is when is the number increased or decreased.
int b1 = 15;
int c1 = b1++;
// c1 = 15, b1 = 16
// Here the variable c1 is initialized with the value of b1 (15), then the value of b1 is increased by 1 (16).
// So it works like this
int c1 = b1;
b1++;

int b2 = 15;
int c2 = ++b2;
// c2 = 16, b2 = 16
// Here the b2 value is increased by 1 first (16), then the variable c2 is initialized with the value of b2 (16).
// So it works like this
b2++;
int c2 = b2;

// Simply said, if ++ is in front of the variable name, it first increases the value and then the line of code is executed
// If ++ is after the variable name, it first executes the line of code and then increases the value

// It is used to "compress" the code to a single line. There is nothing wrong when used on its own line.
// The reason I am explaining this is that you will encounter this very often in other people's code
```
There are also many [operators](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/)
- [Arithmetic operators](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/arithmetic-operators)
  - `a + b` addition
  - `a++` increment (by 1)
  - `a - b` subtraction
  - `a--` decrement (by 1)
  - `a * b` multiplication
  - `a / b` division
  - `a % b` remainder after division
- [Boolean logical operators](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/boolean-logical-operators)
  - `!a` NOT
  - `a & b` AND
  - `a && b` conditional AND
  - `a | b` OR
  - `a || b` conditional OR
- [Comparison operators](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/comparison-operators)
  - `a == b` equal
  - `a != b` not equal
  - `a < b` less than
  - `a <= b` less than or equal
  - `a > b` greater than
  - `a >= b` greater than or equal
- [Bitwise and shift operators](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operatorsbitwise-and-shift-operators)
  - `~a` bitwise complement (flips bits)
  - `a << b` left shift
  - `a >> b` right shift
  - `a >>> b` unsigned right shift
  - `a ! b` NOT
  - `a & b` AND
  - `a | b` OR
  - `a ^ b` XOR
- [Equality operators](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/equality-operators)
  - == equality
  - != inequality
- Final words
  - It may seem weird that there are a few operators multiple times. They are there just to show that they can represent multiple functions for specific data types.
  - You will use the arithmetic and Comparison the most and then !, && and ||. You will meet the others very rarely.
  - Many operators support compound assignment expression (`a op= b` which is `a = a op b`)
    - For example `a += b` is equivalent to `a = a + b`
## Data Types
### Whole Numbers
Most used variable is ``int``.  When working with big numbers sometimes ``long`` needs to be used.\
Notice that you can write ``_`` in the number, which is only for readability and is not changing the behavior of the number itself.
```csharp
// int (Int32) -> integer
// from -2,147,483,648 to 2,147,483,647 (inclusive)
int a = 1;
int b = -15;
int c = 1_000;
// long (Int64) -> long integer
// from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 (inclusive)
long d = 20_000_000_000;
```
### Decimal Numbers
There are 3 types for floating point number.
- `float` is used the most
- `double`  is advised to be used when doing more complicated Math operation
- `decimal` should be used when working with money
Each type needs its character to specify which one we want to choose.
- f -> float
- d -> double
- m -> decimal
Notice that you can write ``e`` in the number or ``_`` as stated earlier.
```csharp
// float (Single, 32bit) -> Floating point number
// 7 digit precision
float a = -0.5f;
float b = 2.3e4f; // 23000
// double (Double, 64bit) -> Double precision floating point number
// 15 digit precision
double c = 2.4d;
// decimal (Decimal, 128bit) -> Quadruple precision floating point number
// 28 digit precision
decimal d = 5.8m;
```
### Character and String
`char` is basically `int` in c#. We can use it interchangeably.
`string` cannot be mutated. Once it is initialized it can never be changed. It only can be initialized with a new value, while the old one is lost.
```csharp
// char (Char) -> a single Character
char a = 'a';
char b = 'A' + 1; // B
char c = 65; // A

// string (String) -> String/Array of characters
string a = "abc";
string b = 'a' + 'b'; // ab
string c = "ab" + 'c' + ' ' + 2; // abc 2
string d = "this string";
d = d + " is something else now" // this string is something else now
```
### Other Whole Numbers
A few types you can encounter. For the beginning you won't probably need them. There are a few more, but lets keep things simple.
```csharp
// short (Int16) -> short integer
// from -32,768 to 32,767 (inclusive)
short a = -51;
// ushort (UInt16) -> Unsigned short integer (only positive numbers)
// from 0 to 65,535 (inclusive)
ushort b = 120;
// uint (UInt32) -> Unsigned integer (only positive numbers)
// from 0 to 4,294,967,295 (inclusive)
uint c = 421;
// ulong (UInt64) -> Unsigned long integer (only positive numbers)
// from 0 to 18,446,744,073,709,551,615 (inclusive)
ulong c = 12_123_123_123_123_123_123;
```
### Array
An important data type is an array.\
It allows you to store multiple values in the same place.\
There are 2 ways to create an array
- By specifying its length (fills the array with the default value of the type -> int's default value is 0) (all numbers have 0 as a default value)
```csharp
int[] array = new int[5]; // Creates an array like this: {0,0,0,0,0}
int[] array2 = new int[3]; // Creates an array like this: {0,0,0}
```
- By specifying the values in the array
```csharp
int[] array = new int[]{5, 4, 8, 9, 6}; // Creates an array like this: {5,4,8,9,6}
int[] array2 = new int[]{7,3,9}; // Creates an array like this: {7,3,9}
```
We can initialize the array like this in newer versions of C#
```csharp
int[] array = [5, 4, 8, 9, 6]; // Creates an array like this: {5,4,8,9,6}
```
Then we can access the numbers in the array with index. Index always starts at 0 in C#.
```csharp
int[] array = [5, 4, 8, 9, 6];
Console.WriteLine(array[0]); // prints 5
Console.WriteLine(array[2]); // prints 8
```
We can edit its values like this.
```csharp
int[] array = [5, 4, 8, 9, 6];
array[0] = 7;
Console.WriteLine(array[0]); // prints 7
Console.WriteLine(array[2]); // still prints 8
```
To swap the numbers, for example the first two, we can do this.
```csharp
int[] array = [5, 4, 8, 9, 6];

int swap = array[0];
array[0] = array[1];
array[1] = swap;

Console.WriteLine(array[0]); // prints 4
Console.WriteLine(array[1]); // prints 5
Console.WriteLine(array[2]); // still prints 8
```
Newer versions of C# allows us to swap the numbers in more fancy way in one line.
```csharp
(array[0], array[1]) = (array[1], array[0]);
```
Why is that so complicated?
```csharp
int a = 2;
int b = 3;

// Because if we did this
a = b; // a = 3
b = a; // b = 3
// We would assign the value "3" to "a"
// Then we would assign the value of "a" which is "3" to "b"
// So both becomes the same value "3"
```
Therefore swapping requires other variable.
```csharp
int a = 2;
int b = 3;

int swap = a; // swap = 2
a = b; // a = 3
b = swap; // b = 2
```
Or in the newer versions of C#.
```csharp
int a = 2;
int b = 3;
(a, b) = (b, a);
```
Commonly with arrays, the for or foreach is utilized.
```csharp
int[] array = [5, 4, 8, 9, 6];
for (int i = 0; i < array.Length; i++){
  Console.WriteLine(array[i]);
}
// prints:
// 5
// 4
// 8
// 9
// 6
```
```csharp
int[] array = [5, 4, 8, 9, 6];
foreach (int number in array){
  Console.WriteLine(number);
}
// prints:
// 5
// 4
// 8
// 9
// 6
```
A common usage is for example sum of all numbers.
```csharp
int[] array = [5, 4, 8, 9, 6];

int sum = 0;
for (int i = 0; i < array.Length; i++){
  sum += array[i];
}

Console.WriteLine(sum); // prints 32
```
# Understanding the code
### Comments `//` `/**/`
- used when we want to write something into the code for ourselves

The comment will not be compiled.
```csharp
// this is a comment
// we can use it to explain what we are doing

// create variable "a" with a type of "int" and a value of "2"
int a = 2; // there it creates a new variable "a" which we can use later in the code
```
The above will be seen by the computer as follows.
```csharp
int a = 2;
```
There is also a multiline or inline comment.
```csharp
/*
 there are many line
 in one comment
*/

int a /* this variable is called a*/ = 3; // please never do comments like this :(
```
The above will be seen by the computer as follows.
```csharp
int a = 3;
```
### Variables
Are used to store a value or a reference.
```csharp
int a = 2;
char c = 'c';
Vector vector = new Vector(100, 150, 25);
int[] numbers = [5, 7, 8, 9, 6];
char[] chars = ['t', 'a', 'r'];
```
Can be one of two types:
- simple type (a value) (``struct``)
- reference type (a reference to a memory where all the values are) (used to pack things up together) (``class``)
```csharp
// simple/primitive data types
int a = 1;
float b = 2.2f;
char c = 'z';
Vector vector = new Vector(); // if created as struct, it becomes behaving like a simple type

// somewhere in the middle
string s = "The behavior of a string is a bit difficult to explain.";
string s2 = "String in general is a reference data type, but c# remade it into simple type.";
string s3 = "Because of the confusion, string was made immutable. Which makes him only usable as a simple data type.";
string s4 = "Therefore be careful and always use "string". Never use "String"";

// reference types
Regex regex = new Regex(); // exists in a library provided by Microsoft
Water water = new Water(); // a data type we can create ourselves // must be created as "class" to behave like reference type
int[] array = new int[2]; // Even tho the int is simple type, there it is "an array of integers" and array is always a reference type
Water[] waters = new Water[2]; // This is "an array of waters" so again it is a reference type
```
### Code body `{}`
Used to define which code is a part of a certain expression and which is not.
```csharp
if (something is true) {
  // do what is in the body
} else {
  // if something is false, do this
}

// then continue here
```
We can also use them without a keyword. (Almost never used but is possible.)
```csharp
{
  // one code goes here
  int a = 1;
}

// variable with the name "a" doesn't exist here

{
  // seconds goes here
  // both bodies are executed
  // this can be used to seal of variables from each other
  int a = 2; // this will not throw a Compile Error as any encapsulating body {} doesn't know about any variable named "a"
  {
    int a = 3; // this will throw a Compile Error as a variable with the name "a" already exists in the current context
  }
}
```
Code body `{}` is not needed to write in `if`, `else if`, `else`, `while`, `for`, `foreach` and `=>` cases if they only contain one line (rather I should say one command)
```csharp
if (somehing is true)
  Console.WriteLine("It was true!");
else
  Console.WriteLine("It was false!");

Console.WriteLine("Anyway lets continue here regardless of what happened.");
```
Or with the `=>`
```csharp
public void SayHello() => Console.WriteLine("Hello");

// is an equivalent of
public void SayHello() {
  Console.WriteLine("Hello");
}
```
#### if, else if, else
`if` consists of `condition check` and `code body`
```csharp
if (something is true) { // condition check
  // code body
}
```
`if` can help us control the flow and say what we want to execute.
```csharp
int myApples = 4; // I have 4 apples
int yourApples = 2; // You have 2 apples

if (myApples == yourApples + 2) { // if I have 2 more apples than you
  myApples--; // I will give you one apple
  yourApples++;
  Console.WriteLine("We both have the same amount of apples!");
} else if (yourApples == myApples + 2) { // if I don't have 2 more apples than you BUT you have 2 move apples than I have
  yourApples--; // you will give one apple to me
  myApples++;
  Console.WriteLine("We both have the same amount of apples!");
} else { // in any other case
  Console.WriteLine("One of us have more apples than the other.");
}
```
Of course you can write more `else if`s or nest them. Nesting means you put one into the other
```csharp
int randomNumber = 3;
if (randomNumber == 0){
  Console.WriteLine("It was 0!");
} else if (randomNumber == 1){
  Console.WriteLine("It was 1!");
} else if (randomNumber == 2){
  Console.WriteLine("It was 2!");
} else if (randomNumber == 3){
  Console.WriteLine("It was 3!");
} else if (randomNumber == 4){
  Console.WriteLine("It was 4!");
} else {
  // nested if
  if (randomNumber % 2 == 0) { // is even
    Console.WriteLine("I don't know what number it is, but I know it is even!");
  } else {
    Console.WriteLine("I don't know what number it is, but I know it is odd!");
  }
}
```
#### while
`while` consists of `condition check` and `code body`
```csharp
while (something is true) { // condition check
  // code body
}
```
It basically does this. `While something is true, repeat all this.`
```csharp
while (something is true) { // if something is true
  // do everything in the code body

  // when all the code in the code body was executed, go again to the condition check
}
```
`while` is a loop used when we want to repeat something until something happens.
```csharp
// something we wait for
int countDown = 5;

// repeat infinitely
while (true) {
  countDown--; // change something in each cycle

  if (countDown <= 0){ // if count down finished, break
    break; // break means "jump out of the repetition", basically out of the code body "{}", which ends the "infinite" loop
  }
}
// the break jumps to this line
Console.WriteLine("Count down finished!");
```
The above could be written like this.
```csharp
int countDown = 5;

while (--countDown > 0) {} // does nothing in the body but decreases the countDown in the condition check

Console.WriteLine("Count down finished!");
```
#### for and foreach
`for` consists of `value initialization`, `condition check`, `end-of-cycle-command` and `code body`
```csharp
for (int i = 0 /*value initialization*/; i < 5 /*condition check*/; i++ /*end-of-cycle-command*/) {
  // code body
}
```
Like this without the comments.
```csharp
for (int i = 0 ; i < 5 ; i++ ) {
  Console.WriteLine(i);
}
```
It is a simplified way to write an iterable while. So the above and the below are equivalent. `for` is always used in such case for readability.
```csharp
int i = 0;
while (i < 5) {
  Console.WriteLine(i);

  i++;
}
```
`foreach` consists of `value initialization`, keyword `in`, `array to iterate` and `code body`
```csharp
foreach (int number /*value initialization*/ in /*keyword in*/ array /*array to iterate*/) {
  // code body
}
```
Like this without the comments.
```csharp
int[] array = [4, 8, 6];
foreach (int number in array) {
  Console.WriteLine(number);
}
```
`for` and `foreach` are mostly used to iterate over an `array` or a `List`. In the future you will figure out that anything that is `IEnumerable` can be used to iterate.\
Main difference is that `for` is used when we need to know the index and `foreach` is used when we do not.
```csharp
for (int i = 0; i < 3; i++){
  Console.WriteLine(i);
}
// prints:
// 0
// 1
// 2
```
```csharp
int[] arr = [4, 5, 6];
for (int i = 0; i < arr.Length; i++){
  Console.WriteLine(i + ") " + arr[i]);
}
// prints:
// 0) 4
// 1) 5
// 2) 6
```
```csharp
int[] arr = [4,5,6];
foreach (int number in arr){
  Console.WriteLine(number);
}
// prints:
// 4
// 5
// 6
```
### Methods (Functions)
- used when we need to:
  - give a name to a certain code
  - execute a certain code multiple times across the program
  - tidy the code and put it where we can find it later
- method is defined by:
  - access modifier
    - `private` (default, if not stated otherwise)
    - `public` (mostly used)
    - `internal` (used commonly in libraries ".dll" when we don't want other programs to use that method on their own)
    - `protected` (can only be used by a class in which it is defined or in inheriting classes)
  - followed by other modifiers
    - `static` (you will mostly use this before you will learn about OOP (classes, structs and such))
    - many others (there are many possibilities, I will talk about them in other lecture)
  - followed by the return type
    - the data type that the method return
    - `void` if nothing is returned
  - followed by its Name
    - by conventions of c# the Name must be in PascalCase having the first letter capitalized
  - parameters
    - after name we put `()` in which we specify parameters
    - the Method is not required to use any parameters and can be parameterless `()`
    - parametr has:
      - data type (which data type the parameter has to be)
      - local name (doesn't need to match the name from where it is called, name of the parameter only applies to the value in the body of this method)

Parameterless method that doesn't return anything
```csharp
public static void SayHello() {
  Console.WriteLine("Hello!");
}
```
Method with a parameter "name" of the type "string" which doesn't return anything
```csharp
public static void SayHelloTo(string name) { // example: if name == "John"
  Console.WriteLine("Hello " + name + "!"); // then this prints: "Hello John!"
}
```
Method that deals damage to the player
```csharp
public void TakeDamage(int damage) {
  this.hp -= damage;

  if (this.hp <= 0) {
    Die();
  }
}
```
In full context would look like this:
```csharp
public class Player {
  private int hp;

  public Player() {
    hp = 100;
  }

  public void TakeDamage(int damage) { // because it is not static, it takes the hp from the one instance of a Player we call it on
    this.hp -= damage;

    if (this.hp <= 0) {
      Die();
    }
  }

}
```
The call would look like this
```csharp
public static void Main(string args[]) {
  Player player = new Player();
  player.TakeDamage(10);
}
```
Method that sums two double numbers, returns the sum
```csharp
public static double Add(double a, double b) {
  return a + b;
}
```
Methods can also have array of any variable types as parameters, as "array" is just another reference data type
```csharp
public static double Sum1(double[] arr) {
  double sum = 0d;
  for (int i = 0; i < arr.Length; i++) { // iterates over all the indices in the array (basically counts from 0 inclusive to arr.Length exclusive)
    sum += arr[i]; // add the value at the index "i" in the provided array "arr" to the total sum "sum"
  }

  return sum;
}
```
Method can use any amount of arguments without putting them into an array manually before calling the method
```csharp
public static double Sum2(params double[] arr) {
  double sum = 0d;
  for (int i = 0; i < arr.Length; i++) { // iterates over all the indices in the array (basically counts from 0 inclusive to arr.Length exclusive)
    sum += arr[i]; // add the value at the index "i" in the provided array "arr" to the total sum "sum"
  }

  return sum;
}
```
The call would look like this
```csharp
public static void Main(string args[]) {
  double d1 = 2.0d;
  double d2 = 3.0d;
  double value1 = Add(d1, d2); // adds these 2
  double value2 = Sum1(new double[] {d1, d2}); // creating an array manually
  double value3 = Sum2(d1, d2); // the array creates automatically thanks to the "params" modifier keyword in the method

  // value1 == value2 == value3 == 5
}
```

### Class
- is always a `reference type` (is an `object`)
- used to pack things together
```csharp
public class Player {
  private string name;
  private int hp;
  private Weapon weapon;
}
```
- used to tidy the code and put it where it belongs
```csharp
public class Player {
  private string name;
  private int hp;
  private Weapon weapon;

  public bool TryShoot() {
    if (weapon is null)
      return false;

    return weapon.TryShoot();
  }
}

public class Weapon {
  private int ammo = 30;

  public bool TryShoot() {
    if (ammo <= 0)
      return false;

    Shoot();
    return true;
  }

  public void Shoot() {
    ammo--;
  }
}
```
#### Inheritance
- can inherit form other classes (can only inherit from one class)
- can inherit from interfaces (can inherit from any amount of interfaces)
```csharp
// class isn't required to inherit
public class Player {

}

// but it can inherit a class
public class Player : Entity {

}

// or it can inherit interfaces
public class Player : IDamageable, IMessageable, ITargetable {

}

// or it can inherit both, but at most 1 class
public class Player : Entity, IDamageable, IMessageable, ITargetable {

}
```
Probably will be used like this
```csharp
// inherits IDamageable, ITargetable
public class Entity : IDamageable, ITargetable {

}

// inherits Entity and IMessageable, inheriting Entity makes it inherit both IDamageable and ITargetable automatically
public class Player : Entity, IMessageable {

}

public class NPC : Entity {

}
// Explanation:
// Entity is expected to be targetable and damageable which means player can damage anything that is an entity and can target anything that is an entity
// Because player inherits from it, that means the player can damage other players and can target other players
// But also NPC inherits from Entity which means player can also damage NPC as well as NPC can damage player
// Because player inherits IMessageable, it means we can send a message to a player, but we cannot send a message to an NPC because NPC doesn't inherit IMessageable
```