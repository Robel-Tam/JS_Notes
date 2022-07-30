# Data Types

Four of the primitive types namely string, number, symbol, Boolean can behave like objects in that the dot operator can be used on them to get some of their “properties” and methods. this is due to the use of “wrapper objects”. 

The four wrapper objects are String, Number, Boolean, Symbol. It’s bad practice to call these constructors with the new keyword.

```jsx
let a = Number("123");
typeof a // number
let b = new Number("123");
typeof b // object
```

<aside>
❓ Good question

</aside>

```jsx
// what will happen ?
let str = "hello";
str.test = 5; // (^)
console.log(str.test); // (*)

//Answer
/* If we are not in strict mode, line (^) will create a String wrapper object and then
 * it creates the property test and gives it 5. then on line (*) it will return
 * undefined, since the previous wrapper object got removed. 
 * If it's in strict mode in will return an error
*/   
```

### Numbers

all numbers are stored in 64 bit format, also called double precision floating point numbers

we can use ‘e’ to write numbers in short hand ( 7e4 = 70000, 23e-1 = 2.3 )

Hexadecimals - prefix 0x

Binaries- prefix - 0b

Octal- prefix -0o

Number.**toString(base): returns the string representation on the number, in the given base, if no base is given 10 is used as a default**

```jsx
let num = 255;
num.toString(2) // 11111111
num.toString(16) // ff
// base 36- is the maximum 0-9 A-Z
```

<aside>
⚠️ two dots ( 34..toString())

</aside>

```jsx
// if we want to call methods directly on the number literals we can use
7..toString(2);
(7).toString(2); // this is also possible
/* If we were to use only one dot, it will imply that there is a decimal part after
   it, but the other dot resolves that issue, */
6e3..toString(); // ERROR ! 6e3.toString()
0x3a2..toString(); // ERROR !  0x3a2.toString()
```

methods to modify numbers

```jsx
Math.floor(1.6) // 1
Math.ceil(1.2) // 2
Math.round(1.4) // 1
Math.trunc(2.12) // 2  removes the decimal part

// to round a number to the n-th digit after the decimal
let num = 1.2345;  // to round this to the 2nd digit
// 1
Math.floor(num * 100)/ 100  // 1.23
// 2
num.toFixed(2)  // '1.23'  string

```

Imprecise calculations:

From the 64 bits, used to represent numbers, 52 are used to store the digits, 11 are used to store the position of the decimal and 1 is used to store the sign. If a number is to big to be stored in this, it will overflow and resolve to infinity.

```jsx
1e500; // infinity
0.1 + 0.2 // 0.30000000000000004
0.1.toFixed(25); // '0.1000000000000000055511151'
```

Decimals that don’t reduce to ( x/ 2), will be endless binary fractions, so there’s no way to store them exactly, but JS rounds them to the nearest possible number, but these precision losses still exist.

Infinity and NaN

-Infinity and Infinity are special values which are smaller than or greater than any number.

-NaN represents and error

**isNaN(arg) : -** converts it’s argument into a Number, and checks to see if it’s NaN

```jsx
NaN == NaN // false
// NaN can't equal any number or even it self
Infinity == Infinity // true
```

**isFinite(arg) : -**  returns true if the arg, is not NaN, Infinity or -Infinity

```jsx
isFinite("123") // true
isFinite("str") // false
isFinite(1) // true
// isFinite() can be used to check if a string holds a number
isFinite("+3") // true
// !!! an empty or space only string resolves to 0 when casted to number
isFinite(" ") // true  !!!
```

**Object.is( arg1, arg2) : -** is the same as arg1 === arg2, except in these two cases

1. it works with NaN 

```jsx
Object.is(NaN, NaN) // true
```

1. Values 0 and -0 are different. Object.is(0, -0) → false

**parseInt(str, base), and parseFloat(str)**

```jsx
parseInt('45px') // 45
parseInt("3 Inch") // 3
parseInt("4.7") // 4

parseFloat("45.4em") // 45.4
parseFloat("4.3.2") // 4.3 

// Fails
parseInt("a45")  // if the first character is not a digit
parseInt("abc") // NaN

// base given
parseInt("ff", 16) // 255

/* this is a great way to check if a number belongs to a base*/
isFinite("ii", 16) // false
isFinite("ii", 36)  // true
```

**Math.random()** : - returns a random number ≥ 0 and < 1

**Math.max(a, b, c, …) and Math.min(a,b,c, …)**

<aside>
❓ Good Question

</aside>

```jsx
/* why is 6.35.toFixed(1) == 6.3 */
// toFixed() works just like round() when rounding to a given precision
// the problem happened because of the way 6.35 is represted
6.35.toFixed(20) // '6.34999999999999964473'
// precision loss made 6.35 smaller
// a better way to do it would be to 
Math.round(6.35 * 10) / 10
```

## Strings

The internal format for strings is UTF-16 and is not tied to page’s encoding.

Back ticks (``) allow for string interpolation, tagged templates and string that span multiple lines.

<aside>
🔥 Tagged strings [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals)

</aside>

Only quotes that are the same as the enclosing quotes will be escaped

strings have a **.length property**

**Accessing characters:** we can use [] operator or .charAt()

```jsx
let str = "hello";
str[0] // s
str.charAt(0) // s
str[100] // undefined
str.charAt(100) // ''  -> empty string
```

<aside>
⚡ for( .. of .. ) loop can be used on string because they are iterable

</aside>

```jsx
let str = "ola";
for ( let c of str) ; 
```

String are immutable

```jsx
let str = "selam";
str[2] = 'd' // won't work, but it won't stop execution
str // selam
```

**toUpperCase() and toLowerCase()**

### searching for a substring

**str.indexOf(substr, index)**

```jsx
let str = "search for sub string";
str.indexOf("search") // 0
str.indexOf("s") // 0
str.indexOf("s", 1) // 11
// if it doesn't find it
str.indexOf("max") // -1
str.indexOf("s", 100) // -1
```

<aside>
❓ Good Question

</aside>

```jsx
/* count occurances of a substring in a string */
let str = "JavaScript is a very cool language";
let target = "a";
let cnt = 0, pos = 0;

while(true){
 if(str.indexOf(target, pos++) != -1) {cnt++}
 else {break}
```

**str.lastIndexOf(substr, index) :-**  searches starting from the end of string to the start

**str.includes(substr, index) : -** returns true if str includes substr

**str.startsWith(prefix) : -** returns true if str starts with prefix

**str.endsWith(suffix)** : - 

### Getting a substring

**str.slice(start, end) :-** returns a in between start index and end index, including the start and excluding the end

```jsx
let str = "string"
str.slice(1, 3) // tr
// if the end is not given it's defaulted to legth of the string
str.slice(2) // ring
str.slice() // string
// negative values are also allowed
str.slice(-4, -1) // rin
```

**str.substring(start, [ end ]) :-** is the same as slice, but allows the start to be greater than end, and it doesn’t support negative arguments, it will just treat them as 0.

**str.substr(start, [ length ]) :-** returns part of the string starting from the start upto the given length. The start can be negative

```jsx
let str = "string"
str.substring(2, 4) // ri
str.substring(4, 2) // ri
str.substring(-2 ) // string

// substr
str.substr(2, 3) // rin
str.substr(3)    // ing
str.substr(-4, 2) // ri
```

### string comparison

methods **codePointAt(index) and String.fromCodePoint(point)**

```jsx
"Z".codePointAt(0) // 90  returns the code for the character at position 0
"a".codePointAt(0) // 97

String.fromCodePoint(90) // Z returns the character at the given code point
String.fromCodePoint(97) // a
```

In-order to do correct string comparisons in different languages, we can use the **localeCompare(str2)** method from the Intl.JS library.

```jsx
let str1 = "apple"
let str2 = "ball"

str1.localeCompare(str2) // -1 because it's less 
str2.localeCompare(str1) // 1
str1.localeCompare(str1) // 0
```

<aside>
📝 **Surrogate pairs** - All frequently used Unicode characters have 2byte codes, but some have two pairs of those. these characters have a length of two.

</aside>

<aside>
📝 **Diacritical marks and normalization -**  some letters have marks above / below them we can do this by adding the marks Unicode after our symbols. eg. ‘S\u0307’ is Ṡ and 'S\u0307\u0323’ is Ṩ. these bring an interesting problem in that similar looking characters will can get represented by different codes, to overcome this we can use the **normalize()** method. it will bring these characters to a common normal form

</aside>

## Arrays

Arrays are objects that can store different types of data, and they can grow dynamically, the elements of an array are stored in contiguous memory area.

```jsx
let A = new Array();
let B = [ "jhon", 4 , 8.3, function() {console.log("hello")} ]
B.length // 4
B[4] = "abc";
B.length // 5
B[3]() // hello
```

In JS arrays support both stacks ( LIFO ) and queues ( FIFO ), this is also called deque

Methods **.push() and .pop(): work with the end of an array, push returns the size of the new array**

Methods **.shift()** and .**unshift()** 

shift() works just like pop() but it operates on the first element

unshift() works just like push() but to the beginning of the array, unshift can take multiple arguments, it returns the size of the new array

Methods push and pop are much faster than shift and unshift, because they don’t have to reorder the elements after they are done.

<aside>
📝 Arrays are optimized to work fast, but if we start treating them like objects we will lose the benefits of the optimizations. The ways we can misuse Arrays are                                                                               ~                     By adding non-numeric properties                                                                              ~                     Creating gaps in between the indexes ar[0] ar[1000] nothing in between                    ~                      Filling the array in the reverse order

</aside>

### Looping through arrays:

```jsx
let arr = [ "name", "age", "school" ]
// using a traditional for loop
for(let i =0; i < arr.length ; i ++ ) arr[ i] ;
// using for .. of loop
for( let val of arr) var
// not recommended ! using for ... in loops
// 10 - 100 times slower than for ... of
for( let val in arr) arr[val]
```

<aside>
☢️ The **length** property : - Automatically updates when the array is modified. length is not the number of values in the array but the largest numeric index + 1.                                                        **The length property is writable:** We can change the value of length. if we increase it nothing special will happen, but if we decrease it, the array will be truncated

</aside>

```jsx
// !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
let arr = [ 1, 2,3 4,5 ]
arr.length = 2;
arr // [ 1, 2 ]
arr.length = 4; 
arr // [ 1, 2, undefined, undefined]
// therefore the simplest way to clear an array is arr.length = 0;
```

<aside>
⚠️ Careful when using new Array(). If we call the constructor with one numeric argument, it will take create an array with that size

</aside>

```jsx
let a = new Array(3);
a[1] // undefined
a.length // 3
```

Multidimensional arrays

```jsx
let a = [
		[ 1, 2, 3],
		[ 4, 5, 6],
		[ 7, 8, 9]
];
a[1][2] // 6
```

Arrays’ toString() : returns a comma separated list of the elements

<aside>
⚙ array.splice( from [ , count, elemt1, elment2, …] ) : returns the array of removed elements. Starting from ‘from’ remove ‘count’ elements and replace them with elemts

</aside>

```jsx
let arr1 = [1, 2, 3, 4, 5, 6]
arr1.splice( 1, 2 ) // =>  [ 4, 5, 6]
arr1 // [ 1, 2, 3 ]

arr1 = [1, 2, 3, 4, 5, 6]
arr1.splice(0, 2, "hello" ,"world"); // [ 1, 2 ]
arr1 // [ "hello", "world", 3, 4, 5, 6 ]

// we can use splice to insert with out deleting by setting count to 0
arr1.splice( 1, 0, "great" ) // []
arr1 // ["hello", "great", "world", 3, 4, 5, 6 ]

// negative indexes are allowed
arr1.splice( -4, 4) // [ 3, 4, 5, 6 ] 
arr1 // [ "hello", "great", "world" ]
```

<aside>
⚙ Method **slice( start , end)**, works on arrays and strings and returns a subarray from index start to end or a substring if it’s called on a string

</aside>

```jsx
let str = "hello"
let arr = ['h', 'e', 'l', 'l', 'o' ]

str.slice(1, 4) // 'ell'
arr.slice(1, 4) // [ 'e', 'l', 'l' ]

str.slice(2) // 'llo'
```

<aside>
⚙ Method array.concat( arg1, arg2, …) : returns array concatenated with the arguments. If the argument is has a Symbol.isConcatSpreadable it will get spreaded and concatenated to array. if not it will be appended to the end of array as it is

</aside>

```jsx
let arr = [ 1,2,3,4 ]
arr.concat("yee", "haa") // [ 1,2,3,4,"yee", "haa" ]
arr // [ 1,2,3,4 ]  concat doesn't alter arr
let arr2 = [ "ping", "pong" ]
arr.concat("I", "love", arr2 ) // [ 1,2,3,4,"I", "Love", "ping", "pong" ]

let obj = {
	0 : 'yee',
	1 : 'ha' , 
	[Symbol.isConcatSpreadable] : true,
}

arr.concat(obj) // [ 1,2,3,4,'yee', 'ha']
	
 
```

<aside>
⚙ Method array.forEach(func) : allows you to run a function on each element of the array. functions called inside forEach get passed the arguments ( item, index, array )

</aside>

```jsx
> let arr = [ 1,2,3 ]
> arr.forEach(console.log());
1 0 [ 1, 2, 3, 4, 5, 6 ]
2 1 [ 1, 2, 3, 4, 5, 6 ]
3 2 [ 1, 2, 3, 4, 5, 6 ]
>
> let func = (item, index, array) => 
			  console.log(`item : ${item}, index : ${index}, array: ${array}`) 
> arr.forEach(func);
item: 1, index: 0, array: 1,2,3,4,,5,6
item: 2, index: 1, array: 1,2,3,4,,5,6
item: 3, index: 2, array: 1,2,3,4,,5,6
```

**Searching in an array**

<aside>
⚙ methods arr.**indexOf(item, from),** arr.**lastIndexOf(item, from),** arr.**includes(item, from)** do the same things as their string counter-parts, they use the === comparison. ****

</aside>

```jsx
// one notable difference is
const arr = [NaN];
arr.indexOf(NaN) // -1
arr.includes(NaN) // true
```

<aside>
⚙ method arr.**find( function )** :- if function returns true, iteration is stopped and the item is returned, if false undefined is returned.

</aside>

```jsx
const arr = [
		{ id: 1, age: 34 },
		{ id: 2, age: 23 },
		{ id: 3, age: 34 },
]
// to find element with id 2
arr.find( (item, index, array) => item.id == 2  );
// { id: 2, age: 23 } 

// to find element with age 23
arr.find( item => item.age == 23 )
// { id: 2, age: 23 }
```

<aside>
⚙ method arr.**findIndex( function )** :- is the same as find() but, it returns the index if its found and -1 if not

</aside>

```jsx
> let arr2 = [ 1,2,3,4,5 ]
> arr2.findIndex( item => item == 3 );
2
```

<aside>
⚙ method **arr.filter( function ) :-** is also the same as find() but it returns an array of objects that fulfill the condition.

</aside>

```jsx
> arr.filter( item => item.age == 34 );
[ { id: 1, age: 34 }, { id: 3, age: 34 } ]
```

### **Transforming arrays**

<aside>
⚙ method arr.map(function) : returns a new array of the results of the function

</aside>

```jsx
> let arr = [ 1,2,3,4,5 ]
> arr.map(item => item + 1);
[ 2,3,4,5,6 ]

> [ "a", "ab", "abc" ].map( item => item.length ) 
[ 1,2,3 ] 
```

<aside>
⚙ method arr.**sort(func)** :- sorts arr based on func, if func is not given it will treat it like a string

</aside>

```jsx
> [ 13, 5, 1].sort();
[ 1, 13, 5 ] // because it tried to sort it like a string
/* if we want it to sort based on a function, we need to supply a function of two
 * arguments */
> function comp( a, b ) { 
			if( a > b ) return 1;
			if( a < b ) return -1; 
			if( a === b) return 0;
 }
> [ 13, 5, 1].sort(comp);
[ 1, 5, 13 ]

/* compare functions are only required to return a positive number to say greate and a
 * negative number to say less, so we can do this instead */
> [ 13, 5, 1].sort( (a, b) => a - b )
[ 1, 5, 13]
```

<aside>
⚙ method **arr.reverse()** :- reverses the array and returns it. it will change the array

</aside>

```jsx
let ar = [ 1, 2, 3, 4 ]
ar.reverse() // [ 4, 3, 2, 1 ]
ar // [ 4, 3, 2, 1 ]
```

**split and join**

<aside>
⚙ method string.split(delimiter, limit) : splits a string into an array based on the delimiter

</aside>

<aside>
⚙ method arr.join(delimiter) : does the reverse of split()

</aside>

```jsx
let str = "hello , world";
str.split() // [ "hello , world" ]
str.split("") // [ 'h', 'e', 'l', 'l', 'o', ' ' , ',', 'w' ...]
arr = str.split(",") // [ "hello ", " world" ]
arr.join(":") // "hello : world"

str.split(",", 1) // [ "hello" ]
```

**reduce and reduceRight**

<aside>
⚙ method arr.reduce( func( previous, item, index, array) ) :- reduces the array into a single value based on the function passed

</aside>

```jsx
let arr = [ 'a', 'b', 'c', 'd' ]
arr.reduce ( (prev, item) => prev + "," + item ); // 'a,b,c,d'
let arr2 = [ 1, 2, 3, 4 ]
arr2.reduce( (prev, current) => prev + current, 0 ); // 10 
/* we can specify an initial prev after the comma, we gave it 0 above
```

reduceRight() : does the same thing but does it from the right

**Array.isArray(arr) :** returns true if arr is an array

all array methods except sort() support an additional thisArg

[Array methods cheat-sheet](Array%20methods%20cheat-sheet%20dae118fbf12e482d92946a2a7f1d8f85.md)

## Iterables

Iterables are objects that can be used by a for … of loop

### **Symbol.iterator**

To make our on iterable objects we have to add a method  called Symbol.iterator, which returns an iterator object which includes a method called next which returns an object of the format { done: boolean, value: next_value }.

```jsx
let range = {
	from: 1,
	to: 5,
}

range[Symbol.iterator] = function(){
	return{
		this.current = this.from,
		this.last = this.to,
		next(){
			if(this.current <= this.last)
					return { done: false, value: this.current++ }
			else
					return {done: true}
		}
}
}

for(i of range) console.log(i) // 1, 2, 3, 4, 5
```

The object we want to iterate over can be used as the iterator itself to make the code simpler

```jsx
let range = {
	from: 5,
	to: 10,

	[Symbol.iterator](){
			this.current = this.from;
			return this
	}
	
	next(){
		if(this.current <= this.to)
			return { done: false, value: this.current++}
		else
			return { done: true }
	}
}

for( i of range ) // 5 6 7 8 9 10

/* But this implementation won't allow us to run simultaneously because they will
 * share the same iteration state */
```

**Calling an iterator explicitly**

We can iterate oven an iterable manually

```jsx
let str = "hello"
let iterator = str[Symbol.iterator]();

while(true){
	let result = iterator.next();
	if(result.done) break;
	console.log(result.value)
}
```

**Iterables and array-likes**

Iterables are objects that implement the Symbol.iterator method and Array likes are those that have indexes and a length property.

Array.from(obj, [mappingFunc, thisArg]) creates arrays from iterables or array-likes

```jsx
let a = Array.from(range) // the previous iterable
a // [ 5,6,7,8,9,10 ]
// we can pass a mapping function to be applied
let b = Array.from(range, num => num*2 ) 
b // [10, 12, 14, 16, 18, 20 ]

// to get array of characters from a string
let str = "hello"
let c = Array.from(str)
// this approach unlike split, works fine with surrogate pairs
```

# Maps

Maps are like objects in that they are keyed collections of data items, but unlike objects maps allow keys of any types.

<aside>
🚨 Main methods                                                                                                                                                    - **new Map()**  - create a map                                                                                                                      - **map.set(key, value)** - stores the value by key                                                                                   - **map.get(key)** - gets value by key                                                                                                                        - **map.has(key)** - returns true if key exists                                                                                                                     - **map.delete(key)** - deletes value by key                                                                                                - **map.clear()** - clears the map                                                                                                                 - **map.size** - returns the size of the map

</aside>

```jsx
let a = new Map()
a.set(1, "a")
a.set('1', "b")
a.set(true, "c")

a.get(1) // 'a'
a.get('1') // 'b'
a.get(true) // 'c'

a.has(false) // false
```

Every map.set() return the map itself so we can chain our sets

```jsx
a.set(1, "a").set('1', "b").set(true, "c")
```

**Maps from objects**

we can pass an array or other iterables with key value pairs when creating a map

```jsx
let a = new Map([ 1, "a"],
								[ "2", "b"],
								[ false, 4 ]);
```

The method **Object.entries(obj)**  creates a key value pair array from an object

```jsx
let obj = {
	name: "dani",
	age: 56,
	sex: "male"
}
let ent = Object.entries(obj);
ent // [ ["name", "dani"], ["age", 56], ["sex", "male"] ]

let m = new Map(ent);
```

**Iteration over maps**

maps.keys() - returns an iterable for keys

maps.values() - returns an iterable for values

maps.entries() - returns an iterable for entries, it’s used by default in a for…of loop

```jsx
> let m = new Map(Object.entries({ name: "jim", age: 45, height: 174 }) );

> for( i of m.keys() ) console.log(i)
name
age
height
> for( i of m.values() ) console.log(i)
jim
45
174
> for( i of m ) console.log(i)  // same as m.entries()
[ "name", "jim" ]
[ "age", 45 ]
[ "height", 175 ]
```

# Set

A set is a collection of values where a value can only occur once.

<aside>
🚨 Methods                                                                                                                                           **new Set( iterable )**  - creates a set, if an iterable is given in copies it in                                     **set.add( value )** - adds a value to a set                                                                                                            **set.delete( value )** - removes a value from a set                                                                                       **set.has( value )** - checks to see if the set has that value                                                                                     **set.clear()** - clears the set                                                                                                                        **set.size** - elements count

</aside>

**iterating over a set**

we can iterate over the set using a for.. of. loop of a forEach()

```jsx
let set = new Set( [ 1, 3, 5, 6 ] ) 
for( i of set ) console.log(i) // 1, 3, 5, 6

set.forEach( (value, valueAgain, map) => console.log(value) ) // 1, 3, 5, 6

/* The callback function called in forEach passes three arguments of which 2 are the 
 * values, this is done for compatibility with maps. In this spirit sets also have
 * the methods to return iterables, set.keys(), set.values(), set.entries() 
 * keys() and values() both return the values, and entries returns [ value, value ]*/
```

# WeakMap and WeakSet

The garbage collector doesn’t free up objects if they are referenced in arrays, maps or sets.

```jsx
let obj = { str: "I'll live forever" }
let arr = new Array(obj);
obj = null;
arr // [ { str: "I'll live forever" } ]
```

WeakMap and WeakSet don’t prevent the garbage collector from doing it’s job.

**WeakMaps and WeakSets**

WeakMaps can’t have primitive values as their keys. WeakMaps don’t support iteration.

<aside>
🚨 WeakMap methods                                                                                                                                **new WeakMap() -** creates a weak map                                                                                                 **weakmap.get( key ) -** gets a value                                                                                                                         **weakmap.set( key, value )                                                                                                                            weakmap.has( key )                                                                                                                           weakmap.delete( key )**

</aside>

The garbage collector performs clean up at a time of it’s choosing, so the size of weakmap can’t   be known, that’s why the other methods can’t operate on weakmaps. Weaksets are analogous to sets

### Object.keys(obj) .values(obj) and .entries(obj)

Objects just like maps and sets also support these methods, but unlike maps and sets they return arrays and not iterables,

```jsx
let obj = { name: "candy", sweet: true }

Object.keys(obj) // [ "name", "sweet" ]
Object.values(obj) // ["candy", true ]
```

These methods also ignore symbol properties, but we can use methods Object.getOwnPropertySymbols(obj)  - to return only the symbolic keys and method Reflect.ownKeys(obj) - to get all the keys

**Object.fromEntries( [] ) -** creates objects from entries, or maps

```jsx
let arr = [ ["name", "candy"], ["sweet",true] ]
Object.fromEntries(arr) // { name: "candy", sweet: true }
// also works on maps
```

## Destructuring assignment

destructuring assignment is a special syntax that allows us to unpack arrays or objects in to variables.

**Array destructuring**

```jsx
// Array destructuring
let [ first, last ] = [ "Barack", "Obama" ]
// we could also do this
let [ first, last ] = "Barack Obama".split(" ");
// we can skip over unwanted elements with commas
```

We can use any iterable on the right side

```jsx
let map = new Map();
map.set("name","jhon").set("age",8).set("height", 174);
let [ first, , last ] = map
first // "name", "jhon"
last // "height", 174
// or string
[a, b, c] = "1234";
a // 1
b // 2

```

We can also store the rest of the elements of the array using …var, with var being an array of everything else

```jsx
let arr = "hello world this is javascript speaking".split(" ");
let [ a, b, ...c] = arr
a // hello
b // world
c[0] // this
c[1] // is
```

When there are fewer elements in the array than the variables, the remaining variables are passed undefined.

We can define default values for the variables if that happens

```jsx
let [ a = "empty", b = "empty"] = ["hello"]
a // hello
b // empty

// the default values can also be calls to functions
```

**Object destructuring**

```jsx
let obj = {
	name: "Andy",
	sex: "male",
	age: 13,
}

let { age, sex, name } = obj; // order doesn't matter
```

If we want to assign the properties to variables with a different name we can use a colon

```jsx
let { name: n, sex, age: years } = obj
```

For potentially missing values we can specify a default using = 

```jsx
let { name, country = "UK", age } = obj
name // "Andy"
country // "UK"
// default values can also be calls to functions
```

If we want to combine both the equality and colon , we can do so as such

```jsx
let { name: n = "Jhon" } = obj
```

If we want the rest of the objects properties we can use …var, where var will be an object with the rest of the properties

```jsx
let { name, ...rest } = obj
name // "Andy"
rest // { sex: "male", age:13 }
rest.age // 13
```

**Nested destructuring**

If an object or array contains other objects and arrays we can use nested destructuring to extract the deeper portions.

```jsx
let box = {
	size: {
		width: 3,
		height: 5,
		depth: 2,
	},
	colors: [ "red", "green" ],
}

let {
		size: {
			width,
			height,
		},
		colors: [ color1, color2 ],
} = box;

width // 3
height // 5
color1 // red
size // undefined
colors // undefined
```

### Smart function parameters

When we have many parameters of which several are optional, it can become hard to remember their position, we can work around this problem by using object destructuring. we can pass them objects and the use the function to destructurize them into variables.

```jsx
let movie = {
	title: "Avengers",
	genre: [ "action", "drama" ],
};

function showMovie ( {title = "no title", duration = 2.00, genre} ){
	console.log(` ${title} , ${duration} , ${genre} `);
}

showMovie(movie);
// Avengers, 2.00, action, drama

// we can also specify default values and new names
function showMovie( { title: name = "no title", duration = 2.00, genre: [ g1 ]})

/* if we want all defaults we can't just do showMovie() but showMovie({}), to work 
 * around this problem we can specify {} as a default for the whole parameter */

function showMovie( {title = "no title", genre: [g1, g2]} = {} ) {} 
// doing ={} will make it possible to just call showMovie()
```

# Date

**Creation**

**new Date()** // without arguments creates a date object for the current date and time.

**new Date( milliseconds )** // creates a date object with the time equal to the number of milliseconds past since the Jan 1- 1970. The number of milliseconds past since since 1970 is called a timestamp

**new Date( date string )** — the date string get parsed with the Date.parse algorithm

**new Date( year, month, day, hours, minutes, seconds, milliseconds )** - The first two arguments are obligatory . Year must have four digits. month count starts from 0 - 11. default of day is 1. If the rest are missing they are assumed to be 0.

```jsx
new Date() // 2022-07-12T07:19:10.943Z -- current time
new Date(0) // 1970-01-01T00:00:00.000Z
new Date("2020-4-8") // 2020-04-07T21:00:00.000Z
new Date(1994, 4) // 1994-04-30T21:00:00.000Z
new Date(2002, 9, 11, 4) // 2002-10-11T01:00:00.000Z
```

**Access date components**

**getFullYear() -** get the year ( 4 digit ) 

**getMonth() -** 

**getDate() -** 

**getHours(), getMinutes(), getSeconds(), getMilliseconds()**

**getDay() //** 0-6 , where 0 is sunday

// These methods also have UTC counterparts like getUTCHours(), getUTCMinutes() etc.

**getTime() //** returns the date’s timestamp since Jan1/ 1970 UTC+0. has no UTC variant

**getTimezoneOffset() //**  returns the difference b/n local time zone and UTC in minutes

**Modify date components**

**setFullYear( year [, month, date ] )**

**setMonth( month [, date] )**

**setDate( date )** 

**setHours( hours [, minutes, seconds, millis ] )**

**setMinutes( minutes [, seconds, millis ] )**

**setSeconds( seconds [, millis ] )**

**setMilliseconds( millis )**

**setTime ( timestamp )** 

**Autocorrection**

```jsx
let a = new Date("2002\1\30");

a.setDate(a.getDate() + 4) 
a // 2002-02-02T21:00:00.000Z

// we can use this technique to get the date after some time has elapsed
a.setMinutes( a.getMinutes() + 40 )  // date after 40 minutes
a.setSeconds( a.getSeconds() + 4900 ) // date after 4900 seconds 
```

We can also set numbers ≤ 0

```jsx
let a = new Date("2002/2/23");
a.setDate(0) // last day of previous month
a.setDate(-3) // before 3 days and before 3 month for some reason
```

**Casting to numbers**

when dates get cast into numbers they return their timestamps.

```jsx
let a = new Date(2002, 1, 23);
+a // 1014411600000
```

**Date.parse()**

The string format should be: YYYY-MM-DDTHH:mm:ss.sssZ , 

where: YYYY-MM-DD – is the date: year-month-day. The character "T" is used as the delimiter. HH:mm:ss.sss – is the time: hours, minutes, seconds and milliseconds.
The optional 'Z' part denotes the time zone in the format +-hh:mm . A single letter Z that would mean UTC+0.

# JSON - ( JavaScript Object Notation )

Is a general format to represent Objects and values. It’s a data only cross language specification.

**JSON.stringify()** — converts objects in to strings, the resulting strings are called **JSON-encoded** or **serialized** or stringified or marshalled object.

**JSON.parse()** — converts JSON into objects

```jsx
let obj = {
	name: "my object",
	description: 'an object',
}
let a = JSON.stringify(obj);
a // '{"name":"my object","description":"an object"}'
typeof a // 'string'

let b = JSON.parse(a);
b // { name: 'my object', description: 'an object' }
```

JSON-encoded objects - use only double quotes for strings, so ‘an object’ becomes “an object”. They also double quote property names so name becomes “name”.

JSON methods can natively be applied to objects, arrays, primitives(strings, numbers, booleans, and null)

JSON.strigify() - skips methods, symbol properties, and properties that store undefined, since it’s data only 

```jsx
let obj = {
	method() { /* do smth */ },
	prop1: undefined,
	[Symbol("id")]: '01',
}

JSON.stringify(obj) // '{}'
```

stringify() — supports nested objects, as long as there’s no circular references.

```jsx
let room = { number: 23 };
let meetup = { 
	title: "Conference",
	participants: ["john", "ann"]
};
meetup.place = room; // meetup references room 
room.occupiedBy = meetup; // room references meetup 
JSON.stringify(meetup); // Error: Converting circular structure to JSON
```

JSON.stringify( value [, replacer, space] )

where replacer is an array of properties to encode or a mapping function(key, value) and space is the amount of space to use for formatting.

**replacer**

passing an array

```jsx
let obj = {
	name: "candy",
	age: 50,
	other: [{height: 5.6}, {weight: 140}],
	sex: "f",
}
let s = JSON.stringify(obj, ["name", "age", "other", "height"])
s // '{"name": "candy", "age": 50, "other": [{"height": 5.6}, {}]}'
// properties "sex" and "weight" are not encoded since they are not mentioned
/* we can safely skip properties that cause circular references by not adding them
 * to the replacer array */
```

passing a function

```jsx
// let's make candy 12 years old
let s = JSON.stringify(obj, (key, value) => {
		return key == "age" ? 12 : value;
});

s // '{"name": "candy", "age": 12, "other": [{"height": 5.6}, {}]}'
```

The first call passes a key value pair with the key(””) and value of the object itself { “”: obj }

**spaces**

used to specify the number of spaces used for indentation, used for logging and output

```jsx
let obj = { continent: "Africa", countries: [ "Ethiopia", "Mali" ] }
/*
{
  "continent": "Africa",
  "countries": [
    "Ethiopia",
    "Mali",
  ]
}
*/
```

**Custom toJSON conversion**

Just like toString() we can add a toJSON() method to our objects. JSON.stringify() calls this method if it exists

```jsx
let room = {
	number: 1,
	toJSON() {
		return this.number;
	}
}

	JSON.stringify(room) // 1
```

JSON.parse(str [, reviver])

we can customize the parsing of json to objects by passing a reviving function.

```jsx
// consider the case 
let obj = { name: "Elias", dob: new Date() }
let json = JSON.stringify(obj)
json // '{"name":"Elias","dob":"2022-07-13T08:24:52.846Z"}'
let obj2 = JSON.parse(json)
obj2 // { name: 'Elias', dob: '2022-07-13T08:24:52.846Z' }  ! dob became a string

let obj3 = JSON.parse(json, (key, value) => { 
	return key == "dob" ? new Date(value) : value;
});
obj3 // '{"name":"Elias","dob":"2022-07-13T08:24:52.846Z"}'
```