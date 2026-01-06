# Data Types

# Methods of Primitives

## **Key Idea**

Primitives (like `string`, `number`, `boolean`) are **not objects**, but JavaScript allows them to **access methods** as if they were objects.

This works because JavaScript **temporarily wraps primitives into objects** when you access a method.

---

## **Why Primitive Methods Exist**

Without methods, we couldn’t do things like:

```javascript
"hello".toUpperCase();
(123).toString();
true.toString();
```

But primitives **cannot store methods** themselves. So JavaScript uses a special mechanism.

---

## **How It Works Internally (Important for Interviews)**

When you write:

```javascript
let str = "hello";
str.toUpperCase();
```

JavaScript does the following behind the scenes:

1. Creates a temporary object:

   ```javascript
   new String("hello")
   ```
2. Calls `.toUpperCase()` on it
3. Returns the result
4. Destroys the temporary object immediately

This is called **temporary object wrapping**.

---

## **Primitive Wrapper Objects**

| Primitive | Wrapper Object |
| --------- | -------------- |
| `string`  | `String`       |
| `number`  | `Number`       |
| `boolean` | `Boolean`      |
| `symbol`  | `Symbol`       |
| `bigint`  | `BigInt`       |

Example:

```javascript
let str = "hi";
typeof str; // "string"

let objStr = new String("hi");
typeof objStr; // "object"
```

---

## **Important Rule**

👉 **Primitives are immutable**

Methods do **not modify** the original value.

```javascript
let str = "hello";
str.toUpperCase();

console.log(str); // "hello" (unchanged)
```

They always return a **new value**.

---

## **Why `null` and `undefined` Have No Methods**

```javascript
null.toString();      // ❌ Error
undefined.toString(); // ❌ Error
```

Reason:

* `null` and `undefined` **do not have wrapper objects**
* They represent “no value”

---

## **Why `new String()`, `new Number()` Are Bad Practice**

```javascript
let a = "hello";
let b = new String("hello");

a === b; // false
```

Because:

* `a` is a primitive
* `b` is an object

This causes **unexpected bugs in comparisons**

✅ **Best practice:**
Always use primitives, never wrapper objects.

---

## **Key Points (Very Important)**

* Primitives can access methods because JavaScript **wraps them temporarily**
* Wrapper objects exist internally, not for regular use
* Primitive values are **immutable**
* `null` and `undefined` have **no methods**
* Avoid `new String()`, `new Number()`, `new Boolean()`
* This mechanism is handled by the JS engine, not visible to developers

---

## **Interview One-Line Explanation**

> JavaScript temporarily converts primitives into wrapper objects so that methods can be accessed, then immediately destroys those objects.

### Try it yourself

<details>
  <summary>
	Click here to show the solution

```js
	let str = "Hello";

	str.test = 5; // (*)

	alert(str.test); // ??
```

</summary>
  <p>
	Depending on whether you have use strict or not, the result may be:

undefined (no strict mode)
An error (strict mode).
Why? Let’s replay what’s happening at line (*):

When a property of str is accessed, a “wrapper object” is created.
In strict mode, writing into it is an error.
Otherwise, the operation with the property is carried on, the object gets the test property, but after that the “wrapper object” disappears, so in the last line str has no trace of the property.
This example clearly shows that primitives are not objects.

They can’t store additional data.
</p>
</details>