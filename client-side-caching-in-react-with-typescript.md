---
title: "Client-side Caching in React with Typescript"
seoTitle: "Client-side Caching in React with Typescript"
seoDescription: "This Post show how to cache data in Single-Page-Applications, This method can be implemented on other Frameworks also."
datePublished: 2022-09-09T06:30:00.000Z
cuid: clniojgna000909kw7wzydlb7
slug: client-side-caching-in-react-with-typescript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696846987610/068be566-2999-4bce-b78d-121f3bab98af.png
tags: reactjs, typescript, clientside-caching

---

### This method is used in this portfolio site, So you can easily implement this method.

#### Create a new React Project with Typescript Support using the command

```shell
npx create-react-app <name> (or) npm create vite@latest
```

Create a typescript file Naming with CacheService or Anything Related to Cache 😅.  Create a class with name similar as the and export It,

```js
export class CacheService { }
```

then create a private static variable with name \*\*data \*\*

```js
export class CacheService 
{
  private static data: { key: string; value: any; }[] = []
}
```

Add a static method to the class with Set as the name that accepts 2 arguments

* Key
    
* Value ( Generic )
    

```js
  public static Set<T>(key: string, value: T) 
  {
    let contains = false;
    this.data.forEach(x => {
  if(x.key === key) 
  {
    contains = true;
  }
    });
    if (contains) {
  this.data = this.data.filter(x => x.key !== key);
    }
    this.data.push({ key, value });
  }
```

The complete method looks like this.

Then moving on to the Getting a value based on the key that's been passed.

Add a static method to the class with **Get** as the name that accepts 1 argument i,e \*\*Key. \*\*

**Note:** This Get method is also a Generic method.

```js
public static Get<T>(key: string): T | any | null {
  let val: T | any | null = null;
  this.data.filter(x => {
if (x.key === key)val = x;
  });
  return val?.value;
}
```

Now **GetAll** Method. This method is so simple to implement i,e Returning the **data** itself 😀.

```js
public static GetAll() {
  return this.data;
}
```

Now It's Time to implement whether the key exists or not in the cache. This is also simple to implement.

```js
public static HasKey(key: string) {
  let flag = false;
  this.data.forEach(x => {
if (x.key === key) {
  flag = true;
}
  });
  return flag;
}
```

Now the Last part is to make a method to delete data from Cache. This accepts only key and returns whether the data is deleted from Cache or not.

```js
public static Delete(key: string): boolean {
  let flag = false;
  this.data = this.data.filter(x => {
if(x.key === key) {
  flag = true;
}
return x.key !== key;
  })
  return flag;
}
```

Now the final Part is to Use and Test the written code.

To Get the data from the cache, simply call the ' **CacheService.Get(key, value) '** method

```js
const loggedIn = CacheService.Get<boolean>("login");
if (loggedIn) {
  console.log('You are Logged in');
} else {
  console.log('Please login again.');  
}
```

To Set a value to Cache

CacheService.Set("login", true);

To check whether a key exists in the Cache or not

```js
const contains = CacheService.HasKey("login");
if(contains) {
  console.log("Exists in Cache");
} else {
  console.log("Does not exists in Cache");
}
```

To Delete a key in Cache

```js
const deleted = CacheService.Delete("login");
if(deleted) {
  console.log("Successfully deleted from Cache");
} else {
  console.log("Key Doesn't Exist in Cache");
}
```

### This way, we can cache any type of data in-memory using this method. I've used this method in this Portfolio Site.

#### Caching can be very useful in RESTful Applications where data can be cached on client-side and we need not request every time the page loads.

#### Get the Full File here [Click here.](https://raw.githubusercontent.com/07prajwal2000/PersonalPortfolio/old_master/prajwal_portfolio/src/Services/CacheService.ts?token=GHSAT0AAAAAACB4IYRF2PJVYFVH53XSRELIZJDYS3Q)

##### NOTE: This method works only on Single-Page-Applications (SPA's). Once User refreshes the page, the cache will be cleared.