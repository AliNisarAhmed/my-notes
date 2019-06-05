# TypeScript Interview Questions

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
