# C# Notes

## Access Modifiers: Accessibility of `methods` and `fields`

- ***private***: a method or field is accessible only from **within the class or struct**.
- ***public***: a method or field is accessible from anywhere inside the namespace.
- ***protected***: method or field accessible from **inside the class** and **in all classes that derive from that class**.
- ***internal***: method or field is accessible from inside its own assembly, but not in other assemblies.
- ***protected internal***: member access in the same assembly, or derived classes in other assemblies.
- ***private protected***: member access inside the containing class, deriving class, but only in the same assembly.

---

## Fields

### Static Fields - Shared fields

- Defining a field as static makes it possible for you to create a single instance of a field that is shared among all objects created from a single class. (Nonstatic fields are local to each instance of an object.)

### Creating a static field using `const` keyword

- By prefixing the field with the `const` keyword, you can declare that a field is `static` but that its value can never change. The keyword `const` is short for constant. 

- A `const` field does not use the `static` keyword in its declaration but is nevertheless static.

---

## Static `using` statements

- Whenever you call a static method or reference a static field, you must specify
the class to which the method or field belongs, such as `Math.Sqrt` or
`Console.WriteLine`. 

- Static `using` statements enable you to bring a class into scope and omit the class name when accessing static members. 

- They operate in much the same way as ordinary using statements that bring namespaces into scope.

```csharp

using static System.Math;
using static System.Console;
...
var root = Sqrt(99.9);
WriteLine($"The square root of 99.9 is ");

```

- Although you are typing less code, you have to balance this with the additional
effort required when someone else has to maintain your code, because it is no
longer clear to which class each method belongs

---

## Contructors

- A `constructor` is a special method that runs automatically when you create an instance of a `class`. It has the same name as the class, and it can take parameters, but it cannot return a value (not even `void`). Every `class` must have a `constructor`.

- If you don’t write one, the compiler automatically generates a default constructor for you. (However, the compiler-generated default constructor doesn’t actually do anything.)

- In C# parlance, the default constructor is a constructor that does not take any parameters. Regardless of whether, the compiler generates the
default construtor or you write it yourself, a constructor that does not take any parameters is still the default constructor. You can also write
nondefault constructors (constructors that do take parameters).

- If `public` keyword is ommitted, the constructor is `private` by default.

- If a `class` has one or more `private` constructors and no `public` constructors, other classes (except nested classes) cannot create instances of this `class`.

#### Overloading contructors

- A constructor is just a special kind of method, and it—like all methods—can be overloaded.

- When you build the application, the compiler works out which constructor it should call based on the parameters that you specify to the new operator.

--- 

## **Methods**

### __`static` Method__

- if you declare a method or a field as static, you can call the method or access the field by using the name of the class. No instance is required.

- A `static` method does not depend on an instance of the `class`, and it cannot access any instance fields or instance methods defined in the class; it can use only fields and other methods that are marked as `static`.


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

**It’s important to understand that only the compiler can make this translation. You can’t write your own method to override `Finalize`, and you can’t call `Finalize` yourself.**

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
- `static` classes: 
    - class that cannot be instantiated, nor can be inherited from (hence they cannot be `Base` Class to other classes). 
    - A static class can contain only static members.  
    - The purpose of a static class is purely to act as a holder of utility methods and fields.
    - A static class cannot contain any instance data or methods, and it does not make sense to try to create an object from a static class by using the new operator. 
    - In fact, you can’t actually use new to create an instance of an object using a static class even if you want to. (The compiler will report an error if you try.) 

- `partial` classes: When you split a class across multiple files, you define the parts of the class by using the partial keyword in each file

- Anonymous Classes: You create an anonymous class simply by using the `new` keyword and a pair of braces defining the fields and values that you want the class to contain.
  
    ```csharp
      var myAnonymousObject = new { Name = "John ", Age = 47 };

      var anotherAnonymousObject = new { Name = "Diana ", Age = 53 };
    ```

  - The C# compiler uses the names, types, number, and order of the fields to
  determine whether two instances of an anonymous class have the same type. 

  - In the case above, the variables myAnonymousObject and anotherAnonymousObject have the same number of fields, with the same name and type, in the same order, so both variables are instances of the same anonymous class. This means that you
  can perform assignment statements such as this:

    ```csharp

    anotherAnonymousObject = myAnonymousObject;

    ```

  - There are quite a few restrictions on the contents of an anonymous class. 

    - anonymous classes can contain only public fields,
    - the fields must all be initialized,
    - they cannot be static,
    - and you cannot define any methods for them.


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

---

### Invariant, Covariant and Contravariant Interfaces

An Interface is said to be invariant when we cannot assign an `IWrapper<A>` object to a reference of type `IWrapper<B>`, even if type `A` is derived from type `B`. So, `IWrapper<object>` cannot be assigned to `IWrapper<string>`.

Following definitions hold: 

##### Covariance

Enables you to use a more derived type than originally specified.

You can assign an instance of `IEnumerable<Derived>` to a variable of type `IEnumerable<Base>`.

##### Contravariance

Enables you to use a more generic (less derived) type than originally specified.

You can assign an instance of `Action<Base>` to a variable of type `Action<Derived>`.

##### Invariance

Means that you can use only the type originally specified; so an invariant generic type parameter is neither covariant nor contravariant.

You cannot assign an instance of `List<Base>` to a variable of type `List<Derived>` or vice versa.

#### Covariance

In situations where the type parameter occurs only as the `return` value of the methods in a generic interface, you can inform the compiler that some implicit conversions are legal and that it does not have to enforce strict type safety. You do this by specifying the `out` keyword
when you declare the type parameter, like this: 

```csharp
interface IRetrieveWrapper<out T>
{
  T GetData();
}

```

This feature is called ***covariance***. You can assign an `IRetrieveWrapper<A>` object to an `IRetrieve-Wrapper<B>` reference as long as there is a valid conversion from type `A` to type `B`, or type `A` derives from type `B`. You can specify the out qualifier with a type parameter only if the type parameter occurs as the return type of methods. If you use the type parameter to specify the type of any method parameters, the out qualifier is illegal, and your code will not compile. Also, covariance works only with reference types. This is because value types cannot form inheritance hierarchies.

#### Contravariance

Contravariance follows a similar principle to covariance except that it works in the opposite direction; it enables you to use a generic interface to reference an object of type `B` through a reference to type `A` as long as type B derives from type `A`. This is done by using the `in` qualifier before the type parameter. The `in` keyword tells the C# compiler that you can either pass the type `T` as the parameter type to methods or pass any type that derives from `T`. You cannot use `T` as the return type from any methods. 

The way I remember, based on the examples in this section, is as follows:
- **Covariance example** If the methods in a generic interface can return `strings`, they can also return `objects`. (All strings are objects.)
- **Contravariance example** If the methods in a generic interface can take `object` parameters, they can take string parameters. (If you can perform an operation by using an object, you can perform the same operation by using a string because all strings are objects.)

---

### Comparing arrays and collections

- An array instance has a fixed size and cannot grow or shrink. A collection can dynamically resize itself as required.
- An array can have more than one dimension. A collection is linear. However, the items in a collection can be collections themselves, so you
can imitate a multidimensional array as a collection of collections.
- You store and retrieve an item in an array by using an index. Not all collections support this notion. For example, to store an item in a `List<T>`
collection, you use the `Add` or `Insert` method, and to retrieve an item, you use the `Find` method.
- Many of the collection classes provide a `ToArray` method that creates and populates an array containing the items in the collection. The items are
copied to the array and are not removed from the collection. Additionally, these collections provide constructors that can populate a collection directly from an array.

---

### `IEnumerable` Interface

The `IEnumerable` interface contains a single method called `GetEnumerator`:

```csharp

IEnumerator GetEnumerator();

```

The `GetEnumerator` method should return an `enumerator` object that implements the `System.Collections.IEnumerator` interface. The enumerator
object is used for stepping through (enumerating) the elements of the collection.

The `IEnumerator` interface specifies the following property and methods:

```csharp
object Current { get; }
bool MoveNext();
void Reset();
```

Think of an enumerator as a pointer indicating elements in a list. Initially, the pointer points before the first element. You call the `MoveNext` method to move the pointer down to the next (first) item in the list; the `MoveNext` method should return `true` if there actually is another item and false if there isn’t. You use the `Current` property to access the item currently pointed to, and you use the `Reset`
method to return the pointer back to before the first item in the list.

By using the `GetEnumerator` method of a collection to create an enumerator, repeatedly calling the `MoveNext` method, and using the enumerator to retrieve the value of the `Current` property, you can move forward through the elements of a collection one item at a time. This is exactly what the foreach statement does. So, if you want to create your own enumerable collection class, you must implement the
`IEnumerable` interface in your collection class and also provide an implementation of the `IEnumerator` interface to be returned by the
`GetEnumerator` method of the collection class.

##### NOTE:  

The `Current` property of the `IEnumerator` interface exhibits non–type-safe behavior in that it returns an `object` rather than a specific type. However, you should be pleased to know that the Microsoft .NET Framework class library also provides the generic
`IEnumerator<T>` interface, which has a Current property that returns a `T` instead. Likewise, there is also an `IEnumerable<T>` interface containing a `GetEnumerator` method that returns an `Enumerator<T>` object. Both of these interfaces are defined in the `System.Collections.Generic` namespace, and if you are building applications for the .NET Framework version 2.0 or later, you should make use of these generic interfaces rather than the nongeneric versions when you define enumerable collections.

---

### Initializing a variable defined with a type parameter using `default`

``` csharp

private TItem currentItem = default(TItem);

```

The `currentItem` variable is defined by using the type parameter `TItem`. When the program is written and compiled, the actual type that will be substituted for `TItem` might not be known; this issue is resolved only when the code is executed. This makes it difficult to specify how the variable should be initialized. The temptation is to set it to `null`. However, if the type substituted for `TItem` is a `value` type, this is an illegal assignment. (You cannot set value types to `null`, only reference types.) Similarly, if you set it to `0` with the expectation that the type will be numeric, this will be illegal if the type used is actually a `reference` type. There are other possibilities as well; `TItem` could be a boolean, for example. 

The default keyword solves this problem. The value used to initialize the variable will be determined when the statement is executed. If `TItem` is a reference type, `default(TItem)` returns null; if TItem is numeric, `default(TItem)` returns 0; if `TItem` is a boolean, `default(TItem)` returns `false`. If TItem is a struct, the individual fields in the struct are initialized in the same way. (Reference fields are set
to null, numeric fields are set to 0, and boolean fields are set to false.)

---

### `Func<T, ...>` vs `Action<T, ...>`

`Func<T1, T2, ..., TResult>` takes 0 or more arguments (`T1`, `T2` and so on) and `return` a result of type `TResult`

`Action<T1, T2, ...>` takes 0 or more arguments, performs some action and returns `void`. Thus, `Action` is more like a subroutine.

---

### Delegates

References to methods, basically.

In a `class`, make a `delegate` type, that create an instance of the `delegate`, and then in the class constructor, add methods to the delegate using `+=` operator. We can also remove methods from a `delegate` using `-=`. 

We can also add methods outside of a constructor of a specific `class`, to make the whole thing more generalized. For example, we can make separate `Add` or `Remove` methods to add or remove delegates.

---

### Method Adapters

Method Adapters are just functions that when executed, execute other functions.

```csharp

void FinishFolding () 
{
  folder.StopFolding(0);
}

```

---

### Events

You declare an event in a class intended to act as an event source. An event source is usually a class that monitors its environment and raises an event when something significant happens.

`event delegateTypeName eventName`