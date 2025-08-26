# Goal
The goal of this article is to help you setup the Lecture_SmallGame example.
# Sections
- Setup
    - Visual Studio
    - Visual Studio Code
- Summary
# Prerequisites
- Requisite
  - Windows OS

The Lecture_SmallGame uses Windows console as an interface. Other OS terminals might not work. Built-in consoles of IDEs can glitch.
# Setup
VS will automatically pop up the Windows CMD. It is recommended to use when you already have it installed.\
However VS takes a lot of space on disc when installed. Therefore VScode is otherwise advised to be used with any programming lang.

If you have VS installed I recommend you to use it.
## Visual Studio
1. Clone my [repo](https://github.com/likoz2/Lecture_SmallGame/tree/master).
    - Open VS
    - Select "Clone a repository"
    - Put in the [url]([repo](https://github.com/likoz2/Lecture_SmallGame/tree/master))
    - Click "Clone"
2. Run the project
    - Press F5 or click "Start" to run the project
3. Common issues
    - The app may crash on start
        - Moving the console window to the top left corner of your screen or making it fullscreen should solve it
        - Maybe your screen is too small, go to the source code of the engine and in `SmallEngine/Writer.cs` change the values `Width` and `Height` so they fit in your screen.
## Visual Studio Code
1. Clone my [repo](https://github.com/likoz2/Lecture_SmallGame/tree/master).
    - Open VScode
    - Open new window
    - Select "Clone Git Repository"
    - Put in the [url]([repo](https://github.com/likoz2/Lecture_SmallGame/tree/master))
    - Enter your project name
    - Press Enter
2. Run the project
    - Press F5 or click "Start" to run the project
        - When you run it for the first time, it will ask you how you want to run it (in the top middle screen prompt), simply select the default or first options
    - Another way is to open Windows cmd and execute the app from there
        1. Build the app first (Simply by trying to run it in the VScode)
        2. Open `Command Prompt`
        3. Navigate to the folder
            - dir /B => this command shows you a list of folders and files in your current folder
            - cd FolderName => this command moves you to a subfolder
            - your path would look something like this "C:\Users\username\Documents\\...\Lecture_SmallGame\Lecture_SmallGame\bin\Debug\net8.0"
            - There you can see something like "Lecture_SmallGame.exe"
        4. Execute the executable with this command "Lecture_SmallGame.exe"
3. Common issues
    - The app may crash on start (When run with the `Command Prompt`)
        - Moving the console window to the top left corner of your screen or making it fullscreen should solve it
        - Maybe your screen is too small, go to the source code of the engine and in `SmallEngine/Writer.cs` change the values `Width` and `Height` so they fit in your screen.
