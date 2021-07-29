# Parametric vs Ad-hoc Polymorphism

For me there is three different concepts involved, all of them serve different purpose.

* Parametric polymorphism.
    
* Ad-hoc polymorphism, i.e. typeclass.
    
* Genericity, i.e. GHC.Generics
    

I'll give you examples of what you can do with them, and their limitations and why we need the others.

Imagine you want to write a function which swap the two values of a tuple, something like:

    swap :: (a, b) -> (b, a)
    swap (v0, v1) = (v1, v0)
    

You want this function to work on any type `a` and `b`, but you really don't care about `a` and `b`. They can be anything, it will work. This is called _Parametric polymorphism_ and you use it when you are able to write a function which is generic enough to work on any type with no limitations. This is what is known in Java as Generics, or in C++ as templates.

Now, suppose you want to write a function which compute the area of something. You cannot write a function which works on any type and returns its area, for two reasons:

* Not all types have an area, what is the area of a `String`?
    
* I don't know any mathematical function able to compute the area of all kind of existing shapes
    

So we need a way to write an unique `area` function which accepts any type for which computing an area is possible and we need to provide a specific implementation for each case. This is _ad-hoc_ polymorphism and _typeclass_ are doing this job in Haskell.

For example, suppose we have theses types:

    data Square = Square Float
    data Rectangle = Rectangle Float Float
    

We can define a typeclass `Area` which, for any `t` which is an instance of the class, is able to compute the area:

    class Area t where
           area :: t -> Float
    

Actually, the type of `area` is in fact `area :: Area t => t -> Float`, which say that the function accept any `t` as long as `t` is an instance of `Area`.

And now, for each instance of the class, we can provide an implementation:

    instance Area Square where
      area (Square s) = s * s
    
    instance Area Rectangle where
       area (Rectangle a b) = a * b
    

This is `ad-hoc` polymorphism. In C++ this is implemented using template specialization or function overloading. It is usually a good idea to ensure that your instances follows some rules. For example, an area is supposed to be positive, so it is usually a good idea to ensure that all your instances returns positives value.

You can do pretty much everything with theses two kind of polymorphism. Two rules :

* If you can write a function which does not care about the input type and does not need any knowledge about them, then you can use parametric polymorphism and write one generic implementation for any type.
    
* Else, use ad-hoc polymorphism, but you'll need one implementation for each type.
    

Sometime, you'll meet a weird polymorphism problem. You have a function which works for any type, but with a different implementation, but you can write an algorithm to define these implementation. You don't want to write all theses instances by hand, there is a solution: `GHC.Generics` and friends.

You may already have seen generic polymorphism, the `Show`, `Ord`, `Eq` typeclasses that you can "inherit" for free:

    Prelude> data Square = Square Float deriving (Show, Ord, Eq)
    Prelude> show (Square 5)
    "Square 5.0"
    Prelude> Square 5 < Square 2
    False
    Prelude> Square 5 == Square 5
    True
    

For all these typeclass you can write an algorithm: `Show` writes the constructor name followed by the different values. `Ord` compare arguments of the constructor in a lexicographic order. `Eq` checks if all arguments of the constructor are equals.

So, to summarize:

* If you can write a function which does not depends on any knowledge of the input types, use parametric polymorphism.
    
* If you need to write a specific implementation for each type, use ad-hoc polymorphism (typeclass)
    
* If you know a way to automatize the specific implementation writing for your ad-hoc polymorphism, you want to use `GHC.Generics` (or any other generic tool).