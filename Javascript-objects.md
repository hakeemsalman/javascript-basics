
# Objects

- [Objects](#objects)
- [Object References](#object-references)
- [object comparision](#object-comparision)
- [Conts object can be modified](#conts-object-can-be-modified)
- [Cloning and merging, Object.assign](#cloning-and-merging-objectassign)
  - [object.assign](#objectassign)
  - [nested cloning](#nested-cloning)
  - [deep cloning](#deep-cloning)
- [Object methods](#object-methods)
  - [method shorthand](#method-shorthand)
    - [this method](#this-method)
    - ["this" is not bound](#this-is-not-bound)
    - [Arrow functions are special:](#arrow-functions-are-special)
- [Constructor](#constructor)
    - [Single - not reuse - new function](#single---not-reuse---new-function)
  - [Return from Constructors](#return-from-constructors)
- [Optional chaining](#optional-chaining)
    - [Use Cases](#use-cases)
    - [Without Optional Chaining](#without-optional-chaining)
    - [With Optional Chaining](#with-optional-chaining)
    - [Invalid for Assignment](#invalid-for-assignment)
- [Symbol](#symbol)
  - [Symbols as Object Keys](#symbols-as-object-keys)
  - [Symbol Properties are Skipped in Loops](#symbol-properties-are-skipped-in-loops)
  - [Global Symbols with Symbol.for()](#global-symbols-with-symbolfor)
- [Conversation rules](#conversation-rules)
  - [Auto conversion](#auto-conversion)
  - [Why Does This Matter?](#why-does-this-matter)
  - [Conversion “Hints”](#conversion-hints)
  - [Algorithm (Order of Attempts)](#algorithm-order-of-attempts)
  - [Core Methods Explained](#core-methods-explained)
    - [1. `Symbol.toPrimitive`](#1-symboltoprimitive)
    - [2. `toString()`](#2-tostring)
    - [3. `valueOf()`](#3-valueof)
  - [Workings in Practice](#workings-in-practice)
  - [**Key Points You Can Include in Notes**](#key-points-you-can-include-in-notes)

```javascript
let user = {     // an object
  name: "John",  // by key "name" store value "John"
  age: 30        // by key "age" store value 30
};
```

- A property has a key (also known as “name” or “identifier”) before the colon `":"` and a value to the right of it.
-  Accessing Properties
	- Dot notation: user.name
	- Bracket notation: user["name"] — useful for dynamic keys and multiwords.
```javascript
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // multiword property name must be quoted
};
```
	- We can use square brackets in an object literal, when creating an object. That’s called _computed properties_.

- The last property in the list may end with a comma, called a “trailing” or “hanging” comma.
- Delete a property
```js
delete user.age;          // Delete
```
- properties have the same names as variables. The use-case of making a property from a variable is so common, that there’s a special _property value shorthand_ to make it shorter.


```javascript
function makeUser(name, age) {
  return {
    name, // same as name: name
    age,  // same as age: age
    // ...
  };
}
```


- there are no limitations on property names. They can be any strings or symbols. For instance, a number `0` becomes a string `"0"` when used as a property key:

```javascript
// these properties are all right
let obj = {
  for: 1,
  let: 2,
  return: 3
};

alert( obj.for + obj.let + obj.return );  // 6
```

Check property existence:
- There will be no error if the property doesn’t exist!
- We check property in Object using, `in` operator
	- `"key"` `in` object

```javascript
let user = { name: "John", age: 30 };

alert( "age" in user ); // true, user.age exists
alert( "blabla" in user ); // false, user.blabla doesn't exist
```

Please note that on the left side of `in` there must be a _property name_. That’s usually a quoted string.

If we omit quotes, that means a variable should contain the actual name to be tested.

```javascript
let user = { age: 30 };

let key = "age";
alert( key in user ); // true, property "age" exists
```

For instance, let’s output all properties of `user`:

```javascript
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // keys
  alert( key );  // name, age, isAdmin
  // values for the keys
  alert( user[key] ); // John, 30, true
}
```

- “ordered in a special fashion”: integer properties are `sorted`, others appear in creation order. The details follow.

As an example, let’s consider an object with the phone codes:

```javascript
let codes = {
  "49": "Germany",
  "41": "Switzerland",
  "44": "Great Britain",
  // ..,
  "1": "USA"
};

for (let code in codes) {
  alert(code); // 1, 41, 44, 49
}
```

Above all objects are called a “plain object”, or just `Object`.

There are many other kinds of objects in JavaScript:

- `Array` to store ordered data collections,
- `Date` to store the information about the date and time,
- `Error` to store the information about an error.
- …And so on.


objects are stored and copied “by reference”, whereas primitive values: strings, numbers, booleans, etc – are always copied “as a whole value”.

# Object References 
Here we put a copy of `message` into `phrase`:

```javascript
let message = "Hello!";
let phrase = message;
```
As a result we have two independent variables, each one storing the string `"Hello!"`.
![[IMG_6675.png]]

**A variable assigned to an object stores not the object itself, but its “address in memory” – in other words “a reference” to it.**

```javascript
let user = {name: "salman"}
```
The object is stored somewhere in memory (at the right of the picture), while the `user` variable (at the left) has a “reference” to it.

![[IMG_6676.png]]


```js
let user = { name: "John" };

let admin = user; // copy the reference
```
Now we have two variables, each storing a reference to the same object, there’s still one object, but now with two variables that reference it.

![[IMG_6674 1.png]]

We can use either variable to access the object and modify its content.

```javascript
let user = { name: 'John' };

let admin = user;

admin.name = 'Pete'; // changed by the "admin" reference

alert(user.name); // 'Pete', changes are seen from the "user" reference
```

# object comparision
  
1. Objects are compared by reference, not by content.
Even if two objects have the exact same properties and values, they are not equal unless they reference the same memory location.

```js
let a = {};
let b = a;
console.log(a === b); // true (same reference)
```


```js
let a = {};
let b = {};
console.log(a === b); // false (different objects)
```

Because JavaScript objects are stored by reference. So:
- == and === check if the two operands point to the same object, not if they “look” the same.

# Conts object can be modified

```javascript
const user = {
  name: "John"
};

user.name = "Pete"; // (*)

alert(user.name); // Pete
```

The value of `user` is constant, it must always reference the same object, but properties of that object are free to change.

In other words, the `const user` gives an error only if we try to set `user=...` as a whole.

# Cloning and merging, Object.assign

We can create a new object and replicate the structure of the existing one, by iterating over its properties and copying them on the primitive level.

```javascript
let user = {
  name: "John",
  age: 30
};

let clone = {}; // the new empty object

// let's copy all user properties into it
for (let key in user) {
  clone[key] = user[key];
}

// now clone is a fully independent object with the same content
clone.name = "Pete"; // changed the data in it

alert( user.name ); // still John in the original object
```

## object.assign

The syntax is:

```javascript
Object.assign(dest, ...sources)
```

- The first argument `dest` is a target object.
- Further arguments is a list of source objects.

It copies the properties of all source objects into the target `dest`, and then returns it as the result.

Eg:

```js
let user = {
  name: "John",
  age: 30
};

let clone = {}; // the new empty object

Object.assign(clone, user);

// now clone is a fully independent object with the same content
clone.name = "Pete"; // changed the data in it

alert( user.name ); // still John in the original object
```

## nested cloning

```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, same object

// user and clone share sizes
user.sizes.width = 60;    // change a property from one place
alert(clone.sizes.width); // 60, get the result from the other one
```

Now it’s not enough to copy `clone.sizes = user.sizes`, because `user.sizes` is an object, and will be copied by reference, so `clone` and `user` will share the same sizes:

## deep cloning 

we should use a cloning loop that examines each value of `user[key]` and, if it’s an object, then replicate its structure as well.
That is called a “deep cloning” or “structured cloning”. There’s [structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone) method that implements deep cloning.

The call `structuredClone(object)` clones the `object`with all nested properties.

```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = structuredClone(user);

alert( user.sizes === clone.sizes ); // false, different objects

// user and clone are totally unrelated now
user.sizes.width = 60;    // change a property from one place
alert(clone.sizes.width); // 50, not related
```

- The `structuredClone` method can clone most data types, such as objects, arrays, primitive values. *(Except **function**)*

# Object methods

A function that is a property of an object is called its _method_.

So, here we’ve got a method `sayHi` of the object `user`.

```javascript
let user = {
  name: "John",
  age: 30
};

user.sayHi = function() {
  alert("Hello!");
};

user.sayHi(); // Hello!
```

we could use a pre-declared function as a method, like this:

```javascript
let user = {
  // ...
};

// first, declare
function sayHi() {
  alert("Hello!");
}

// then add as a method
user.sayHi = sayHi;

user.sayHi(); // Hello!
```

## method shorthand

```javascript
// these objects do the same

user = {
  sayHi: function() {
    alert("Hello");
  }
};

// method shorthand looks better, right?
user = {
  sayHi() { // same as "sayHi: function(){...}"
    alert("Hello");
  }
};
```

### this method
- **To access the object, a method can use the `this` keyword.**
- Rules:
	- `this` do not look at object definition. Only the moment of call matters.
	- If the value of `this` inside `foo()` is `undefined`, because it is called as a function, not as a method with “dot” syntax.

```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // "this" is the "current object"
    alert(this.name);
  }

};

user.sayHi(); // John
```

### "this" is not bound
The value of `this` is evaluated during the run-time, depending on the context.

```javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

> Calling without an object: `this == undefined`

We can even call the function without an object at all:

```javascript
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```

In this case `this` is `undefined` in strict mode. If we try to access `this.name`, there will be an error.

In non-strict mode the value of `this` in such case will be the _global object_ (`window` in a browser, we’ll get to it later in the chapter [Global object](https://javascript.info/global-object)). This is a historical behavior that `"use strict"` fixes.

Usually such call is a programming error. If there’s `this`inside a function, it expects to be called in an object context.

### Arrow functions are special:
- they don’t have their “own” `this`. If we reference `this` from such a function, it’s taken from the outer “normal” function.

For instance, here `arrow()` uses `this` from the outer `user.sayHi()` method:

```javascript
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```

That’s a special feature of arrow functions, it’s useful when we actually do not want to have a separate `this`, but rather to take it from the outer context.

Try this [Question](https://javascript.info/task/object-property-this), you’ll get better understanding on it.

---

# Constructor
Constructor functions technically are regular functions. There are two conventions though:

1. They are named with **capital letter first**.
2. They should be executed only with `"new"` operator.

```javascript
function Person(name) {
  this.name = name;
  this.sayHi = function () {
    console.log(`Hi, I am ${this.name}`);
  };
}

let user = new Person("Salman");
user.sayHi(); // Hi, I am Salman
```

When a function is executed with `new`, it does the following steps:

1. A new empty object is created and assigned to `this`.
2. The function body executes. Usually it modifies `this`, adds new properties to it.
3. The value of `this` is returned.

In other words, `new Person(...)` does something like:

```javascript
function Person(name) {
  // this = {};  (implicitly)

  // add properties to this
  this.name = name;
  this.sayHi(){
     console.log(`Hi, I am ${this.name}`);
  }

  // return this;  (implicitly)
}
```

Of course, we can add to `this` not only properties, but methods as well.

So `let person = new Person("Salman")` gives the same result as:

```javascript
let person = {
  name: "Salman",
  sayHi(){
   console.log(`Hi, I am ${this.name}`);
  }
};
```

### Single - not reuse - new function
If we have many lines of code all about creation of a **single complex object**, we can wrap them in an immediately called constructor function, like this:

```javascript
// create a function and immediately call it with new
let user = new function() {
  this.name = "John";
  this.isAdmin = false;

  // ...other code for user creation
  // maybe complex logic and statements
  // local variables etc
};
```

This constructor **can’t be called again**, because it is not saved anywhere, just *created* and *called*.


## Return from Constructors

Usually, constructors do not have a `return` statement.

But if there is a `return` statement, then the rule is simple:

- If `return` is called with an object, then the object is returned instead of `this`.
- If `return` is called with a primitive, it’s ignored and return `this`

```javascript
function BigUser() {

  this.name = "John";

  return { name: "Godzilla" };  // <-- returns this object
  // return; <-- returns this
}

alert( new BigUser().name );  // Godzilla, got that object
```

By the way, we can omit parentheses after `new`:

```javascript
let user = new User; // <-- no parentheses
// same as
let user = new User();
```

Omitting parentheses here is not considered a “good style”, but the syntax is permitted by specification.

# Optional chaining

Optional chaining lets you safely access nested properties or methods without throwing errors if something is null or undefined.
```js
let user = {};

console.log(user.address?.street); // undefined (no error)
```

### Use Cases

- Access nested properties:  
    user?.address?.city
- Call optional methods:  
    user.sayHi?.()
- Access optional array elements:  
    arr?.[0]


### Without Optional Chaining

```js
let user = {};

console.log(user.address.city); // ❌ Error: Cannot read property 'city' of undefined
```
### With Optional Chaining
```js
console.log(user.address?.city); // ✅ undefined (no error)
```

### Invalid for Assignment
You can’t use optional chaining on the left-hand side of an assignment:
```js
user.address?.city = "Kurnool"; // ❌ SyntaxError
```


- Optional chaining only works for declared variables.
	- console.log(undeclaredVar?.something); // ❌ ReferenceError
    
- It short-circuits — if any part before ?. is null or undefined, evaluation stops and returns undefined.
- It doesn’t catch other errors like wrong method names or syntax issues.
- Use it when the property/method may not exist, especially in API data or config objects.
- Avoid overusing it — optional chaining hides bugs if used carelessly.

---

# Symbol
A Symbol is a primitive data type used to create unique identifiers for object properties.
```js
let id = Symbol("id");
```
Even if two symbols have the same description, they are never equal:
```js
Symbol("id") === Symbol("id") // false
```

## Symbols as Object Keys

Symbols can be used as hidden or non-enumerable keys in objects:
```js
let user = {
  name: "Salman"
};  
let id = Symbol("id");
user[id] = 123;
console.log(user[id]); // 123
```

## Symbol Properties are Skipped in Loops

Symbol-keyed properties are not visible in:
```js
- for...in
- Object.keys()
- JSON.stringify()
```
But they are accessible via:
```js
Object.getOwnPropertySymbols(user)
```
  
## Global Symbols with Symbol.for()

Creates or retrieves a shared symbol from the global registry:
```js
let id1 = Symbol.for("id");
let id2 = Symbol.for("id");
console.log(id1 === id2); // true
```

Use Symbol.keyFor() to get the key for a global symbol:
```js
Symbol.keyFor(id1); // "id"
```

Key Points:

- Symbols are always unique, even with the same description.
- They’re useful to avoid property name conflicts in objects.
- Symbol properties are not enumerable or included in JSON.
- Use Symbol.for() for shared/global symbols.
- Use Object.getOwnPropertySymbols(obj) to access symbol keys.
- Symbols help in creating private-like fields (not truly private, but hidden from common enumeration).

---

# Conversation rules

> objects are auto-converted to primitives, and then the operation is carried out over these primitives and results in a primitive value.

[Type Conversions](https://javascript.info/type-conversions) the rules for numeric, string and boolean conversions of primitives. Now for objects,

1. There’s no conversion to boolean. All objects are `true` in a boolean context.
2. The numeric conversion happens when we subtract objects or apply mathematical functions. For instance, `Date`objects (to be covered in the chapter [Date and time](https://javascript.info/date)) can be subtracted, and the result of `date1 - date2` is the time difference between two dates.
3. As for the string conversion – it usually happens when we output an object with `alert(obj)` and in similar contexts.

## Auto conversion

When an **object is used where a primitive value is expected**, JavaScript automatically converts it to a primitive. This happens for operators like `+`, `-`, comparisons, `alert()`, and other built-in operations that *need* a primitive value first.

---

## Why Does This Matter?

Objects do not behave like primitives (e.g., numbers or strings). But many operations require primitives, so JavaScript tries to **coerce objects into primitive values** before performing those operations. 

For example:

```javascript
{} + 2      // "[object Object]2" (string concatenation)
{} * 2      // NaN         (number conversion fails)
alert({}); // "[object Object]" (string conversion)
```

---

## Conversion “Hints”

JavaScript uses a **hint** to decide what type of primitive is wanted:

* `"string"` — for string contexts such as `alert(obj)` or template literals.
* `"number"` — for maths like `obj * 2` or unary `+obj`.
* `"default"` — for binary `+` and `==` comparisons.
  Most built-in objects treat `"default"` the same way as `"number"`. 

---

## Algorithm (Order of Attempts)

When converting an object to a primitive, JavaScript tries:

1. **`obj[Symbol.toPrimitive](hint)`**
   If this exists, it is used first. It must return a primitive or a `TypeError` is thrown. 

2. **If `hint == "string"`**

   * Call `toString()`
   * If that returns a primitive, use it.
   * Otherwise try `valueOf()`. 

3. **If `hint == "number"` or `"default"`**

   * Call `valueOf()`
   * If that returns a primitive, use it.
   * Otherwise try `toString()`. 

If none returns a primitive, a `TypeError` is thrown. 

---

## Core Methods Explained

### 1. `Symbol.toPrimitive`

**Modern, most flexible method**—it handles all hints:

```javascript
obj[Symbol.toPrimitive] = function(hint) {
  if (hint === "string") return "...";
  if (hint === "number") return 123;
  return "..."; // for default
};
```

This tells JavaScript *exactly* what to return for each context. 

---

### 2. `toString()`

Older technique, prioritized for **string** conversion:

* For plain objects, default `.toString()` returns `"[object Object]"`.
* It should return a primitive (usually a string). 

---

### 3. `valueOf()`

Older technique, prioritized for **number/default** conversion:

* For plain objects, default `.valueOf()` returns the object itself (ignored).
* If you override it to return a primitive (like a number), that value is used. ([MDN Web Docs][2])

---

## Workings in Practice

* `alert(obj)` → attempts string conversion.
* `+obj` or `obj - 1` → numeric conversion.
* `obj + "foo"` → default (`toPrimitive` or numeric), then string concatenation if a string is produced. 

Example with defined methods:

```javascript
let user = {
  name: "John",
  money: 1000,
  [Symbol.toPrimitive](hint) {
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};

alert(user);     // "{name: "John"}"
alert(+user);    // 1000
alert(user + 0); // 1000
```

Or with old-style methods:

```javascript
let user = {
  name: "John",
  toString() {
    return this.name;
  },
  valueOf() {
    return this.name.length;
  }
};

alert(user);  // toString → "John"
alert(+user); // valueOf → 4
```

Note: `toString()` may be used even for `+` if `valueOf` doesn’t return a primitive. 

---

## **Key Points You Can Include in Notes**

* Conversion happens **automatically** when a primitive is required. 
* There are **three “hints”**: `"string"`, `"number"`, `"default"`. 
* **Priority order:** `Symbol.toPrimitive` → `toString/valueOf` depending on hint. 
* **Methods must return a primitive** (string, number, boolean, symbol, etc.). 
* Default `.valueOf()` returns the object itself and is ignored. ([MDN Web Docs][2])
* This is useful when customizing how objects behave with operators or functions that expect primitives. 

---

If you want, I can format this as **ready-to-copy Markdown** for your GitHub notes.

[1]: https://javascript.info/object-toprimitive?utm_source=chatgpt.com "Object to primitive conversion"
[2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf?utm_source=chatgpt.com "Object.prototype.valueOf() - JavaScript | MDN"
