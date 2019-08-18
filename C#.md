# C# Notes

## **Methods**

### __`virtual` Method__

A method with its own (default) implementation in the `Base` Class, but can be `override` in the `Derived` Class.

### __`abstarct` Method__

A method that does not have an implementation in the `Base` Class, and therefore, must be `override` in the `Derived` Class.

### `sealed` Method

A method which a `Derived` Class cannot `override`. You can seal only a method declared with `override` keyword. Hence the method is declared as `sealed override`.

### __Extension Method__

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. We use the `this` keyword, in front of the **FIRST** parameter of a method, to indicate which type is being extended. To enable extension methods for a particular type, just add a using directive for the namespace in which the methods are defined.

Extension methods can be defined on `interfaces`, `classes`, `sealed` classes, `System.Object`, but cannot hide an already existing instance method.

### `Deconstructor`s

A `deconstructor` enables us to examine an object and extract the value of its fields.

```csharp
class Point 
{
  private int x, y;  // private fields

  // x and y will be populated with the values of private fileds x and y
  public void Deconstruct (out int x, out int y)
  {
    x = this.x;
    y = this.y;
  }
}

Point point = new Point();

(int x, int y) = point;

```

#### Rules:

- It is always named `Deconstruct`.
- It must be a `void` method.
- It must take one or more parameters. These parameters will be populated with the values from the fields in the objects.
- The parameters are marked with the `out` modifier. This means that if you assign values to them, these values will be passed back to the caller.
- The code in the body of the method assigns the values to be returned to the parameters.

### __`Destructor`__

Destructors (opposite of `constructors`) is a special method which is called by CLR after the reference to an object has disappeared. It is written by prepending the Class name with `~`.

e.g. `~Class`

#### Rules: 

- Destructors apply only to `reference` types only. Not for `value` types, like `struct`
- Access modifiers are not allowed.
- It cannot take any parameters. (this and above due to the fact the destructor is called by "garbage collector")

Internally, the C# compiler automatically translates a `destructor` into an override of the `Object.Finalize` method.
Hence

```csharp

class FileProcessor
{
  ~FileProcessor()
  {
  // your code goes here
  }
}

// gets converted to

class FileProcessor
{
  protected override void Finalize()
  {
    try
    {
      // your code goes here
    }
    finally
    {
      base.Finalize();
    }
  }
}
```

**It’s important to understand that only the compiler can make this translation. You can’t write your own method to override `Finalize`, and you can’t call
`Finalize` yourself.**

### Garbage Collection

Without Garbage collector, following problems coudl arise:

- We'd forget to destroy the object, meaning it's `destructor` would not be run, hence important tidying up operations could be missed and the memory would not be returned to heap.
- An active object could mistakenly be destroyed, but one or more variables could still hold a reference to that deleted object, resulting in ***dangling reference***. A dangling reference refers either to unused memory or possibly to a completely different object that now happens to occupy the same piece of memory. Either way, the outcome of using a dangling reference would be undefined at best or a security risk at worst.
- We might try to destroy an object multiple times.

##### Garbage Collector algo

1. It builds a map of all reachable objects. It does this by repeatedly following
reference fields inside objects. The garbage collector builds this map very
carefully and ensures that circular references do not cause infinite
recursion. Any object not in this map is deemed to be unreachable.
2. It checks whether any of the unreachable objects has a destructor that needs
to be run (a process called finalization). Any unreachable object that
requires finalization is placed in a special queue called the freachable queue
(pronounced “F-reachable”).
3. It deallocates the remaining unreachable objects (those that don’t require
finalization) by moving the reachable objects down the heap, thus
defragmenting the heap and freeing memory at its top. When the garbage
collector moves a reachable object, it also updates any references to the
object.
4. At this point, it allows other threads to resume.
5. It finalizes the unreachable objects that require finalization (now in the
freachable queue) by running the Finalize methods on its own thread

The garbage collector runs in its own thread and can execute only at certain
times—typically when your application reaches the end of a method. While it
runs, other threads running in your application will temporarily halt because the
garbage collector might need to move objects around and update object
references, and it cannot do this while objects are in use.

One feature of the garbage collector is that you don’t know, and should not
rely upon, the order in which objects will be destroyed. The final point to
understand is arguably the most important: destructors do not run until objects
are garbage collected. If you write a destructor, you know it will be executed, but
you just don’t know when. Consequently, you should never write code that
depends on destructors running in a particular sequence or at a specific point in
your application.


---

## **Classes**

- `abstract` classes: A class that is meant to be a Base class to other classes, and not be instantiated from (but can be inherited from).
- Normal or Concrete `clases` may have virtual methods.
- `sealed` classes: prevents other classes from inheriting from it.
- `static` classes: class that cannot be instantiated, nor can be inherited from (hence they cannot be `Base` Class to other classes)

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

---

## Generic Collections

| Name                                                   | Strengths                                         |
| ------------------------------------------------------ | ------------------------------------------------- |
| `List<T>`                                              | A growing array                                   |
| `Queue<T>`, `Stack<T>`                                 | for FIFO and LIFO                                 |
| `Hashset<T>`                                           | Unique items only                                 |
| `LinkedList<T>`                                        | Flexible inserts                                  |
| `Dictionary<TKey, TValue>`                             | Quick look up by key                              |
| `SortedSet<T>`                                         | Sorted and unique                                 |
| `SortedList<T>`                                        | Sorted and Memory efficient                       |
| `SortedDictionary<TKey, TValue>`                       | Sorted, Fast inserts and removals                 |
| Concurrent Collections `System.Collections.Concurrent` | Multiple Writers and Readers                      |
| Immutable Collections (NuGet: Microsoft.BcI.Immutable) | Threadsafe, modifications produce new collections |

---

### `using` statement and `IDisposable` interface

Instead of using nested `try catch finally` blocks, `using` statements are used (not to be confused with `using` directive at the top of the file)

`using` statement provides a clean mechanism for controlling the lifetimes of resources, you create an object, and that object will be destroyed when the `using` statement block finishes.

```csharp
using ( type variable = initialization)
{
  statement block
}

```