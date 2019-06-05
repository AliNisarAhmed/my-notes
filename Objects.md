### Objects

The `new` keyword automates 4 things

1. creates a new empty object.
2. Sets the instance \[[Prototype]] (an internal reference) to Constructor.prototype, so that Object.getPrototypeOf(instance) === Constructor.prototype and instance.**proto** === Constructor.prototype.
3. assigns `this` to the new object
4. returns the new object (if function itself does not return any object, that is).

---

The `new` keyword is used to invoke a constructor. What it actually does is:

- Create a new instance
- Bind `this` to the new instance
- Reference the new object’s delegate [[Prototype]] to the object referenced by the constructor function’s `prototype` property.
- Reference the new object’s `.constructor` property to the constructor that was invoked.
- Names the object type after the constructor, which you’ll notice mostly in the debugging console. You’ll see `[Object Foo]`, for example, instead of `[Object object]`.
- Allows `instanceof` to check whether or not an object’s prototype reference is the same object referenced by the `.prototype` property of the constructor.

`new` is weird

WAT? `new` also does some weird stuff to return values. If you try to return a primitive, it won’t work. If you return any other arbitrary object, that does work, but `this` gets thrown away, breaking all references to it (including `.call()` and `.apply()`), and breaking the link to the constructor’s `.prototype` reference.

---

- constructors do not have a return statement, But if there is a `return` statement, then the rule is simple:

* If return is called with object, then it is returned instead of this.
* If return is called with a primitive, it’s ignored.

Parentheses after `new User()` can also be ommitted, like `new User`, thought it is nor considered a good style.

---

Inside a function we can check whether it was called with `new` or not, using `new.target` property.

```js
function User() {
  alert(new.target);
}

// without "new":
User(); // undefined

// with "new":
new User(); // function User { ... }
```

**This approach is sometimes used in libraries to make the syntax more flexible. So that people may call the function with or without new, and it still works.**

```js
function User(name) {
  if (!new.target) {
    // if you run me without new
    return new User(name); // ...I will add new for you
  }

  this.name = name;
}

let john = User("John"); // redirects call to new User
alert(john.name); // John
```

<br>

---

<br>

- Objects are built by "constructor" calls `=>` a function with a `new` keyword
- A _constructor_ is a function called with the `new` keyword

<br>

---

- Instantiation of classical "classes" is a "copy" operation (like heredity in children, or blueprint), changing one does not alter the other.
- But in JS, a constructor makes an object linked to its own `prototype`.

<br>

---

\[[Prototype]] - the linkage b/w two objects, created at the time when the object is created

\[[Prototype]] - affects the behaviour of an obj, how? `=>` we can call methods up the proto chain

Three ways to find out the \[[Prototype]]

1.  `__proto__` (dunder proto)
2.  `Object.getPrototypeOf`
3.  `Object.constructor.prototype`

---

`super` keyword is static, unlike `this`, reason being performance, hence it is statically fixed at compile time.

---

#### Behaviour Delegation

When an object finds a method inside of an object it it protypcally linked to.

---

OLOO -> Objects linked to other objects (embraces true "Object Oriented Programming", without `classes`)

---

The power of `JS` is to created objects, link them together, and use behaviour delegation between them, w/o bothering with classes and paraphernalia.

---

##### How is JS's \[[Prototype]] chain different than traditional/classical inheritence

Instead of copying things down the `class` instance, we have _live_ links up the proto chain.

---

#### What does \[[Prototype]] delegation means and how does it describe object linking in JS?

Prototype chain is a delegation link from one object to another that allows those objects to share context at call-time _aka_ virtual composition.

###### Advantages

- Independent testablility

###### Disadvantages

- Loss of encapsulation (everything is public)
- more complex code

---

#### Object to Primitive Conversion

When an Object is used in a context where a primitve is required, internal `ToPrimitive` algortihm is used.

the algorith uses a "hint", which can be "string", "number" or default.

Algorithm

1. Call `obj[Symbol.toPrimitive](hint)` function, if it exists on `obj`.
2. Otherwise, if "hint" is "string":
   - Try `obj.toString()`
   - then `obj.valueOf()`
3. if hint is "number" or default:
   - Try `obj.valueOf`,
   - then try `obj.toString()`.

##### Symbol.toPrimitive

```JS
obj[Symbol.toPrimitive] = function(hint) {
  // return a primitive value
  // hint = one of "string", "number", "default"
}
```

##### toString(), or valueOf()

```JS
let user = {
  name: "John",
  money: 1000,

  // for hint="string"
  toString() {
    return `{name: "${this.name}"}`;
  },

  // for hint="number" or "default"
  valueOf() {
    return this.money;
  }

};
```

**Often we want a single “catch-all” place to handle all primitive conversions. In this case we can implement `toString()` only**

**The only mandatory thing: these methods must return a primitive.**

1. For historical reasons, methods toString or valueOf should return a primitive: if any of them returns an object, then there’s no error, but that object is ignored (like if the method didn’t exist).

2. In contrast, Symbol.toPrimitive must return a primitive, otherwise, there will be an error.

#### Object Property Descriptors

Added in ES5

```js
var myObject = {
  a: 2
};

Object.getOwnPropertyDescriptor(myObject, "a");
// {
//    value: 2,
//    writable: true,
//    enumerable: true,
//    configurable: true
// }
```

##### Writable

The ability for you to change the value of a property is controlled by writable.

Consider:

```js
var myObject = {};

Object.defineProperty(myObject, "a", {
  value: 2,
  writable: false, // not writable!
  configurable: true,
  enumerable: true
});

myObject.a = 3;

myObject.a; // 2 (fails silently in sloppy-mode, TypeError in strict-mode)
```

##### Configurable

As long as a property is currently configurable, we can modify its descriptor definition, using the same`defineProperty(..)` utility.

```js
var myObject = {
  a: 2
};

myObject.a = 3;
myObject.a; // 3

Object.defineProperty(myObject, "a", {
  value: 4,
  writable: true,
  configurable: false, // not configurable!
  enumerable: true
});

myObject.a; // 4
myObject.a = 5;
myObject.a; // 5

Object.defineProperty(myObject, "a", {
  value: 6,
  writable: true,
  configurable: true,
  enumerable: true
}); // TypeError (strict mode or not)
```

**Be careful: as you can see, changing configurable to false is a one-way action, and cannot be undone!**

**Note:** There's a nuanced exception to be aware of: even if the property is already `configurable:false`, writable can always be changed from `true` to `false` without error, but not back to `true` if already `false`.

###### Another thing configurable:false prevents is the ability to use the delete operator to remove an existing property.

```js
var myObject = {
  a: 2
};

myObject.a; // 2
delete myObject.a;
myObject.a; // undefined

Object.defineProperty(myObject, "a", {
  value: 2,
  writable: true,
  configurable: false,
  enumerable: true
});

myObject.a; // 2
delete myObject.a;
myObject.a; // 2
```

As you can see, the last delete call failed (silently) because we made the a property non-configurable.

#### Enumerable

this characteristic controls if a property will show up in certain object-property enumerations, such as the `for..in` loop. Set to `false` to keep it from showing up in such enumerations, even though it's still completely accessible. Set to `true` to keep it present.

---

### Immutability

**Note:** It's important to note that all of these approaches create shallow immutability. That is, they affect only the object and its direct property characteristics. If an object has a reference to another object (array, object, function, etc), the contents of that object are not affected, and remain mutable.

##### Object Constants

By combining writable:false and configurable:false, you can essentially create a constant (cannot be changed, redefined or deleted) as an object property, like:

```js
var myObject = {};

Object.defineProperty(myObject, "FAVORITE_NUMBER", {
  value: 42,
  writable: false,
  configurable: false
});
```

##### Prevent Extensions

If you want to prevent an object from having new properties added to it, but otherwise leave the rest of the object's properties alone, call `Object.preventExtensions(..)`

```js
var myObject = {
  a: 2
};

Object.preventExtensions(myObject);

myObject.b = 3;
myObject.b; // undefined
```

In non-strict mode, the creation of b fails silently. In strict mode, it throws a TypeError.

##### Seal

`Object.seal(..)` creates a "sealed" object, which means it takes an existing object and essentially calls `Object.preventExtensions(..)` on it, but also marks all its existing properties as configurable:false.

So, not only can you not add any more properties, but you also cannot reconfigure or delete any existing properties (though you can still modify their values).

##### Freeze

`Object.freeze(..)` creates a frozen object, which means it takes an existing object and essentially calls `Object.seal(..)` on it, but it also marks all "data accessor" properties as `writable:false`, so that their values cannot be changed.

This approach is the highest level of immutability that you can attain for an object itself, as it prevents any changes to the object or to any of its direct properties (though, as mentioned above, the contents of any referenced other objects are unaffected).

---

#### Getters & Setters

The default [[Put]] and [[Get]] operations for objects completely control how values are set to existing or new properties, or retrieved from existing properties, respectively.

When you define a property to have either a getter or a setter or both, its definition becomes an "accessor descriptor" (as opposed to a "data descriptor"). For accessor-descriptors, the `value` and `writable` characteristics of the descriptor are moot and ignored, and instead JS considers the `set` and `get` characteristics of the property (as well as `configurable` and `enumerable`).

```js
var myObject = {
  // define a getter for `a`
  get a() {
    return 2;
  }
};

Object.defineProperty(
  myObject, // target
  "b", // property name
  {
    // descriptor
    // define a getter for `b`
    get: function() {
      return this.a * 2;
    },

    // make sure `b` shows up as an object property
    enumerable: true
  }
);

myObject.a; // 2

myObject.b; // 4
```

Either through object-literal syntax with get a() { .. } or through explicit definition with defineProperty(..), in both cases we created a property on the object that actually doesn't hold a value, but whose access automatically results in a hidden function call to the getter function, with whatever value it returns being the result of the property access.

```js
var myObject = {
  // define a getter for `a`
  get a() {
    return 2;
  }
};

myObject.a = 3;

myObject.a; // 2
```

Since we only defined a getter for a, if we try to set the value of a later, the set operation won't throw an error but will just silently throw the assignment away. Even if there was a valid setter, our custom getter is hard-coded to return only 2, so the set operation would be moot.

```js
var myObject = {
  // define a getter for `a`
  get a() {
    return this._a_;
  },

  // define a setter for `a`
  set a(val) {
    this._a_ = val * 2;
  }
};

myObject.a = 2;

myObject.a; // 4
```

#### Existence of a property

We can ask an object if it has a certain property without asking to get that property's value:

```js
var myObject = {
  a: 2
};

"a" in myObject; // true
"b" in myObject; // false

myObject.hasOwnProperty("a"); // true
myObject.hasOwnProperty("b"); // false
```

The in operator will check to see if the property is in the object, or if it exists at any higher level of the [[Prototype]] chain object traversal. By contrast, hasOwnProperty(..) checks to see if only myObject has the property or not, and will not consult the [[Prototype]] chain.

`hasOwnProperty(..)` is accessible for all normal objects via delegation to Object.prototype. But it's possible to create an object that does not link to Object.prototype (via `Object.create(null)`). In this case, a method call like `myObject.hasOwnProperty(..)` would fail.

In that scenario, a more robust way of performing such a check is `Object.prototype.hasOwnProperty.call(myObject,"a")`, which borrows the base `hasOwnProperty(..)` method and uses explicit this binding to apply it against our myObject.

##### Enumerability

`myObj.propertyIsEnumerable(..)` tests whether the given property name exists directly on the object and is also enumerable:true.

`Object.keys(..)` returns an array of all enumerable properties, whereas `Object.getOwnPropertyNames(..)` returns an array of all properties, enumerable or not.

Whereas `in` vs. `hasOwnProperty(..)` differ in whether they consult the [[Prototype]] chain or not, `Object.keys(..)` and `Object.getOwnPropertyNames(..)` both inspect only the direct object specified.

There's (currently) no built-in way to get a list of all properties which is equivalent to what the in operator test would consult (traversing all properties on the entire [[Prototype]] chain). You could approximate such a utility by recursively traversing the [[Prototype]] chain of an object, and for each level, capturing the list from `Object.keys(..)` -- only enumerable properties.

---

### Setting and Shadowing Properties

`myObject.foo = "bar";`

If the myObject object already has a normal data accessor property called foo directly present on it, the assignment is as simple as changing the value of the existing property.

If foo is not already present directly on myObject, the \[[Prototype]] chain is traversed, just like for the \[[Get]] operation. If foo is not found anywhere in the chain, the property `foo` is added directly to myObject with the specified value, as expected.

However, if foo is already present somewhere higher in the chain, nuanced (and perhaps surprising) behavior can occur with the myObject.foo = "bar" assignment. We'll examine that more in just a moment.

If the property name foo ends up both on `myObject` itself and at a higher level of the \[[Prototype]] chain that starts at myObject, this is called shadowing. The `foo` property directly on `myObject` shadows any `foo` property which appears higher in the chain, because the `myObject.foo` look-up would always find the foo property that's lowest in the chain.

As we just hinted, shadowing `foo` on `myObject` is not as simple as it may seem. We will now examine three scenarios for the myObject.foo = "bar" assignment when foo is not already on myObject directly, but is at a higher level of myObject's \[[Prototype]] chain:

1. If a normal data accessor property named foo is found anywhere higher on the \[[Prototype]] chain, and it's not marked as read-only (writable:false) then a new property called foo is added directly to myObject, resulting in a shadowed property.
2. If a `foo` is found higher on the \[[Prototype]] chain, but it's marked as read-only (`writable:false`), then both the setting of that existing property as well as the creation of the shadowed property on `myObject` are disallowed. If the code is running in strict mode, an error will be thrown. Otherwise, the setting of the property value will silently be ignored. Either way, no shadowing occurs.
3. If a `foo` is found higher on the \[[Prototype]] chain and it's a setter, then the setter will always be called. No `foo` will be added to (aka, shadowed on) `myObject`, nor will the `foo` setter be redefined.

Most developers assume that assignment of a property ( \[[Put]] ) will always result in shadowing if the property already exists higher on the \[[Prototype]] chain, but as you can see, that's only true in one (#1) of the three situations just described.

If you want to shadow foo in cases #2 and #3, you cannot use `=` assignment, but must instead use `Object.defineProperty(..)` (see Chapter 3) to add foo to myObject.

Note: Case #2 may be the most surprising of the three. The presence of a read-only property prevents a property of the same name being implicitly created (shadowed) at a lower level of a [[Prototype]] chain. The reason for this restriction is primarily to reinforce the illusion of class-inherited properties. If you think of the foo at a higher level of the chain as having been inherited (copied down) to myObject, then it makes sense to enforce the non-writable nature of that foo property on myObject. If you however separate the illusion from the fact, and recognize that no such inheritance copying actually occurred, it's a little unnatural that myObject would be prevented from having a foo property just because some other object had a non-writable foo on it. It's even stranger that this restriction only applies to = assignment, but is not enforced when using `Object.defineProperty(..)`.

**Shadowing can even occur implicitly in subtle ways, so care must be taken if trying to avoid it. Consider:**

```js
var anotherObject = {
  a: 2
};

var myObject = Object.create(anotherObject);

anotherObject.a; // 2
myObject.a; // 2

anotherObject.hasOwnProperty("a"); // true
myObject.hasOwnProperty("a"); // false

myObject.a++; // oops, implicit shadowing!

anotherObject.a; // 2
myObject.a; // 3

myObject.hasOwnProperty("a"); // true
```

Though it may appear that myObject.a++ should (via delegation) look-up and just increment the anotherObject.a property itself in place, instead the ++ operation corresponds to myObject.a = myObject.a + 1. The result is [[Get]] looking up a property via [[Prototype]] to get the current value 2 from `anotherObject.a`, incrementing the value by one, then [[Put]] assigning the 3 value to a new shadowed property a on myObject. **Oops!**
