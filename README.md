# ES6-Learning
List of resources to learn ECMAScript 6.
<br>

### Table of Contents

* [`let`, `const` and block scope](#1-let-const-and-block-scope)
* [Destructuring Assignment](#2-destructuring-assignment)
* [Default Parameter Values](#3-default-parameters-values)
* [Rest Parameters](#4-rest-parameters)
* [Template Strings](#5-template-strings)
* [Modules](#6-modules)

### 1. let, const and block scope

`let` allows you to declare variable that is scope to containing block rather than its containing function. The behavior of `var` itself has not changed in any way.   

```javascript
console.log(a); // Uncaught ReferenceError: a is not defined

if (true) {
  let a = 'ES6';
}
```

Above `let` creates the `a` variable only in the scope within the curly braces of the `if`. Now even you put console after the if statement, error will be same.

Another important point, variables declared with `let` are hoisted but cannot be accessed before their declaration, because of `Temporal Dead Zone`. Here's a simple example:

```javascript

if (true) {
  console.log(a); // undefined
  let a = 'ES6';
}
```

At the top level of programs and functions, let, unlike var, does not create a property on the global object. For example:

```javascript
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```

Redeclaring the same variable within the same function or block scope raises a SyntaxError.

```javascript
if (true) {
  let a = 'ES6';
  let a = 'ES7'; // SyntaxError thrown.
}
```

`const` are block-scoped, much like variables defined using the `let` statement. The value of a constant cannot change through re-assignment, and it can't be redeclared.

```javascript
const GRADE = 16;  // Constants can be declared with uppercase or lowercase, but a common convention is to use all-uppercase letters.
var GRADE = 60; // the name GRADE is reserved for constant above, so this will fail - Duplicate declaration "GRADE"
let GRADE = 10;  // this throws an error also
```
it is important to note the nature of block scoping

```javascript
if (GRADE === 16) {     
    let GRADE = 20; // this is fine because it creates a block scoped GRADE variable 
    console.log(GRADE); // 20    
    var GRADE = 20; // this gets hoisted into the global context and throws an error
}
console.log(GRADE); // 16
```

You have to initialize `const` in it's declaration otherwise it will throw an error 

```javascript
const PI; // throws an error, missing initializer
```

`const` also works on objects. But attempting to overwrite the object throws an error.

```javascript
const OBJ = {'name': 'Sajjad'};
OBJ = {'name': 'Scott'}; // throws an error "OBJ" is read-only
```

However, object keys are not protected, so the following statement is executed without problem.

```javascript
OBJ.name = 'Scott'; 
```
The same applies to arrays. It is possible to push items into array but assigning a new array will throw error

```javascript
const ARR = [];

ARR.push('Sajjad'); // this is fine
ARR = ['Scott']; // this will throw an error - "ARR" is read-only
```
<br>

> :thumbsup: :thumbsup: To me always use `const` because it minimizes mutable state and will make developer life bit easy. If you REALLY need to change state, use `let`. `var` is dead to me. :speak_no_evil: :speak_no_evil:

### 2. Destructuring Assignment

The destructuring assignment makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

```javascript
const x = 2;
const y = 3;
[x,y] = [y,x]; 
console.log(x); // 3
console.log(y); // 2

// [x,y] = [y,x];  Right-hand side is an array built with the values that are in `y` and `x`. and left-hand side of this assignment is not an array.
// It looks like we are building array literal but really here we are working with individual variables `x` and `y` and sorrounding with square brackets because we are destructuring right-hand side array. 
// In other words we are telling javascript to take the first value of array (right-hand side) and put it in `x` and take second value and put it into `y`
// let [x,y] = [3,2] is also valid

const a = [1,2,3,4,5];
const [b,c] = a;
console.log(b); // 1
console.log(c); // 2

// Parsing an array returned from a function

function doWork() {
  return [1, 2];
}

const [a, b] = doWork();
console.log(a); // 1
console.log(b); // 2
```

You can ignore some returned values:

```javascript
function doWork() {
  return [1, 2, 3];
}
const [a, , b] = doWork();
console.log(a); // 1
console.log(b); // 3
```

You can ignore all returned values:

```javascript
function doWork() {
  return [1, 2, 3];
}
const [,,] = doWork();
```

When destructuring an array, you can assign the remaining part of it to a variable using the rest pattern.

```javascript
const [a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]
```

A variable can be assigned a default, in the case that the value pulled from the array is undefined.

```javascript
const [a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```
Lets destructure an object now:

```javascript
const obj = {a: 12, b: true}
const {a, b}: obj;

console.log(a); // 12
console.log(b); // true
``` 
A property can be unpacked from an object and assigned to a variable with a different name than the object property.
 
```javascript
const doWork = function() {  
  return {    
    empName: "Sajjad",    
    empCity: "Dubai",    
    empTwitter: "twitti",  
  };
};

const {empName: name, empCity: city, empTwitter: twitter} = doWork();

console.log(name); // Sajjad
console.log(city); // Dubai
console.log(twitter); // twitti
``` 

One can also drill into complex object this way:

```javascript
const doWork = function() {  
  return {    
    empName: "Sajjad",    
    empCity: "Dubai",    
    handles: {
      empTwitter: "twitti",
      empInstagram: "insti",  
    }
  };
};

const {empName: name, empCity: city, handles: {empTwitter: twitter, empInstagram: insta} } = doWork();

console.log(name); // Sajjad
console.log(city); // Dubai
console.log(twitter); // twitti
console.log(insta); // insti
```

For of iteration and destructuring:

```javascript
const people = [
  {
    name: 'Mike Smith',
    family: {
      mother: 'Jane Smith',
      father: 'Harry Smith',
      sister: 'Samantha Smith'
    },
    age: 35
  },
  {
    name: 'Tom Jones',
    family: {
      mother: 'Norah Jones',
      father: 'Richard Jones',
      brother: 'Howard Jones'
    },
    age: 25
  }
];

for (const {name: n, family: {father: f, mother: m}} of people) {
  console.log('Name: ' + n + ', Parents ==> Father: ' + f + ' & Mother: ' + m);
  // Name: Mike Smith, Parents ==> Father: Harry Smith & Mother: Jane Smith
  // Name: Tom Jones, Parents ==> Father: Richard Jones & Mother: Norah Jones
}
```

### 3. Default Parameters Values

We all know that in JavaScript, parameters of functions default to `undefined`. However, in some situations it might be useful to set a different default value. This is where default parameters can help. 

Default function parameters allow formal parameters to be initialized with default values if no value or `undefined` is passed.


```javascript
const doWork = function(name="Sajjad") {
  return name;
};

doWork();  // Sajjad
doWork("Andy");  // Andy
```
If we set an argument explicitly to undefined (not null as null is not the same as undefined, they both do evalute to false but null is something we typically use when we want to intentionally specify that there is some absence of a value.).  

```javascript
const doWork = function(a=1, b=2, c=3) {
  return [a,b,c];
};

const [a,b,c] = doWork(10,undefined);

console.log(a); // 10
console.log(b); // 2 as we are passing undefined so it will take default value
console.log(c); // 3 as we are not passing any value for third parameter so it will also take defualt value 
```

Default parameters are available to later default parameters:

```javascript
const doWork = function(a=1, b=a) {
  return [a,b];
};

const [a,b] = doWork(5);

console.log(a); // 5
console.log(b); // 5
```

You can use default value assignment with the destructuring assignment notation:

```javascript
const doWork = function([x,y]=[5, 16], {marks: stuMarks}={marks: 70}) {
  return x + y + stuMarks;
};

doWork() // 91
```

### 4. Rest Parameters

Rest parameters make it is easy to work with an unknown or variable number of arguments in a function. In other words we can say, the `rest parameter` allows to represent an indefinite number of arguments as an array. A `rest parameter` is always the last parameter in a function that has `...` prefix.  


```javascript
const sum = function(...numbers) {
  let result = 0;
  for(let i=0; i<numbers.length; i++) {
    result += numbers[i];
  }
  return result;
}

sum(4,6,9); // 19
```  

sum(); // 0 
// If you do not pass any parameters that can populate a `rest parameter` but it will be an empty array.
// So good thing is you do not need to check for `null` or `undefined`, you can just perform operations on that array.   
```

Traditionally in JavaScript we used to achieve above goal by using the implicit `arguments` object. `arguments` will hold all of the parameters passed to a function in an array like structure. The `arguments` object is not a real array, while `rest parameters` are array instances, meaning methods like `sort, map, forEach or pop` can be applied on it directly. In order to use array methods on the `arguments object`, it must be converted to a real array first. 

Rest parameters can be destructured, that means that their data can be extracted into distinct variables.

```javascript
const sum = function(...[x, y, z]) {
  return x + y + z;  
}

sum(1) // Nan as (b & c are undefined)
sum(3, 6, 8) // 17
sum(2, 4, 6, 8) // 12 the fourth parameter  is not destructured
```

Another example to count `rest parameter` elements:

```javascript
const fn1 = function(...stus) {
  return stus.length;
}

console.log(fn1()); // 0
console.log(fn1(5)); // 1
console.log(fn1(1,2,3)); // 3
```

### 5. Template Strings

JavaScript’s strings have always had limited functionality compared to strings in other languages. In ES6 they brought better
* String interpolation
* Embedded expressions
* Multiline strings without hacks
* String formatting
* String tagging for safe HTML escaping, localisation and more.

Template Strings use back-ticks `(``)` rather than the single or double quotes we're used to with regular strings.
```javascript
let message = `Hello World!`;
```

One of their first real benefits is string substitution. Substitution allows us to take any valid JavaScript expression (including say, the addition of variables) and inside a Template Literal, the result will be output as part of the same string.

Template Strings can contain placeholders for string substitution using the ${ } syntax, as demonstrated below:

```javascript
let name = "Sajjad";
console.log(`Hi, ${name}!`);  // "Hi, Sajjad!"
```
As all string substitutions in Template Strings are JavaScript expressions, we can substitute a lot more than variable names. For example, below we can use expression interpolation to embed for some readable inline math:

```javascript
let a = 10, b = 10;
console.log(`JavaScript first appeared ${a+b} years ago. Crazy!`); // JavaScript first appeared 20 years ago. Crazy!

console.log(`The number of JS MVC frameworks is ${2 * (a + b)} and not ${10 * (a + b)}.`); // The number of JS frameworks is 40 and not 200.
```

They are also very useful for functions inside expressions:

```javascript
function baz() { return "I am a result. Rarr"; }
console.log(`foo ${baz()} bar`);
//=> foo I am a result. Rarr bar.
```

The `${}` works fine with any kind of expression, including member expressions and method calls:

```javascript
// Example-1
let user = {name: 'Harry Potter'};
console.log(`Thanks for getting this into V8, ${user.name.toUpperCase()}.`); 
// "Thanks for getting this into V8, HARRY POTTER";


// Example-2
let thing = 'drugs';
console.log(`Say no to ${thing}. Although if you're talking to ${thing} you may already be on ${thing}.`);

// => Say no to drugs. Although if you're talking to drugs you may already be on drugs.
```

If you require backticks inside of your string, it can be escaped using the backslash character \ as follows:

```javascript
let greeting = `\`Hello\` World!`; // `Hello` World!
```

Multiline strings in JavaScript have required hacky workarounds for some time. Current solutions for them require that strings either exist on a single line or be split into multiline strings using a ` \ (blackslash)` before each newline. For example:

```javascript
var greeting = "Hello \
World";
```

Whilst this should work fine in most modern JavaScript engines, the behaviour itself is still a bit of a hack. One can also use string concatenation to fake multiline support, but this equally leaves something to be desired:


```javascript
var greeting = "Hello " +
"World";
```

Template Strings significantly simplify multiline strings. Simply include newlines where they are needed and BOOM.


```javascript
console.log(`string text line 1
string text line 2`);

// string text line 1
// string text line 2
```


Template Strings bring another powerful feature and that is `tagged templates`. Tagged Templates transform a Template String by placing a function name before the template string.

```javascript
function upper(strings,...values) {
  let str = "";
  for(let i=0; i<strings.length; i++) {
    if(i > 0) {
      if(typeof values[i-1] == "string") {
        str += values[i-1].toUpperCase();
      } else {
        str += values[i-1];
      }
    }
    str += strings[i];
  }
  return str;
}

var name = "sajjad",
	twitter = "sani723",
	classname = "es6 workshop";

console.log(
	upper`Hello ${name} (@${twitter}), welcome to the ${classname}!` ===
	"Hello SAJJAD (@SANI723), welcome to the ES6 WORKSHOP!"
);

// true
```

### 6. Modules

Before ES6 everything in every JS file of an application shared one global scope, that caused problems like naming collisions and security concerns.

One goal of ES6 was to solve the scope problem and bring some order to JS applications. That’s where modules come in.

> Modules are JavaScript files that are loaded in a different mode.

It helps with organization, maintenance, testing and most importantly dependency management. This separation allows us to pull in various modules only when we need them.
