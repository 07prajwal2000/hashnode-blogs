---
title: "Javascript: Var vs Let vs Const"
seoTitle: "Javascript: Var vs Let vs Const"
seoDescription: "Here are some of the differences between var, let and const keywords in JavaScript."
datePublished: 2024-02-27T18:03:25.575Z
cuid: clt4oexaf000509lb1rpmfws4
slug: javascript-var-vs-let-vs-const
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1709056879247/03734d7b-ba0c-46a0-902e-f2751dfa346d.png
tags: variables, javascript, typescript

---

**Var** is the first keyword introduced to Javascript for declaring variables. This keyword is different from others and has unique properties and use cases. So before ES6 was introduced most people used the **var** keyword, but nowadays people stopped using this keyword because of these new keywords

ES6 Introduced 2 new keywords for declaring variables in Javascript.

1. Let: Accessible only Block level and can be reassigned.
    
2. Const: Accessible only Block level and can't be reassigned.
    

Introducing new keywords made **var** obsolete.

## Differences

|  | Var | Let | Const |
| --- | --- | --- | --- |
| Scope | Anywhere | Block | Block |
| Reassign | Can be Reassigned | Can be Reassigned | Cannot be Reassigned |
| Redeclaration | Supported | Not Supported | Not Supported |
| Hoisting | Supported | Not Supported | Not Supported |
| Initialize on Declaration | Not required | Not required | Required |

## Code Examples

* Var Keyword
    

```javascript
var name = "Prajwal Aradhya";
var counter = 3;
console.log(name); // Prajwal Aradhya
if (counter > 2) {
    var name = "Aradhya, Prajwal";
}
console.log(name); // Aradhya, Prajwal
```

* Let Keyword
    

```javascript
let name = "Prajwal Aradhya";
let age = 10;

if (age % 5 === 0) {
    console.log("😁");
} else {
    console.log("😟");
}
```

* Const Keyword
    

```javascript
const title = "A good movie";
const rating = 5;
// Movie "A good movie" has 5 🌟 rating.
console.log(`Movie "${title}" has ${rating} 🌟 rating.`);
```