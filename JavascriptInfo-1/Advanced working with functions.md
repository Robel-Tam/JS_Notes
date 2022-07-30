# Advanced working with functions

The number of times a recursive function calls itself is called a recursion depth.

The maximum recursion depth is limited by engines. read about Tail call optimizations

Recursive structures - Linked lists

```jsx
let lList = {  // a linked list
	value: 1,
	next:{
		value: 2,
		next:{
			value: 3,
			next: null
		}
	}
}

function printList(list){  // recursive function for printing elements inside the lList
	if(list.next == null) return list.value.toString();
	return list.value.toString() + printList(list.next)
}

printList(lList) // 123
```

# Rest parameters

In JS it’s ok to pass more arguments to a function that it need, the function will simply ignore it.

To store the passed arguments to an array we can use rest parameters ( …rest ). The rest parameter must come at the end if other parameters exist.

```jsx
function func( a, b, ...c)
```

**arguments** object also contains all arguments passed by index

```jsx
function fun( a, b, c ) {
	console.log(arguments);
	console.log(arguments[1]);
	console.log(arguments.length);
}

fun(1,2,3,4);
/* output
[Arguments] { '0': 1, '1': 2, '2': 3, '3': 4 }
2
4
*/
```

The downside of the arguments object is that it’s not an array, so we can’t use array methods on it.

Arrow functions don’t have the **arguments** object, they will take it from an outer function if it exists

# Spread operator 🌟

The syntax for the rest operator (…arr) can also spread an iterable object in to a list of values.

```jsx
let str = "hello"
let arr = [...str]  // ["h","e","l","l","o"]

// we can use this operator to pass arrays to functions expecting list of arguments
arr = [ 1,2,3,4,5 ]
Math.max(...arr) // 5

// we can also pass multiple iterables, and we can also combine them with non iterables
arr2 = [ 6,7,8,9 ]
Math.max(...arr, 34, ...arr2, 1) // 34

// we can use spread operators to merge arrays
arr2  = [ ...arr2, ...arr1 ]
```

Unlike the Array.from() method, the spread operator only operates on iterables and not array-likes, so Array.from() is a more universal way to convert stuff into arrays

# Closures

Are functions that remember their outer variables and can access them. All JS functions are naturally closures.

## Lexical Environment

A lexical environment is a specification (internal / hidden ) object associated with every function call / block / script. i.e. every running function/ block / script have their own.

A lexical environment has two parts, **environment record** and **reference to an outer lexical environment.** 

An environment record - holds all the local variables as it’s properties, and the value of **this.** therefore a **variable** is just a property of the internal object- environment record.

```jsx
// A visualization for lexical environments
Lexical_Environment{
	Environment_Record: {  /*var1 : smth, ... */ },
	Outer: /*null in the case of a global lexical env */
}

let phrase;  // { Environment_Record:{ phrase: undefined }, Outer: null }
phrase = "hello"; // { Environment_Record:{phrase: "hello"}, Outer: null }
phrase = "bye" ;  // {Environment_Record:{pharse: "bye"}, Outer: null }
```

**Function declarations** get initialized when the lexical environments that holds them gets created, unlike those with **let** , that get initialized when execution reaches it. That’s why we can call functions before their declaration. All functions on birth get a hidden property **[[Environment]]** which contains a reference to the lexical environment of their creation. functions remember where they were created using this hidden property.

```jsx
// A visulization
/* Execution starts -> { 
			Env_record:{
					greet: function
			},
			Outer: null  } */
let name = "Jhon"        /* -> {
			 Env_record:{
						greet: function,
						name: "Jhon"
				},
				Outer: null   } */
function greet(title){  // [[Environment]] -> global lexical environment
		console.log("hello " + title + " " + name);
}
```

**Nested lexical environments :** Let’s call function greet

```jsx
greet("Mister") // hello Mister Jhon
// The execution environment created for this function call is
/* [[Environment]] -> {
 Env_record:{
			title: "Mister",
	},
	Outer: /* A referance to the outer Lexical env with the value for name */
} 
*/
```

When the code wants to access a variable, it’s lexical environment is scanned first, if the variable is not found then the outer lexical environments are scanned progressively until it reaches the global lexical environment. That’s how local variables override outer ones.

<aside>
⚠️ **Strict mode alert**                                                                                                                                     If a variable is not found in any of the lexical environments, its an error in strict mode. But if we are not in strict mode, An assignment to an undefined variable creates a new global variable

</aside>

```jsx

function f(){ b = 5 };
console.log(b) // 5 in non-strict mode
// if we are in strict mode -This will throw a ReferanceError
// if we are not, variable b will get created in the global context
```

## Nested Functions

We can use them to organize code

```jsx
function printSquare(n){
	function square(n) { 
			return n * n;
	}
	console.log(square(n));
}
```

We can use them inside constructors to create methods

```jsx
function User(name){
	this.sayName = function(){
		console.log("My name is " + name;
	}
}
```

We can return the functions themselves

```jsx
function makeCounter(){
		let count = 0;
		return function(){
				return count++;
		}
}
let c = makeCounter();
let d = makeCounter();
c() // 0
c() // 1
d() // 0
```

When the first c() gets called, this happens

```jsx
c() [[Environment]] -> {  
	Env_record:{},
			Outer: -> {
					 Env_record:{
								count: 0,
						},
						Outer: -> // global lexical env
					} 
				}
		 } 
```

**Code** **Blocks, loops and IIFE**

Lexical environments can exist for any code block. A lexical environment is created when a block runs and contains block local variables

```jsx
if(true){
	let v = 5;
}
v // ReferanceError
// v can't be accessed from outside of the if blocks lexical environment
```

Inside loops, every loop will have it’s own lexical environment

We can use a bare code block {} to isolate variables into a local scope.

In a web browser all scripts except the ones with type=”module” share the same global area, So if we create a global variable in one script it will become available to the others, this may cause conflicts. But we can isolate each script by putting it inside its own block

There used to be no block-level lexical environment in JS, so a work around called **IIFE ( immediately invoked function expressions )** was used.

```jsx
(function (){ /* script */ })()
// other ways
( function(){ /* script */ }() )
!function(){ /* script */ )()
+function(){ /* script */ )()
```

we use the parenthesis to say that the function is created in the context of another expression.

## The old **var**

variables declared with **var** also have lexical environments that are bound to function scopes.

```jsx
function fun(){
	let a = 1;
	var b = 2;
}
alert(a) // ReferanceError
alert(b) // ReferanceError  because vars are also bound to function scopes
```

<aside>
📢  But unlike those declared with **let** and **const** they are not bound to block scopes.

</aside>

```jsx
{
	let a = 1;
	var b = 2;
}

alert(a) // ReferanceError
alert(b) // 2     var ignores code blocks

// inside if blocks
if(true){
	var test = 4;
}
alert(test) // 4

// inside loops
for( var i = 0; i < 10; i++);
alert(i) // 9
```

<aside>
📢 **var** declarations get hoisted i.e. they get processed as soon as the script ( for global L.E ) or the function they are in gets executed. In other words their declaration gets moved up to the function or the script. Therefore we can use them before they get declared

</aside>

```jsx
function greet(){
	str = "hello";
	if(false){ 
		var str
	}
	console.log(str);
}

greet() // hello
```

Declarations are hoisted but assignments are not

```jsx
function greet(){
	console.log(str);
	var str = "hello"
}
greet() // undefined
```

# The global object

Provides variables and functions that are available anywhere. Mostly the ones that are built into the language or the environment. 

In the browser it’s name **window**, **global** in node. But **globalThis** has been added to the language as a standardized name for the global object.

we can access members of the global object directly but it’s good practice to refer to them through the global object. i.e. globalThis.x instead of x

In-browser unless we’re using modules, variables declared with var and global functions get added to the global object.

We can use the global object to test for support of modern language features

# The function object

Functions in JS are objects. So they have some usable properties of their own

```jsx
function f(){}
Reflect.ownKeys(f) // [ 'length', 'name', 'arguments', 'caller', 'prototype' ]

Reflect.ownKeys((a, b) => {}) // [ 'length', 'name' ]
```

func.**name :**  returns a read only name of the function

```jsx
function sayHi() {}
sayHi.name // 'sayHi'

let sayHi() = function(){}
sayHi.name // 'sayHi'

let user = { sayHi(){} }
user.sayHi.name // 'sayHi'
```

func.**length :** returns the number of arguments a function accepts

```jsx
function s( a, b, ...c){}
s.length // 2  rest parameters are not counted 
```

**custom properties**

we can add properties of our own to functions.

```jsx
// lets add a count property which stores the number of calls made to func
function func(){
		func.count++;
		console.log(func.count)
}

func.count = 0;
func() // 1
func() // 2
func() // 3
```

Closures can sometimes be replaced with function properties

```jsx
// let's redo the makeCounter function with a custom property
function makeCounter(){
		function counter(){
			return counter.count++;
		}
		counter.count = 0 ;
		reutrn counter;
}

let c = makeCounter();
c() // 0
c() // 1
c.count = 40;
c() // 40

// In this case we can modify the counter
```

## Named function expressions ( NFE )

NFE is when we give names to the function on the right side of the function in our functional expressions.

```jsx
// normal functional expression
let func = function() { }

// NFE
let func = function sayHi(){ } 
// NFE allows us to reference the function from within
// the identifier sayHi isn't visible outside the function
```

We use NFE when we want to reference the function from within the function.

```jsx
let sayHi = function func(ph){
								if(ph) console.log(ph);
								else func("hello");
}

// We could have done 
let sayHi = function func(ph){
								if(ph) console.log(ph);
								else sayHi(ph)
}
// but the above code will stop working if we change the value of sayHi
let anotherFunc = sayHi;
sayHi = null;
anotherFunc("d") // d
anotherFunc() // Error sayHi is not a function
// This happens because the identifier sayHi was taken from the outer L-Env

// This can't be done with function declarations
```

<aside>
📢 **Question**                                                                                                                                                     Write a function sum that would work like this:                                                                                     sum(1)(2) == 3; // 1 + 2                                                                                                        sum(1)(2)(3) == 6; // 1 + 2 + 3                                                                                                       sum(5)(-1)(2) == 6

</aside>

## new Function() syntax

```jsx
let func = new Function ([arg1, arg2, ...argN], functionBody);
```

This syntax creates a function with arguments arg1 - argN with functionBody 

```jsx
let func = new Function(functionBody) // a function without arguments
```

Functions created with new Function() have an [[ Environment ]] referencing to the global Lex-Environment. This helps us to hide our local variables from them and to avoid running into problems when using minifiers.

The arguments can also be given in a comma separated list.

```jsx
new Function("a", "b", "c", "alert(a)" );
new Function("a,b", "c", "alert(a)");
new Function("a,b,c", "alert(a)" ) ;
// all work the same
```

## Scheduling : setTimeout & setInterval

These methods are not part of the JS specification. But most environments have an internal scheduler that provides these methods.

setTimeout - causes a function or piece of code to run once after a period of time

setInterval - causes a function to run regularly with the given interval

```jsx
// setTimeout( func, [delay] , [arg1], [arg2], [argn] );  delay in ms
function sayHi(name){
		console.log(`Hi ${name}`
}

let timerId = setTimeout(sayHi, 9000, "Smith"); 
// after 9 seconds => Hi Smith
// a call to setTimeout returns a timerId we can use this to cancel it

// setInterval(func, [delay], [arg1], [arg2], [argn] );
let timerId2 = setInterval(sayHi, 2000, "Jack" );
// Hi Jack will be displayed every 2 seconds
```

**canceling with clearTimeout and clearInterval**

```jsx
clearTimeout( timerId )
clearInterval( timerId2 )
```

### Recursive setTimeout

There are two ways of running something regularly, using setInterval and with recursively calling setTimeout.

```jsx
let clock = setTimeout( function f(){
			console.log("Tick Tok");
			setTimeout( f, 1000);
		}, 1000 );
```

This is a more flexible way because we can manipulate the delays.

The recursive setTimout unlike setInterval guarantees the fixed delay ??

when a function is passed to setTimeout or setInterval, an internal reference to it is created and saved in the scheduler. This prevents the function from being garbage collected. But it also saved the outer lexical environment of the function, This might take much more memory than the function itself so when we don't need the scheduled function anymore it's better to cancel it. we can do that with clearTimeout and clearInterval

# Decorators and forwarding, call/apply
Let's see how cool JS functions can be

### Transparent caching
If we want to cache(remember) the results for different arguments of a CPU intensive function we can create a wrapper function like so.

```javascript
function slow(x){
	// slow calculations
	return x;
}

function cachingDecorator(func){
	let cache = new Map();
	return function(x){
		if(cache.has(x)){
			return cache.get(x);
		}
		let result = func(x);
		cache.add(x, result;
		return result;
	}
}

let slowFunc = cachingDecorator(slow);
slow(1) // got computed and cached
slow(1) // returned from cache
```

This code will fail if we try to pass Object methods to the wrapper and access properties that belong to the "this" context of that object.
```javascript
let obj = {
	method(){
		return 1;
	}
	slow(x){
		// kicka$$ code
		return x + this.method();
	}
}

let obj.slow = cachingDecorator(obj.slow);
// this will fail because this.method() will be undefined
```

To overcome this problem we can use function.call() to pass the context as well.
func.call() allows us to explicitly pass "this"

```javascript
func.call( context, arg1, arg2, ... ); // syntax for .call()
```
```javascript
function greet(){
	console.log(`Hello my name is ${this.name}`);
}

let jack = {name: "Jack Taylor"}
let bob = {name: "Bob Smith"}

greet.call(jack)// Hello my name is Jack Taylor
greet.call(bob) // Hello my name is Bob Smith
```

So we can simply fix our wrapper function by changing the assignment to
```javascript
let result = func.call(this, x);
```

We can also pass the context to a function call using func.**apply( context, args)** where **args** is an **array like**, we can use this when we want to pass an array like containing the multiple arguments for the function we are about to call.
```javascript
let args = [1, "2", 3];

func.apply(context, args);
// the same effect can be acheived with
func.call(context, ...args);

// But func.apply() can get optimized better by engines
```
func.apply() doesn't accept iterables only array likes.

<aside>⚠ **read about method borrowing**</aside> 
<aside><a href="http://sinonjs.org/">spy decorator</a></aside>

<aside>❓ **Good Question**
Create a decorator delay(f, ms) that delays each call of f by ms milliseconds. page# 383
<pre>
<code>
function print(a) { console.log(a) }
let f3000 = delay(print, 3000);
f3000("hello") // prints hello after 3 seconds
</code>
</pre>
</aside>

<aside>
<b>Topics I have to look into again</b>
<ul>
<li>Function binding</li>
<li>Currying and partials</li>
<li>Arrow functions revisited</li>
<li><i>Every thing from page 355 onward</i></li>
</ul>
</aside>
