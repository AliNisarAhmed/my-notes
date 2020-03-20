# C# Interview questions

## Difference between Class and Struct

- Common answer is class are a Reference types stored on the heap, while structs are value types stored on the stack.
- Structs implement the semantic of copying by value
- Classes implement the semantci of copying by reference.

#### NOTE

- Structs are value types which means they are copied when they are passed around.

- So if you change a copy you are changing only that copy, not the original and not any other copies which might be around.

- If your struct is immutable then all automatic copies resulting from being passed by value will be the same.

- If you want to change it you have to consciously do it by creating a new instance of the struct with the modified data. (not a copy)

**Mutable Structs are PURE EVIL**

READ MORE [here](https://docs.microsoft.com/en-us/archive/blogs/ericlippert/mutating-readonly-structs)

```csharp
struct Mutable
{
    private int x;
    public int Mutate()
    {
      this.x = this.x + 1;
      return this.x;
    }
}

class Test
{
    public readonly Mutable m = new Mutable();
    static void Main(string[] args)
    {
        Test t = new Test();
        System.Console.WriteLine(t.m.Mutate());  // 1
        System.Console.WriteLine(t.m.Mutate());  // 1
        System.Console.WriteLine(t.m.Mutate());  // 1 ??? WTF ???
    }
}
```
