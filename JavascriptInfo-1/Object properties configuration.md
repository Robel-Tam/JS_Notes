# Object properties configuration
## Property flags and descriptors
Object properties besides a value have three special attributes. writable, enumarable, and configurable.
#### <code>Object.getOwnPropertyDescriptor(obj, propertyName)</code>
 We can list these attributes using <code>Object.getOwnPropertyDescriptor(obj, propertyName)</code>
```javascript
>> let obj = {
	name: "Smith",
	age: 23,
}

>> let descriptor = Object.getOwnPropertyDescriptor(obj, "name");
>> descriptor
{
value: 'Smith',
writable: true,
enumarable: true,
configurable: true,
}
```

<ul>
<li><mark>Writable</mark> - can be changed, otherwise is read only</li>
<li><mark>enumerable</mark> - get's listed in loops, otherwise not</li>
<li><mark>configurable</mark> - property can get deleted, and these attributes can be modified, or else not</li>
</ul>
<h4> <code>Object.defineProperty( obj, property, descriptor)</code> </h4>

To change the flags we can use <code>Object.defineProperty( obj, property, descriptor)</code>

```javascript
>> Object.definePropery(obj, age, {value: 4, writable: false});
>> Object.getOwnPropertyDescriptor(obj, "age")
{
	value: 4,
	writable: false,
	enumarable: true,
	configurable: true,
}
```

If the propery exists , <code>defineProperty</code> updates it's flags, if it doesn't, it will create the property and set all unspecified flags as false. 
```javascript
>> Object.defineProperty(obj, height, {value: 174});
>> let desc = Object.getOwnPropertyDescriptor(obj, "height");
>> desc
{
	value: 174,
	writable: false,
	enumerable: false,
	configurable: false
}
```

<ul>The effects of the flags</ul>
<b>writable</b> - If we try to change the value of a non writable ( read only ) property, in strict mode it will cause an error, and if we are not in strict mode it will simply ingnore it.

```javascript
let user = {};
Object.defineProperty(user, "name", {value: "Andy"});

user.name = "candy"
// if we are in strict mode this will cause an error
// in non-strict mode flag violating operations will simply get ignored

user.name // "Andy"
```

<b>enumerable</b> - If this flag for a property is false, then it won't get show listed in a <mark><code>for...in</code></mark>, and it will also get excluded from an <mark><code>Object.keys</code></mark> 
```javascript
>> let user = {
		name: "andy",
	    toString(){ return this.name }
    }

>> Object.keys(user);
['name', 'toString']

>> Object.defineProperty(user, "toString", {enumerable: false});
>> for( i in user) console.log(i);
'name'

```

**configurable** - A non-configurable property can't get deleted or modified by another <code>Object.defineProperty()</code> , trying to do so will cause an error.

#### <code>Object.defineProperties(obj, descriptors)</code>
Allows us to define multiple properties at once.  <code>Object.defineProperties(obj, descriptors)</code>. It returns the object
```javascript
let user = {}
Object.defineProperties(user, {
	name: {value: "max", writable: true, enumerable: true},
	age: {value: 10, writable: true, enumerable: true}
})
```

#### <code>Object.getOwnPropertyDescriptors(obj)</code>
returns an object with all the properties of obj, and their descriptors.
This method can be used to create a flag aware clone of an object.
```javascript

let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```
## Sealing an object globally
There are methods which limit access to whole objects. These methods are rarely used in practice

<ul>
<big><strong><code>Object.preventExtension(obj)</code></strong></big><br/>
	<ul>Forbids addition of new properties to a obj</ul>
</ul>

<ul>
<big><strong><code>Object.seal(obj)</code></strong></big><br/>
	<ul>Forbids adding/ removing of properties, set's <code>configurable: false</code> for all existing properties</ul>
</ul>

<ul>
<big><strong><code>Object.freeze(obj)</code></strong></big><br/>
	<ul>Forbids adding/removing/changing of properties, set's <code>writable: false, configurable: false</code> for all existing properties.</ul>
</ul>

<ul>
<big><strong><code>Object.isExtensible(obj)</code></strong></big><br/>
	<ul>returns true if obj is extensible</ul>
</ul>

<ul>
<big><strong><code>Object.isSealed(obj)</code></strong></big><br/>
	<ul>returns true if obj is sealed</ul>
</ul>

<ul>
<big><strong><code>Object.isFrozen(obj)</code></strong></big><br/>
	<ul>returns true if obj is frozen</ul> 
</ul>


## Getter and Setter properties
There are two types of properties, **data properties** and **accessor properties**. Accessor properties are essentially functions that work on getting and setting properties. They are denoted by <code>get</code> and <code>set</code> literals.

```javascript
// Their syntax
let obj = {
	get propName(){}
	set propName(value){}
}
```

Let's declare a fullname accessor property inside object user
```javascript

let user = {
	firstName: "Biruk",
	lastName: "Tesfa",
	get fullName(){
		return `${this.firstName} ${this.lastName}`
	}
	set fullName(str){
		[this.firstName, this.lastName] = str.split(" ");
	}
}

user.firstName // 'Biruk'
typeof user.fullName // String
user.fullName // 'Biruk Tesfa'
user.fullName = "Girma Taye"
user.fullName // 'Girma Taye'
user.firstName // 'Girma'
user.lastName // 'Taye'
```

#### accessor property's descriptors
Descriptors for accessor properties, have no <code>value</code> and <code>writable</code> fields but they have <ul>
<li><code>get</code> - function</li>
<li><code>set</code> - function with one argument</li>
<li><code>enumerable</code></li>
<li><code>configurable</code></li>
</ul>
We can use <code>Object.defineProperty()</code> to create accessor properties

```javascript

let obj = {}

Object.defineProperty(obj, "propName", {
	get(){},
	set(arg){},
	enumerable: true, 
})

Object.getOwnProperties(obj);
/*
{
	propName: {
		get: [Function: get],
	    set: [Function: set],
	    enumerable: true,
	    configurable: false
	}
}
*/
```


If we run the following code <br/> <code>Object.defineProperty(obj, "propName", {value: "smth";})</code><br/>
Then <code>propName</code> will no longer be an accessor method but a data property.

If we try to pass a descriptor containing, say a <code>value</code> or a <code>writable</code> and a <code>get</code> or <code>set</code>, it will give us a TypeError because we can't specify both an accessor and a value attribute.

Getters and Setters can be used as wrappers over data properties to gain more control over them.

```javascript

let obj = {
	get average(){ return this._average},
	set average(value){
		if(value < 0 || value > 100) return;
		this._average = value;
	}
}

obj.average // undefined
obj.average = -2  // will revert because value passed is < 0
obj.average // undefined
obj.average = 79 
obj.average // 79


// we can also call _average directly
obj._average // 79
// But it's attributes that start with underscore are regarded to be internal and 
// we should not directly interact with them from outside.
```

