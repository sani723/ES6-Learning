# ES6-Learning
List of resources to learn ECMAScript 6.
<br>

### Table of Contents

* [`let`, `const` and block scope](#1-let-const-and-block-scope)

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
