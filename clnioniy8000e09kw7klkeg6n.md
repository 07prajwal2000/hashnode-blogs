---
title: "Asynchronous Programming in C#"
seoTitle: "Asynchronous Programming in C#"
seoDescription: "The Asynchronous Programming in C# is a way to write Programs that run in multiple threads, which allows the main thread to execute other programs."
datePublished: 2022-09-30T06:30:00.000Z
cuid: clnioniy8000e09kw7klkeg6n
slug: asynchronous-programming-in-csharp
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696846473519/b0ea6569-5e02-46f1-90a9-1612908213cc.png
tags: csharp, asynchronous, async, dotnet

---

# C# Asynchronous Programming

C# was the first programming language which implemented the Asynchronous Programming in 2011 and officially released on 2012, At that time the version of the language was C# 5.0\. Later then other Programming languages like Javascript, Typescript, Python just to name a few.

The Asynchronous Programming in C# is a way to write Programs that run in multiple threads, which allows the main thread to execute other programs.

By default in C# when we create a Task from async methods it runs on a Thread pool created by the CLR ( Common Language Runtime ) on startup of the application.

To make a method Async use async keyword on the method after the access-modifiers.

**for example**

```csharp
public async Task RunInAsync() { }
```

The task is used to keep track of the method is Completed the execution or not and also if any async methods that return a value they must return a task with a generic type

**for example**

```csharp
public async Task<User> RunInAsync() { }
```

### So the C# 's asynchronous topics are very complex and huge. There are many books which covers only these topics.


| Synchronous | Asynchronous |
| ------ | ------ |
| Blocks main thread | Runs on separate thread and doesn't block main thread |
| Easy to write programs | Async programs are complex and hard to understand |
| Runs a bit faster than async | This needs to spawn a thread or find a thread pool and run on it. |
| Sharing of data is easier | Sharing of the data from main thread to other thread is harder or sometimes not possible or lead to race conditions |
| Waiting or pausing the thread will freeze the program | Waiting or pausing will only block the child thread and don't affect the main thread |


Now without wasting time lets get started with the implementation.

#### Create a new C# Console Project and open in your favorite IDE. 

```shell
dotnet new console
```

I am using Dotnet 6.0 stable release. You can use any version greater than Dotnet core 3.1 .

## Example 01 : Reading and Writing Files from Disk.

Create a class and name it whatever you like and Read and Write methods to it.

```csharp
internal class FileAccessDemo
{
  public void ReadFile() { }
  public void WriteFile() { }
}
```

These methods run on main thread and will run synchronously.

```csharp
internal class FileAccessDemo
{
  public void ReadFile()
  {
    var path = @"D:\AsyncDemo\read-file.txt";
    var fileInfo = new FileInfo(path);
    var bytes = new byte[fileInfo.Length];

    using var fs = new FileStream(path, FileMode.OpenOrCreate);

    fs.Read(bytes);

    var data = Encoding.UTF8.GetString(bytes);
  }

  public void WriteFile()
  {
    var path = @"D:\AsyncDemo\write-file.txt";
    using var fs = new FileStream(path, FileMode.OpenOrCreate);

    var modelAsString = JsonSerializer.Serialize(new
    {
    FirstName = "Prajwal",
    LastName = "Aradhya"
    });

    var data = Encoding.UTF8.GetBytes(modelAsString); // Json Data

    fs.Write(data);
  }
}
```

Then add ReadFileAsync and WriteFileAsync methods

```csharp
internal class FileAccessDemo
{
  public async Task ReadFileAsync()
  {
    var path = @"D:\AsyncDemo\read-file.txt";
    var fileInfo = new FileInfo(path);
    var bytes = new byte[fileInfo.Length];

    using var fs = new FileStream(path, FileMode.OpenOrCreate);
    await fs.ReadAsync(bytes);

    var data = Encoding.UTF8.GetString(bytes);
  }

  public async Task WriteFileAsync()
  {
    var path = @"D:\AsyncDemo\write-file-async.txt";
    using var fs = new FileStream(path, FileMode.OpenOrCreate);

    var modelAsString = JsonSerializer.Serialize(new
    {
      FirstName = "Prajwal",
      LastName = "Aradhya"
    });

    var data = Encoding.UTF8.GetBytes(modelAsString); // Json Data

    await fs.WriteAsync(data);
  }
}
```

## Example 2 : Calling an API or Web services

Async / await can be used to call Web API's and Web services. By using the `HttpClient` from class from the C# we are able to request data from web servers.

Now create a class with any name you want,

internal class HttpClientDemo { }

I'm calling Google for the sake of demonstration, This runs synchronously.

```csharp
internal class HttpClientDemo
{
  public void CallTo_WebService()
  {
    var url = "https://www.google.com/";

    using var client = new HttpClient();
    using var request = new HttpRequestMessage(HttpMethod.Get, url);
    using var response = client.Send(request);
    using var stream = new MemoryStream();

    response.Content.CopyTo(stream, null, default);
    stream.Flush();
  }
}
```

The HttpClient has built-in mechanism for Async, Lets use that one

```csharp
internal class HttpClientDemo
{
  public async Task CallTo_WebServiceAsync()
  {
    var url = "https://www.google.com/";

    using var client = new HttpClient();
    using var request = new HttpRequestMessage(HttpMethod.Get, url);
    using var response = await client.SendAsync(request);

    await using var stream = new MemoryStream();

    await response.Content.CopyToAsync(stream);
    await stream.FlushAsync();
  }
}
```

## Example 3 : Running a Synchronous methods Asynchronous

This question was posted in all over the internet, and so many of them answered this question and i will to,

Here is how to do it in most simple way.

```csharp
  public async Task RunInAsync()
  {
    await Task.Run(() =>
    {
      var i = 0;
      while (i++ < 10_000_000) // 10M Executions might block main thread.
      {
        // Write Synchronous code here.
      }
    });
  }
```

This function runs a single Task and spawns single thread for execution.

```csharp
  public async Task RunMultipleTasksAsync()
  {
    await Task.Run(() =>
    {
      var i = 0;
      while (i++ < 1_000_000)
      {
        // Write Synchronous code here.
      }
    });

    // Async function with Return value.
    var iterations = await Task.Run(() =>
    {
      var i = 0;
      while (i++ < 1_000_000)
      {
        // Write Synchronous code here.
      }
      return i;
    });

  }
```

You can return some results from Asynchronous functions also. This approach of writing multiple async functions will be slow in terms of performance because for every async method execution it needs to wait until the previous thread completes and it runs is serial.

The next method i will show you is How to speed the multiple async functions without waiting for every single one. This will run in parallel.

```csharp
  public async Task RunMultipleTasks_FastAsync()
  {
    var t1 = Task.Run(() =>
    {
      var i = 0;
      while (i++ < 1_000_000)
      {
        // Write Synchronous code here.
      }
    });
    var t2 = Task.Run(() =>
    {
      var i = 0;
      while (i++ < 1_000_000)
      {
        // Write Synchronous code here.
      }
    });
    var t3 = Task.Run(() =>
    {
      var i = 0;
      while (i++ < 1_000_000)
      {
        // Write Synchronous code here.
      }
    });

    await Task.WhenAll(t1, t2, t3); // Waits until all the tasks to complete execution
  }
```

When you want to return a value synchronously and also asynchronously , We can return `ValueTask` instead of `Task` Object

here is a small code snippet
```csharp
  public async ValueTask<int> SyncAndAsyncReturns()
  {
    var random = new Random();
    var i = random.Next(0, 150);

    if (i < 85)
    {
      return i;
    }

    await Task.Run(() =>
    {
      while (i++ < 250)
      {

      }
    });
    return i;
  }
```
These are some of the ways you can speed things up in your C# Dotnet Applications by using these better programming patterns and styles.

The whole Project repository is in <span style="text-decoration: underline; color: rgb(224, 62, 45);">[Github, you can click here](https://github.com/07prajwal2000/AsynchronousDemo)</span>

Thank you for reading this post. Hope you liked it 😀 .