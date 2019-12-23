# Code Trick Questions
```JavaScript
var baz = 2;
typeof baz = "number";
var baz;      // This declaration is ignored by the JS engine
typeof baz;   // still "number"
```
---
```JS
function Foo() { // }
Foo.prototype.constructor === Foo     // true
var a = new Foo();
a.constructor === Foo;       // true
// However, "a" does not have a constructor prop, it is being searched via proto chain.
// Constructor property does not mean "constructed by", it is just an arbitrary property.
```

---
```JS
var x = 1;
function foo (x = 2, f = function () { return x; }) {
  console.log(f());
}
foo();        // which x will be output?
```
- x will be equal to 2, the x in parameters "shadows" the x declared previously

---
```JS
var x = 1;
function foo (x = 2, f = function () { return x; }) {
  var x = 5;
  console.log(x);
}
foo();        // which x will be output?
```
Mozilla and Chrome used to ouput 2,  which was a bug, but now it has been fixed.

---


---
```JS
1 + '2' === '12'
'1' + 2 === '12'
```
---
```JS
"" + 1 + 0
"" - 1 + 0
true + false
6 / "3"
"2" * "3"
4 + 5 + "px"
"$" + 4 + 5
"4" - 2
"4px" - 2
7 / 0
"  -9  " + 5
"  -9  " - 5
null + 1
undefined + 1
``` 
<details>
```JS
"" + 1 + 0 = "10" // (1)
"" - 1 + 0 = -1 // (2)
true + false = 1
6 / "3" = 2
"2" * "3" = 6
4 + 5 + "px" = "9px"
"$" + 4 + 5 = "$45"
"4" - 2 = 2
"4px" - 2 = NaN
7 / 0 = Infinity
" -9  " + 5 = " -9  5" // (3)
" -9  " - 5 = -14 // (4)
null + 1 = 1 // (5)
undefined + 1 = NaN // (6)
```
</details>

---

Question: Is it possible to create functions A and B such as new A()==new B()?
```js
function A() { ... }
function B() { ... }

let a = new A;
let b = new B;

alert( a == b ); // true
```

<details>
  <summary>Solution</summary>
  It is possible, they can both refer to some common object outside and       return it, instead of not returing an object at all
  ```js
    let obj = {};
    function A() { return obj; }
    function B() { return obj; }
    alert( new A() == new B() ); // true
  \```
</details>

---

### How to empty an array in JS

`arrayList = [1, 2, 3, 4, 5];`

###### Method 1
`arrayList = [];`

###### Method 2
`arrayList.length = 0`;

###### Method 3
`arrayList.splice(0, arrayList.length);`
###### Method 4
```js
while (arrayList.length) {
  arrayList.pop();
}
```
---

### Output?
```js
var Employee = {
  company: 'xyz'
}
var emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company);
```

<details><summary>Solution</summary>
The code above will output `xyz` as output. Here `emp1` object got company as prototype property. delete operator doesn't delete prototype property.<br>
</details>

---

### Uninitialized Array Slots
```js
var trees = ["redwood", "bay", "cedar", "oak", "maple"];
delete trees[3];

//Chrome Shows
trees // ["redwood", "bay", "cedar", empty, "maple"]

// Firefox shows
trees // ["redwood", "bay", "cedar", <1 empty slot>, "maple"]
```
---

### Output
```js
var trees = ["xyz", "xxxx", "test", "ryan", "apple"];
delete trees[3];
console.log(trees.length);
```
<details><summary>Solution</summary>
<p>
The code above will output 5 as output. When we used delete operator for deleting an array element then, the array length is not affected by this. This holds even if you deleted all elements of an array using delete operator.
</p>

</details>

---

### Output
```js
var bar = true;
console.log(bar + 0);   
console.log(bar + "xyz");  
console.log(bar + true);  
console.log(bar + false);
```
<details><summary>Solution</summary>
The code above will output 1, "truexyz", 2, 1
</details>

**General Guidelines for `+` operator**

    Number + Number -> Addition
    Boolean + Number -> Addition
    Boolean + Boolean -> Addition
    Number + String -> Concatenation
    String + Boolean -> Concatenation
    String + String -> Concatenation
---

### Output
```js
var z = 1, y = z = typeof y;
console.log(y);  
console.log(z);
```
<details><summary>Solution</summary>
The code above will print string "undefined" as output. According to associativity rule operator with the same precedence are processed based on their associativity property of operator. Here associativity of the assignment operator is Right to Left so first typeof y will evaluate first which is string "undefined" and assigned to z and then y would be assigned the value of z. The overall sequence will look like that:
</details>

---

### Output
```js
var salary = "1000$";

(function () {
  console.log("Original salary was " + salary);

  var salary = "5000$";

  console.log("My New Salary " + salary);
})();
```
<details><summary>Solution</summary>
The code above will output: undefined, 5000$ because of hoisting. In the code presented above, you might be expecting salary to retain it values from outer scope until the point that salary was re-declared in the inner scope. But due to hoisting salary value was undefined instead. To understand it better have a look of the following code, here salary variable is hoisted and declared at the top in function scope. When we print its value using console.log the result is undefined. Afterwards the variable is redeclared and the new value "5000$" is assigned to it.
      
      var salary = "1000$";
      (function () {
        var salary = undefined;
        console.log("Original salary was " + salary);
        salary = "5000$";
        console.log("My New Salary " + salary);
      })();
</details>

---

#### What happens when a property is created on a primitive.
```js
let str = "Hello";

str.test = 5;

alert(str.test);
```
<details><summary>Solution</summary>
In strict mode there will be an error <br>
In non-strict mode the process ill fail silently.
This is because the property is created on the wrapper object, which is discarded as soon as its use is done.
</details>

---

#### Output?

```js
let arr = ["a", "b"];

arr.push(function() {
  alert( this );
})

arr[2](); // ?
```

<details><summary>Solution</summary>
<code><pre>
arr.push(function() {
  alert( this );
})
arr[2](); // "a","b", function
</pre></code>
arr[2](); call behaves like object[method](), hence this is assinged to the array.
</details>

---

What is call stack, how can you calculate max-stack size? what is the max-stack size in Chrome, FireFox, Node?

```js
function computeMaxCallStackSize() {
        try {
            return 1 + computeMaxCallStackSize();
        } catch (e) {
            // Call stack overflow
            return 1;
        }
    }
    
// or

function computeMaxCallStackSize(num = 0) {
        try {
            return computeMaxCallStackSize(num + 1);
        } catch (e) {
            // Call stack overflow
            return 1 + num;
        }
    }
```

Chrome: Code 1: 12572, Code 2: 10476
FireFox: Code 123700, Code2: 98261
Node: Code1: 12301, Code2: 10250

In all these, values are approximate to the nearest thousand as the value varies each time code is re-run

---
Q: Write a function `deepClone` to clone the whole object, including nested objects

```js
function deepClone(object){
	var newObject = {};
	for(var key in object){
		if(typeof object[key] === 'object'  && object[key] !== null ){
		 newObject[key] = deepClone(object[key]);
		}else{
		 newObject[key] = object[key];
		}
	}
	return newObject;
}
```
