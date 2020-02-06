# C# in Depth

## Generics

### Benefits of Generic Types

Generics (like `List<T>`), solve

- You don’t need to know the size of the collection beforehand, unlike with arrays.

- The exposed API uses `T` everywhere it needs to refer to the element type, so you know that a `List<string>` will contain only string references. You’ll get a compile-time error if you try to add anything else, unlike with `ArrayList`.

- You can use it with any element type without worrying about generating code and managing the result, unlike with `StringCollection` and similar types.

### Type Parameters and Type Arguments

- Same concept as function paramteres and arguments, but for types

  ```csharp
    public class List<T>  // T is type parameter
    {
      // ...
    }
    List<string> list = new List<string>();  // string is Type argument
  ```

### Generic Arity

- The generic arity of a declaration is the number of type parameters it has.

  ```csharp
    public class Dictionary<TKey, TValue> // Generic Arity of 2
  ```

- You can think of a non-generic declaration as one with generic arity 0.

### What can be Generic

- some members may look like they’re generic because they use other generic types. Remember that a declaration is generic only if it introduces new type parameters.

- Methods and nested types can be generic, but all of the following have to be non-generic:
  - Fields
  - Properties
  - Indexers
  - Constructors
  - Events
  - Finalizers

```csharp
  public class ValidatingList<TItem>
  {
    private readonly List<TItem> items = new List<TItem>();
  }
```

- Here, the items field is of type `List<TItem>`. It uses the type parameter `TItem` as a type argument for `List<T>`, but that’s a type parameter introduced by the class declaration, not by the field declaration.

### Type Inference

- Although type inference applies only to methods, it can be used to more easily construct instances of generic types. For example, consider the `Tuple` family of types introduced in .NET 4.0. This consists of a nongeneric static `Tuple` class and multiple generic classes: `Tuple<T1>`, `Tuple<T1,T2>`, `Tuple<T1,T2,T3>`, and so forth. The static class has a set of overloaded `Create` factory methods like this

  ```csharp
    public static Tuple<T1> Create<T1>(T1 item1)
    {
      return new Tuple<T1>(item1);
    }

    public static Tuple<T1, T2> Create<T1, T2>(T1 item1, T2 item2)
    {
      return new Tuple<T1, T2>(item1, item2);
    }
  ```

  These look pointlessly trivial, but they allow type inference to be used where otherwise the type arguments would have to be explicitly specified when creating tuples. Instead of this `new Tuple<int, string, int>(10, "x", 20)` you can write this: `Tuple.Create(10, "x", 20)`

### `default` keyword

- The `default` operator can be used with type parameters and with generic types with appropriate type arguments supplied (where those arguments can be typeparameters, too). e.g

  - `default(T)`
  - `default(int)`
  - `default(string)`
  - `default(List<T>)`
  - `default(List<List<string>>)`

- The type of the `default` operator is the type that’s named inside it.

  ```csharp
    public T LastOrDefault(IEnumerable<T> source)
    {
      T ret = default(T);  // Declare a local variable and assign the default value of T to it.
      foreach (T item in source)
      {
        ret = item;
      }
      return ret;
    }
  ```

### `typeof` Operator

- `typeof(string)` - System.String
- `typeof(List<int>)` - returns the Type representing `List<T>` with a type argument of `int`
- `typeof(T)` or `typeof(List<T>)` - returns whatever the type argument for `T` is at that point in the code. This will always be a closed, constructed type ( it’s a real type with no type parameters involved anywhere)
- `typeof(List<>)` - That appears to be missing a type argument altogether. This syntax is valid only in the `typeof` operator and refers to the generic type definition. The syntax for types with generic arity 1 is just `TypeName<>`; for each additional type parameter, you add a comma within the angle brackets. To get the generic type definition for `Dictionary<TKey,TValue>`, you’d use `typeof(Dictionary<,>)`. To get the definition for `Tuple<T1,T2,T3>`, you’d use `typeof(Tuple<,,>)`.

  ```csharp
    public static void Print<T>()
    {
      Console.WriteLine(typeof(string));
      Console.WriteLine(typeof(List<int>));
      Console.WriteLine(typeof(List<T>));
      Console.WriteLine(typeof(Dictionary<,>));

    }

    // somewhere
    Print<string>()

    /*

    System.String
    System.Collections.Generic.List`1[System.Int32]
    System.Collections.Generic.List`1[System.String]
    System.Collections.Generic.Dictionary`2[TKey,TValue]

    */

  ```

---

## `Nullable<T>`

- Represents a value type that can be assigned `null`. (defined like this)

  ```csharp
    public struct Nullable<T> where T : struct
    {
      private readonly T value;
      private readonly bool hasValue;

      public Nullable(T value)
      {
        this.value = value;
        this.hasValue = true;
      }
      public bool HasValue { get { return hasValue; } }
      public T Value
      {
        get
        {
          if (!hasValue)
          {
            throw new InvalidOperationException();
          }
          return value;
        }
      }
    }
  ```

  - The type `T` is also known as the underlying type of `Nullable<T>`. For example, the
    underlying type of `Nullable<int>` is `int`.

  - The Nullable<T> struct has methods and operators available, too:

    - The parameterless GetValueOrDefault() method will return the value in the struct or the default value for the type if HasValue is false.

    - The parameterized GetValueOrDefault(T defaultValue) method will return the value in the struct or the specified default value if HasValue is false .

    - The Equals(object) and GetHashCode() methods declared in object are overridden in a reasonably obvious way, first comparing the HasValue properties and then comparing the Value properties for equality if HasValue is true for both values.

    - There’s an implicit conversion from T to Nullable<T> , which always succeeds and returns a value where HasValue is true . This is equivalent to calling the parameterized constructor.

    - There’s an explicit conversion from Nullable<T> to T , which either returns the encapsulated value (if HasValue is true ) or throws an InvalidOperationException (if HasValue is false ). This is equivalent to using the Value property.

#### Boxing behavior

- `Nullable` value types behave differently than non-nullable value types when it comes to boxing.

- When a value of a non-nullable value type is boxed, the result is a reference to an object of a type that’s the boxed form of the original type.

- Say, for example, you write this:

  ```csharp
    int x = 5;
    object o = x;
  ```

  The value of o is a reference to an object of type “boxed int.” The difference between boxed int and int isn’t normally visible via C#. If you call `o.GetType()`, the Type returned will be equal to `typeof(int)`.

- But, Nullable value types have no boxed equivalent, however.

- The result of boxing avalue of type `Nullable<T>` depends on the `HasValue` property:

  - If `HasValue` is `false`, the result is a `null` reference.
  - If `HasValue` is true, the result is a reference to an object of type “boxed T".

- Calling `GetType` on nullable values leads to surprising results

  ```csharp
    Nullable<int> noValue = new Nullable<int>();
    // Console.WriteLine(noValue.GetType());  => wOULD THROW NULL REFERENCE EXCEPTION
    Nullable<int> someValue = new Nullable<int>(5);
    Console.WriteLine(someValue.GetType());  // Prints System.Int32, the same as you'd typed typeof(int)
  ```

- If you add a `?` to the end of the name of a non-nullable value type, that’s precisely equivalent to using `Nullable<T>` for the same type.

  - Nullable<int> x;
  - Nullable<Int32> x;
  - int? x;
  - Int32? x;

- The `null` literal refers to - either a `null` reference or a value of a nullable value type where `HasValue` is `false`.

  - for value types the following two are equivalent
    - `int? x = new int?();`
    - `int? x = null;`

---

## Automatically implemented Properties

- Prior to C# 3, every property had to be implemented manually with bodies for the get
  and/or set accessors.

```csharp
private string name;
public string Name
{
get { return name; }
set { name = value; }
}
```

- C# 3 made this much simpler by using automatically implemented properties (often
  referred to as automatic properties or even autoprops). These are properties with no
  accessor bodies; the compiler provides the implementation. The whole of the preced-
  ing code can be replaced with a single line:

```csharp
public string Name { get; set; }
```

- Note that there’s no field declaration in the source code now. There’s still a field, but
  it’s created for you automatically by the compiler and given a name that can’t be
  referred to anywhere in the C# code.

---

## Implicit Typing

### Statically typed vs dynamically typed

- Languages that are statically typed are typically compiled languages; the compiler is
  able to determine the type of each expression and check that it’s used correctly.

- Determining the meaning of something like a method call or field access is called binding.

- Languages that are dynamically typed leave all or most of the binding to execution time.

### Explicit vs Implicit Typing

- In a language that’s explicitly typed, the source code specifies all the types involved. This
  could be for local variables, fields, method parameters, or method return types.

- A language that’s implicitly typed allows the developer to omit the types from
  the source code so some other mechanism (whether it’s a compiler or something at
  execution time) can infer which type is meant based on other context.

### Implicitly typed local variables (var)

- Implicitly typed local variables are variables declared with the contextual keyword `var`
  instead of the name of a type, such as the following:

```csharp
var language = "C#";
```

- The way the type is inferred leads to two important rules for implicitly typed local
  variables:
  - The variable must be initialized at the point of declaration.
  - The expression used to initialize the variable must have a type.

### Implicitly typed arrays

- Sometimes you need to create an array without populating it and keep all the ele-
  ments with their default values. The syntax for that hasn’t changed since C# 1; it’s
  always something like this:

```csharp
int[] array = new int[10];
```

- But you often want to create an array with specific initial content. Before C# 3, there
  were two ways of doing this:

```csharp
int[] array1 = { 1, 2, 3, 4, 5};
int[] array2 = new int[] { 1, 2, 3, 4, 5};
```

- The first form of this is valid only when it’s part of a variable declaration that specifies
  the array type. This is invalid, for example:

```csharp
int[] array;
array = { 1, 2, 3, 4, 5 };
```

- The second form is always valid, so the second line in the preceding example could’ve
  been as follows:

```csharp
array = new int[] { 1, 2, 3, 4, 5 };
```

- C# 3 introduced a third form in which the type of the array is implicit based on the
  content:

```csharp
array = new[] { 1, 2, 3, 4, 5 };
```

- This can be used anywhere, so long as the compiler is able to infer the array element
  type from the array elements specified. It also works with multidimensional arrays, as
  in the following example:

```csharp
var array = new[,] { { 1, 2, 3 }, { 4, 5, 6 } };
```

- The next obvious question is how the compiler infers that type. As is so often the case,
  the precise details are complex in order to handle all kinds of corner cases, but the sim-
  plified sequence of steps is as follows:

  1. Find a set of candidate types by considering the type of each array element that
     has a type.
  2. For each candidate type, check whether every array element has an implicit con-
     version to that type. Remove any candidate type that doesn’t meet this condition.
  3. If there’s exactly one type left, that’s the inferred element type, and the com-
     piler creates an appropriate array. Otherwise (if there are no types or more
     than one type left), a compile-time error occurs.

- Example of Rules:

| Expression                      | Result     | Notes                                                                                               |
| ------------------------------- | ---------- | --------------------------------------------------------------------------------------------------- |
| `new[] { 10, 20 }`              | `int[]`    | All elements are of type int.                                                                       |
| `new[] { null, null }`          | Error      | No elements have types.                                                                             |
| `new[] { "xyz", null }`         | `string[]` | Only candidate type is string, and the null literal can be converted to string.                     |
| `new[] { "abc", new object() }` | `object[]` | Candidate types of string and object; implicit conversion from string to object but not vice versa. |
| `new[] { 10, new DateTime() }`  | Error      | Candidate types of `int` and `DateTime` but no conversion from either to the other.                 |
| `new[] { 10, null }`            | Error      | Only candidate type is `int`, but there’s no conversion from `null` to `int`.                       |

---

### Object & Collection initializers

- Creating and populating with object and collection initializers

```csharp
var order = new Order
{
  OrderId = "xyz",
  Customer = new Customer { Name = "Jon", Address = "UK" },
  Items =
    {
      new OrderItem { ItemId = "abcd123", Quantity = 1 },
      new OrderItem { ItemId = "fghi456", Quantity = 2 }
    }
};
```

---

### Anonymous Types

- Created like this

  ```csharp
    var player = new
    {
      Name = "Rajesh",
      Score = 3500
    };
  ```

- A shortcut is _projection initializer_

  ```csharp
    var flattenedItem = new
    {
      order.OrderId,
      CustomerName = customer.Name,
      customer.Address,
      item.ItemId,
      item.Quantity
    };
  ```

- An anonymous type has the following characteristics

- It’s a class (guaranteed).
- Its base class is `object` (guaranteed).
- It’s sealed (not guaranteed, although it would be hard to see how it would be
  useful to make it unsealed).
- The properties are all read-only (guaranteed).
- The constructor parameters have the same names as the properties (not guar-
  anteed; can be useful for reflection occasionally).
- It’s internal to the assembly (not guaranteed; can be irritating when working
  with dynamic typing).
- It overrides `GetHashCode()` and `Equals()` so that two instances are equal only if all their properties are equal. (It handles properties being null.) The fact that these methods are overridden is guaranteed, but the precise way of computing the hash code isn’t.
- It overrides `ToString()` in a helpful way and lists the property names and their values. This isn’t guaranteed, but it is super helpful when diagnosing issues.
- The type is generic with one type parameter for each property. Multiple anonymous types with the same property names but different property types will use different type arguments for the same generic type. This isn’t guaranteed and
  could easily vary by compiler.
- If two anonymous object creation expressions use the same property names in the same order with the same property types in the same assembly, the result is guaranteed to be two objects of the same type.

## Lambda Expressions

- The Compiler converts the Lambda expression to a delegate
- To create a delegate instance from a lambda expression, the compiler converts the code in the lambda expression to a method somewhere. The delegate can then be created at execution time exactly as if you had a method group.

### Captured Variables aka Closure

Within a lambda expression, you can use any variable that you’d be able to use in regular code at that
point. That could be a static field, an instance field (if you’re writing the lambda expression within an instance method 1 ), the `this` variable, method parameters, or local variables. All of these are captured variables, because they’re variables declared outside the immediate context of the lambda expression.

```csharp
class CapturedVariablesDemo
{
  private string instanceField = "instance field";
  public Action<string> CreateAction(string methodParameter)
  {
    string methodLocal = "method local";
    string uncaptured = "uncaptured local";
    Action<string> action = lambdaParameter =>
    {
      string lambdaLocal = "lambda local";
      Console.WriteLine("Instance field: {0}", instanceField);
      Console.WriteLine("Method parameter: {0}", methodParameter);
      Console.WriteLine("Method local: {0}", methodLocal);
      Console.WriteLine("Lambda parameter: {0}", lambdaParameter);
      Console.WriteLine("Lambda local: {0}", lambdaLocal);
    };
    methodLocal = "modified method local";
  return action;
  }
}
```

Normal Closure rules apply here

- `instanceField` is an instance field in the `CapturedVariablesDemo` class and is captured by the lambda expression.
- `methodParameter` is a parameter in the CreateAction method and is cap-
  tured by the lambda expression.
- `methodLocal` is a local variable in the `CreateAction` method and is captured by the lambda expression.
- `uncaptured` is a local variable in the `CreateAction` method, but it’s never used by the lambda expression, so it’s not captured by it.
- `lambdaParameter` is a parameter in the lambda expression itself, so it isn’t a captured variable.
- `lambdaLocal` is a local variable in the lambda expression, so it isn’t a captured variable.

It’s important to understand that the lambda expression captures the variables themselves, not the values of the variables at the point when the delegate is created.

### Implementation of Captured variables with a generated class

- If no variables are captured at all, the compiler can create a static method. No extra context is required.
- If the only variables captured are instance fields, the compiler can create an instance method. Capturing one instance field is equivalent to capturing 100 of them, because you need access only to `this`.
- If local variables or parameters are captured, the compiler creates a private nested class to contain that context and then an instance method in that class containing the lambda expression code. The method containing the lambda expression is changed to use that nested class for every access to the captured variables.

The above code is compiled to:

```csharp
private class LambdaContext  // Generated class to hold the captured variables
{
  // Captured Variables
  public CapturedVariablesDemoImpl originalThis;
  public string methodParameter;
  public string methodLocal;

  // Body of the lambda expression becomes an instance method
  public void Method(string lambdaParameter)
  {
    string lambdaLocal = "lambda local";
    Console.WriteLine("Instance field: {0}",
    originalThis.instanceField);
    Console.WriteLine("Method parameter: {0}", methodParameter);
    Console.WriteLine("Method local: {0}", methodLocal);
    Console.WriteLine("Lambda parameter: {0}", lambdaParameter);
    Console.WriteLine("Lambda local: {0}", lambdaLocal);
  }
}

// Generated class is used for all captured variables
public Action<string> CreateAction(string methodParameter)
{
  LambdaContext context = new LambdaContext();
  context.originalThis = this;
  context.methodParameter = methodParameter;
  context.methodLocal = "method local";
  string uncaptured = "uncaptured local";
  Action<string> action = context.Method;
  context.methodLocal = "modified method local";
  return action;
}

```

## Extension methods

- Extension methods are static methods that can be called as if they’re instance methods, based on their first parameter.

Suppose you have a static method call like this:

`ExampleClass.Method(x, y);`

If you turn ExampleClass.Method into an extension method, you can call it like this instead:

`x.Method(y);`

- Extension methods are declared by adding the keyword this before the first parameter. The method must be declared in a non-nested, nongeneric static class, and until C# 7.2, the first parameter can’t be a ref parameter.

- The type of the first parameter is sometimes called the target of the extension
  method and sometimes called the extended type. (The specification doesn’t give this
  concept a name, unfortunately.)

- Note:
  - if there’s a regular instance method that’s valid
    for the method invocation, the compiler will always prefer that over an extension
    method. It doesn’t matter whether the extension method has “better” parameters; if
    the compiler can use an instance method, it won’t even look for extension methods.
  - After it has exhausted its search for instance methods, the compiler will look for
    extension methods based on the namespace the calling code is in and any using
    directives present.
  - The compiler effectively works its way outward from the deepest namespace out
    toward the global namespace and looks at each step for static classes either in that
    namespace or provided by classes made available by using directives in the namespace
    declaration.
