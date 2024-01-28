# Resources
[Creating a .NET Class Library using VS Code](https://learn.microsoft.com/en-us/dotnet/core/tutorials/library-with-visual-studio-code?pivots=dotnet-8-0)
[Testing a .NET Class Library using VS Code](https://learn.microsoft.com/en-us/dotnet/core/tutorials/testing-library-with-visual-studio-code?pivots=dotnet-8-0)

# Assignment 2 - GitHub actions - build and test
## Creating The App
### Prerequisites
- [Visual Studio Code](https://code.visualstudio.com/) with the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) installed
- The [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)


### Step One: Creating a solution
- We start by creating a blank solution to put our class library project in. The soultion serves as a container for one or more projects. We'll add additional, related projects to the same solution
    - Create a project directory (a folder)
    - Open the terminal and run <br>
    `dotnet new sln`

### Step Two: Creating a class library project
- We then add a new .NET clas library project named "NameOfProject" to the soltuion
    - In the terminal, run the following command to create the library project <br>
    `dotnet new classlib -o NameOfProject `
    - Run thew following command to add the libray project to the solution <br>
    `dotnet sln add NameOfProject/NameOfProject.csproj`
    - The `TargetFramework` element shows that the project targets .NET 8.0 
    ```XML
    <Project Sdk="Microsoft.NET.Sdk">

        <PropertyGroup>
            <TargetFramework>net8.0</TargetFramework>
        </PropertyGroup>

    </Project>
    ```
    - Open Class1.cs and replace the code with the following code.
    ```C#
    namespace UtilityLibraries;

    public static class NameOfProject
    {
        public static bool StartsWithUpper(this string? str)
        {
            if (string.IsNullOrWhiteSpace(str))
                return false;

            char ch = str[0];
            return char.IsUpper(ch);
        }
    }
    ```
        
    - Save your changes
    - Save the file <br>
    ```dotnet build```

### Step Three: Add a console app to the solution
Add a console application that uses the class library. The app will prompt the user to enter a string and report whether the string begins with an uppercase character. 
- In the terminal, run the following command to create the console app project:
```dotnet new console -o NameOfConsoleApp```
- Run the following command to add the console app project to the solution
```dotnet sln add NameOfConsoleApp/NameOfConsoleApp.csproj```
- Open NameOfConsoleApp/Program.cs and replace all of the code with the following code 
```C#
using UtilityLibraries;

class Program
{
    static void Main(string[] args)
    {
        int row = 0;

        do
        {
            if (row == 0 || row >= 25)
                ResetConsole();

            string? input = Console.ReadLine();
            if (string.IsNullOrEmpty(input)) break;
            Console.WriteLine($"Input: {input}");
            Console.WriteLine("Begins with uppercase? " +
                 $"{(input.StartsWithUpper() ? "Yes" : "No")}");
            Console.WriteLine();
            row += 4;
        } while (true);
        return;

        // Declare a ResetConsole local method
        void ResetConsole()
        {
            if (row > 0)
            {
                Console.WriteLine("Press any key to continue...");
                Console.ReadKey();
            }
            Console.Clear();
            Console.WriteLine($"{Environment.NewLine}Press <Enter> only to exit; otherwise, enter a string and press <Enter>:{Environment.NewLine}");
            row = 3;
        }
    }
}
```

- Save your changes

### Step Four: Add a project reference
- Run the following command dotnet<br>
```dotnet add NameOfConsoleApp/NameOfConsoleApp.csproj reference NameOfProject/NameOfProject.csproj```  

### Step Five: Run the app
- Run the following command in the terminal<br>
```dotnet run --project NameOfConsoleApp/NameOfConsoleApp.csproj```

## Creating The Tests
### Step One: Creating the test
- Create a unit test project named "StringLibraryTest"
```
dotnet new mstest -o StringLibraryTest
```
- add the test project to the solution
```
dotnet sln add StringLibraryTest/StringLibraryTest.csproj
```
### Step Two: Add a project reference
- Run the following command
```dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj```

### Step Three: Run the test
- Run the following command
```
dotnet test StringLibraryTest/StringLibraryTest.csproj
```