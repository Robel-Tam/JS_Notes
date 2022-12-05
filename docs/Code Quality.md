# Code Quality 

We can write strings on multiple lines by adding \ to the end of every line, this won’t include the newline to the string.

If we want to write multiple lines ( including the newline character) to a string we can include them inside backticks ( `` )

```jsx
let str = " hello world \
how are you";
console.log(str); // hello world how are you
str = ` hello
world`; // hello\nworld
```

Avoid nesting levels

```jsx
// instead of 
for( i=0; i<5; i++){
	if(cond){ /*smth*/ }
} 
// we can write
for( i =0 ; i<5; i++){
	if(!cond) continue;
	// smth
}
```

# polyfills and babel ?

transpiler