# JavaScript Built-in Functions

---

## Objects

#### `Object.create()`

```js
Object.create(proto, [propertiesObject]);
```

**Parameters**

`proto` - The object which should be the prototype of the newly-created object.
`propertiesObject` - Optional. If specified and not `undefined`, an object whose enumerable own properties (that is, those properties defined upon itself and not enumerable properties along its prototype chain) specify property descriptors to be added to the newly-created object, with the corresponding property names. These properties correspond to the second argument of Object.defineProperties().

**Return value**

A new object with the specified prototype object and properties.

**Exceptions**

A TypeError exception if the propertiesObject parameter is null or a non-primitive-wrapper object.

#### `Object.assign()`

`Object.assign()` is a new ES6 feature championed by Rick Waldron that was previously implemented in a few dozen libraries. You might know it as `$.extend()` from jQuery or `_.extend()` from Underscore. Lodash has a version of it called `assign()`.

You pass in a destination object, and as many source objects as you like, separated by commas. It will copy all of the enumerable own properties by assignment from the source objects to the destination objects with last in priority. If there are any property name conflicts, the version from the last object passed in wins.

`Object.assign(target, ...sources)`

**Parameters**

`target` - The target object.
`sources` - The source object(s).

**Return Value** - The target object.

##### Description

Properties in the target object will be overwritten by properties in the sources if they have the same key. Later sources' properties will similarly overwrite earlier ones.

The Object.assign() method only copies enumerable and own properties from a source object to a target object. It uses `[[Get]]` on the source and `[[Set]]` on the target, so it will invoke getters and setters. Therefore it assigns properties versus just copying or defining new properties. This may make it unsuitable for merging new properties into a prototype if the merge sources contain getters. For copying property definitions, including their enumerability, into prototypes `Object.getOwnPropertyDescriptor()` and `Object.defineProperty()` should be used instead.

Both String and Symbol properties are copied.

In case of an error, for example if a property is non-writable, a `TypeError` will be raised, and the target object can be changed if any properties are added before error is raised.

Note that `Object.assign()` does not throw on null or undefined source values.

---

### `get`

```js

{get prop() { ... } }
{get [expression]() { ... } }

```

**Parameters**

`prop` - The name of the property to bind to the given function.
`expression` - Starting with ECMAScript 2015, you can also use expressions for a computed property name to bind to the given function.

##### Description

Sometimes it is desirable to allow access to a property that returns a dynamically computed value, or you may want to reflect the status of an internal variable without requiring the use of explicit method calls. In JavaScript, this can be accomplished with the use of a getter. It is not possible to simultaneously have a getter bound to a property and have that property actually hold a value, although it is possible to use a getter and a setter in conjunction to create a type of pseudo-property.

Note the following when working with the get syntax:

    It can have an identifier which is either a number or a string;
    It must have exactly zero parameters (see Incompatible ES5 change: literal getter and setter functions must now have exactly zero or one arguments for more information);
    It must not appear in an object literal with another get or with a data entry for the same property ({ get x() { }, get x() { } } and { x: ..., get x() { } } are forbidden).

A getter can be removed using the delete operator.

### `set`

```js
{set prop(val) { . . . }}
{set [expression](val) { . . . }}
```

##### Parameters

prop - The name of the property to bind to the given function.

val - An alias for the variable that holds the value attempted to be assigned to prop.
expression - Starting with ECMAScript 2015, you can also use expressions for a computed property name to bind to the given function.

##### Decsription

In JavaScript, a setter can be used to execute a function whenever a specified property is attempted to be changed. Setters are most often used in conjunction with getters to create a type of pseudo-property. It is not possible to simultaneously have a setter on a property that holds an actual value.

Note the following when working with the set syntax:

    It can have an identifier which is either a number or a string;
    It must have exactly one parameter
    It must not appear in an object literal with another set or with a data entry for the same property.
    ( { set x(v) { }, set x(v) { } } and { x: ..., set x(v) { } } are forbidden )

A setter can be removed using the delete operator.

---

### `Object.defineProperty(obj, prop, descriptor)`

**Parameters**
obj - The object on which to define the property.
prop - The name or Symbol of the property to be defined or modified.
descriptor - The descriptor for the property being defined or modified.

**Return value**

The object that was passed to the function.

##### Descriptor

Property descriptors present in objects come in two main flavors: data descriptors and accessor descriptors. A _data descriptor_ is a property that has a value, which may or may not be writable. An _accessor descriptor_ is a property described by a getter-setter pair of functions. A descriptor must be one of these two flavors; it cannot be both.

Both data and accessor descriptors are objects. They share the following optional keys:

`configurable` - true if and only if the type of this property descriptor may be changed and if the property may be deleted from the corresponding object. Defaults to false.

`enumerable` - true if and only if this property shows up during enumeration of the properties on the corresponding object. Defaults to false.

A data descriptor also has the following optional keys:

`value` - The value associated with the property. Can be any valid JavaScript value (number, object, function, etc).
Defaults to undefined.

`writable` - true if and only if the value associated with the property may be changed with an assignment operator.
Defaults to false.

An accessor descriptor also has the following optional keys:

`get`
Defaults to undefined.

`set`
Defaults to undefined.

**If a descriptor has neither of `value`, `writable`, `get` and `set` keys, it is treated as a data descriptor. If a descriptor has both `value` or `writable` and `get` or `set` keys, an exception is thrown.**

---

### `Object.defineProperties(obj, props)`

**Parameters**
`obj` - The object on which to define or modify properties.

`props` -
An object whose keys represent the names of properties to be defined or modified and whose values are objects describing those properties. Each value in props must be either a data descriptor or an accessor descriptor; it cannot be both (see Object.defineProperty() for more details).

```js
var obj = {};
Object.defineProperties(obj, {
  property1: {
    value: true,
    writable: true
  },
  property2: {
    value: "Hello",
    writable: false
  }
  // etc. etc.
});
```

---

### `Object.entries(obj)`

**Parameters**

`obj` - The object whose own enumerable string-keyed property [key, value] pairs are to be returned.

**Return value**

An array of the given object's own enumerable string-keyed property [key, value] pairs, in the same order as that provided by a `for...in` loop (the difference being that a for-in loop enumerates properties in the prototype chain as well). The order of the array returned by Object.entries() does not depend on how an object is defined. If there is a need for certain ordering then the array should be sorted first like `Object.entries(obj).sort((a, b) => b[0].localeCompare(a[0]));`.

---

### `Object.fromEntries(iterable)`

**Parameters**

`iterable` -
An iterable such as Array or Map or other objects implementing the iterable protocol.

**Return value**

A new object whose properties are given by the entries of the iterable.

##### Description

The `Object.fromEntries()` method takes a list of key-value pairs and returns a new object whose properties are given by those entries. The iterable argument is expected to be an object that implements an `@@iterator` method, that returns an iterator object, that produces a two element array-like object, whose first element is a value that will be used as a property key, and whose second element is the value to associate with that property key.

`Object.fromEntries()` performs the reverse of `Object.entries()`.

##### Examples

**Converting a Map to an Object**

With `Object.fromEntries`, you can convert from Map to Object:

```js
const map = new Map([["foo", "bar"], ["baz", 42]]);
const obj = Object.fromEntries(map);
console.log(obj); // { foo: "bar", baz: 42 }
```

**Converting an Array to an Object**

With Object.fromEntries, you can convert from Array to Object:

```js
const arr = [["0", "a"], ["1", "b"], ["2", "c"]];
const obj = Object.fromEntries(arr);
console.log(obj); // { 0: "a", 1: "b", 2: "c" }
```

**Object transformations**

With Object.fromEntries, its reverse method Object.entries(), and array manipulation methods, you are able to transform objects like this:

```js
const object1 = { a: 1, b: 2, c: 3 };

const object2 = Object.fromEntries(
  Object.entries(object1).map(([key, val]) => [key, val * 2])
);

console.log(object2);
// { a: 2, b: 4, c: 6 }
```

---

### `Object.getOwnPropertyDescriptor(obj, prop)`

Parameters

`obj` - The object in which to look for the property.

`prop` - The name or Symbol of the property whose description is to be retrieved.

**Return value**

A property descriptor of the given property if it exists on the object, `undefined` otherwise.

### `Object.getOwnPropertyDescriptors(obj)`

**Parameters**

`obj` - The object for which to get all own property descriptors.

**Return value**

An object containing all own property descriptors of an object. Might be an empty object, if there are no properties.

---

### `Object.getOwnPropertyNames(obj)`

`Object.getOwnPropertyNames()` returns an array whose elements are strings corresponding to the enumerable and non-enumerable properties found directly in a given object obj. (`Obect.keys()` returns only enumerable properties) The ordering of the enumerable properties in the array is consistent with the ordering exposed by a `for...in` loop (or by `Object.keys()`) over the properties of the object. The ordering of the non-enumerable properties in the array and the ordering among the enumerable properties is not defined.

If you want only the enumerable properties, see `Object.keys()` or use a `for...in` loop (note that this will also return enumerable properties found along the prototype chain for the object unless the latter is filtered with `hasOwnProperty()`).

### `Object.getOwnPropertySymbols(obj)`

Similar to `Object.getOwnPropertyNames()`, you can get all symbol properties of a given object as an array of symbols. Note that `Object.getOwnPropertyNames()` itself does not contain the symbol properties of an object and only the string properties.

As all objects have no own symbol properties initially, `Object.getOwnPropertySymbols()` returns an empty array unless you have set symbol properties on your object.

### `Object.getPrototypeOf()`

The `Object.getPrototypeOf()` method returns the prototype (i.e. the value of the internal `[[Prototype]]` property) of the specified object.

In ES5, it will throw a `TypeError` exception if the obj parameter isn't an object. In ES2015, the parameter will be coerced to an Object.

```js
Object.getPrototypeOf("foo");
// TypeError: "foo" is not an object (ES5 code)
Object.getPrototypeOf("foo");
// String.prototype                  (ES2015 code)
```

---

### `Object.is(val1, val2)

`Object.is()` determines whether two values are the same value. Two values are the same if one of the following holds:

    both `undefined`
    both `null`
    both `true` or both `false`
    both strings of the same length with the same characters in the same order
    both the same object (means both object have same reference)
    both numbers and
        both `+0`
        both `-0`
        both `NaN`
        or both non-zero and both not `NaN` and both have the same value

This is not the same as being equal according to the `==` operator. The `==` operator applies various coercions to both sides (if they are not the same Type) before testing for equality (resulting in such behavior as `"" == false` being true), but `Object.is` doesn't coerce either value.

This is also not the same as being equal according to the `===` operator. The `===` operator (and the `==` operator as well) treats the number values `-0` and `+0` as equal and treats `Number.NaN` as not equal to `NaN`.

---

The `Object.isExtensible()` method determines if an object is extensible (whether it can have new properties added to it).

The `Object.isFrozen()` determines if an object is frozen.

The `Object.isSealed()` method determines if an object is sealed

---

### `Object.preventExtensions()`

An object is extensible if new properties can be added to it. `Object.preventExtensions()` marks an object as no longer extensible, so that it will never have properties beyond the ones it had at the time it was marked as non-extensible. Note that the properties of a non-extensible object, in general, may still be deleted. Attempting to add new properties to a non-extensible object will fail, either silently or by throwing a TypeError (most commonly, but not exclusively, when in strict mode).

`Object.preventExtensions()` only prevents addition of own properties. Properties can still be added to the object prototype.

This method makes the `[[prototype]]` of the target immutable; any `[[prototype]]` re-assignment will throw a TypeError. This behavior is specific to the internal `[[prototype]]` property, other properties of the target object will remain mutable.

There is no way to make an object extensible again once it has been made non-extensible.

## Arrays
