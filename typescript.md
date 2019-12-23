# TypeScript

## Compiler Options

- `noImplicitAny` rule will not allow us to allow the compiler to infer `any` type, though we can set a variable to `any` ourselves.

- `declaration: true` will enable the compiler to generate an `index.d.ts` file, with which we can inpect the types infered by the compiler.

- if the `noUnusedParameters` option is enabled, the compiler will warn you if a function defines parameters that it doesn’t use.

- In `strictNullChecks` mode, the `null` and `undefined` values are not in the domain of every type and are only assignable to themselves and any (the one exception being that undefined is also assignable to void).

- When the `noImplicitReturns` setting is true, the compiler will report an error when there are paths through functions that don’t explicitly produce a result with the `return` keyword or throw an error.

- `suppressExcessPropertyErrors: true` allows objects to be defined with additioinal properties, over and above those specified by their interface or type alias.

## Union types

- Union types can be defined using the `|` operator. e.g. `string | number`.

- It is important to understand that a type union is handled as a type in its own right, whose features are the intersection of the individual types.

### Using shape type unions

```typescript
  type Product = {
    id: number,
    name: string,
    price?: number
    };
  type Person = {
    id: string,
    name: string,
    city: string
    };
  let hat = { id: 1, name: "Hat", price: 100 };
  let gloves = { id: 2, name: "Gloves", price: 75 };
  let umbrella = { id: 3, name: "Umbrella", price: 30 };
  let bob = { id: "bsmith", name: "Bob", city: "London" };
  let dataItems: (Product | Person)[] = [hat, gloves, umbrella, bob];

  dataItems.forEach(item => console.log(`ID: ${item.id}, Name: ${item.name}`));
```

- The dataItems array in this example has been annotated with a union of the `Product` and `Person` types. These types have two properties in common, `id` and `name`, which means these properties can be used when processing the array without having to narrow to a single type.

- These are the only properties that can be accessed because they are the only properties shared by all types in the union. Any attempt to access the `price` property defined by the `Product` type or the `city` property defined by the `Person` type will produce an error because these properties are not part of the `Product | Person` union.

## Type Assertions

- A `type assertion` tells the TypeScript compiler to treat a value as a specific type, known as `type narrowing`. A `type assertion` is one of the ways that you can narrow a type from a union.

- the `as` keyword is used for type assertions.

- **Caution** - no type conversion is performed by a type assertion, which only tells the compiler what type it should apply to a value for the purposes of type checking.

## Type Guards

- For primitive types only, `typeof` operator can be used to test for a specific type without needing a type assertion.

- The simplest way to differentiate between shape/object types is to use the JavaScript `in` keyword to check for a property.

### Type Guarding with a Type Predicate Function

- The `in` keyword is a useful way to identify whether an object conforms to a shape, but it requires the same checks to be written each time types need to be identified. TypeScript also supports guarding object types using a function:

  ```typescript
    function isPerson(testObj: any): testObj is Person
    {
      return testObj.city !== undefined;
    }
  ```

## `any`, `unknown` and `never` types

- `never` type arises when the compiler cannot figure out the type, usually happens when all the types have been exhausted in a type gaurd.

- `any` value can be assigned to all other types, which creates a gap in the compiler’s type checking. TypeScript also supports the unknown type, which is a safer alternative to any. An unknown value can be assigned only any or itself unless a type assertion or type guard is used.

- An `unknown` value can’t be assigned to another type without a type assertion.

```typescript
function calculateTax(amount: number, format: boolean): string | number {
  const calcAmount = amount * 1.2;
  return format ? `$${calcAmount.toFixed(2)}` : calcAmount;
}
let taxValue = calculateTax(100, false);

switch (typeof taxValue) {
  case 'number':
    console.log(`Number Value: ${taxValue.toFixed(2)}`);
    break;
  case 'string':
    console.log(`String Value: ${taxValue.charAt(0)}`);
    break;
  default:
    let value: never = taxValue;
    console.log(`Unexpected type for value: ${value}`);
}

let newResult: unknown = calculateTax(200, false);
let myNumber: number = newResult;
console.log(`Number value: ${myNumber.toFixed(2)}`);

// This raises the error below:
// src/index.ts(18,5): error TS2322: Type 'unknown' is not assignable to type 'number'.
// typing like below will reolve the issue

let myNumber: number = newResult as number; // forcing unknown to be a number
```

## Nullable types: `undefined` and `null`

- Under normal circumstances, the compiler will report an error if a value of one type is assigned to a variable of a different type, but the compiler remains silent because it allows `null` and `undefined` to be treated as values for all types.

- The use of `null` and `undefined` can be restricted by enabling the `strictNullChecks` compiler setting.

```json
{
  "compilerOptions": {
    "target": "es2018",
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

- using `typeof` on `null` values returns `"object"`, so guarding against `null` values is done using an explicit value check `if (value === null)`, which the TypeScript compiler understands as a type guard.

- A non-null value is asserted by applying the `!` character after the value. An alternative approach is to filter out `null` or `undefined` values using a type guard (e.g. `value !== undefined`).

- The second approach has the advantage of runtime safety, while first approach may result in runtime errors (TODO: Find an example for this in React).

## Function overloads

- TS allows type overloads of functions, but they are not like C# overloads with unique implementations. All TS overloads have the same implementation, and different type paths should be handled inside the function body.

```typescript
function calculateTax(amount: number): number;
function calculateTax(amount: null): null;
```

## Dynamically creating properties

- The TypeScript compiler only allows values to be assigned to properties that are part of an object’s type, which means that `interfaces` and `classes` have to define all the properties that the application requires.

- By contrast, JavaScript allows new properties to be created on objects simply by assigning a value to an unused property name. The TypeScript index signature feature bridges these two models, allowing properties to be defined dynamically while preserving type safety,

  ```typescript
    interface object {
      [prop: string]: any;
    }
  ```

---

## TyepScript advantages

### Static typing

### Code documentation

- typed code is self-documenting, and types around functions increase readability and understandability multiple folds.

### Code readability

### Code maintainability

### Less runtime errors, more bugs caught at compile time

### Editor support and Intellisense

### New JS features

### Union types for props

### Interface for typing state and props

### But it wont' save you from shooting your foot, runtime errors may not always be caught at compile time.


## Conditional Types (from https://www.youtube.com/watch?v=O1rn-d_P_Rc)

`any` type is JS land, no support by TS compiler during runtime.

```typescript
  type StringOrNull<T> = T extends string ? string : null;  // if the given type is a string then return the type string else return the type null

  // or to restrict types, provide type constraints

  type StringOrNull<T extends string | null> = T extends string ? string : null;

  // or

  type StringOrNull <T> = T extends string
   ? string
   : T extends null
   ? null
   : never; // or unknown
```

---

## **TypeScript Interview Questions**

## What is TypeScript? Why should we use it?

<details><summary>Answer: </summary>
  TypeScript is a typed superset of JavaScript that compiles to plain JavaScript which runs on any browser or JavaScript engine.

TypeScript offers support for the latest JavaScript features and also has some additional features like static typing, object oriented programming and automatic assignment of constructor.

</details>

## What are Types in TypeScript?

<details><summary>Answer: </summary>
The type represents the type of the value we are using in our programs. TypeScript supports simplest units of data such as numbers, strings, boolean as well as additional types like enum, any, never.

In TypeScript, we are declaring a variable with its type explicitly by appending the : with the variable name followed by the type.

The reason adding types are:

- Types have proven ability to enhance code quality and understandability.
- It’s better for the compiler to catch errors than to have things fail at runtime.
- Types are one of the best forms of documentation we can have.
  </details>

## What is Type assertions in TypeScript?

<details><summary>Answer: </summary>

A type assertion is like a type cast in other languages, but performs no special checking or restructuring of data. It has no runtime impact, and is used purely by the compiler. TypeScript assumes that we have performed any special checks that we need.

`let strength: number = (string).length`

</details>

## What is `as` syntax in TypeScript?

<details><summary>Answer: </summary>

The as is additional syntax for Type assertion in TypeScript. The reason for introducing the as-syntax is that the original syntax (<type>) conflicted with JSX.

`let strLength: number = (str as string).length;`

When using TypeScript with JSX, only as-style assertions are allowed.

</details>

## What is Compilation Context?

<details><summary>Answer: </summary>

The compilation context is basically grouping of the files that TypeScript will parse and analyze to determine what is valid and what isn’t. Along with the information about which files, the compilation context contains information about which compiler options. A great way to define this logical grouping is using a tsconfig.json file.

</details>

## Can an `interface` extends a `class` just like a `class` implements interface?

<details><summary>Answer: </summary>

Yes, an interface extends a class, when it does it inherits the members of the class but not their implementations. Interfaces inherit even the private and protected members of a base class. This means that when you create an interface that extends a class with private or protected members, that interface type can only be implemented by that class or a subclass of it.

</details>

## What are all the other access modifiers that TypeScript supports?

<details>
  <summary>Answer: </summary>

TypeScript supports access modifiers `public`, `private` and `protected` which determine the accessibility of a class member as given below:

- public - All the members of the class, its child classes, and the instance of the class can access. (default)
- protected - All the members of the class and its child classes can access them. But the instance of the class can not access. (A constructor may also be marked protected. This means that the class cannot be instantiated outside of its containing class, but can be extended.)
- private - Only the members of the class can access them.

If an access modifier is not specified it is implicitly public as that matches the convenient nature of JavaScript.

Also note that at runtime (in the generated JS) these have no significance but will give you compile time errors if you use them incorrectly.

</details>
