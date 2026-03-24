---
title: "Builder Design Pattern in C# Dotnet"
seoTitle: "Builder Design Pattern in C# Dotnet"
seoDescription: "A Common and Easy design pattern every developer should know. Here I showed an example of how to use the Builder Design Pattern to make the MySql Connection"
datePublished: 2023-01-08T06:30:00.000Z
cuid: clnip8fsh000p0amifgfp0ex1
slug: builder-design-pattern-in-csharp-dotnet
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696845966536/ed835080-aabf-43da-99be-ae8304a03ad3.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1696845919228/d7c6d55a-b485-41e5-9aa0-7c92ad45e1e4.png
tags: design-patterns, csharp, dotnet, builder-pattern

---

According to [Wikipedia](https://en.wikipedia.org/wiki/Builder_pattern), The Builder pattern is a design pattern designed to provide a flexible solution to various object creation problems in object-oriented programming. The builder design pattern intends to separate a complex object's construction from its representation.

In this post, I will talk about how to implement the Builder pattern in C# in a more clear and concise way. I will be creating a class, and a couple of interfaces to help for restricting the access of methods. 

There are 2 types of Creating a Builder pattern,
1. There will be a flow to create anything from start to finish.
2. No flow will be there, and this is used for building conditions or filtering <u>etc...</u>

First, we need to create a C# project (any type of project(template) is good).

# The Example here I am going to take is a builder for MySql Connection String. This is one of the basic example to understand how the builder pattern works.

## I will go by Step-by-Step
# STEP 1: Creating a C# file named <u>MySqlConnectionBuilder</u>

Inside that file, It should contain similar to this
```csharp
public sealed class MySqlConnectionBuilder { }
```

 > I have made it the sealed class, so it should not be inherited by any other class.

# STEP 2: Five Interfaces need to be created
These interfaces will help restrict access to other methods by calling randomly.


I've also defined the MethodName, Those names are self-explained.

```csharp
public interface IMySqlPasswordConstructor
{
    IMySqlHostConstructor WithPassword(in string password);
}

public interface IMySqlHostConstructor
{
    IMySqlPortConstructor HasHost(in string host);
}

public interface IMySqlPortConstructor
{
    IMySqlDatabaseConstructor HavingPort(in int port);
}

public interface IMySqlDatabaseConstructor
{
    IMySqlFinalConfigBuilder ContainsDatabase(in string databaseName);
}

public interface IMySqlFinalConfigBuilder
{
    string Build();
}
```
Just for convenience and better readability, I created one more interface and implemented all 5 to it, and that, like the below one. This is just an empty interface that has nothing on it.

```csharp
public interface IMySqlConnectionBuilder : IMySqlPasswordConstructor, IMySqlHostConstructor,
    IMySqlPortConstructor, IMySqlFinalConfigBuilder, IMySqlDatabaseConstructor { }
``` 

# STEP 3: The BIG Picture. Implementing the Interface to the Main class that has been created in Step - 1

This step is not as hard as you think it is, It is super simple just needs a bit more concentration on how everything is structured.

```csharp
using System.Text;

public sealed class MySqlConnectionBuilder : IMySqlConnectionBuilder
{
    private readonly StringBuilder _stringBuilder = new();

    private MySqlConnectionBuilder() { }

    public static IMySqlPasswordConstructor FromUsername(in string username)
    {
        var builder = new MySqlConnectionBuilder();
        builder._stringBuilder.AppendFormat("uid={0};", username);
        return builder;
    }
    
    public IMySqlHostConstructor WithPassword(in string password)
    {
        _stringBuilder.AppendFormat("pwd={0};", password);
        return this;
    }

    public IMySqlPortConstructor HasHost(in string host)
    {
        _stringBuilder.AppendFormat("server={0};", host);
        return this;
    }

    public IMySqlDatabaseConstructor HavingPort(in int port)
    {
        _stringBuilder.AppendFormat("port={0};", port);
        return this;
    }

    public IMySqlFinalConfigBuilder ContainsDatabase(in string dbName)
    {
        _stringBuilder.AppendFormat("database={0};", dbName);
        return this;
    }

    public string Build()
    {
        return _stringBuilder.ToString();
    }
}
```

Here I've used string builder for creating the connection string, as it allocates the memory only when we call 
<u>ToString()</u> Method. 

And you might have not noticed, that I made the constructor private, GOTCHA 😜, Just kidding.
Why I made it private because it should not be created outside this class just because of the restrictions that I've mentioned above. Instead, One function can be made static for the entry point to create a class. 

# STEP 4: The Testing
This project uses Dotnet 7.0 and Top level statements are enabled. If the Project's Dotnet version is 5.0 or below, write the code inside the Main() function.

```csharp
var connectionString = MySqlConnectionBuilder
    .FromUsername("prajwal")
    .WithPassword("12345")
    .HasHost("127.0.0.1")
    .HavingPort(3306)
    .ContainsDatabase("builder_pattern_demo")
    .Build();

Console.WriteLine(connectionString);

// output: uid=prajwal;pwd=12345;server=127.0.0.1;port=3306;database=builder_pattern_demo;
```

# TL;DR. Here is the link for the [GitHub repository](https://github.com/07prajwal2000/Builder-Pattern-Demo).