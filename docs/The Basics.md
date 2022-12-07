# The Basics 

If we give a ``src`` attribute inside our &lt;script&gt; tag, and at the same time define some code inside the &lt;script&gt; tags, the script inside will get ignored

```html
<script src="smt.js">
		console.log('hello'); // this code will get ignored
</script>

```
```

<aside>
💡 As a rule, only the simplest scripts are put into HTML. More complex ones reside in separate files. The benefit of a separate file is that the browser will download it and store it in its cache. Other pages that reference the same script will take it from the cache instead of downloading it, so the file is actually downloaded only once. That reduces traffic and makes pages faster.

</aside>

```jsx
"use strict";
// we can do ^ this to switch the engine to modern mode
// they must be at the top
// we can also use them in functions
// several language features like classes and modules enable strict mode by default
```

we can declare multiple variables in one line

```jsx
let name="jim", age=8n, height=45.4n;
```

<aside>
⚠️ **“strict” mode alert**

</aside>

<aside>
🔥 We could create variables without declaration merely through assignment, but this is not allowed in strict mode.

</aside>

**Dynamically typed-** there are datatypes but variables are not bound to them. 

## **The 7 basic Datatypes:**

- ***Number:***  integers and floating point numbers, plus Special numeric values (Infinity, -Infinity, NaN ).
    
    **Infinity** is a result of division by 0. It’s greater than any number. 
    
    **NaN** is the result of a computational error. If there is a NaN inside a mathematical expression it will propagate through the whole expression.
    
     Doing math in JS is safe, i.e. it won’t die with a fatal error, it will result in a NaN in the worst case
    
- ***Strings:*** can be surrounded with “” ‘’ or backticks(``).
- ***Booleans:*** either true or false
- ***Null:*** In JS null is not a reference to a non-existing object, but an object of itself
- ***Undefined:*** Is also an object of it’s own, and its assigned to variables which have been declared but not assigned, we can also explicitly assign it, but that’s not good practice since that a job for null. null acts like a zero when used in operations. i.e. null - 5 == -5
- ***Objects***
- ***Symbols:*** named objects

### The **typeof** operator:

```jsx
> let x = 5
> typeof x;  // as an operator
> typeof(x); // as a function
```

typeof(null) returns “object”, This is an officially recognized error of JS, kept for compatibility. null is not an object but a special type of it’s own .

## Casting

```jsx
// casting
> let x;
> x = String(x)  // To string
> x = "45" / true  // Numeric conversions happen automatically with operators
> x = Number(x)  // To Number
// if one of the operands on + is a string, it will concatenate it
> "45" + 1 // 451
> x = Boolean("0"); // true
// casting for booleans, values which are intuitively empty("", null, undefined, NaN)
// get evaluated to false. note "0" and " " are evaluated to true
```

Numeric conversion rules:

| Value | Becomes.. |
| --- | --- |
| undefined | NaN |
| null | 0 |
| true and false | 1 and 0 |
| string |  Whitespaces from the start and end are removed. If the remaining string is empty, the result is 0. Otherwise, the number is “read” from the string. An error gives NaN |

<aside>
🔥 Tip : We can use the unary + to cast to Number.

</aside>

```jsx
let x = "45";
typeof +x; // Number
+true // 1
// It doesn't do anything if the operand is not a Number
```

```jsx
// weird
4 + 5 + "px"  // 9px
"px" + 4 + 5  // px45
// operations run from left to rigth
```

Unary operators have higher precedence than their binary counterparts.

For more on operator precedence > [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

Chained assignments are evaluated from right to left

The assignment operator returns the value that it just assigned. 

```jsx
let a = 5; 
5 + (a = a + 5); // returns 15
```

<aside>
📢 **The comma operator**

</aside>

```jsx
// The comma operator evaluates all expressions and returns the last one
	let a = ( 2 + 4, 3 + 8); // a will equal 11
// This trick is used in many frameworks so look up
// the comma has lower precedence than even the =
for (a = 1, b = 3, c = a * b; a < 10; a++) { ... }
```

```jsx
	// Really weird 
let a = 0;
let b = "0";
Boolean(a) // false
Boolean(b) // true
a == b // true
```

**Strict equality ===**

Strict equality operator, checks for equality without type conversion

```jsx
null > 0 // false
null >= 0 // true
null == 0 // false  since equality doesn't cast null, like comparisions do

null == undefined // true
null === undefined // false
```

<aside>
🛠 **The very special OR || and AND &&**

</aside>

OR evaluates from left to right and returns the first truthy value, if none it returns the last

AND also evaluates from left to right and returns the first faulty value, if none it returns the last

```jsx
result = value1 || value2 || value3
// result will be the first truthy value, if none it will be value3
// && has higher precedence than ||

let v1 = null;
let v2 = "";

let result = v1 || v2 || "no-name"; // no-name
let result = v1 && v2 && "no-name"; // null 

/* we can also use OR as an if statement due to them being short-circuit*/
true || expression; // won't get evaluated
false || expression; // gets evaluated
true && expression; // gets evaluated
false && expression; // won't get evaluated
```

**The NOT ! operator**

! converts to a Boolean and returns its inverse

 

<aside>
🛠 Double NOT ( !! ) is used to cast to Boolean.  !!(”0”) // true

</aside>

<aside>
⚠️ Inside a switch cases are matched with strict equality ===

</aside>

```jsx
switch("3"){
	case 3: // this will not become true since their types are different
}
```

## Functions

```jsx
function smth(){ console.log("smth") } // function declaration 
let smth = function(){ } // function expression
let smth = () => {} // arrow functions,more concise than function expressions

// we can specify parameters like

function sayhello( person, message ) {
	console.log( person + " : " + message )
}

// to specify a default value
function sayHello( person, message = "no message") {}

// we can assign the return of another function or expression as the default
function sayHello ( person = expression , message = anotherFunction() )
/* we can use the fact that the default expression or function call only gets executed
	 if the function is missing an argument */
// another way we can assign default values is with 
function sayHello(person, message){
person = person || "no-name";
message = message || "no-message";
}

/* Functions can be copied, passed to other functions in which case they are called
 callbacks ... */

```

<aside>
⚠️ In JavaScript, a default parameter is evaluated every time the function is called without the
respective parameter. In the example above, anotherFunction() is called every time
showMessage() is called without the text parameter. This is in contrast to some other
languages like Python, where any default parameters are evaluated only once during the
initial interpretation.

</aside>

An empty or no-return returns undefined

```jsx
function a(){ return };  // a() -> undefined
function b(){}; // b() -> undefined
```

Modal function - pause the code execution until they finish eg. alert() …

<aside>
⚠️ **Function declaration VS Function expression:** 
Functions initiated with function expressions can only be used after they have been declared, unlike those with function declarations. In other words. **A Function Expression is created when the execution reaches it and is usable only from that moment.**

</aside>

<aside>
⚠️ “strict” mode alert

</aside>

<aside>
⚠️ In strict mode functions will only be visible inside the block they were defined in.

</aside>
