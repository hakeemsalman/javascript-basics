# Data Types

- [Data Types](#data-types)
- [Methods of Primitives](#methods-of-primitives)
	- [**Key Idea**](#key-idea)
	- [**Why Primitive Methods Exist**](#why-primitive-methods-exist)
	- [**How It Works Internally (Important for Interviews)**](#how-it-works-internally-important-for-interviews)
	- [**Primitive Wrapper Objects**](#primitive-wrapper-objects)
	- [**Important Rule**](#important-rule)
	- [**Why `null` and `undefined` Have No Methods**](#why-null-and-undefined-have-no-methods)
	- [**Why `new String()`, `new Number()` Are Bad Practice**](#why-new-string-new-number-are-bad-practice)
	- [**Key Points (Very Important)**](#key-points-very-important)
	- [**Interview One-Line Explanation**](#interview-one-line-explanation)
		- [Try it yourself](#try-it-yourself)
- [Numbers](#numbers)
	- [**1. Number Type Overview**](#1-number-type-overview)
	- [3. More Ways to Write Numbers](#3-more-ways-to-write-numbers)
		- [Large Numbers](#large-numbers)
		- [Small Numbers](#small-numbers)
	- [4. Hexadecimal, Binary, Octal Numbers](#4-hexadecimal-binary-octal-numbers)
		- [Hexadecimal (`0x`)](#hexadecimal-0x)
		- [Binary (`0b`) and Octal (`0o`)](#binary-0b-and-octal-0o)
	- [5. `toString(base)`](#5-tostringbase)
	- [6. Rounding Methods](#6-rounding-methods)
	- [7. `toFixed()`](#7-tofixed)
	- [Precision Issues (Very Important)](#precision-issues-very-important)
	- [8. Number Limits](#8-number-limits)
	- [Special Numeric Values](#special-numeric-values)
		- [Infinity](#infinity)
		- [NaN (Not a Number)](#nan-not-a-number)
	- [9. Checking Numbers](#9-checking-numbers)
	- [10. `parseInt(str, [radix])` and `parseFloat`](#10-parseintstr-radix-and-parsefloat)
	- [Math Functions](#math-functions)
		- [Math.random()](#mathrandom)
		- [Math.max() and Math.min()](#mathmax-and-mathmin)
		- [Math.pow()](#mathpow)
	- [Key Points (Interview-Critical)](#key-points-interview-critical)
	- [One-Line Interview Explanation](#one-line-interview-explanation)


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

Below is a **corrected, expanded, and interview-ready note** on **JavaScript Numbers**, aligned with
the official article you shared and **building on your existing notes**.
I’ll also point out **what is correct** and **what needs improvement** (silently integrated).

---

# Numbers

## **1. Number Type Overview**

* JavaScript has **one numeric type** called `number` (excluding `BigInt`)
* Regular numbers are stored in **64-bit IEEE-754 format**
* Also called **double-precision floating-point numbers**
* Used for **integers and floating-point values**

```js
let a = 1;
let b = 1.5;
let c = -10;
```

## 3. More Ways to Write Numbers

### Large Numbers

```js
let billion = 1000000000;
let billion = 1_000_000_000; // underscore separator
let billion = 1e9;           // exponential form
```

```js
1e9 === 1 * 1_000_000_000;
1.23e9 === 1.23 * 1_000_000_000;
```

---

### Small Numbers

```js
let ms = 0.000001;
let shortMs = 1e-6;
```

```js
1e-3 === 1 / 1000;
1.23e-6 === 1.23 / 1_000_000;
1234e-2 === 12.34;
```

---

## 4. Hexadecimal, Binary, Octal Numbers

### Hexadecimal (`0x`)

Used for:

* Colors
* Bitmasks
* Encoding

```js
0xff === 255;
0xFF === 255;
```

---

### Binary (`0b`) and Octal (`0o`)

```js
let a = 0b11111111; // 255
let b = 0o377;      // 255

a === b; // true
```

## 5. `toString(base)`

- Returns a string representation of num in the numeral system with the given base.
- The base can vary from `2` to `36`. By default, it’s `10`.
- `base=16` is used for **hex** colors, **character encodings etc**, digits can be `0..9` or `A..F`.
- `base=2` is mostly for debugging **bitwise** operations, digits can be `0` or `1`.
- `base=36` is the **maximum**, digits can be `0..9` or `A..Z`
  
```js
let num = 255;

alert( num.toString(2) );   // 11111111
alert( num.toString(8) );   // 377
alert( num.toString(10) );  // 255
alert( num.toString(16) );  // ff
alert( num.toString(36) );  // 73

alert( 123456..toString(36)  ); // 2n9c
alert( (123456).toString(36) ); // 2n9c
```

> NOTE: Two dots to call a method. Also could write (123456).toString(36).

---

## 6. Rounding Methods

```js
Math.floor(3.7); // 3
// Rounds down: `3.1` becomes `3`, and `-1.1` becomes `-2`

Math.ceil(3.1);  // 4
// Rounds up: 3.1 becomes 4, and -1.1 becomes -1

Math.round(3.5); // 4
// Rounds to the nearest integer: 3.1 becomes 3, 3.6 becomes 4. In the middle cases 3.5 rounds up to 4, and -3.5 rounds up to -3.

Math.trunc(3.9); // 3 (not supported by Internet Explorer)
// Removes anything after the decimal point without rounding: 3.1 becomes 3, -1.1 becomes -1.

// decimals rounding
// Two methods:
// One is Multiply and divide
alert( Math.round(num * 100) / 100 ); // 1.23456 -> 123.456 -> 123 -> 1.23

// Second is toFixed()
let num = 12.34;
alert( num.toFixed(1) ); // "12.3"
```

||Math.floor|	Math.ceil|	Math.round|	Math.trunc|
|---|---|---|---|---|
|3.1	|3 | 4|	3|	3|
|3.5	|3 | 4|	4|	3|
|3.6	|3 | 4|	4|	3|
|-1.1	|-2	|-1	|-1	|-1|
|-1.5	|-2	|-1	|-1	|-1|
|-1.6	|-2	|-1	|-2	|-1|

---

## 7. `toFixed()`

Rounds and returns a `string`:

```js
let n = 1.23456;
alert( n.toFixed(2) ); // "1.23"

alert( n.toFixed(10) ); // "1.2345600000", added zeroes to make exactly 5 digits
```

## Precision Issues (Very Important)

Because numbers are floating-point:

```js
0.1 + 0.2 === 0.3; // false
alert( 0.1 + 0.2 ); // 0.30000000000000004
```

Reason:

- Binary cannot represent some decimals exactly.
- A number is stored in memory in its binary form, a sequence of bits – ones and zeroes. But fractions like `0.1`, `0.2` that look simple in the decimal numeric system are actually unending fractions in their binary form.

Please go through this [link](https://javascript.info/number#imprecise-calculations)
Correct way:

```js
+(0.1 + 0.2).toFixed(2); // 0.3
```

---

## 8. Number Limits

```js
Number.MAX_VALUE
Number.MIN_VALUE
Number.MAX_SAFE_INTEGER   // 9007199254740991
Number.MIN_SAFE_INTEGER
```

Beyond safe integers:

```js
9007199254740991 + 1 === 9007199254740991 + 2; // true (precision loss)
```

## Special Numeric Values

JavaScript has three special numeric values:

```js
Infinity
-Infinity
NaN
```

### Infinity

```js
alert(1 / 0);       // Infinity
alert(Infinity);   // Infinity
```

### NaN (Not a Number)

Represents an invalid numeric operation.

```js
alert("abc" / 2); // NaN
alert(NaN + 1);   // NaN
```

Important:

```js
alert( NaN === NaN ); // false
alert( Object.is(NaN, NaN) ); // false
```

> Comparison with `Object.is` 
> 
> There is a special built-in method `Object.is` that compares values like `===`, but is more reliable for two edge cases:
>
> It works with `NaN: Object.is(NaN, NaN) === true`, that’s a good thing.
> Values 0 and -0 are different: Object.is(0, -0) === false, technically that’s correct because internally the number has a sign bit that may be different even if all other bits are zeroes.
> In all other cases, Object.is(a, b) is the same as a === b.
> 
> We mention Object.is here, because it’s often used in JavaScript specification. When an internal algorithm needs to compare two values for being exactly the same, it uses Object.is (internally called SameValue).

## 9. Checking Numbers

```js
alert( isNaN(NaN) ); 								// true
alert( isNaN("str") );							// true (loose)
alert( isNaN(15) ); 								// false
alert( isNaN(true) ); 							// false

alert( NaN === NaN ); 							// false

alert( Number.isNaN("abc") );      	// false (strict)
alert( Number.isFinite("15") );    	// false

alert( isFinite("15") ); 						// true
alert( isFinite(15) ); 							// true
alert( isFinite("str") ); 					// false, because a special value: NaN
alert( isFinite(Infinity) ); 				// false, because a special value: Infinity
```

---

## 10. `parseInt(str, [radix])` and `parseFloat`

- Extract numbers from strings:
- Stops reading when a non-number is found.
- `parseInt(str)`, second argument is optional

> Numeric conversion using a plus `+` or `Number()` is strict. If a value is not exactly a number, it fails:
> `alert( +"100px" ); // NaN`

```js
alert( parseInt("100px") );   // 100
alert( parseInt('12.3') ); // 12, only the integer part is returned
alert( parseInt('a123') ); // NaN, the first symbol stops the process
alert( parseFloat("12.5em") ); // 12.5
alert( parseFloat('12.3.4') ); // 12.3, the second point stops the reading

alert( parseInt('0xff', 16) ); // 255
alert( parseInt('ff', 16) ); // 255, without 0x also works
alert( parseInt('2n9c', 36) ); // 123456
```

## Math Functions

### Math.random()

- Returns a random number from 0 to 1 (not including 1).

```js
alert( Math.random() ); // 0.1234567894322
alert( Math.random() ); // 0.5435252343232
alert( Math.random() ); // ... (any random numbers)
```

### Math.max() and Math.min()

- Returns the greatest and smallest from the arbitrary number of arguments.

```js
alert( Math.max(3, 5, -10, 0, 1) ); // 5
alert( Math.min(1, 2) ); // 1
```

### Math.pow()

- Returns n raised to the given power.
  
```js
alert( Math.pow(2, 10) ); // 2 in power 10 = 1024
alert( Math.pow(2, 5) ); // 2 in power 5 = 32
```


## Key Points (Interview-Critical)

* JavaScript numbers use **IEEE-754 floating-point**
* Precision errors are **expected behavior**
* `NaN` is contagious and never equals itself
* Use `Number.isNaN()` and `Number.isFinite()`
* Underscores improve readability
* Hex, binary, and octal represent the same numeric value
* Avoid math with decimals for money → use integers or libraries

---

## One-Line Interview Explanation

> JavaScript uses double-precision floating-point numbers, which can cause precision issues, so numeric operations must be handled carefully.

