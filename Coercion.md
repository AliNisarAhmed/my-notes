# Coercion

#### Object to Primitive Conversion

Objects (and arrays) will first be converted to their primitive value equivalent, and the resulting value (if a primitive but not already a number) is coerced to a number according to the ToNumber rules just mentioned.

To convert to this primitive value equivalent, the \[[ToPrimitive]] abstract operation (ES5 spec, section 9.1) will consult the value (using the internal DefaultValue operation -- ES5 spec, section 8.12.8) in question to see if it has a valueOf() method. If `valueOf()` is available and it returns a primitive value, that value is used for the coercion. If not, but `toString()` is available, it will provide the value for the coercion.

If neither operation can provide a primitive value, a `TypeError` is thrown.

As of ES5, you can create such a noncoercible object -- one without `valueOf()` and `toString()` -- if it has a null value for its \[[Prototype]], typically created with Object.create(null).

---

#### Implicit *String <--> Numbers* Conversion

```js
var a = "42";
var b = "0";

var c = 42;
var d = 0;

a + b; // "420"
c + d; // 42
```
However, saying that `+` needs one `string` to coerce whole to string is not correct

e.g.

```js
var a = [1,2];
var b = [3,4];

a + b; // "1,23,4"
```

The above is a consequence of the `ToPrimitive` coercion

*According to ES5 spec section 11.6.1, the `+` algorithm (when an object value is an operand) will concatenate if either operand is either already a string, or if the following steps produce a string representation. So, when `+` receives an object (including array) for either operand, it first calls the [[ToPrimitive]] abstract operation on the value, which then calls the [[DefaultValue]] algorithm with a context hint of `number`.**

*This operation is now identical to how the [[ToNumber]] abstract operation handles objects. The `valueOf()` operation on the array will fail to produce a simple primitive, so it then falls to a `toString()` representation. The two arrays thus become `"1,2"` and `"3,4"`, respectively. Now, `+` concatenates the two strings as you'd normally expect: `"1,23,4"`.*

---

 A commonly cited coercion gotcha is 
 ```js
{} + []     // 0 -> {} is treated as an empty code block
            // hence +[] results in 0
            // HOWEVER
({}) + []   // "[object Object]"
[] + {}     // "[object Object]" - The above rules of Object + any applies
 ```
 
 [] + {} vs. {} + [], as those two expressions result, respectively, in "[object Object]" and 0.
 
 ---
 
 #### Difference between `a + ""` vs `String(a)`
 
Because of how the [[ToPrimitive]] abstract operation works, `a + ""` invokes `valueOf()` on the a value, whose return value is then finally converted to a string via the internal `ToString` abstract operation. But `String(a)` just invokes `toString()` directly.

```js
var a = {
	valueOf: function() { return 42; },
	toString: function() { return 4; }
};

a + "";			// "42"

String( a );	// "4"
```

###### What about the other direction? How can we implicitly coerce from string to number?

```js
var a = "3.14";
var b = a - 0;

b; // 3.14
```

While far less common, `a * 1` or `a / 1` would accomplish the same result, as those operators are also only defined for numeric operations.

What about object values with the `-` operator? Similar story as for `+` above:
```js
var a = [3];
var b = [1];

a - b; // 2
```
Both array values have to become numbers, but they end up first being coerced to strings (using the expected toString() serialization), and then are coerced to numbers, for the - subtraction to perform on.

#### Operators `||` and `&&`

They don't actually result in a logic value (aka boolean) in JavaScript, as they do in some other languages.

So what do they result in? They result in the value of one (and only one) of their two operands. In other words, they select one of the two operand's values.

```js
var a = 42;
var b = "abc";
var c = null;

a || b;		// 42
a && b;		// "abc"

c || b;		// "abc"
c && b;		// null
```

#### Symbol Coercion
explicit coercion of a symbol to a string is allowed, but implicit coercion of the same is disallowed and throws an error.

```js
var s1 = Symbol( "cool" );
String( s1 );					// "Symbol(cool)"

var s2 = Symbol( "not cool" );
s2 + "";						// TypeError

```

Symbol values cannot coerce to number at all (throws an error either way), but strangely they can both explicitly and implicitly coerce to boolean (always true).

#### Loose Equals (==) vs Strict Equals (===)

A very common misconception about these two operators is: "== checks values for equality and === checks both values and types for equality." While that sounds nice and reasonable, it's inaccurate. Countless well-respected JavaScript books and blogs have said exactly that, but unfortunately they're all wrong.

The correct description is: "== allows coercion in the equality comparison and === disallows coercion."

##### Algorithm in the spec

The comparison `x == y`, where x and y are values, produces true or false. Such a comparison is performed as follows:

1. If Type(x) is the same as Type(y), then
    Return the result of performing Strict Equality Comparison `x === y`.
3. If x is `null` and y is `undefined`, return `true`.
4. If x is `undefined` and y is `null`, return `true`.
5. If Type(x) is `Number` and Type(y) is `String`, return the result of the comparison `x == !ToNumber(y)`.
6. If Type(x) is `String` and Type(y) is `Number`, return the result of the comparison `! ToNumber(x) == y`.
7. If Type(x) is `Boolean`, return the result of the comparison `!ToNumber(x) == y`.
8. If Type(y) is `Boolean`, return the result of the comparison `x == ! ToNumber(y)`.
9. If Type(x) is either `String`, `Number`, or `Symbol` and Type(y) is `Object`, return the result of the comparison `x == ToPrimitive(y)`.
10. If Type(x) is `Object` and Type(y) is either `String`, `Number`, or `Symbol`, return the result of the comparison `ToPrimitive(x) == y`.
11. Return `false`. 

The comparison `x === y`, where x and y are values, produces true or false. Such a comparison is performed as follows:

1. If Type(x) is different from Type(y), return false.
2. If Type(x) is Number, then
   * If x is NaN, return false.
   * If y is NaN, return false.
   * If x is the same Number value as y, return true.
   * If x is +0 and y is -0, return true.
   * If x is -0 and y is +0, return true.
   * Return false.
3. Return SameValueNonNumber(x, y). 

#### Comparing anything to Boolean
```js
var x = true;
var y = "42";

x == y; // false
```

The Type(x) is indeed Boolean, so it performs ToNumber(x), which coerces true to 1. Now, 1 == "42" is evaluated. The types are still different, so (essentially recursively) we reconsult the algorithm, which just as above will coerce "42" to 42, and 1 == 42 is clearly false.

Reverse it, and we still get the same outcome:
```js
var x = "42";
var y = false;

x == y; // false
```

The Type(y) is Boolean this time, so ToNumber(y) yields 0. "42" == 0 recursively becomes 42 == 0, which is of course false.

In other words, the value "42" is neither == true nor == false. At first, that statement might seem crazy. How can a value be neither truthy nor falsy?

But that's the problem! You're asking the wrong question, entirely. It's not your fault, really. Your brain is tricking you.

"42" is indeed truthy, but "42" == true is not performing a boolean test/coercion at all, no matter what your brain says. "42" is not being coerced to a boolean (true), but instead true is being coerced to a 1, and then "42" is being coerced to 42.

Whether we like it or not, ToBoolean is not even involved here, so the truthiness or falsiness of "42" is irrelevant to the == operation!

###### It is recommended to never, ever, under any circumstances, use `== true` or `== false`. Ever.
