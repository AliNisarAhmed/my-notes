# C# Notes

## **Methods**

### `virtual` Method

A method with its own (default) implementation in the `Base` Class, but can be `override` in the `Derived` Class.

### `abstarct` Method

A method that does not have an implementation in the `Base` Class, and therefore, must be `override` in the `Derived` Class.

### `sealed` Method

A method which a `Derived` Class cannot `override`. You can seal only a method declared with `override` keyword. Hence the method is declared as `sealed override`.

### Extension Method

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. We use the `this` keyword, in front of the **FIRST** parameter of a method, to indicate which type is being extended. To enable extension methods for a particular type, just add a using directive for the namespace in which the methods are defined.

---

## **Classes**

- `abstract` classes: A class that is meant to be a Base class to other classes, and not be instantiated from.
- Normal or Concrete clases may have virtual methods.
- `sealed` classes: prevents other classes from inheriting from it.

---

### `new` modifier

When used as a declaration modifier, the `new` keyword explicitly hides a member that is inherited from a `base` class. When you hide an inherited member, the derived version of the member replaces the `base` class version. Although you can hide members without using the new modifier, you get a compiler warning. If you use new to explicitly hide a member, it suppresses this warning.

---

## Summary of keywords

| Keyword   | Interface | Abstract class | Class | Sealed Class | Structure |
| --------- | :-------: | :------------: | :---: | :----------: | :-------: |
| Abstract  |    No     |      Yes       |  No   |      No      |    No     |
| New       |    Yes    |      Yes       |  Yes  |     Yes      |    No     |
| Override  |    No     |      Yes       |  Yes  |     Yes      |    No     |
| Private   |    No     |      Yes       |  Yes  |     Yes      |    Yes    |
| Protected |    No     |      Yes       |  Yes  |     Yes      |    No     |
| Public    |    No     |      Yes       |  Yes  |     Yes      |    Yes    |
| Sealed    |    No     |      Yes       |  Yes  |     Yes      |    No     |
| Virtual   |    No     |      Yes       |  Yes  |      No      |    No     |
