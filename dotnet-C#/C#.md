# C# Notes

## **Access Modifiers: Accessibility of `methods` and `fields`**

- **_private_**: a method or field is accessible only from **within the class or struct**.
- **_public_**: a method or field is accessible from anywhere inside the namespace.
- **_protected_**: method or field accessible from **inside the class** and **in all classes that derive from that class**.
- **_internal_**: method or field is accessible from inside its own assembly, but not in other assemblies.
- **_protected internal_**: member access in the same assembly, or derived classes in other assemblies.
- **_private protected_**: member access inside the containing class, deriving class, but only in the same assembly.

### other modifiers

- `abstract` - The `abstract` modifier indicates that the thing being modified has a missing or incomplete implementation. The abstract modifier can be used with classes, methods, properties, indexers, and events.

---

## **Fields**

### Static Fields - Shared fields

- Defining a field as static makes it possible for you to create a single instance of a field that is shared among all objects created from a single class. (Nonstatic fields are local to each instance of an object.)

### Creating a static field using `const` keyword

- By prefixing the field with the `const` keyword, you can declare that a field is `static` but that its value can never change. The keyword `const` is short for constant.

- A `const` field does not use the `static` keyword in its declaration but is nevertheless static.

---

## **Static `using` statements**

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

## **Constructors**

- A `constructor` is a special method that runs automatically when you create an instance of a `class`. It has the same name as the class, and it can take parameters, but it cannot return a value (not even `void`). Every `class` must have a `constructor`.

- If you don’t write one, the compiler automatically generates a default constructor for you. (However, the compiler-generated default constructor doesn’t actually do anything.)

- In C# parlance, the default constructor is a constructor that does not take any parameters. Regardless of whether, the compiler generates the
  default construtor or you write it yourself, a constructor that does not take any parameters is still the default constructor. You can also write
  nondefault constructors (constructors that do take parameters).

- If `public` keyword is ommitted, the constructor is `private` by default.

- If a `class` has one or more `private` constructors and no `public` constructors, other classes (except nested classes) cannot create instances of this `class`.

### Overloading contructors

- A constructor is just a special kind of method, and it—like all methods—can be overloaded.

- When you build the application, the compiler works out which constructor it should call based on the parameters that you specify to the new operator.

---

## **Methods**

### Method Signature and hiding

- The method signature refers to the name of the method and the number and types of its parameters, but not its return type. Two methods that have the same name and that take the same list of parameters have the same signature, even if they return different types.

- If a base class and a derived class happen to declare two methods that have the same signature, you will receive a warning when you compile the application.

- A method in a derived class masks (or hides) a method in a base class that has the same signature. However, we can use the `new` keyword before a method.

  ```csharp
    class Mammal
    {
        //...
        public void Talk()
        {
            //...
        }
    }
    class Horse : Mammal
    {
        // ...
        new public void Talk()
        {
              //...
        }
    }
  ```

- Using the `new` keyword like this does not change the fact that the two methods are completely unrelated and that hiding still occurs. It just turns the warning off. In effect, the new keyword says, “I know what I’m doing, so stopshowing me these warnings.”

### **`static` Method**

- if you declare a method or a field as static, you can call the method or access the field by using the name of the class. No instance is required.

- A `static` method does not depend on an instance of the `class`, and it cannot access any instance fields or instance methods defined in the class; it can use only fields and other methods that are marked as `static`.

### **`virtual` Method**

- A method with its own (default) implementation in the `Base` Class, but can be `override` in the `Derived` Class.

- A method that is intended to be overridden is called a virtual method.

  #### over-riding vs hiding

  - Overriding a method is a mechanism for providing different implementations of the same method—the methods are all related because they are intended to perform the same task, but in a class-specific manner.

  - Hiding amethod is a means of replacing one method with another—the methods are usually unrelated and might perform totally different tasks.

  - Overriding a method is a useful programming concept; hiding a method is often an error.

  ```csharp
    namespace System
    {
      class Object
      {
        public virtual string ToString()
        {
          //...
        }
        //...
      }
      //...
    }

    class Horse : Mammal
    {
      ...
      public override string ToString()
      {
        ...
        string temp = base.ToString(); // calling the original implementation in the base class.
      }
    }

  ```

  - Points to Note about Virtual methods
    - A `virtual` method cannot be `private`; it is intended to be exposed to other classes through inheritance. Similarly, `override` methods cannot be `private` because a class cannot change the protection level of a method that it inherits. However, override methods can be `protected`.
    - The signatures of the `virtual` and `override` methods must be identical; they must have the same name, number, and types of parameters. Also, both methods must return the same type.
    - You can only `override` a `virtual` method. If the base class method is not `virtual` and you try to override it, you’ll get a compile-time error.
    - If the `derived` class does not declare the method by using the `override` keyword, it does not `override` the base class method; it hides the method. In other words, it becomes an implementation of a completely different method that happens to have the same name.
    - An `override` method is implicitly `virtual` and can itself be overridden in a further derived class. However, you are not allowed to explicitly declare that an `override` method is virtual by using the `virtual` keyword.

### **`abstarct` Method**

- A method that does not have an implementation in the `Base` Class, and therefore, must be `overriden` in the `Derived` Class.

- `abstract` methods are defined in `abstract` classes (classes that cannot be instantiated)

### `sealed` Method

- A method which a `Derived` Class cannot `override`. You can seal only a method declared with `override` keyword. Hence the method is declared as `sealed override`.

- You can also use the `sealed` keyword to declare that an individual method in an unsealed class is sealed.

### **Extension Method**

- Add a `static` `public` method to a `static` class. The first parameter must be of the type being extended, preceded by the `this` keyword.

Extension methods enable you to "add" methods to existing types (a `class` or `struct`) without creating a new derived type, recompiling, or otherwise modifying the original type. We use the `this` keyword, in front of the **FIRST** parameter of a method, to indicate which type is being extended. To enable extension methods for a particular type, just add a using directive for the namespace in which the methods are defined.

Extension methods can be defined on `interfaces`, `classes`, `sealed` classes, `System.Object`, but cannot hide an already existing instance method.

### `virtual` methods and Polymorphism

- Consider

  ```csharp
    class Mammal
    {
      ...
      public virtual string GetTypeName()
      {
        return "This is a mammal" ;

    }
    class Horse : Mammal
    {
      ...
      public override string GetTypeName()
      {
        return "This is a horse";
      }
    }

    class Whale : Mammal
    {
      ...
      public override string GetTypeName()
      {
        return "This is a whale";
      }
    }

    class Aardvark : Mammal
    {
      ...
    }

    // Now

    Mammal myMammal;
    Horse myHorse = new Horse(...);
    Whale myWhale = new Whale(...);
    Aardvark myAardvark = new Aardvark(...);

    myMammal = myHorse;
    Console.WriteLine(myMammal.GetTypeName()); // Horse
    myMammal = myWhale;
    Console.WriteLine(myMammal.GetTypeName()); // Whale
    myMammal = myAardvark;
    Console.WriteLine(myMammal.GetTypeName()); // Mammal
  ```

- This phenomenon of the same statement invoking a different method depending on its context is called "Polymorphism", which literally means“many forms.”

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

### **`Destructor`**

Destructors (opposite of `constructors`) is a special method which is called by CLR after the reference to an object has disappeared. It is written by prepending the Class name with `~`.

e.g. `~ClassName`

- You can use a `destructor` to perform any tidying up that’s required when anobject is garbage collected. The CLR will automatically clear up any managed resources that an object uses, so in many of these cases, writing a destructor is unnecessary.

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

Without Garbage collector, following problems could arise:

- We'd forget to destroy the object, meaning it's `destructor` would not be run, hence important tidying up operations could be missed and the memory would not be returned to heap.

- An active object could mistakenly be destroyed, but one or more variables could still hold a reference to that deleted object, resulting in **_dangling reference_**. A dangling reference refers either to unused memory or possibly to a completely different object that now happens to occupy the same piece of memory. Either way, the outcome of using a dangling reference would be undefined at best or a security risk at worst.

- We might try to destroy an object multiple times.

The garbage collector makes the following guarantees:

- Every object will be destroyed, and its destructor will be run. When a program ends, all outstanding objects will be destroyed.
- Every object will be destroyed exactly once.
- Every object will be destroyed only when it becomes unreachable—that is,when there are no references to the object in the process running your application.

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

### `using` statement and `IDisposable` interface

Instead of using nested `try catch finally` blocks, `using` statements are used (not to be confused with `using` directive at the top of the file)

`using` statement provides a clean mechanism for controlling the lifetimes of resources, you create an object, and that object will be destroyed when the `using` statement block finishes.

```csharp
using ( type variable = initialization)
{
  statement block
}

```

- The variable you declare in a `using` statement must be of a type that implements the `IDisposable` interface. The `IDisposable` interface lives in the `System` namespace and contains just one method, named `Dispose`

  ```csharp
    namespace System
    {
      interface IDisposable
      {
        void Dispose();
      }
    }
  ```

- Support exception-safe disposal in a class

  ```csharp
    class SafeResource : IDisposable
    {
      // ...
      public void Dispose()
      {
        // Dispose resources here
      }
    }
  ```

---

## **Classes**

- `abstract` classes: A class that is meant to be a Base class to other classes, and not be instantiated from (but can be inherited from).
- Normal or Concrete `clases` may have virtual methods.
- `sealed` classes: prevents other classes from inheriting from it.

  - Unless you consciously design a class with the intention of using it as a base class, it’s extremely unlikely that it will function well as a base class. With C#, you can use the `sealed` keyword to prevent a classfrom being used as a base class if you decide that it should not be.

    ```csharp
    sealed class Horse : GrazingMammal, ILandBound
    {
      // ...
    }
    ```

  - If any class attempts to use `Horse` as a base class, a compile-time error will be generated.

  - Note that a sealed class cannot declare any `virtual` methods (bacause `virtual` methods need an implementation in derived class) and that an `abstract` class cannot be sealed (`abstract` classes are meant to be derived from).

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

- The string type in C# is actually a class. This is because there is no standard size for a string (different strings can contain different numbers of characters), and allocating memory for a string dynamically when the program runs is far more efficient than doing so statically at compile time. In fact, the string keyword in C# is just an alias for the `System.String` class.

### Calling base-class constructors

```csharp
class Mammal // base class
{
    public Mammal(string name) // constructor for base class
    {
      // ...
    }
  //...
}
class Horse : Mammal // derived class
{
    public Horse(string name): base(name) // calls Mammal(name)
    {
      // ...
    }
    //...
}
```

- If you don’t explicitly call a base-class constructor in a derived-class constructor, the compiler attempts to silently insert a call to the base class’s default constructor before executing the code in the derived-class constructor.

- This works if base class has a public default constructor. However, not all classes have a public default constructor (for example, remember that the compiler generates a default constructor only if you don’t write any non-default constructors), in which case, forgetting to call the correct base-class constructor results in a compile-time error.

### `System.Object` and `ToString()` Method

- The `System.Object` class is the root class of all classes. All classes implicitly derive from `System.Object`. This means that all classes that you define automatically inherit all the features of the `System.Object` class. This includes methods such as `ToString`.

- You are not expected to call the `ToString` method defined by `System.Object`; it is simply a placeholder. Instead, you might find it more useful to provide your own version of the `ToString` method in each class you define, overriding the default implementation in `System.Object`. The version in `System.Object` is there only as a safety net, in case a class does not implement or require its own specific version of the `ToString` method.

---

## Property

### Property Restrictions

- You can assign a value through a property of a structure or class only after the structure or class has been initialized. The following code example is illegal because the `location` variable has not been initialized (by using `new`):

  ```csharp
    ScreenPosition location;
    location.X = 40; // compile-time error, location not assigned

    // This might seem trivial, but if X were a field rather than a property,the code would be legal.
  ```

- You can’t use a property as a `ref` or an `out` argument to a method (although you can use a writable field as a `ref` or an `out` argument). This makes sense because the property doesn’t really point to a memory location; rather, it points to an accessor method.

  ```csharp
    MyMethod(ref location.X); // compile-time error
  ```

- A property can contain at most one `get` accessor and one `set` accessor. A property cannot contain other methods, fields, or properties.

- The `get` and `set` accessors cannot take any parameters. The data being assigned is passed to the `set` accessor automatically by using the `value` variable.

- You can’t declare properties by using `const`. (`const` is used to create static fields)

### object initializer

```csharp
  public class Triangle
  {
    private int side1Length = 10;
    private int side2Length = 10;
    private int side3Length = 10;
    public int Side1Length
    {
      set => this.side1Length = value;
    }
    public int Side2Length
    {
      set => this.side2Length = value;
    }
    public int Side3Length
    {
      set => this.side3Length = value;
    }
  }

  // When you create an instance of a class, you can initialize it by specifying the names and values for any public properties that have set accessors

  Triangle tri1 = new Triangle { Side3Length = 15 };
  Triangle tri2 = new Triangle { Side1Length = 15, Side3Length = 20 };
  Triangle tri3 = new Triangle { Side2Length = 12, Side3Length = 17 };
  Triangle tri4 = new Triangle { Side1Length = 9, Side2Length = 12,Side3Length = 15 };

  // This syntax is known as an object initializer.
```

---

## **Reference vs Value types**

- When a value type is copied, the value in the variable is copied as a whole.

- The value in the reference variable is also copied as a whole, but that value in itself is actually a reference. So, in effect, a reference (to the actual value) is copied from one variable to another.

- In C#, you can assign the null value to any reference variable.

- C# defines a modifier that you can use to declare that a variable is a "nullable value type". A nullable value type behaves similarly to the original value type, but you can assign the null value to it. You use the question mark (?, e.g. int?) to indicate that a value type is nullable.

- You can assign an expression of the appropriate value type directly to a nullable variable. The following code is legal

  ```csharp
    int? i = null;
    int j = 99;
    i = 100; // Copy a value type constant to a nullable type
    i = j; // Copy a value type variable to a nullable type

    // but the converse is illegal
    j = i; // Illegal
  ```

- This makes sense when you consider that the variable `i` might contain `null`, and `j` is a value type that cannot contain `null`.

- This also means that you cannot use a nullable variable as a parameter to a method that expects an ordinary value type

#### `HasValue` and `.Value` for nullable types

- The `HasValue` property indicates whether a nullable type contains a value or is `null`.

- You can retrieve the value of a nonnull nullable type by reading the Valueproperty

  ```csharp
    int? i = null;

    if (!i.HasValue)
    {
      // If i is null, then assign it the value 99
      i = 99;
    }
    else
    {
      // If i is not null, then display its value
      Console.WriteLine(i.Value);
    }
  ```

### `ref` and `out` parameters

- Ordinarily, when you pass an argument to a method, the corresponding parameter is initialized with a copy of the argument. This is true regardless of whether the parameter is a value type (such as an `int`), a nullable type (such as `int?`), or a reference type.

- This arrangement means that it’s impossible for any change to the parameter to affect the value of the argument passed in

  ```csharp
    static void doIncrement(int param)
    {
      param++;
    }

    static void Main()
    {
      int arg = 42;
      doIncrement(arg);
      Console.WriteLine(arg); // writes 42, not 43
    }
  ```

- However, if the parameter to a method is a reference type, any changes made by using that parameter **_change_** the data referenced by the argument passed in.

- The key point is this: Although the data that was referenced changed, the argument passed in as the parameter did not—it still references the same object

- In other words, although it is possible to modify the object that the argument refers to through the parameter, it’s not possible to modify the argument itself (for example, to set it to refer to a completely different object).

- Occasionally, however, you might wantto write a method that actually needs to modify an argument. C# provides the `ref` and `out` keywords so that you can do this.

- If you prefix a parameter with the `ref` keyword, the C# compiler generates code that passes a reference to the actual argument rather than a copy of the argument.When using a `ref` parameter, anything you do to the parameter you also do to the original argument because the parameter and the argument both reference the same data.

- When you pass an argument as a `ref` parameter, you must also prefix the argument with the `ref` keyword.

  ```csharp
    static void doIncrement(ref int param) // using ref
    {
      param++;
    }
    static void Main()
    {
      int arg = 42;
      doIncrement(ref arg); // using ref
      Console.WriteLine(arg); // writes 43
    }
  ```

- This time, the `doIncrement` method receives a reference to the original argument rather than a copy, so any changes the method makes by using this reference actually change the original value. That’s why the value 43 is displayed on the console.

- Remember that C# enforces the rule that you must assign a value to a variable before you can read it. This rule also applies to method arguments; you cannot pass an uninitialized value as an argument to a method even if an argument is defined as a `ref` argument

  ```csharp
    static void doIncrement(ref int param)
    {
      param++;
    }
    static void Main()
    {
      int arg; // not initialized
      doIncrement(ref arg); // Error
      Console.WriteLine(arg);
    }
  ```

- The `out` keyword is syntactically similar to the `ref` keyword. You can prefix a parameter with the `out` keyword so that the parameter becomes an alias for the argument.

- When you pass an `out` parameter to a method, the method must assign a value to it before it finishes or returns.

  ```csharp
    static void doInitialize(out int param)
    {
      param = 42; // Initialize param before finishing
      // if param is not initialized, it will be an Error
    }
  ```

- Because an `out` parameter must be assigned a value by the method, you’re allowed to call the method without initializing its argument.

  ```csharp
    static void doInitialize(out int param)
    {
      param = 42;
    }
    static void Main()
    {
      int arg; // not initialized
      doInitialize(out arg); // legal
      Console.WriteLine(arg); // writes 42
    }
  ```

- **Note** You can use the `ref` and `out` modifiers on reference type parameters as well as on value type parameters. The effect is the same: the parameter becomes an alias for the argument.

---

## **Stack vs Heap**

- All value types are created on the stack. All reference types(objects) are created on the heap (although the reference itself is onthe stack). Nullable types are actually reference types, and they arecreated on the heap.

- When you call a method, the memory required for its parameters and its local variables is always acquired from the stack. When the method finishes (because it either returns or throws an exception), the memory acquired for the parameters and local variables is automatically released back to the stack and is available again when another method is called.

- Method parameters and local variables on the stack have a well-defined lifespan: they come into existence when the method starts, and they disappear as soon as the method completes.

- When you create an object (an instance of a class) by using the `new` keyword, the memory required to build the object is always acquired from the heap.

- When the last reference to an object disappears, the memory used by the object becomes available again (although it might not be reclaimed immediately).

- Returning reference data from a method (as a `ref`) is a powerful technique, but you must treat it with care. You can only return a reference to data that still exists when the method has finished, such as an element in an array. For
  example, you cannot return a reference to a local variable created on the stack.

---

## **`System.Object` OR `object` Class**

- all classes are specialized types of `System.Object` and that you can use `System.Object` to create a variable that can refer to any reference type. `System.Object` is such an important class that C# provides the `object` keyword as an alias for `System.Object`. In your code, you can use `object`, or you can write `System.Object` — they mean the same thing.

---

## **Boxing, Un-Boxing, Casting, `is` operator, `as` operator**

```csharp
  int i = 42;
  object o = i;
```

- Remember that `i` is a value type and that it lives on the stack.If the reference inside `o` referred directly to `i`, the reference would refer to the stack. However, all references must refer to objects on the heap; creating references to items on the stack could seriously compromise the robustness of the runtime and create a potential security flaw, so it is not allowed. Therefore,the runtime allocates a piece of memory from the heap, copies the value of integer `i` to this piece of memory, and then refers the object `o` to this copy. This automatic copying of an item from the stack to the heap is called **Boxing**.

![Boxing](./boxing.png)

- **NOTE**: If you modify the original value of the variable `i`, the value on the heap referenced through `o` will not change. Likewise, if you modify the value on the heap, the original value of the variable will not change.

- You might expect to be able to access the boxed `int` value that a variable `o` refers toby using a simple assignment statement such as `int i = o;` However, if you try this syntax, you’ll get a compile-time error. After all, `o` could be referencing absolutely anything and not just an int.

- To obtain the value of the boxed copy, you must use what is known as a **cast**.This is an operation that checks whether converting an item of one type to another is safe before actually making the copy.

  ```csharp
    int i = 42;
    object o = i; // boxes
    i = (int)o; // compiles okay
  ```

- The effect of this cast is subtle. The compiler notices that you’ve specified the type `int` in the cast. Next, the compiler generates code to check what `o` actually refers to at runtime. It could be absolutely anything. Just because your cast says `o` refers to an int, that doesn’t mean it actually does. If `o` really does refer to a boxed `int` and everything matches, the cast succeeds, and the compiler-generated code extracts the value from the boxed int and copies it to i. This is called **unboxing**.

![Un-Boxing](./unboxing.png)

- On the other hand, if `o` does not refer to a boxed `int`, there is a type mismatch, causing the cast to fail. The compiler-generated code throws an `InvalidCastException` exception at runtime.

- **NOTE**: Keep in mind that boxing and unboxing are expensive operations because of the amount of checking required and the need to allocate additional heap memory. Boxing has its uses, but injudicious use can severely impair the performance of a program.

### `is` operator

- You can use the is operator to verify that the type of an object is what you expectit to be.

  ```csharp
    WrappedInt wi = new WrappedInt();
    object o = wi;
    if (o is WrappedInt)
    {
      WrappedInt temp = (WrappedInt)o; // This is safe; o is a WrappedInt...
    }
  ```

- The `is` operator takes two operands: a reference to an object on the left, and the name of a type on the right, and returns a `bool`

- If the type of the object referenced on the heap has the specified type, is evaluates to `true`; otherwise, is evaluates to `false`.

- Another form of the is operator enables you to abbreviate this code by combining the type check and the assignment

  ```csharp
    WrappedInt wi = new WrappedInt();
    object o = wi;
    if (o is WrappedInt temp)
    {
      // Use temp here
    }
  ```

- In this example, if the test for the `WrappedInt` type is successful, the `is` operator creates a new reference variable (called `temp`), and assigns it a referenceto the `WrappedInt` object.

### `as` operator

- The `as` operator fulfills a similar role to `is` but in a slightly truncated manner. If the cast is successful, the value exists, else the value becomes `null`, so we can simply null check after using `as` operator

  ```csharp
    WrappedInt wi = new WrappedInt();
    object o = wi;
    WrappedInt temp = o as WrappedInt;
    if (temp != null)
    {
      // Cast was successful
    }
  ```

---

## Enums

- You define an enumeration by using the `enum` keyword, followed by a set of symbols identifying the legal values that the type can have, enclosing them between braces.

  ```csharp
    enum Season { Spring, Summer, Fall, Winter }
    class Example
    {
      public void Method(Season parameter) // method parameter example
      {
        Season localVariable = Season.Fall; // local variable example...
        Console.WriteLine(localVariable);
        // writes out 'Fall', converted automatically by the compiler
        Season? colorful = null;  // Enums are value types
      }
      private Season currentSeason; // field example
    }
  ```

- Many of the standard operators that you can use on integer variables you can also use on enumeration variables (except the bitwise and shift operators)

- Internally, an enumeration type associates an integer value with each element of the enumeration. By default, the numbering starts at `0` for the first element and goes up in steps of `1`.

- It’s possible to retrieve the underlying integer value of anenumeration variable, by casting it to an int using `(int)`

- you can associate a specific integer constant (such as 1) with an enumeration literal

  ```csharp
    enum Season { Sping = 1, Summer, Fall, Winter }  // Summer will be 2 and so on...
  ```

- You can base an enumeration on any of the eight integer types: `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.

  ```csharp
    enum Season : short { Spring, Summer, Fall, Winter }
  ```

---

## Structs

- A `structure` is a value type. Because structures are stored on the stack, as long as the `structure` is reasonably small, the memory management overhead is often reduced, compared to classes which are stored on the heap.

- Like a class, a structure can have its own fields, methods, and constructors.

- Tuples are actually examples of the `System.ValueTuple` structure. Rather more interestingly, in C#, the primitive numeric types `int`, `long`, and `float` are aliases for the structures `System.Int32`, `System.Int64`, and `System.Single`, respectively.

- Apart fromm `System.Object` and `System.String`, primitive types are mostly structs.

  ```csharp
    struct Time
    {
      public int hours, minutes, seconds;
    }
  ```

- you cannot use operators such as the equality operator (==) and the inequality operator (!=) on your own structure type variables. However, you can use the built-in `Equals()` method exposed by all structures to compare structure type variables, and you can also explicitly declare and implement operators for your own structure types.

- A structure and a class are syntactically similar, but they have a few important differences.

  - You can’t declare a default constructor (a constructor with no parameters)for a structure, the reason being that the compiler always generates one
  - In a class, you can initialize instance fields at their point of declaration. Ina structure, you cannot
    ```csharp
      struct Time
      {
        private int hours = 0; // compile-time error, compiles if class
        private int minutes;
        private int seconds;
      }
    ```

- As with enumerations, you can create a nullable version of a structure variable by using the ? modifier. You can then assign the null value to the variable:

  ```csharp
    Time? currentTime = null;
  ```

- because structures are value types, you can also create structure variables without calling a constructor, but the fields would be left un-initialiezed, and any attempt to get their value is a compile time error.

- You’re allowed to initialize or assign one `structure` variable to another `structure` variable, but only if the `structure` variable on the right side is completely initialized (that is, if all its fields are populated with valid data rather than undefined values).

---

## Arrays

- An array is an unordered sequence of items.

- All the items in an array have the same type, unlike the fields in a structure or class, which can have different types.

- The items in an array live in a contiguous block of memory and areaccessed by using an index, unlike fields in a structure or class, which are accessed by name.

- Arrays are reference types, regardless of the type of their elements.

- Creating an array also initializes its elements by using the default values (`0`, `null`, or `false`, depending on whether the type is numeric, a reference, or a Boolean, respectively).

```csharp
  int[] pins;  // Array declaration - no memory is allocated
  pins = new int[4];  // now memory is allocated, all four values are default values
  pins = new int[4]{ 9, 3, 7, 2 };
```

- When you’re initializing an array, you can actually omit the `new` expression and the size of the array. The compiler calculates the size from the number of initializers and generates code to create the array.

  ```csharp
    int[] pins = { 9, 3, 7, 2 };
  ```

- Implicitly typed arrays - the C# compiler determines that the names variable is an array of strings. you must specify the new operator and square brackets before the initializer list.

  ```csharp
    var names = new[]{ "John", "Diana", "James", "Francesca" }

    // Implicitly typed arrays are most useful when you are working withanonymous types

    var names = new[]
    {
      new { Name = "John", Age = 53 },
      new { Name = "Diana", Age = 53 },
      new { Name = "James", Age = 26 },
      new { Name = "Francesca", Age = 23 }
    };
  ```

- All array element access is bounds-checked. If you specify an index that is less than `0` or greater than or equal to the `length` of the array, the compiler throws an `IndexOutOfRangeException` exception.

- **Copying an Array** - `CopyTo` method copies the contents of one array into another array given a specified starting index. Another way to copy the values is to use the `System.Array` static method named `Copy`

  ```csharp
    int[] pins = { 9, 3, 7, 2 };
    int[] copy = new int[pins.Length];
    pins.CopyTo(copy, 0);

    // OR

    int[] pins = { 9, 3, 7, 2 };
    int[] copy = new int[pins.Length];
    Array.Copy(pins, copy, copy.Length);

    // Yet another alternative is to use the System.Array instance method named Clone.

    int[] pins = { 9, 3, 7, 2 };
    int[] copy = (int[])pins.Clone();

    // The Clone method of the Array class returns an object rather than Array, which is why you must cast it to an array of the appropriate type when you use it.

    // Furthermore, the Clone, CopyTo, and Copy methods all create a shallow copy of an array
  ```

- **Multi-dimensional Arrays** aka rectangular arrays - The following code creates a two-dimensional array of 24 integers called items.

  ```csharp
    int[,] items = new int[4, 6];

    // To access an element in the array, you provide two index values to specify the “cell” (the intersection of a row and a column) holding the element.

    items[2, 3] = 99; // set the element at cell(2,3) to 99
    items[2, 4] = items [2,3]; // copy the element in cell(2, 3) to cell(2,4)
    items[2, 4]++; // increment the integer value at cell(2, 4)

  ```

- **Jagged Arrays** or array of Arrays - where each column has a different length

  ```csharp

    int[][] items = new int[4][];
    // 3 elements in the first column, 10 elements in the second column, 40 elements in the third column, and 25 elements in the final column.
    int[] columnForRow0 = new int[3];
    int[] columnForRow1 = new int[10];
    int[] columnForRow2 = new int[40];
    int[] columnForRow3 = new int[25];
    items[0] = columnForRow0;
    items[1] = columnForRow1;
    items[2] = columnForRow2;
    items[3] = columnForRow3;
  ```

---

## Overloading and Parameter arrays

- **Overloading** - is the technical term for declaring two or more mehods with the same name in the same scope, but with different signatures.

- Overloading a method is very useful for cases in which you want to perform the same action on arguments of different types.

- `params` array allows us to pass a variable number of arguments to a method, just like `function abc(...params)` (rest parameter) in JS

  ```csharp
    class Util
    {
      public static int Min(params int[] paramList)
      {
        // code exactly as before
      }
    }
  ```

- There are several points worth noting about params arrays:

  - You can’t use the params keyword with multidimensional arrays. The code in the following example will not compile:

    ```csharp
      // compile-time error
      public static int Min(params int[,] table)
    ```

  - You can’t overload a method based solely on the `params` keyword. The `params` keyword does not form part of a method’s signature, as shown in this example. Here, the compiler would not be able to distinguish between these methods in code that calls them:

    ```csharp
      // compile-time error: duplicate declaration
      public static int Min(int[] paramList)
      // ...
      public static int Min(params int[] paramList)
      // ...
    ```

  - You’re not allowed to specify the `ref` or `out` modifier with params arrays,as shown in this example:

    ```csharp
      // compile-time errors
      public static int Min(ref params int[] paramList)
      // ...
      public static int Min(out params int[] paramList)
      // ...
    ```

  - A params array must be the last parameter. (This means that you can have only one params array per method.) Consider this example:

    ```csharp
        // compile-time error
        public static int Min(params int[] paramList, int i)
        // ...
    ```

  - A non-params method always takes priority over a `params` method. This means that you can still create an overloaded version of a method for the common cases, such as in the following example:

    ```csharp
      public static int Min(int leftHandSide, int rightHandSide)
      // ...
      public static int Min(params int[] paramList)
      // ...
    ```

    The first version of the `Min` method is used when it’s called using two `int` arguments. The second version is used if any other number of `int` arguments is supplied. This includes the case in which the method is called with no arguments. Adding the non-params array method might be a useful optimization technique because the compiler won’t have to create and populate so many arrays.

- `params object[]`

  - `params` object array is useful when not only the number, but types of argument varies as well.

  - You can use a `params` array of type `object` to declare a method that accepts any number of `object` arguments, allowing the arguments passed in to be of any type. (Thanks to boxing)

  ```csharp

    class Black
    {
      public static void Hole(params object[] paramList)
      //...
    }

    // You can pass the method no arguments at all, in which case the compiler will pass an object array whose length is 0
    Black.Hole();
    // converted to
    Black.Hole(new object[0]);

    // You can call the Black.Hole method by passing null as the argument. An array is a reference type, so you’re allowed to initialize an array with null:
    Black.Hole(null);

    // You can pass the Black.Hole method an actual array. In other words, you can manually create the array normally generated by the compiler
    object[] array = new object[2];
    array[0] = "forty two";
    array[1] = 42;
    Black.Hole(array);

    // You can pass the Black.Hole method arguments of different types and these arguments will automatically be wrapped inside an object array:
    Black.Hole("forty two", 42);
    //converted to
    Black.Hole(new object[]{"forty two", 42});

  ```

- `params` array vs optional parameters

  - A method that takes optional parameters still has a fixed parameter list,and you cannot pass an arbitrary list of arguments.

  - A method that uses a parameter array effectively has a completely arbitrary list of parameters, and none of them has a default value. Furthermore, the method can determine exactly how many arguments the caller provided.

  - There is one further situation worth pondering. If you define a method thattakes a parameter list and provide an overload that takes optional parameters, itis not always immediately apparent which version of the method will be called ifthe argument list in the calling statement matches both method signatures.

    - So in a function with two overloads, one with 4 optional parameters, vs, the one with `params` array, with 4 or less arguments, the first version is called, and with 5 or more arguments the latter version is called.

---

## Generics

### Generics vs Generalized Classes (using `object` type)

- Consider a `Queue` class implemented using `object` type for generalization of its constituents.

- you can use the `object` type to refer to a value or variable of any type.

  ```csharp
    Queue queue = new Queue();
    Horse myHorse = new Horse();
    queue.Enqueue(myHorse); // legal – Horse is an object...
    Horse dequeuedHorse = (Horse)queue.Dequeue(); // Need to cast object back to a Horse
  ```

- However, there are 2 problems with using general `object` class

  1. Runtime Errors:

     ```csharp
     Queue queue = new Queue();
     Horse myHorse = new Horse();
     queue.Enqueue(myHorse);// ...
     Circle myCircle = (Circle)queue.Dequeue(); // run-time error
     ```

     Although this code will compile, it is not valid and throws a `System.InvalidCastException` at runtime. The error is caused by trying to store a reference to a `Horse` in a `Circle` variable when it is dequeued, and the two types are not compatible. This error is not spotted until runtime because the compiler does not have enough information to perform this check at compiletime. The real type of the object being dequeued becomes apparent only whenthe code runs.

  2. Consumtpion of additional memory and process time due to effort required to box and unbox value types to `object` type. This overhead is small for each item, but it adds up when a program creates queues of large numbers of value types.

- Compare this with the `Queue<T>` class. Each time you use this class with a type parameter (such as `Queue<int>` or `Queue<Horse>`), you cause the compiler to generate an entirely new class that happens to have functionality defined by the generic class. This means that `Queue<int>` is a completely different type from `Queue<Horse>`, but they both happen to have the same behavior.

### Generic Type constraints

- When a type parameter is declared by a generic type or method, it can also specify type constraints that restrict which types can be provided as type arguments.

  ```csharp
    public class PrintableCollection<T> where T : IPrintable

    // OR

    static void PrintItems<T>(List<T> items) where T : IFormattable

  ```

- The following constraints are available -

  - _Reference type constraint_ — `where T : class` - The type argument must be a refer-ence type. (Don’t be fooled by the use of the `class` keyword; it can be any reference type, including `interfaces` and `delegates`.)
  - _Value type constraint_ — `where T : struct` - The type argument must be a non-nullable value type (either a `struct` or an `enum`. Nullable value types don’t meet this constraint.
  - _Constructor constraint_ — `where T : new()` - The type argument must have a public parameterless constructor. This enables the use of `new T()` within the body of the code to construct a new instance of `T`.
  - _Conversion constraint_ — `where T : SomeType`. Here, `SomeType` can be a `class`, an `interface`, or another type parameter as shown here:
    – `where T : Control`
    – `where T : IFormattable`
    – `where T1: T2`

- When multiple type parameters exist in a generic declaration, each type parameter can have an entirely different set of constraints as in the following example:

  ```csharp
  TResult Method<TArg, TResult>(TArg input)
    where TArg : IComparable<TArg>
    where TResult : class, new()
  ```

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

- `LinkedList<T>`
  - is a doubly-linked list
  - does not support array notation for insertion or examining elements, unlike `List<T>`.
  - Also only supports `AddFirst`, `AddLast`, `AddBefore`, `AddAfter`, `Remove`, `RemoveFirst`, `RemoveLast` methods.

- `Dictionary<TKey, TValue>`
  - cannot contain duplicate keys.
  - Calling `Add` method with an existing key is an exception.
  - Supports square bracket notation
  - Uses relatively more memory.
  - `foreach` iterate over a `KeyValuePair<TKey, TValue>` pair with `Key` and `Value` props.

- `SortedList<TKey, TValue>`
  - is similar to Dictionary
  - is sorted by `Key` automatically
  - `foreach` returns a KeyValuePair sorted by `Key`

- `HashSet<T>`
  - is a hash table internally
  - enables fast lookup of items

---

## Interfaces

- An interface gives you only the name, return type, and parameters of the method. Exactly how the method is implemented is not a concern of the interface. The interface describes the functionality that a class should provide but not how this functionality is implemented.

- Within the interface, you declare methods exactly as in a `class` or `structure`, except that you never specify an access modifier (`public`, `private`, or `protected`).

- An interface cannot contain any data; you cannot add fields (not even private ones) to an interface.

- Interfaces can define properties as well as methods. To do this,you specify the `get` or `set` keyword or both, but you replace the body of the `get` or `set` accessor with a semicolon,

  ```csharp
    interface IScreenPosition
    {
      int X { get; set; }
      int Y { get; set; }
    }
  ```

  Any class or structure that implements this interface must implement the X and Y properties with `get` and `set` accessor methods (or expression-bodied members).

  ```csharp
    struct ScreenPosition : IScreenPosition
    {
      // ...
      public int X
      {
        get { ... } // or get => ...
        set { ... } // or set => ...
      }
      public int Y
      {
        get { ... }
        set { ... }
      }
    }
  ```

  If you implement the interface properties in a class, you can declare the property implementations as `virtual`, which enables derived classes to `override` the implementations.

  ```csharp
    class ScreenPosition : IScreenPosition
    {
      //...
      public virtual int X
      {
        get { ... }
        set { ... }
      }
      public virtual int Y
      {
        get { ... }
        set { ... }
      }
    }
  ```

### Implementing an Interface

- When you implement an interface, you must ensure that each method matches its corresponding interface method exactly, according to the following rules:

  - The method names and return types match exactly.
  - Any parameters (including `ref` and `out` keyword modifiers) match exactly.
  - All methods implementing an interface must be publicly accessible.However, if you are using an explicit interface implementation, the method should not have an access qualifier.

- A class can inherit from another class and implement an interface at the same time. The base class is always named first, followed by a comma, followed by the interface.

- **Note** An interface, InterfaceA, can inherit from another interface,InterfaceB. Technically, this is known as interface extension rather than inheritance. In this case, any class or struct that implements InterfaceA must provide implementations of all the methods in InterfaceB and InterfaceA.

### Referencing a `class` through its `interface`

- In the same way that you can reference an `object` by using a variable defined as a `class` that is higher up the hierarchy, you can reference an `object` by using a variable defined as an `interface` that the object’s `class` implements.

- The technique of referencing an object through an interface is useful because you can use it to define methods that can take different types as parameters, as long as the types implement a specified interface.

  ```csharp
    int FindLandSpeed(ILandBound landBoundMammal)
    {
      // ...
    }
  ```

- We can verify that an `object` is an instance of a `class` that implements a
  specific `interface` by using the `is` operator. You use the `is` operator to determine whether an `object` has a specified type, and it works with `interfaces` as well as with classes and structs.

  ```csharp
    if (myHorse is ILandBound)
    {
      ILandBound iLandBoundAnimal = myHorse;
    }
  ```

- **Note** that when referencing an `object` through an `interface`, you can invoke only methods that are visible through the `interface`.

- A `class` can have at most one base class, but it is allowed to implement an unlimited number of `interfaces`, specified as comma separated list.

### Explicitly implementing interfaces

- To solve the problem of a class inheriting from multiple interfaces which have a method of same names and disambiguate which method is part of which interface implementation, you can implement interfaces explicitly.

  ```csharp
    class Horse : ILandBound, IJourney
    {
      // ...
      int ILandBound.NumberOfLegs()
      {
        return 4;
      }
      int IJourney.NumberOfLegs()
      {
        return 3;
      }
    }
  ```

- **NOTE** - Apart from prefixing the name of the method with the `interface` name, there is one other subtle difference in this syntax: the methods are not marked public. You cannot specify the protection for methods that are part of an explicit `interface` implementation. This leads to another interesting phenomenon. If you create a `Horse` variable in the code, you cannot actually invoke either of `theNumberOfLegs` methods because they are not visible. As far as the `Horse` class is concerned, they are both `private`. In fact, this makes sense. If the methods were visible through the `Horse` class, which method would the following code actually invoke, the one for the `ILandBound` interface or the one for the `IJourney` interface?

  ```csharp
    Horse horse = new Horse();
    // ...
    // The following statement will not compile
    int legs = horse.NumberOfLegs();
  ```

  So, how do you access these methods? The answer is that you reference the `Horse` object through the appropriate interface, like this:

  ```csharp
    Horse horse = new Horse();
    // ...
    IJourney journeyHorse = horse;
    int legsInJourney = journeyHorse.NumberOfLegs();
    ILandBound landBoundHorse = horse;
    int legsOnHorse = landBoundHorse.NumberOfLegs();
  ```

### Interface restrictions

- You’re not allowed to define any fields in an interface, not even static fields. A field is an implementation detail of a `class` or `structure`.
- You’re not allowed to define any constructors in an interface. A `constructor` is also considered to be an implementation detail of a `class` or `structure`.
- You’re not allowed to define a `destructor` in an interface. A `destructor` contains the statements used to destroy an object instance.
- You cannot specify an access modifier for any method. All methods in an interface are implicitly public.
- You cannot nest any types (such as enumerations, structures, classes, or interfaces) inside an interface.
- An `interface` is not allowed to inherit from a `structure` or a `class`, although an `interface` can inherit from another `interface`. Structures and classes contain implementation; if an `interface` were allowed to inherit from either, it would be inheriting some implementation.

---

### Invariant, Covariant and Contravariant Interfaces

An Interface is said to be invariant when we cannot assign an `IWrapper<A>` object to a reference of type `IWrapper<B>`, even if type `A` is derived from type `B`. So, `IWrapper<object>` cannot be assigned to `IWrapper<string>`, because, even though all strings are objects, not all objects are strings.

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

This feature is called **_covariance_**. You can assign an `IRetrieveWrapper<A>` object to an `IRetrieve-Wrapper<B>` reference as long as there is a valid conversion from type `A` to type `B`, or type `A` derives from type `B`. You can specify the `out` qualifier with a type parameter only if the type parameter occurs as the return type of methods. If you use the type parameter to specify the type of any method parameters, the out qualifier is illegal, and your code will not compile. Also, covariance works only with reference types. This is because value types cannot form inheritance hierarchies.

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
`IEnumerator<T>` interface, which has a `Current` property that returns a `T` instead. Likewise, there is also an `IEnumerable<T>` interface containing a `GetEnumerator` method that returns an `Enumerator<T>` object. Both of these interfaces are defined in the `System.Collections.Generic` namespace, and if you are building applications for the .NET Framework version 2.0 or later, you should make use of these generic interfaces rather than the nongeneric versions when you define enumerable collections.

### System.IComparable and System.IComparable<T> interfaces

- If you need to create a class that requires you to be able to compare values according to some natural (or possibly unnatural) ordering, you should implement the `IComparable<T>` interface. This interface contains a method called `CompareTo`, which takes a single parameter specifying the object to be compared with the current instance and returns an integer that indicates the result of the comparison. less than 0 means current instance < value of the parameter. Equal to 0 means current instance = value of the parameter. Greater than 0 means current instance > value of the parameter.

---

### Initializing a variable defined with a type parameter using `default`

```csharp

private TItem currentItem = default(TItem);

```

The `currentItem` variable is defined by using the type parameter `TItem`. When the program is written and compiled, the actual type that will be substituted for `TItem` might not be known; this issue is resolved only when the code is executed. This makes it difficult to specify how the variable should be initialized. The temptation is to set it to `null`. However, if the type substituted for `TItem` is a `value` type, this is an illegal assignment. (You cannot set value types to `null`, only reference types.) Similarly, if you set it to `0` with the expectation that the type will be numeric, this will be illegal if the type used is actually a `reference` type. There are other possibilities as well; `TItem` could be a boolean, for example.

The default keyword solves this problem. The value used to initialize the variable will be determined when the statement is executed. If `TItem` is a reference type, `default(TItem)` returns null; if TItem is numeric, `default(TItem)` returns 0; if `TItem` is a boolean, `default(TItem)` returns `false`. If TItem is a struct, the individual fields in the struct are initialized in the same way. (Reference fields are set
to null, numeric fields are set to 0, and boolean fields are set to false.)

---

### Iterators

```csharp
using System.Collections.Generic;
using System.Collections;

class BasicCollection<T> : IEnumerable<T>
{
  private List<T> data = new List<T>();
  public void FillList(params T [] items)
  {
    foreach (var datum in items)
    {
      data.Add(datum);
    }
  }
  IEnumerator<T> IEnumerable<T>.GetEnumerator()
  {
    foreach (var datum in data)
    {
      yield return datum;
    }
  }
}

```

- The code in the `GetEnumeratormethod` defines an iterator. The compiler uses this code to generate an implementation of the `IEnumerator<T>` class containing a `Current` method and a `MoveNext` method.

---

### `Func<T, ...>` vs `Action<T, ...>`

`Func<T1, T2, ..., TResult>` takes 0 or more arguments (`T1`, `T2` and so on) and `return` a result of type `TResult`

`Action<T1, T2, ...>` takes 0 or more arguments, performs some action and returns `void`. Thus, `Action` is more like a subroutine.

---

### Delegates

References to methods, basically.

- it is a type/object that represents references to methods with a particular parameter list and return type and then calls the method in a program for execution when it is needed.

- A delegate will call only a method which agrees with its signature and return type. A method can be a static method associated with a class or can be an instance method associated with an object, it doesn’t matter.

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

---

## Binary Numbers and Indexers

### Binary and Hexadecimal numbers

- To make life a little easier for handling data that you want to treat as a collection of binary values, C# enables you to specify integer constants using binary notation. You indicate that a constant should be treated as a binary representation by prefixing it with `0b0`.

  ```csharp
    uint binData = 0b01111;
  ```

  **Note** that this is a 4-bit value, but an integer occupies 32 bits; any bits not specified are initialized to zero. You should also observe that when you specify an integer as a binary value, it is good practice to store the result as an unsigned int (`uint`).

- To help cope with long strings of bits, you can also insert the `_` character as a separator between blocks of digits, like this:

  ```csharp
    uint moreBinData = 0b0_11110000_01011010_11001100_00001111;  // the "_" is ignored by C# Compiler
  ```

- The same values can be specified using Hexa notation `0x0`

  ```csharp
    uint hexData = 0x0_0F;  // == binData
    uint moreHexData = 0x0_F0_5A_CC_0F;  // == moreBinData
  ```

- If you need to display the binary representation of an integer, you can use the `Convert.ToString` method. `Convert.ToString` is a heavily overloaded method that can generate a string representation of a range of data values held in different types. If you are converting integer data, you can additionally specify a numeric base (2, 8, 10, or 16), and the method will convert the integer to that base.

  ```csharp
    uint moreHexData = 0x0_F0_5A_CC_0F;
    Console.WriteLine($"{Convert.ToString(moreHexData, 2)}"); // displays 11110000010110101100110000001111
  ```

### Bitwise Operators

(all the examples below are accurate to 8 bits, C# though, uses 32 bits to store numbers)

- **The NOT (`~`) operator**: This is a unary operator that performs a bitwise complement. For example, if you take the 8-bit value `0b0_11001100` (204 decimal) and apply the `~` operator to it, you obtain the result `0b0_00110011` (51 decimal); all the `1`s in the original value become `0`s,and all the `0`s become `1`s.

- **The left-shift (`<<`)**: operator This is a binary operator that performs a left shift. The 8-bit expression `204 << 2` returns the value `48`. (In binary, `204` decimal is `0b0_11001100`, and shifting it left by two places yields `0b0_00110000`, or `48` decimal.) The far-left bits are discarded, and zeros are introduced from the right. There is a corresponding **right-shift operator(`>>`)**.

- **The OR (`|`) operator**: This is a binary operator that performs a bitwise OR operation, returning a value containing a 1 in each position in which either of the operands has a 1. For example, the 8-bit expression `204 | 24` has the value 220 (204 is `0b0_11001100`, 24 is `0b0_00011000`, and 220 is `0b0_11011100`).

- **The AND (`&`) operator**: This operator performs a bitwise AND operation. AND is similar to the bitwise OR operator, but it returns a value containing a 1 in each position where both of the operands have a 1. So,the 8-bit expression `204 & 24` is 8 (204 is `0b0_11001100`, 24 is `0b0_00011000`, and 8 is `0b0_00001000`).

- **The XOR (`^`) operator**: This operator performs a bitwise exclusive OR operation, returning a 1 in each bit where there is a 1 in one operand or theother but not both. (Two 1s yield a 0; this is the “exclusive” part of theoperator.) So the 8-bit expression 204 ^ 24 is 212 (0b0_11001100 ^0b0_00011000 is 0b0_11010100).

- Reasoning about bitwise operators on numbers is difficult, that where indexer come in.

### Indexers

- Indexers allow us to use square bracket notation to access individual values from a collection, which, without indexers, does not usually allow square brackets as data access operator.

- So, if we define an indexer, we can use `bits[5]`, where bits is a binary number, to access 6th value from the right.

- You introduce the indexer with the `this` keyword, specify the type of the value returned by the indexer, and also specify the type of the value to use as the index into the indexer between square brackets. The indexer for the `IntBits` struct uses an `integer` as its index type and returns a `Boolean` value.

  ```csharp
    struct IntBits
    {
      private int bits;
      public IntBits(int initialBitValue) => bits = initialBitValue;

      public bool this [ int index ]
      {
        get => (bits & (1 << index)) != 0;
        set
        {
          if (value) // turn the bit on if value is true; otherwise, turn it off
            bits |= (1 << index);
          else
            bits &= ~(1 << index);
        }
      }

      public override string ToString()
      {
        return (Convert.ToString(bits, 2);
      }
    }

  ```

- **NOTE:**

  - An indexer is not a method; there are no parentheses containing a parameter, but there are square brackets that specify an index. This index is used to specify which element is being accessed.
  - All indexers use the `this` keyword. A `class` or `structure` can define at most one indexer (although you can overload it and have several implementations), and it is always named `this`.
  - Indexers contain `get` and `set` accessors just like properties. In this example, the `get` and `set` accessors contain the complicated bitwise expressions.The `index` specified in the indexer declaration is populated with the index value specified when the indexer is called. The `get` and `set` accessor methods can read this argument to determine which element should be accessed.

- It is also good practice to provide a way to display the data in this structure. You can do this by overriding the `ToString` method and converting the value held in the structure to a string containing its binary representation.

- After you have created the indexer, you can use a variable of type `IntBits` instead of an `int` and apply the square bracket notation.

  ```csharp
    int adapted = 0b0_01111110;
    IntBits bits = new IntBits(adapted);
    bool peek = bits[6]; // retrieve bool at index 6; should be true (1)
    bits[0] = true; // set the bit at index 0 to true (1)
    bits[3] = false; // set the bit at index 3 to false (0)
    Console.WriteLine($""); // displays 1110111 (0b0_01110111)
  ```

---

---

## **`new` modifier**

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
