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
