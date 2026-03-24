---
title: "Mastering File and Directory Operations in C# .NET"
seoTitle: "Mastering File and Directory Operations in C# .NET"
seoDescription: "A Fully Comprehensive Guide for exploring the features and different ways for efficient file(s) and directory management in C# Dotnet."
datePublished: 2023-10-08T18:30:00.000Z
cuid: clnj11yhb000509lc07itausx
slug: mastering-file-and-directory-operations-in-csharp
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696864917874/42a6679a-c4a3-490f-a82f-d3c1d4423405.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1696865799009/d6a3294a-3387-49da-9de0-7a009e747fce.png
tags: csharp, dotnet, dotnetcore, filesystem

---

Working With Files and Folders in C#.Net is a breeze and super-easy. This blog will give you a complete guide to managing files and folders in an intuitive way.

# The Namespace

We need to use the namespace to get all the Classes and methods available.

> In Dotnet 6 and up, using statement is optional, because there will be a global usings file that takes care of these imports which we do not required to add this statement.

```csharp
using System.IO;
```

# Working with the Files

There are many ways and classes for accessing files, Here are some of the class names

* `System.IO.File`
    
* `System.IO.FileStream`
    
* `System.IO.FileInfo`
    
* `System.IO.StreamReader` and `System.IO.StreamWriter`
    

### Creating a File

Let's create a file by using one of the classes mentioned above.

```csharp
using System.IO;

var path = "c:\\temp.txt"; // Absolute path
path = "temp.txt"; // Relative path
using var fs = File.Create(path); // using File class
using var fs1 = new FileStream(path, FileMode.Create);

var fileInfo = new FileInfo(path); // takes only single argument
using var fs = fileInfo.Create();
```

The `Create()` method takes one argument - FileName (It can be absolute or relative to the project where it is running) and returns a `FileStream` class which should be disposed to avoid memory leaks, the using keyword will take care of the disposal of the class. The `FileStream` constructor takes multiple arguments, we just need to focus on creating a file.

> **Quick Note**: If a file exists in the path, it will be overwritten with empty contents.

### Reading a file

Now, let's look at reading a file from the disk. Here, for reading the same classes will apply that mentioned above.

```csharp
    private void ReadFile_FileClass()
    {
        var text = File.ReadAllText("temp.txt"); // returns string
        string[] lines = File.ReadAllLines("temp.txt"); // returns string[] of lines
        byte[] fileAsBytes = File.ReadAllBytes("temp.txt"); // returns binary data
    }
    
    private void ReadFile_FileStreamClass()
    {
        using var fs = new FileStream("temp.txt", FileMode.Open);
        var buffer = new byte[1024];
        var totalRead = fs.Read(buffer, 0, buffer.Length);
    }
    
    private void ReadFile_FileInfoClass()
    {
        var fileInfo = new FileInfo("temp.txt");
        using var fs = fileInfo.OpenRead();
        var buffer = new byte[1024];
        var totalRead = fs.Read(buffer, 0, buffer.Length);
    }

    private void ReadFile_StreamReaderClass()
    {
        using var fs = new StreamReader("temp.txt");
        var charBuffer = new char[1024];
        var totalRead = fs.Read(charBuffer); // Reads the string
        var fileContents = new string(charBuffer);
    }
```

The `File` the class has a bunch of methods, like Reading files as string, getting the lines as string\[\], and a bytes buffer.

But the last one I,e `StreamReader` class is special because it is used only for reading string content, The constructor accepts file paths, and other streams like NetworkStream, MemoryStream, *etc*. This class is very useful for reading string content.

### Writing Files

Okay, now let's talk about writing files.

```csharp
    private void WriteFile_FileClass()
    {
        File.WriteAllText("temp.txt", "hello world");
        File.AppendAllText("temp.txt", "hello world appended");

        File.WriteAllBytes("temp.txt", Encoding.UTF8.GetBytes("Hello world from bytes"));

        var lines = new[] {"Line 1", "Line 2"};
        File.WriteAllLines("temp.txt", lines);

        var linesToAppend = new[] {"Line 1 append", "Line 2 append"};
        File.AppendAllLines("temp.txt", linesToAppend);
    }
    
    private void WriteFile_FileStreamClass()
    {
        using var fs = new FileStream("temp.txt", FileMode.Open);
        var buffer = Encoding.UTF8.GetBytes("Hello world");
        fs.Write(buffer);
    }
    
    private void WriteFile_FileInfoClass()
    {
        var fileInfo = new FileInfo("temp.txt");
        using var fs = fileInfo.OpenRead();
        var buffer = Encoding.UTF8.GetBytes("Hello world");
        fs.Write(buffer);
    }
    
    private void WriteFile_StreamReaderClass()
    {
        using var fs = new StreamWriter("temp.txt");
        var strContents = "Hello world";
        fs.Write(strContents); // anything which supports ToString() method
        fs.WriteLine(strContents); // same here
    }
```

The `File` the class has methods that are helpful like Writing and Appending Text, Bytes, and Lines of Text.

> **Note for Read/Write operations**: If the file doesn't exist in the hard drive, it will throw an exception like `FileNotFoundException`. Make sure to handle those cases as well.

### Other IO Operations like Exists/Copy/Move/Delete

The Dotnet provides functionality to do these kinds of operations very easily. Here is an example of that.

```csharp
var fileExists = File.Exists("temp.txt");
File.Move("temp.txt", "folder/temp.txt");
File.Copy("temp.txt", "folder/temp.txt");
File.Delete("folder/temp.txt");

var file = new FileInfo("temp.txt");
fileExists = file.Exists;
file.MoveTo("destination/temp.txt");
file.Delete();
file.CopyTo("destination/temp.txt");
```

* Exists: This method is used to check if the file exists in the disk (or) not.
    
* Move/MoveTo: Will move the file from source to destination.
    
* Copy/CopyTo: Will copy the file and its contents to the specified destination.
    
* Delete: Will delete the file from the disk.
    

> **Warning** ⚠: The Move, Copy and Delete will throw Exceptions if file doesn't exist **or** locked/used by some other process.

# Working with the Directories 📁

Directory operation is similar to files. The dotnet provides classes for handling this stuff more efficiently.

Here we have the list of classes

* `System.IO.Directory`
    
* `System.IO.DirectoryInfo`
    

Let's start by getting a list of directories in a **Directory**.

### Listing Directories

Both classes have support for Listing the Sub-directories.

```csharp
var path = "path_of_the_dir";
var directoryInfo = new DirectoryInfo(path);
string[] dirPaths = Directory.GetDirectories(path);
DirectoryInfo[] directoryInfos = directoryInfo.GetDirectories();
```

The static method `Directory.GetDirectories()` will return string\[\] containing all the directories in it. But `directoryInfo.GetDirectories()` will give you an instance of the class `DirectoryInfo` . The DirectoryInfo class contains some extra methods for other operations that are helpful.

### Listing Directories with a pattern

There are 2 types of patterns. Note that the pattern means they don't support Regex.

| Pattern | Description |
| --- | --- |
| \* | Zero or more characters in that position. |
| ? | Exactly one character in that position. |

```csharp
var path = "C:\\Personal\\Temp\\folder ex";
var pattern = "*folder_*";
var directoryInfo = new DirectoryInfo(path);
string[] dirPaths = Directory.GetDirectories(path, pattern, SearchOption.TopDirectoryOnly);
DirectoryInfo[] directoryInfos = directoryInfo.GetDirectories(pattern, SearchOption.AllDirectories);

foreach (var dirInfo in directoryInfos)
{
    Console.WriteLine(dirInfo.Name);
}
```

Folder structure and output

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696860550858/5dc188c6-c271-4e6b-a1df-af0e72e862d6.png align="center")

The `GetDirectories()` the method has an overload for the SearchPattern enum.

* SearchPattern.TopDirectoryOnly: Gets all folders in the current directory.
    
* SearchPattern.AllDirectories: Gets all the folders and sub-folders in the current directory.
    

### Get Root Directory

Gets the root folder/directory

* In Windows: Gets the Drive name
    
* In Mac/Linux: Gets the root folder
    

```csharp
var path = "path";
var root = Directory.GetDirectoryRoot(path);
// output: C:\
```

### Other Directory Operations: Move/Delete

* Deletion of a folder, there are 2 options.
    
    1. The folder must be empty to delete.
        
    2. Use the Recursive flag to remove all files and folders.
        
* Moving a folder needs two paths, A source and a destination path.
    

```csharp
// Deleting directory
var path = "path";
var recursiveDeletion = true;
Directory.Delete(path);
Directory.Delete(path, recursiveDeletion);

// Moving directory
Directory.Move("source", "destination");
```

### Listing files in a Directory

When you list out the files in a directory, you get the absolute path to each file.

```csharp
var path = "C:\\Personal\\Temp\\folder ex";
var pattern = "*.txt";
var files = Directory.GetFiles(path);
Console.WriteLine("Without pattern: " + JsonSerializer.Serialize(files, new JsonSerializerOptions{WriteIndented = true}));
files = Directory.GetFiles(path, pattern);
Console.WriteLine("With pattern: " + JsonSerializer.Serialize(files, new JsonSerializerOptions{WriteIndented = true}));
```

Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696862587251/24873f7e-f49d-4a65-aca1-5beebd6c1d23.png align="center")

> Using DirectoryInfo will give the FileInfo\[\] instead of string\[\].

### Optional: Getting Logical Drive

Returns the path of Logical drives defined in your operating system.

```csharp
string[] drivePaths = Directory.GetLogicalDrives();
```

### My Thoughts

* Use `File` and `Directory` class for quickly accessing the files and folders as it gives freedom with static class and easy-to-call methods.
    
* A better approach is to use `FileInfo` or `DirectoryInfo` class because it has a lot of features to deal with files and folders.
    

**What about you guys, Share your thoughts and ideas in the comment section below.**

Thank you for reading till the end. Here is the [Github repository](https://github.com/07prajwal2000/WorkingWithDirectoryAndFilesInCsharp) of this project.