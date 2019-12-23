# Built-in Objects & Methods


### Iterables & Array-Likes
1. **_Iterables_** are objects that implement the `[Symbol.iterator]` method.
2. **_Array-likes_** are objects that have a length and indexes.

e.g `arrayLike = { 0: 'Hello', 1: "world", length: 2}` is an array-like, but not an iterator as it does not have `[Symbol.iterator]` method defined.

We can make the above object into an iterable by using `Array.from`.

The Array like must have a `length` prop for the above operation to be successful.

`Array.from(obj[, mapFn, thisArg])` takes the obj, and a mapping function which gets `(elem, i)` as arguments, and a `this` arg.

---

### Maps

Map is a collection of keyed data items, just like an Object. But the main difference is that **Map allows keys of any type.**

The main methods are:

    new Map() – creates the map.
    map.set(key, value) – stores the value by the key.
    map.get(key) – returns the value by the key, undefined if key doesn’t exist in map.
    map.has(key) – returns true if the key exists, false otherwise.
    map.delete(key) – removes the value by the key.
    map.clear() – clears the map
    map.size – returns the current element count.
    map.keys()
    map.values()
    map.entries() - gives [key, value] pair iterator
    we can also make a map using [ [key,value] ],  array of arrays of key,value pairs
      the same made by Object.entries();

**Map use the `SameValueZero` internal algo to compare keys with inputs**
**Map iteration order is same as insertion order, unlike objects**

---

### Sets

A Set is a collection of values, where each value may occur only once.

Its main methods are:

    new Set(iterable) – creates the set, optionally from an array of values (any iterable will do).
    set.add(value) – adds a value, returns the set itself.
    set.delete(value) – removes the value, returns true if value existed at the moment of the call, otherwise false.
    set.has(value) – returns true if the value exists in the set, otherwise false.
    set.clear() – removes everything from the set.
    set.size – is the elements count.
    set.keys()
    set.values() (same as set.keys()) 
    set.entries - [value, value] pair - to make them compatible with maps

### WeakSet & WeakMap
WeakSet is a special kind of Set that does not prevent JavaScript from removing its items from memory. WeakMap is the same thing for Map.

WeakMap does not support iteration and methods keys(), values(), entries(), so there’s no way to get all keys or values from it.

WeakMap has only the following methods:

    weakMap.get(key)
    weakMap.set(key, value)
    weakMap.delete(key)
    weakMap.has(key)

Why such a limitation? That’s for technical reasons. If an object has lost all other references (like john in the code above), then it is to be garbage-collected automatically. But technically it’s not exactly specified when the cleanup happens.

The JavaScript engine decides that. It may choose to perform the memory cleanup immediately or to wait and do the cleaning later when more deletions happen. So, technically the current element count of a WeakMap is not known. The engine may have cleaned it up or not, or did it partially. For that reason, methods that access WeakMap as a whole are not supported.

Now where do we need such thing?

The idea of WeakMap is that we can store something for an object that should exist only while the object exists. But we do not force the object to live by the mere fact that we store something for it.

---

### JSON

JavaScript provides methods:

    JSON.stringify to convert objects into JSON.
    JSON.parse to convert JSON back into an object.
JSON.stringify can be applied to primitives as well.

Natively supported JSON types are:

    Objects { ... }
    Arrays [ ... ]
    Primitives:
        strings,
        numbers,
        boolean values true/false,
        null.
JSON is data-only cross-language specification, so some JavaScript-specific object properties are skipped by JSON.stringify.

Namely:

    Function properties (methods).
    Symbolic properties.
    Properties that store undefined.

**Circular References inside Objects give rise to Error**

The full syntax of JSON.stringify is:

`let json = JSON.stringify(value[, replacer, space])`

**replacer**
    Array of properties to encode OR a mapping function `function(key, value)`.
**space**
    Amount of space to use for formatting

```js
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup references room
};

room.occupiedBy = meetup; // room references meetup

alert( JSON.stringify(meetup, function replacer(key, value) {
  alert(`${key}: ${value}`); // to see what replacer gets
  return (key == 'occupiedBy') ? undefined : value;
}));

/* key:value pairs that come to replacer:
:             [object Object]
title:        Conference
participants: [object Object],[object Object]
0:            [object Object]
name:         John
1:            [object Object]
name:         Alice
place:        [object Object]
number:       23
*/
```

Please note that replacer function gets every key/value pair including nested objects and array items. It is applied recursively. The value of this inside replacer is the object that contains the current property.

The first call is special. It is made using a special “wrapper object”: {"": meetup}. In other words, the first (key, value) pair has an empty key, and the value is the target object as a whole. That’s why the first line is ":[object Object]" in the example above.

The idea is to provide as much power for replacer as possible: it has a chance to analyze and replace/skip the whole object if necessary.

##### Custom toJSON

Like toString for string conversion, an object may provide method toJSON for to-JSON conversion. JSON.stringify automatically calls it if available.

```js
let room = {
  number: 23,
  toJSON() {
    return this.number;
  }
};

let meetup = {
  title: "Conference",
  room
};

alert( JSON.stringify(room) ); // 23

alert( JSON.stringify(meetup) );
/*
  {
    "title":"Conference",
    "room": 23
  }
*/
```

##### JSON.parse

To decode a JSON-string, we need another method named JSON.parse.

The syntax:

`let value = JSON.parse(str[, reviver]);`

**str**
    JSON-string to parse.
**reviver**
    Optional function(key,value) that will be called for each (key, value) pair and can transform the value.

For instance:

```js
// stringified array
let numbers = "[0, 1, 2, 3]";

numbers = JSON.parse(numbers);

alert( numbers[1] ); // 1

// Or for nested objects:

let user = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

user = JSON.parse(user);

alert( user.friends[1] ); // 1
```
