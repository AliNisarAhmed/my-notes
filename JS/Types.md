# Types

In statically typed languages, we refer to type as "variable" or "container" having a type, whereas in JS we are focussed on value itself having types.

---

#### Type
The set of intrinsic behaviour we can expect, given any value e.g. `42` vs `"42"`.

---

Primitive Types in JS 
1. Boolean
2. Null
3. undefined
4. Number
5. String
6. Symbol

---
### Arrays
Arrays are numerically indexed (as you'd expect), but the tricky thing is that they also are objects that can have string keys/properties added to them (but which don't count toward the length of the array):

```js
var a = [];

a[0] = 1;
a["foobar"] = 2;

a.length;		// 1
a["foobar"];	// 2
a.foobar;		// 2
```

However, a gotcha to be aware of is that if a string value intended as a key can be coerced to a standard base-10 number, then it is assumed that you wanted to use it as a number index rather than as a string key!

```js
var a = [ ];

a["13"] = 42;

a.length; // 14
```

---

### `undefined` is an identifier, meaning it can be assigned a value!!! 

```js
function foo() {
	undefined = 2; // really bad idea!
}

foo();
```
```js
function foo() {
	"use strict";
	undefined = 2; // TypeError!
}

foo();
```

In both non-strict mode and strict mode, however, you can create a local variable of the name undefined. But again, this is a terrible idea!

```js
function foo() {
	"use strict";
	var undefined = 2;
	console.log( undefined ); // 2
}

foo();
```

##### Friends don't let friends override `undefined`. Ever.

---

### void Operator

While `undefined` is a built-in identifier that holds (unless modified -- see above!) the built-in `undefined` value, another way to get this value is the void operator.

The expression `void ___` "voids" out any value, so that the result of the expression is always the undefined value. It doesn't modify the existing value; it just ensures that no value comes back from the operator expression.

`var a = 42;`

`console.log( void a, a ); // undefined 42`

By convention (mostly from C-language programming), to represent the `undefined` value stand-alone by using void, you'd use void 0 (though clearly even void true or any other void expression does the same thing). There's no practical difference between void 0, void 1, and undefined.

But the void operator can be useful in a few other circumstances, if you need to ensure that an expression has no result value (even if it has side effects).

---

### NaN

NaN is a sentinel value - an otherwise notmal value that's assigned a special meaning.

NaN literally stands for "not a number", though this label/description is very poor and misleading, as we'll see shortly. It would be much more accurate to think of NaN as being "invalid number," "failed number," or even "bad number," than to think of it as "not a number."

For example:
```js
var a = 2 / "foo";		// NaN

typeof a === "number";	// true
```

In other words: "the type of not-a-number is 'number'!" Hooray for confusing names and semantics.

###### NaN is a very special value in that it's never equal to another NaN value (i.e., it's never equal to itself). It's the only value, in fact, that is not reflexive (without the Identity characteristic x === x). So, `NaN !== NaN`.

##### isNaN( )

The `isNaN(..)` utility has a fatal flaw. It appears it tried to take the meaning of NaN ("Not a Number") too literally -- that its job is basically: "test if the thing passed in is either not a number or is a number.
```js
var a = 2 / "foo";
var b = "foo";

a; // NaN
b; // "foo"

window.isNaN( a ); // true
window.isNaN( b ); // true -- ouch!
```

That is why ES6 added `Number.isNaN()`

---
### typeof Operator

`typeof` operator always returns a string ("boolean", "number"...)
```JavaScript
typeof undefined = "undefined" // (not undefined)
typeof "42" = "string"
typeof {a: 1} = "object"
typeof function () {} = "function"
```
---

```JavaScript
var a = "a" / 2;
a; // =>  NaN
typeof a = "number";
isNaN(a) = true;
isNaN("foo") = true //!!! => coz isNaN tries to first coerce to 
                    // a number and then checks. 
```
---

```
NaN !== NaN // The only value in JS not equal to itself
```
Using the above, we can check for `NaN` by
` return value !== value ? true: false`

---

```JavaScript
var foo = 0 / -3;
foo === -0         // true
foo === 0           // true
0 === -0            // true
(0 / -3) === (0 / 3)    // true
foo:        // 0 (!not -0)
//  (=== lies to us when it comes to -0 and NaN)

// to check for -0
return x === 0 && 1 / x === -Infinity
```
---
`Object.is()` is like "quadruple" equals (does not do any lying)
```JavaScript
Object.is("foo", NaN)       // false
Object.is(NaN, NaN)         // true
Object.is(0, -0)            // false
Object.is(-0, -0)           // true
```
---

`typeof Infinity` => "number"

---
```JS
Number(undefined) === NaN
Number(null) === 0
Number(true) === 1
Number(false) === 0
```
---

### Natives (built in functions)

1. String()
2. Boolean()
3. Number()
4. Array()
5. Object()
6. Function()
7. RegExp()
8. Date()
9. Error()
10. Symbol()  -- added ES6
11. BigInt() -- added ES10

```js
var a = new String( "abc" );

typeof a; // "object" ... not "String"

a instanceof String; // true

Object.prototype.toString.call( a ); // "[object String]"
```
The result of the constructor form of value creation (new String("abc")) is an object wrapper around the primitive ("abc") value.

## Internal \[[Class]]

Values that are typeof `"object"` (such as an array) are additionally tagged with an internal `\[[Class]]` property (think of this more as an internal classification rather than related to classes from traditional class-oriented coding). This property cannot be accessed directly, but can generally be revealed indirectly by borrowing the default `Object.prototype.toString(..)` method called against the value. For example:

```js
Object.prototype.toString.call( [1,2,3] );			// "[object Array]"

Object.prototype.toString.call( /regex-literal/i );	// "[object RegExp]"
```

So, for the array in this example, the internal `[[Class]]` value is "Array", and for the regular expression, it's "RegExp". In most cases, this internal `[[Class]]` value corresponds to the built-in native constructor (see below) that's related to the value, but that's not always the case. e.g.

```js
Object.prototype.toString.call( null );			// "[object Null]"
Object.prototype.toString.call( undefined );	// "[object Undefined]"
```
