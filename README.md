# ES6-Learning
List of resources to learn ECMAScript 6.
<br>

### Table of Contents

* [`let`, `const` and block scope](#1-let-const-and-block-scope)
* [Destructuring Assignment](#2-destructuring-assignment)
* [Default Parameter Values](#3-default-parameters-values)
* [Rest Parameters](#4-rest-parameters)
* [Template Strings](#5-template-strings)

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

Template Strings use back-ticks (``) rather than the single or double quotes we're used to with regular strings.
