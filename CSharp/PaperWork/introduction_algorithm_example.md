# Summarization
In the introduction you have been introduced to multiple C# features. \
Now that you know it exists we can build upon that and actually get to coding. \
There is a lot of theory at the start of C#. But once you grasp on the basics, there is nothing more and everything just behaves the same way.

## Your first app
In your first app we want to get the computer print some output.
```csharp
public static void Main(string[] args) {
    int a = 2;
    int b = 3;

    Console.WriteLine(a + b); // prints 5
}
```

Okay that ain't much but it's an honest start.

## Your first algorithm
Lets create a sorting algorithm that sorts 4 values.
```csharp
public static void Main(string[] args) {
    int a = 5;
    int b = 3;
    int c = 1;
    int d = 2;

    if (a > b) {
        int swap = a;
        a = b;
        b = swap;
    }
    if (b > c) {
        int swap = b;
        b = c;
        c = swap;
    }
    if (c > d) {
        int swap = c;
        c = d;
        d = swap;
    }

    if (a > b) {
        int swap = a;
        a = b;
        b = swap;
    }
    if (b > c) {
        int swap = b;
        b = c;
        c = swap;
    }

    if (a > b) {
        int swap = a;
        a = b;
        b = swap;
    }

    Console.WriteLine("a: " + a); // prints 1
    Console.WriteLine("b: " + b); // prints 2
    Console.WriteLine("c: " + c); // prints 3
    Console.WriteLine("d: " + d); // prints 5
}
```
You can notice 2 main things. First being how I utilize empty lines. Second how I don't try to sort everything 3 times, as each sort iteration makes the last number the biggest.
- if you don't understand how the sort works, feel free to take a pen and a paper and write it down, follow the instructions and and try to sort the numbers on the paper
  - having a pen and a paper is very common in programming, don't feel bad for doing so, it is often absolutely necessary
  - for example I have a drawing board right next to me and whenever I stumble upon some issue, I draw it on the board

## Your second algorithm
Also you can notice that this is too many lines and only works for 4 numbers.\
So lets create a function that sorts an array.
```csharp
public static void Main(string[] args) {
    int[] arr = [4, 3, 5, 2, 1];

    for (int i = 0; i < arr.Length - 1; i++) {
        for (int j = 0; j < arr.Length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int swap = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = swap;
            }
        }
    }

    for (int i = 0; i < arr.Length; i++) {
        Console.WriteLine(i + ") " + arr[i]);
    }
}
```
This might seem chaotic at first so lets break it down.

First we randomly by hand create an array.
```csharp
int[] arr = [4, 3, 5, 2, 1];
```
Next we copy the swap `if a > b` that we used before and make it work with an array.
```csharp
int[] arr = [4, 3, 5, 2, 1];

if (arr[0] > arr[0 + 1]) {
    int swap = arr[0];
    arr[0] = arr[0 + 1];
    arr[0 + 1] = swap;
}
```
Now lets see what is the `for` doing.
```csharp
for (int i = 0; i < 5; i++) {
    // The for can seem a little hard to read but all we care about is the 5 there.
    // That means the for will count from 0 inclusive to 5 exclusive
    // Which means it will iterate thru all indices of the array.
    // 0, 1, 2, 3, 4 -> Which is 5 repetitions, which means that the for makes the same amount of repetitions as the number said.
}
```
We can therefore read the `for` as this and ignore the other text.
```csharp
for (5){

}

// can be read as

repeat (5 times){

}
```
Next we create for and can notice that we must not exceed the bound with the indexing. It is always better to start creating the logic from the inside.\
That is the reason why I first created the `if a > b` and now I continue with the inner loop (inner `for`)\
Now for the inner for:
```csharp
for (int j = 0; j < arr.Length - 1; j++) {
    // because we index "arr[0]" and "arr[0+1]" we must subtract 1 from the length
    // note that there are 2 ways of for that you will ever use, first being the one with "arr.Length" repetitions and the second being "arr.Length - 1" repetitions
    // even tho there are infinitely many possible things you can code, you will never use anything else in the for
    // so all the confusion aside:
    //      use ".Length" if you want to iterate over the full array
    //      use ".Length - 1" if you are looking not only at the current "arr[i]" but also for the next "arr[i+1]"

    // the reason for that is if array's max index is 4 and we iterate over the entire array
    // last cycle will be i = 4, so when we call arr[i+1] it asks for index 5 which is out of bounds and the program crashes
    // therefore we need to stop at index .Length -1 which would be 3 and arr[3+1] exists, so it runs normally
}
```
Now we have this which goes once over the array and swaps numbers that are in the wrong order.
```csharp
int[] arr = [4, 3, 5, 2, 1];

for (int j = 0; j < arr.Length - 1; j++) {
    if (arr[j] > arr[j + 1]) {
        int swap = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = swap;
    }
}
```
We need to keep swapping them as long as there is something to swap. Therefore we add a surrounding `for` which repeats the sort process `.Length` times.
```csharp
int[] arr = [4, 3, 5, 2, 1];

for (int i = 0; i < arr.Length; i++) {
    for (int j = 0; j < arr.Length - 1; j++) {
        if (arr[j] > arr[j + 1]) {
            int swap = arr[j];
            arr[j] = arr[j + 1];
            arr[j + 1] = swap;
        }
    }
}

// The reason for repeating the outer for .Length times is that each cycle moves the smallest number to the beginning of the array.
// So if the smallest number is at the end of the array when the sort starts, it needs .Length iterations to move it to the beginning, therefore sort it to its correct position.
// Again, if you find it confusing, use a pen and a paper to figure it out.
```
And there we have it. This algorithm can be optimized and made much faster. In the original code I provided was a part of the optimization which I put aside in this explanation to keep things simple.\
You can try to come up with optimization features on your own. For example imagine what happens when the array is sorted before we even begin to sort? Or imagine it is sorted after the first iteration. Can you update it so it skips unnecessary computation when its already sorted?

This sort is called the `Bubble sort` and is not very effective. Our implementation is just a simple showcase of an actual algorithm.\
If you ever stumble upon a sorting needs in coding, there is a Library that does it for you. (And is faster than what you would implement most likely.)
```csharp
int[] arr = [4, 3, 5, 2, 1];
Array.Sort(arr);
```
And its done :)
