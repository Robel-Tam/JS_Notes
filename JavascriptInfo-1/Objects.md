# Objects

An object is a key: value pair collection.

Object properties can only be of type string or symbol. Properties of other types are casted into strings

```jsx
let user = new Object(); // object constructor syntax
let user = {}; // object literal syntax

let user  = {
	name: "no-name", 
	age: 6,
	"favorite book": "Harry poter",  // multiword identifiers
}

user.name // no-name
user["name"] // no-name
user['age'] // 5
user.favorite book // error
user."favorite book" // error
user["favorite book"] // harry poter

user // { name: 'no-name', age: 6, 'favorite book': 'harry poter' }
user["profession"] // undefinedd
user // { name: 'no-name', age: 6, 'favorite book': 'harry poter' }
user["profession"] = "nurse";
user // { name: 'no-name', age: 6, 'favorite book': 'harry poter' , profession: 'nurse' }

user.height = 7.8;
user // { name: 'no-name', age: 6, 'favorite book': 'harry poter' , 
			//  profession: 'nurse', height: 7.8 }

delete user.heigth;
user //  { name: 'no-name', age: 6, 'favorite book': 'harry poter' , profession: 'nurse' }
```

### computed properties

[ ] denote computed properties

```jsx
// we can use computed properties in two ways
let user = {
	name: "smt",
	age: 5,
}
let key = "name";
user[key] // -> smt , [] will compute the evaluate key to name

// in declaration
key = "name";
user = {
	[key]: "nameless"
	[ "$".repeat(5) ]: "smth",
}

user // { name: "nameless", '$$$$$': "smth" } 
```

<aside>
⚠️ Reserved key words are allowed as property names

</aside>

```jsx
let user = {
	for: 'smth',
	let: 'smth else',
}

// __proto__ . is allowed but will cause issues
```

<aside>
⚡ Property value shorthand

</aside>

```jsx
(name, age) => {
	return { 
		name: name,
		age: age,
	}
}

// is the same as

(name, age) => {
	return {
		name, age
	}
}

// we can mix them up if we like
(name, age) => {
	return {
		name, age, height: 1.78
	}
}
		
```

<aside>
⚠️ **Checking for existence**

</aside>

If a property doesn’t exist and we try to access it, it will return undefined. we can use this

```jsx
user.property === undefined // it doesn't exist
/* checking if it === undefined will fail if the property exists by contains undefined*/

/* We can also use the (in) operator 
	"key" in object
	 key in object
*/

let user =  {
	key: 45,
	tag: 4,
	1: 8,
}
let m = "tag";  

"key" in user // true
m in user // true   -- variable m containig the name " tag " got tested
1 in user // true
"1" in user // true
```

<aside>
📌 The for … in … loop

</aside>

```jsx
for( key in object ); // iterates over the propeties of objects, typeof(key) -> string
```

Integer properties are sorted, others appear in order of creation . 

```jsx
//if we don't want our integer property names to be sorted we can cheat by adding + 
let code = {"+1": 4, "+4": 9,}
```

<aside>
⚠️ Object variables store references

</aside>

```jsx

//because of this == and === on objects equate the referances
// and const objects can be changed without changing their reference
const a = { name: "cara" }
a.name = "jhon" // will work
a = {}  // error

```

### cloning objects

There are several way for cloning objects

```jsx
 
let obj1 = { p1: 1, p2: 2 }
let obj2 = {}

// using for... in
for( key in obj1 ) {
	obj2[key] = obj1[key]
}

// using Object.assign()
// Object.assing(dest, [arg1, arg2, ...] )  => returns dest
// we can use it to copy multiple objects into one
// the destination objects properties will be overwritten if they already exist

obj2 = Object.assign({}, obj1); 
// or simply Object.assign(obj2, obj1)

```

<aside>
⚠️ READ GARBAGE COLLECTION

</aside>

A general book “The Garbage Collection Handbook: The Art of Automatic Memory
Management” (R. Jones et al) covers some of them.

### Symbols:

A symbol value represents a unique value.

```jsx
// symbols can be created like
let l = Symbol([description string]);
let m = Symbol.for("smth"); // global Symbols
```

Symbols don’t auto-convert into strings

To use symbols as object properties we can do the following

```jsx
let sym = Symbol();
let sym2 = Symbol();

let obj = { 
	[sym]: "smth"
} ;

obj[sym2] = "smth else"; 
```

*Hiding symbolic properties principle*

Symbols are skipped in a for… in loop

Symbols are skipped in a Object .keys(object)

<aside>
⚠️ In contrast Object.assign(), copies both string and symbolic properties

</aside>

<aside>
🌐 Global symbols:  If we want to access the same symbols from different parts of our code we can use the global symbol registry. We can do that by creating symbols by using ( Symbol .for([”description”]). if there exists a symbol with a matching description it will be returned otherwise it will be created

</aside>

```jsx
let a = Symbol.for("abc");
let b = Symbol.for("abc");
a == b // true

Symbol.keyFor(a) // abc
```

```jsx
// System Symbols
Symbol.hasInstance
Symbol.toPrimitive  // and etc 
```

```jsx
// Symbols are not really hidden
// we can use 
Object.getOwnPropertySymbols(obj) // return symbol properties
Reflect.ownKeys(obj) // returns all keys including symbolic ones
```

## Methods

Are properties of an object that are functions

```jsx
// ways to declare methods
let a = { } 
a.method1 = function () { /* */ }

func2 = function() { } 
let b = { }
b.func2 = func2

let c = {
	func3: function(){}
	func4() { } // short hand
}

// this. keyword

let user = {
	name: "usr1",
	sayHi(){
		console.log("hi " + this.name);
	}
}
```

<aside>
👉🏿 this. is evaluated at runtime, so the following things are possible

</aside>

```jsx
let f = function() { alert(this.id) }
let o1 = { id: 1 } 
let o2 = { id: 2 } 
o1.f = f
o2.f = f

o1.f() // 1
o2.f() // 2

/* this. is not bound i.e. it can be called anywhere, if it has no
 * referance, it will return the window object if it's no in strict mode
 * and undefined if it is in strict mode
*/
```

Reference type:

The way method calls work, is by breaking up the process in to two operations

```jsx
obj.func() // there are two operations here, 1- the dot operator, and 2- ()
/* In method calls, the dot operator doesn't return a function but a special
 * intermediate internal reference type with the purpose of passing info from
 * the dot operator to the parenthesis, its composed of: 
 *                      1. base (i.e. the object)
 *                      2. name (i.e. the property name)
 *                      3. strict(i.e. bool whether it's in strict mode) 
 * when the parenthesis are called on the reference type, they recieve full info
 * about the object and it's methods, so this. will work
 * other operations like assignment( a = obj.func ), discard the reference type and 
 * return the function instead. 
 */ 
let obj = {
	id: "01",
	printId() { 
		alert("id: " + this.id )
	}
}
obj.printId() // id: 01
let func = obj.printId
func() // id: undefined
```

<aside>
👉🏿 Arrow functions have no this, if this. is used inside them it’s taken from the outer context

</aside>

## object to primitive conversion

All objects are true in Boolean context. there are only numeric and string conversions

During object to primitive conversion, JavaScript tries to find and call three object methods. 

1.  ***Symbol.toPrimitive(hint)*** is called if it exists, hint is either “string” , “number”, or  “default” , hint is the type of the conversion, “default” is called when the conversion is unclear, eg ( the binary + operator) can add numbers and concat strings and also the == operator resolves to ‘default’
2. for “string” hints , it tries to find and do toString() then valueOf()
3. for non-string hints, valueOf() → toString() 

All built in Objects except Date implement ‘default’ as number.   >< resolve to ‘number’

```jsx
let user = {
	name: "jhon",
	salary: 4000,
	[Symbol.toPrimitive](hint){
		console.log(hint);
		return hint == 'string' ? this.name : this.salary;
	}
}

console.log(+user) // number -> 4000
console.log(`${user}`) // string -> jhon
console.log(user + 2) // default -> 4002   

///
let usrz = {
    name: "dani",
    age: 15,
    toString(){
        console.log("inside toString...");
        return `toString -> ${this.name}`
    },
    valueOf(){
        console.log("inside valueOf...")
        return this.age
    }
}

`${usrz}` // inside toString...  -> dani
+usrz  // inside valueOf... -> 15
usrz + 1 // inside valueOf... -> 15

/* toString() and valueOf() must return a primitive and not an object */
/* if toPrimitive doesn't return a primitive there will be an error */
```

## constructors, and the new keyword

we can use constructors to create multiple similar objects, instead of using the regular {…} syntax.

constructor are like any other functions except for being called with the new operator and starting with capital letters.

```jsx
function User(name, age) { 
	// this = {} ;implicitly
		this.name = name;
		this.age = age;
		this.happy = true;
		// we can also add methods
		this.smile = function(){ console.log("Smiling..") }
	// return this; implicitly
}

let usr1 = new User("dani", 20);
```

To create a single object with new

```jsx
let user = new function(){ this.name ... }
```

To know if a function got called with new or not we can user [new.target](http://new.target) 

```jsx
let funky = function() {
	if(new.target) return "called with new";
	return "called without";
}

funky() // called without
new funky() // called with new

// this can be used to make functions a constructor regardless of the use of new
function User(name) {
	this.name = name;
	if(!new.target) return new User(name)
}
```

<aside>
👉🏿 Return in Constructors

</aside>

if there is a return in a constructor, and it returns an object , it will override this by returning the object, and if a primitive is returned it will be ignored.

<aside>
👉🏿 We can omit parenthesis if constructor has no arguments

</aside>

```jsx
function Man() { this.sex = 'male' }
let man = new Man
```