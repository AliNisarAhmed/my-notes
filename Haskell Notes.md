# Haskell Notes

### Lambda Calculus

\x.x is called an abstraction
x in itself is a variable
x is also an expression

the fact that \x.x === \y.y is called **Alpha Equivalence**

The process of application of a function is called **Beta Reduction**

e.g.

```haskell

(\x.x)(\y.y)z
[x :== \y.y] -> substituting
(\y.y)z
z

```

_Free Variables_ -> e.g. \x.xy (y is a free variable, not in the 'head' of the lambda expression)

Alpha Equivalence does not apply to free variables e.g. \x.xz and \x.xy are not equivalent as x and y might be different things, however, \xy.yx and \ab.ba hold alpha equivalence

##### Beta Normal Form

When you cannot beta reduce the terms any further (cannot apply lambdas to arguments)

##### Combinators

are lambda terms with no free variables
λy.x
Here y is bound (it occurs in the head of the lambda) but x is free.

Below are Combinators
λxy.x
λxyz.xz(yz)

## Lazy Evaluation

In the expression `isOdd (1 + 2)` for the function `isOdd n = mod n 2 == 1`, the `(1 + 2)` part is not evaluated, unlike strict languages.

It is evaluated only when needed, before that, we create a “promise” that when the value of the expression `isOdd (1 + 2)` is needed, we'll be able to compute it. The record that we use to track an unevaluated expression is referred to as a _thunk_. This is all that happens: we create a thunk, and defer the actual evaluation until it's really needed.

### `seq` function

- The `foldl` function is not the only place where space leaks can arise in Haskell code.

- We refer to an expression that is not evaluated lazily as strict, so `foldl'` is a strict left fold. It bypasses Haskell's usual non-strict evaluation through the use of a special function named `seq`.

  ```haskell
    seq :: a -> t -> t
  ```

- `seq` forces the first argument to be evalated, then returns the second argument.

- This avoids the creation of **thunks** in `foldl'`

- Some rules for use of `seq`

  - To have any effect, a seq expression must be the first thing evaluated in an expression.

  ```haskell
    -- incorrect: seq is hidden by the application of someFunc
    -- since someFunc will be evaluated first, seq may occur too late
    hiddenInside x y = someFunc (x `seq` y)

    -- incorrect: a variation of the above mistake
    hiddenByLet x y z = let a = x `seq` someFunc y
      in anotherFunc a z

    -- correct: seq will be evaluated first, forcing evaluation of x
      onTheOutside x y = x `seq` someFunc y
  ```

  - To strictly evaluate several values, chain applications of `seq` together.

  ```haskell
    chained x y z = x `seq` y `seq` someFunc z
  ```

  - A common mistake is to try to use `seq` with two unrelated expressions.

  ```haskell
    badExpression step zero (x:xs) =
    seq (step zero x)
        (badExpression step (step zero x) xs)

    -- Here, the apparent intention is to evaluate step zero x strictly. Since the expression is duplicated in the body of the function, strictly evaluating the first instance of it will have no effect on the second. The use of let from the definition of foldl' above shows how to achieve this effect correctly.
  ```

  - When evaluating an expression, `seq` stops as soon as it reaches a constructor. For simple types like numbers, this means that it will evaluate them completely. Algebraic data types are a different story. Consider the value `(1+2):(3+4):[]`. If we apply `seq` to this, it will evaluate the `(1+2)` thunk. Since it will stop when it reaches the first `(:)` constructor, it will have no effect on the second thunk. The same is true for tuples: `seq ((1+2),(3+4)) True` will do nothing to the thunks inside the pair, since it immediately hits the pair's constructor.

---

## Selected Numeric Types

<details>

  <summary>Click Here to show diagram</summary>

![Numeric Types](./images/numericTypes.png)

</details>

---

## Selected Numeric Functions and Constants

<details>

  <summary>Click Here to show diagram</summary>

![Selected Numeric Functions](./images/numericFunctions.png)
![Selected Numeric Functions](./images/numericFunctions2.png)

</details>

---

## Typeclass Instances for Numeric Types

<details>

  <summary>Click Here to show diagram</summary>

![Instances](./images/typclassInstances.png)

</details>

---

## Conversion between Numeric Types

<details>

  <summary>Click Here to show diagram</summary>

![Conversion](./images/conversion.png)

</details>

---

## Langugae Extensions

- Language extensions are enabled in this manner `{-# LANGUAGE FlexibleInstances #-}`
- Read more [here](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html)

| Extension            | Explanation                                                                                                                                                                                                                                                                                                                                                                               |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TypeSynonymInstances | Allow definition of type class instances for type synonyms. like `instance TypeClass String where`                                                                                                                                                                                                                                                                                        |
| FlexibleInstances    | implies `TypeSynonymInstances`. Allow definition of type class instances with arbitrary nested types in the instance head                                                                                                                                                                                                                                                                 |
| OverlappingInstances | **Deprecated**, but allows to define instances of more particular types if two type instances match, e.g. `[Char]` and `[a]`. Deprectaed in favor or specifying the overlap behavior for individual instances with a pragma, written immediately after the instance keyword. The pragma may be one of: {-# OVERLAPPING #-}, {-# OVERLAPPABLE #-}, {-# OVERLAPS #-}, or {-# INCOHERENT #-} |

- Related notes on Langugae extensions

  - ### Why use `FlexibleInstances`

    First: Synonyms are not allowed.
    Second: We should take a closer look at the `[Char]` type.
    `[]` is a type: `data [] a = [] | a : [a] -- Defined in ‘GHC.Types’`
    Char is a type: data Char = GHC.Types.C# GHC.Prim.Char# -- Defined in ‘GHC.Types’

    But `[Char]` is not a type defined by 'data'. In fact, it is an instance of type `[] a`, which is obtained by replacing the type variable `a` with `Char`. In fact, this "replacing" is what Haskell does not allowed.

  - ### Example:

    given the following type and typeclass:

    ```haskell
    data (Num a) => Vector a = Vector a a
    deriving (Show)

    class MyClass a where
    myFun :: a -> a
    ```

    it seems perfectly ok to define (without the need of any pragma)

    ```haskell
    instance MyClass (Vector a) where
    myFun = id

    myFun (Vector 1 2) :: Vector Int gives Vector 1 2, myFun (Vector 1 2) :: Vector Double gives Vector 1.0 2.0 instead.
    ```

    but if you try to define

    ```haskell
    instance MyClass (Vector Int) where etc…
    instance MyClass (Vector Double) where etc…
    ```

    perhaps because you want to handle those types of vectors differently - you will need the `FlexibleInstances` pragma. and if you try to work around this problem by introducing two new type synonyms

    ```haskell
    type VectorInt = Vector Int
    type VectorDouble = Vector Double
    ```

    and using them in the instance declaration

    `instance MyClass VectorInt where` etc…

    you will need the TypeSynonymInstances pragma instead.

## Functions

- In Haskell, function application is left associative. This is best illustrated by example: the expression `a b c d` is equivalent to `(((a b) c) d)`

To use Infix Operators as prefix (like 1 + 1), use parens => (+) 1 1 // 2
To use prefix operators as infix (div 3 2), use backticks => 3 `div` 2

`div` is integer division
`mod` is remainder operator

| Operator | Name      | Purpose/application                   |
| :------- | --------- | ------------------------------------- |
| \+       | plus      | addition                              |
| \-       | minus     | subtraction                           |
| /        | slash     | fractional division                   |
| div      | divide    | integral division                     |
| mod      | modulo    | like 'rem' but after modular div      |
| quot     | quotient  | integral division, round towards zero |
| rem      | remainder | remainder after division              |

#### Laws for quotients and remainders

(quot x y)*y + (rem x y) == x
(div x y)*y + (mod x y) == x

###### Difference between `rem` and `mod`

One key difference here is that, in Haskell (not in all languages), if
one or both arguments are negative, the results of mod will have the
same sign as the divisor, while the result of rem will have the same
sign as the dividend:

```haskell
(-5) `mod` 2  => 1
5 `mod` (-2) => -1
(-5) `mod` (-2) => -1
mod (-9) 7 => 5

(-5) `rem` 2 => -1
5 `rem` (-2) => 1
(-5) `rem` (-2) => -1
rem (-9) 7 => -2
```

##### `$` Operator

here’s the definition of (`$`):
f $ a = f a
Immediately this seems a bit pointless until we remember that it’s defined as an infix operator with the lowest possible precedence.
The ($) operator is a convenience for when you want to express something with fewer pairs of parentheses:

`(2^)(2 + 2)` -> `(2^) $ 2 + 2`

Writing expressions like (*30) is called *sectioning*. (*30) is a function in itself

With commutative functions, such as addition, it makes no difference if you use (+1) or (1+) because the order of the arguments won’t change the result.
If you use sectioning with a function that is not commutative, the order matters:

`(1/) 2` => 0.5

`(/1) 2` => 2.0

`2 - 1` => 1

`(-) 2 1` => 1

`(-2) 1` => Error, (-2) is being used as a unary negation

`(2 -) 1` => 1, this is ok

---

#### List Comprehensions

[ Output function | drawing from, condition1, condition2 ...]
e.g.

```haskell
[x * 2 | x <- [1..10], x >= 4]
-- Only those x, which are >= 4, will be chosen
```

---

#### `fromIntegral`

```haskell
fromIntegral :: (Num b, Integral a) => a -> b
-- converts an Integral type (Int or Int) to generic Num type
-- Num includes (Int, Integer, Float, Double)
```

#### TypeClasses

**Integral** -> includes `Int`, `Integer` and `Word` (Whole numbers)
**Fractional** -> `Float`, `Double`, `Rational`, `Fixed`, `Scientific`
**Num** includes all the above types

---

#### Sectioning

The term sectioning specifically refers to partial application of infix operators, which has a special syntax and allows you to choose whether the argument you’re partially applying the operator to is the first or second argument.

```haskell
(+ 1) /= (1 +)
```

---

#### Partial Application

A prefix function can be partially applied two ways

```haskell
-- First
elem :: a -> [a] -> Bool
x = (elem 9)
x [1, 2, 3]  -- False
x [1..10]  -- True

-- Second
y = (`elem` [1..10])
y 9  -- True
y 20 -- False

-- Hence, a prefix function can be reverse by making it infix
-- and partially applying it

```

---

#### Polymorphism

type signatures may have three kinds of types: **concrete**, **constrained polymorphic** (aka ad-hoc polymorphism), or **parametrically polymorphic**.

If a variable represents a set of possible values, then a type variable represents a set of possible types. When there is no type class constraint, the set of possible types a variable could represent is effectively unlimited. Type class constraints limit the set of potential types (and, thus, potential values) while also passing along the common functions that can be used with those values.

In sum, if a variable could be anything, then there’s little that can be done to it because it has no methods. If it can be some types (say, a type that has an instance of `Num`), then it has some methods. If it is a concrete type, you lose the type flexibility but, due to the additive nature of type class inheritance, gain more potential methods. It’s important to note that this inheritance extends downward from a superclass, such as `Num`, to subclasses, such as `Integral` and then `Int`, but not the other way around. That is, if something has an instance of `Num` but not an instance of `Integral`, it can’t implement the methods of the `Integral` type class. A subclass cannot override the methods of its superclass.

A function is polymorphic when its type signature has variables that can represent more than one type. That is, its parameters are polymorphic. **Parametric polymorphism** refers to fully polymorphic (unconstrained by a type class) parameters. Parametricity is the property we get from having parametric polymorphism. **Parametricity** means that the behavior of a function with respect to the types of its (parametrically polymorphic) arguments is uniform. The behavior can not change just because it was applied to an argument of a different type.

#### Partial Functions

A partial function is one that doesn’t handle all the possible cases, so there are possible scenarios in which we haven’t defined any way for the code to evaluate.

We need to take care to avoid partial functions in general in Haskell, but this must be especially kept in mind when we have a type with multiple cases

#### List Comprehensions

List Comprehensions must have at least one list, called the generator, that gives the input for the comprehension, that is, provides the set of items from which the new list will be constructed. They may have conditions to determine which elements are drawn from the list and/or functions applied to those elements.

```haskell
  [ x ^ 2 | x <- [1..4] ] -- [1, 4, 9, 16]
```

#### Weak Head Normal Form

Values in Haskell get reduced to weak head normal form by default. By ‘normal form’ we mean that the expression is fully evaluated. ‘Weak head normal form’ means the expression is only evaluated as far as is necessary to reach a data constructor.
