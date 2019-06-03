# Concepts

### Scope 

Scope is the accessibility of variables, functions, and objects in some particular part of your code during runtime. In other words, scope determines the visibility of variables and other resources in areas of your code.

Scope is a set of rules for storing and looking up variables by their indentifier name.

---

### Closure
when a function remembers and accesses variables from its lexical scope, even when it is executed in a different scope.

---

`in` traverses the whole prototype chain, whether the property is `enumerable` or not

`for..in` loop only check for "enumerable" properties in the whole proto chain

---

### Lazy Expression
not evaluated unless & until when needed, e.g.
```JS
function bar () { console.log("!"); }
function foo(x = bar()) {
  return x;
}
```
- `bar()` will not be called, unless foo is called
- in `foo(1)`, bar() still will not be evaluated, as default parameter was not needed at all.
- only in `foo()`, will `bar()` will be evaluated.

---


### Lexical Environment

In JavaScript, every running function, code block, and the script as a whole have an associated object known as the Lexical Environment.

The Lexical Environment object consists of two parts:

 1.   Environment Record – an object that has all local variables as its properties (and some other information like the value of this).
 2.   A reference to the outer lexical environment, usually the one associated with the code lexically right outside of it (outside of the current curly brackets).

---

### Lexical Scope 

- Lexical scope means that scope is defined by author-time decisions of where functions are declared.
- No matter where a function is invoked from, or even how it is invoked, its lexical scope is only defined by where the function was declared.

---

### `Eval` & `With`
-  The former can modify existing lexical scope (at runtime) by evaluating a string of "code" which has one or more declarations in it.
-   The latter essentially creates a whole new lexical scope (again, at runtime) by treating an object reference as a "scope" and that object's properties as scoped identifiers.
- `eval` and `with` hurt performance optimizationsc carried out by JS engine.

---

### IIFE

IIFE allows us to not pollute the global scope with a "named" function, and it automatically executes the code without doing that.

---

### Anonymous Functions

Function declaration cannot be anonymous, only function Expressions can be

**Drawbacks of Anonymous Functions**
1. Anonymous functions have no useful name to display in stack traces, which can make debugging more difficult.

2. Without a name, if the function needs to refer to itself, for recursion, etc., the deprecated `arguments.callee` reference is unfortunately required. Another example of needing to self-reference is when an event handler function wants to unbind itself after it fires.

3. Anonymous functions omit a name that is often helpful in providing more readable/understandable code. A descriptive name helps self-document the code in question.

---

### try/catch

JavaScript in ES3 specified the variable declaration in the catch clause of a try/catch to be block-scoped to the catch block.

---

### Hoisting

- The Engine actually will compile your JavaScript code before it interprets it. Part of the compilation phase was to find and associate all declarations with their appropriate scopes.
- When you see `var a = 2;`  you probably think of that as one statement. But JavaScript actually thinks of it as two statements: `var a;` and `a = 2;`. The first statement, the declaration, is processed during the compilation phase. The second statement, the assignment, is left in place for the execution phase.
- **Declaration comes before Assignment**
- Important to note that hoisting is **per-scope**.
- Functions are hoisted first, and then variables.
```js
foo(); // 1

var foo;

function foo() {
	console.log( 1 );
}

foo = function() {
	console.log( 2 );
};
```
- While multiple/duplicate var declarations are effectively ignored, subsequent function declarations do override previous ones.
```js
foo(); // 3

function foo() {
	console.log( 1 );
}

var foo = function() {
	console.log( 2 );
};

function foo() {
	console.log( 3 );
}
```

--- 

##### Why is `0.1 + 0.2 !== 0.3`?

A number is stored in memory in its binary form, a sequence of ones and zeroes. But fractions like 0.1, 0.2 that look simple in the decimal numeric system are actually unending fractions in their binary form.

In other words, what is 0.1? It is one divided by ten 1/10, one-tenth. In decimal numeral system such numbers are easily representable. Compare it to one-third: 1/3. It becomes an endless fraction 0.33333(3).

So, division by powers 10 is guaranteed to work well in the decimal system, but division by 3 is not. For the same reason, in the binary numeral system, the division by powers of 2 is guaranteed to work, but 1/10 becomes an endless binary fraction.

There’s just no way to store exactly 0.1 or exactly 0.2 using the binary system, just like there is no way to store one-third as a decimal fraction.

The numeric format IEEE-754 solves this by rounding to the nearest possible number. These rounding rules normally don’t allow us to see that “tiny precision loss”, so the number shows up as 0.3. But beware, the loss still exists.

---

#### Why 6.35.toFixed(1) === 6.3? while 1.35.toFixed(1) === 1.4?

In JS, 0-4 round down, while 5-9 round up.
Internall, 6.35 is an endless binary
`6.35.toFixed(20)  // 6.34999999999999964473`
while 
`1.35.toFixed(20)  // 1.35000000000000008882`
hence, 1.35 rounds up, while 6.35 rounds down.

To Counter this, we can multiply and divide by 10
`Math.round((6.35 * 10) / 10)`  // 6.35 -> 63.5 -> 64
(63.5 has 0.5 as decimal part, Fractions divided by power of 2 are exactly represented in Binary)

---

### `this` Binding Rules

The only thing that matters for `this` is the function call-site. Call-site is the previous function call on the current call-stack, so if `foo` calls `bar`, which calls `baz`, which calls `qux`, the call sites are foo:global, bar:foo, baz:bar, qux: baz

#### 1. Default Binding

If the call-site is the global scope, then `this` defaults to 
 - `undefined` in strict-mode
 - global object in non-strict mode

e.g.
```js
function foo() {
	console.log( this.a );
}

var a = 2;

foo(); // 2
```

However, only the contents of the function declaration need to be in strict-mode, global strict-mode does not matter if the contents of the function's declaration are in sloppy mode.

```js
function foo() {
	"use strict";

	console.log( this.a );
}

var a = 2;

foo(); // TypeError: `this` is `undefined`
```

vs 

```js
function foo() {
	console.log( this.a );
}

var a = 2;

(function(){
	"use strict";

	foo(); // 2
})();
```

#### Implicit Binding

Does the call site have a context object.

```js
function foo() {
	console.log( this.a );
}
var obj = {
	a: 2,
	foo: foo
};
obj.foo(); // 2
```
Firstly, notice the manner in which foo() is declared and then later added as a reference property onto obj. **Regardless of whether foo() is initially declared on obj, or is added as a reference later (as this snippet shows), in neither case is the function really "owned" or "contained" by the obj object.**

At the point that foo() is called, it's preceded by an object reference to obj. When there is a context object for a function reference, the implicit binding rule says that it's that object which should be used for the function call's this binding.

- Only the top/last level of an object property reference chain matters to the call-site. For instance:
```js
function foo() {
	console.log( this.a );
}

var obj2 = {
	a: 42,
	foo: foo
};

var obj1 = {
	a: 2,
	obj2: obj2
};

obj1.obj2.foo(); // 42 -> from obj2
```

###### Sometimes the binding is implicitly lost:

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

var bar = obj.foo; // function reference/alias!

var a = "oops, global"; // `a` also property on global object

bar(); // "oops, global"
```
Even though bar appears to be a reference to obj.foo, in fact, it's really just another reference to foo itself. Moreover, the call-site is what matters, and the call-site is `bar()`, which is a plain, un-decorated call and thus the default binding applies.

Similarly
```js
let obj = {
  a: 2,
  foo() {
    console.log(this.a);
  }
}
let a = 'global'
let bar = obj.foo;
bar()     // 'global'
```

Or, more subtly

```js
function foo() {
	console.log( this.a );
}

function doFoo(fn) {
	// `fn` is just another reference to `foo`

	fn(); // <-- call-site!
}

var obj = {
	a: 2,
	foo: foo
};

var a = "oops, global"; // `a` also property on global object

doFoo( obj.foo ); // "oops, global"
```

Parameter passing is just an implicit assignment, and since we're passing a function, it's an implicit reference assignment, so the end result is the same as the previous snippet.

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

var a = "oops, global"; // `a` also property on global object

setTimeout( obj.foo, 100 ); // "oops, global"
```

**So, whenever we pass a reference of a function to another variable, `this` binding is lost.**

---

#### Explicit Binding

using `call(..)`, `apply()` & `bind()` available on `Function.prototype`.

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};
foo.call( obj ); // 2
```
`bind()` created hard-binding
```js
function foo(something) {
	console.log( this.a, something );
	return this.a + something;
}

var obj = {
	a: 2
};

var bar = foo.bind( obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5

bar.name = "bound foo";
```

###### Note: As of ES6, the hard-bound function produced by bind(..) has a .name property that derives from the original target function. For example: bar = foo.bind(..) should have a `bar.name` value of "bound foo", which is the function call name that should show up in a stack trace.

---

### `new` Binding
```js
function foo(a) {
	this.a = a;
}

var bar = new foo( 2 );
console.log( bar.a ); // 2
```
---
**`new` Binding > hard binding > implicit binding > default**

Now, we can summarize the rules for determining this from a function call's call-site, in their order of precedence. Ask these questions in this order, and stop when the first rule applies.

    1. Is the function called with new (new binding)? 
       If so, this is the newly constructed object.
  
        var bar = new foo();

    2. Is the function called with call or apply (explicit binding),
       even hidden inside a bind hard binding? 
       If so, this is the explicitly specified object.

        var bar = foo.call( obj2 )

    3. Is the function called with a context (implicit binding),
       otherwise known as an owning or containing object? 
       If so, this is that context object.

        var bar = obj1.foo()

    4. Otherwise, default the this (default binding). 
       If in strict mode, pick undefined, otherwise pick the global object.

        var bar = foo()
        
---

It should be clear that the default binding is the lowest priority rule of the 4. So we'll just set that one aside.

Which is more precedent, implicit binding or explicit binding? Let's test it:
```js
function foo() {
  console.log( this.a );
}

var obj1 = {
	a: 2,
	foo: foo
};

var obj2 = {
	a: 3,
	foo: foo
};

obj1.foo(); // 2
obj2.foo(); // 3

obj1.foo.call( obj2 ); // 3
obj2.foo.call( obj1 ); // 2
```

**So, explicit binding takes precedence over implicit binding, which means you should ask first if explicit binding applies before checking for implicit binding.**

Now, we just need to figure out where new binding fits in the precedence.

```js
function foo(something) {
	this.a = something;
}

var obj1 = {
	foo: foo
};

var obj2 = {};

obj1.foo( 2 );
console.log( obj1.a ); // 2

obj1.foo.call( obj2, 3 );
console.log( obj2.a ); // 3

var bar = new obj1.foo( 4 );    // despite calling as obj1.foo, new takes precedence and creates a new object for bar
console.log( obj1.a ); // 2
console.log( bar.a ); // 4
```

OK, new binding is more precedent than implicit binding. But do you think new binding is more or less precedent than explicit binding?

```js
function foo(something) {
	this.a = something;
}

var obj1 = {};

var bar = foo.bind( obj1 );
bar( 2 );
console.log( obj1.a ); // 2

var baz = new bar( 3 );   // bar is  bound to obj1, so "this" would refer to obj1, or will new create a wholly new object for baz?
console.log( obj1.a ); // 2
console.log( baz.a ); // 3
```
Whoa! bar is hard-bound against obj1, but new bar(3) did not change obj1.a to be 3 as we would have expected. Instead, the hard bound (to obj1) call to bar(..) is able to be overridden with new. Since new was applied, we got the newly created object back, which we named baz, and we see in fact that baz.a has the value 3.



---

### Exceptions

#### Passing null to call, bind or apply

If you pass null or undefined as a this binding parameter to call, apply, or bind, those values are effectively ignored, and instead the default binding rule applies to the invocation.

```js
function foo() {
	console.log( this.a );
}

var a = 2;

foo.call( null ); // 2
```

#### Indirect reference

Another thing to be aware of is you can (intentionally or not!) create "indirect references" to functions, and in those cases, when that function reference is invoked, the default binding rule also applies.

```js
function foo() {
	console.log( this.a );
}

var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };

o.foo(); // 3
(p.foo = o.foo)(); // 2
```

#### Arrow Functions

Arrow-functions adopt the this binding from the enclosing (function or global) scope.

```js
function foo() {
	// return an arrow function
	return (a) => {
		// `this` here is lexically adopted from `foo()`
		console.log( this.a );
	};
}

var obj1 = {
	a: 2
};

var obj2 = {
	a: 3
};

var bar = foo.call( obj1 );
bar.call( obj2 ); // 2, not 3!
```

```js
function foo() {
	setTimeout(() => {
		// `this` here is lexically adopted from `foo()`
		console.log( this.a );
	},100);
}

var obj = {
	a: 2
};
foo.call( obj ); // 2
```
* Arrow functions do not have their own `this`
* They also don't have `arguments` array-like object inside them.

---

### `let` is not attached to `global` object

```js
let user = "John";
alert(user); // John

alert(window.user); // undefined, don't have let
alert("user" in window); // false
```

The global object is not a global Environment Record

In versions of ECMAScript prior to ES-2015, there were no let/const variables, only var. And global object was used as a global Environment Record (wordings were a bit different, but that’s the gist).

But starting from ES-2015, these entities are split apart. There’s a global Lexical Environment with its Environment Record. And there’s a global object that provides some of the global variables.

As a practical difference, global let/const variables are definitively properties of the global Environment Record, but they do not exist in the global object.

Naturally, that’s because the idea of a global object as a way to access “all global things” comes from ancient times. Nowadays is not considered to be a good thing. Modern language features like let/const do not make friends with it, but old ones are still compatible.

---

### Recursive setTimeout

Recursive setTimeout guarantees a delay between the executions, setInterval – does not.

Let’s compare two code fragments. The first one uses setInterval:

```js
let i = 1;
setInterval(function() {
  func(i);
}, 100);

The second one uses recursive setTimeout:

let i = 1;
setTimeout(function run() {
  func(i);
  setTimeout(run, 100);
}, 100);

```
The recursive setTimeout guarantees the fixed delay (here 100ms).

That’s because a new call is planned at the end of the previous one.

###### Another Example


```js
let timerId = setTimeout(function tick() {
  alert('tick');
  timerId = setTimeout(tick, 2000); // (*)
}, 2000);
```

###### Recursive setTimeout guarantees a delay between the executions, setInterval – does not.

---

### SetTimeouts and Garbage Collection

When a function is passed in setInterval/setTimeout, an internal reference is created to it and saved in the scheduler. It prevents the function from being garbage collected, even if there are no other references to it.

```js
// the function stays in memory until the scheduler calls it
setTimeout(function() {...}, 100);
```

For `setInterval` the function stays in memory until clearInterval is called.

There’s a side-effect. A function references the outer lexical environment, so, while it lives, outer variables live too. They may take much more memory than the function itself. So when we don’t need the scheduled function anymore, it’s better to cancel it, even if it’s very small.

---

### Strict Mode

JavaScript's strict mode, introduced in ECMAScript 5, is a way to opt in to a restricted variant of JavaScript, thereby implicitly opting-out of "sloppy mode". Strict mode isn't just a subset: it intentionally has different semantics from normal code.

Strict mode introduces three kinds of breaking changes:

**Syntactic changes**: some previously legal syntax is forbidden in strict mode. For example:
 - The with statement is forbidden. It lets users add arbitrary objects to the chain of variable scopes, which slows down execution and makes it tricky to figure out what a variable refers to.
- Deleting an unqualified identifier (a variable, not a property) is forbidden.
- Functions can only be declared at the top level of a scope.
- More identifiers are reserved: implements interface let package private protected public static yield
**More errors**. For example:
- Assigning to an undeclared variable causes a ReferenceError. In non-strict mode, a global variable is created in this case.
- Changing read-only properties (such as the length of a string) causes a TypeError. In non-strict mode, it simply has no effect.
**Different semantics**: Some constructs behave differently in strict mode. For example:
- arguments doesn’t track the current values of parameters, anymore.
- `this` is undefined in non-method functions. In non-strict mode, it refers to the global object (window), which meant that global variables were created if you called a constructor without new.
