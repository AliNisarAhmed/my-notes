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

*Free Variables* -> e.g. \x.xy (y is a free variable, not in the 'head' of the lambda expression)

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

## Functions

To use Infix Operators as prefix (like 1 + 1), use parens => (+) 1 1  // 2
To use prefix operators as infix (div 3 2), use backticks => 3 `div` 2

`div` is integer division
`mod` is remainder operator

| Operator | Name      | Purpose/application                   |
|:-------- | --------- | ------------------------------------- |
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
