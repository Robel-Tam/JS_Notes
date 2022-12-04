# Prototypal inheritance

Objects have a hidden specification property  [ [ Prototype ] ] that is either <code>null</code> or a referance to another object.
If object <em>B</em> prototypically inherits from object <em>A</em>, and we try to read a property from <em>B</em> and it's missing, JS automatically tries to take it from <em>A</em>, this is called __prototypal inheritance__.
Even though the [ [ Prototype] ] property is hidden, there are several ways to set it, one of them is <code>__proto__</code>

### <code>__proto__</code>

```javascript
let animal = {
	move(){},
	breate(){alert("moving")}
}

let mammal = {
	giveBirth(){}
	breastFeed(){}
}

mammal.__proto__ = animal;
mammal.move() // moving

// we can add other Objects which inherit from mammal
let avian = {
	shitOnCars() {} 
	__proto__ : animal;
}

// we can make the inheritance chain longer
let human = {
	__proto__: mammal;
	inventStuff(){};
}
```

The only limitations to inheritance are that 
<ul>
<li>References can't go in circles</li>
<li>The value of <code>__proto__</code> can either be <code>null</code> or an object</li>
</ul>
In modern language <code>__proto__</code> is replaced by methods like <code>Object.getPrototypeOf</code> and <code>Object.setPrototypeOf</code> 

If we try to write to inherited data properties we will simply be overriding them.
```javascript
let animal = {
	move(){
		alert("animal on the move");
	}
}

let mammal = {
	__proto__: animal;
}

mammal.move = function(){
	alert("A mammal on the move");
}

mammal.move() // A mammal on the move
```

In prototypal inheritance methods can be shared but not object state i.e. <code>this</code> 
```javascript
let animal = {
	walk(){
		if(!this.isSleeping) alert("I'm walking")
	},
	sleep(){
		this.isSleeping = true;
	}
}

let rabbit = {
	__proto__ : animal;
	name: "White Rabbit"
}

rabit.sleep();

rabit.isSleeping // true
animal.isSleeping // undefined
```

<aside>
<big>for...in loop</big> <br/>
A for...in loop iterates over inherited properties too.
</aside>
#### <code>obj.hasOwnProperty(key)</code>
This method returns true if <code>obj</code> has it's own uninherited property named <code>key</code> 

## F.prototype
The way Objects get prototypes when they are created with a constructor i.e. using the <code>new</code> keyword, is a bit different.
Every function has a <code>prototype</code> property even if we don't supply it. And if <code>F.prototype</code> is an object, the new operator will automatically set it as the [ [prototype] ] for the new Object.

```javascript

let animal = {
	eats: true;
}

let Rabbit(name){
	this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("Fluffy");

rabbit.eats // true
```

<aside>If after the creation of the an object, the <code>prototype</code> of the constructor changes, the former object will retain the old prototype, but new object created with the constructor will have the new prototype</aside>

All objects have a <code>constructor</code> property.

The default <code>prototype</code> for a function is an object with the only property <code>constructor</code> that points back to the function itself, like so.

```javascript

let Plant(species){
	this.species = species;
	this.hasRoot = true;

	/* default prototype
	this.prototype = {constructor: Plant}
	*/
}

let rose = new Plant("Rosa");

// If we want to check which constructor rose was created with
rose.constructor // 
[Function: Plant]
```

## Native prototypes
The <code>prototype</code> property is also widely used in the core of javascript as well.

```jsx
let obj = {}

// when obj is created the default new Object() constructor is used internally

obj.constructor // [Function : Object]

Object.prototype == obj.__proto__  // true

// methods like toString() hasOwnProperty() etc, get inherited from the prototype
// of the object constructor
```

Other built-in Objects such as <code>Array</code> <code>Date</code> and <code>Function</code> also keep their methods prototypes. 
By specification all built in prototypes have <code>Object.prototype</code> at the top.

![[Screenshot from 2022-07-30 13-02-17.png]]

```jsx

let arr = []

arr.__proto__ == Array.prototype // true
arr.__proto__.__proto__ == Object.prototype // true
arr.__proto__.__proto__.__proto__  // Null
```

### primitives
Primitives are not objects, but if we try to access their properties, then temporary wrapper objects will be created using the built-in constructors <code>String</code> <code>Number</code> and <code>Boolean</code>. The methods of these objects also reside in prototypes. They are available as <code>String.prototype Number.prototype Boolean.prototype </code>

<aside>values <code>null</code> and <code>undefined</code> stand apart in that they have no Object wrappers. </aside>

We can modify native prototypes.
```jsx
String.prototype.show = function(){
	console.log(this.toString());
}

// the show() method will become available for all Strings

"Hello".show()  // prints out Hello
```
