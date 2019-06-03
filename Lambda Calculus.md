# Lambda Calculus

As explained in this **GREAT** [YouTube](https://www.youtube.com/watch?v=3VQ382QG-y4) video.

#### Abstractions
```
\a.b      a => b
\a.b x    a => b(x)
\a.(b x)  a => b(x)
(\a.b)x   (a => b)(x)
\a.\b.a   a => b => a

```

### Combinators

#### MockingBird

```
\f.ff
or 
M = f => f(f)

I = a => a (Indentity function)

M(I) = I(I) = I
M(M) = error (it diverges)  MM = MM = MM ... 

(This particular divergent term (MM) is called Omega Combinator) (sometimes Mockingbird is called little Omega)

There is no single algorithm that will show us whether 
an given lambda expression diverges or not => This is 
called the Halting Problem (By Alan Turing)

```

#### Kestrel - K Combinator
```
\ab.a
K = a => b => a

K(I)(M) = I
K(K)(M) = K

in Haskell this is called const
Why is it called const?
because 
let f = K(5)
f(1) = 5
f(2) = 5
f(any) = 5, hence the name, const

consider: 
K I x = I which is a function, hence we can apply it
K I x y = I y 
K I x y = I y = y
K I x y = y
now, (K I) becomes a function that takes two arguments, x & y, and returns the second.
Hence we have derived the Kite Combinator from the K-Combinator!!!
```

#### Kite Combinator - KI Combinator
```
\ab.a
KI = a => b => b

KI(M)(KI) -> KI
KI(M)(K) -> K
KI(K)(M) -> K

KI is also known as first, in haskell written as (const id)

```

#### Cardinal
```
\fab.fba  (flips the arguments)
C = f => a => b => f(b)(a)

C K I M = K M I = M (returning the second argument)

hence C K = K I (Cardinal of Kestrel is Kestral of Identity)

in Haskell, known as flip

```

---

Since Lambda Calculus and Turing Machines are equivalent, we can represent EVERYTHING in Lambda Calculus.

Let's start with `True` & `False`

###### True and False

True  x y = x
False x y = y

True and False are used as "Choice" "Selectors"

So True returns the first arg, and False returns the second... which means

```
True  === K
False === KI
```

###### NOT

To Implement the `NOT` function, we notice that it takes a Boolean, and returns its opposite, So, `not` will be a function, when given two Boolean values `F` and `T`, will choose its own opposite, and we have already seen which functions specifically do the choosing stuff in Lambda Calc. they are `K` and `KI`, hence not can be written as

```
not = \p.pFT
not = f => f(F)(T)

if given K  (ie True),  then, \K.KFT => F
if given KI (ie False), then, \K.KFT => T

so

not K = KI
not KI = K

or in Lambda Terms

not \ab.a = \ba.a
not \ba.a = \ab.a

Or, in other words, we can use the C combinator (flip)

C K = KI
C KI = K

hence C is equivalent to Boolean not (F***ing amazing!)
```

###### AND

`AND` will be function that will take two Boolean args

\f(K)(K) or \f(KI)(K) or \f(K)(KI) or \f(KI)(KI)

so, since we are passing the args, we should use them inside the function

```
AND = \pq.p??

In the above, if p is False (which means it is KI, since KI is equivalent to F), then it returns False, w/o checking on q

hence 

AND = \pq.p?F

if p is True (which means it is K), the result of the expression then depends on what p is, if it is F, we return F, if it is T, then we return T

hence

AND = \pq.pqF

or more generally

AND = \pq.pqp

explanation: if p is F, we return F, which is second arg in pqp
             if p is T, we return whatever q is
```

###### OR

Similar to reasoning above, we can derive OR

```
OR = \pq.ppq

But

(\pq.ppq)xy = xxy = (Mx)y = Mxy

So, OR Function is extensionally equivalent to M*, that is, tey behave similarly.
```

###### Exclusive OR (equivalent to Boolean Equality)
```
\pq.p (q T F) (q F T)

applying it

\TT. T (F T F) () -> T
\FF. F () (F F T) -> T
\TF. T (T F T) () -> F
\FT. F () (T F T) -> F

Hence, when both inputs are T or both are F, it gives T, otherwise F.
In other words, it tells us when both inputs are same
hence it is equivalent to Boolean Equality

In \pq.p (q T F) () when p is T, q when F selects q, when T selects q, which makes it redundant, we can replace it with q only

\pq.pq(q F T)
now in (q F T), we know that p is F (that is why we have reached the second option) so, if q is F, it returns T, and if q is T, it returns F. Hence (q F T) is just NOT q

therefore

\pq.pq(not q) is xor/boolean equality

\pq.p (q p q) (q p q)

```

#### Numbers in Lambda Calculus

Numbers can be represented as application of a function to an argument, n times, where n is the number

so

```haskell
zero f a = a -- zero is False/KI, returns the second value as is.
one f a = f a  -- One is ID, takes two things and returns them
two f a = (f . f) a -- or f $ f $ a
three f a = (f . f . f) a
```
These are called Church Encodings and the numbers are called Church Numerals.

***A Church numeral `n` takes in a function and an arg and applies the function on the arg `n` times.***

But, in all the examples above, we are hard coding the numbers, we want a way to generate these numbers dynamically.

We need a `succ` function, the function that takes a number and returns its successor.

A `succ` function will take the original number of function application, and apply the function once more, to get the next number.

so

```haskell
succ (\f a -> f a) = (\f a -> f (f a))
succ (\f a -> (f . f) a) = (\f a -> (f . f . f) a) 

-- so successor takes a number function, 
-- and applies the function one more than its predecessor to get the successor.

succ n f a = f $ n f a -- where n is a church numeral

let three = succ two
three (+1) 2 = 5
```

To visualize Church Numeral in Haskell, we can make a function like this

```haskell
jsnum n = n (\x -> x + 1) 0

jsnum one -- 1
jsnum (succ one) -- 2
```

#### The BlueBird Combinator `\f g a -> f g a` aka Function Composition

B f g a = f(g a)

`succ` from above can be defined in terms of function composition

`succ n f = f . (n f) -- point free`

#### Addition in Lambda Calc using Church Numerals and Composition

How do we define an add function, what does an add function do?

It takes two Church numerals

```haskell
add n1 n5 f = succ n5 $ f
-- or simply
add n1 n5 = succ n5
add n2 n5 = succ . succ $ n5
add n3 n5 = (succ . succ . succ) n5

-- or we can use one church numeral to apply the composition

add n k = n succ k
-- explanation: what does a church numeral n do? 
-- it applies a function n times
-- so here, in definition of add, n is applying succ to k n times
```

### Multiplication in Lambda Calc

Multiplication between two numbers `n` and `k` just means that we want to apply a function `nxk` times. So

```haskell
Mult n2 n3 f a = (f . f . f . f . f . f) a
-- or
Mult n2 n3 f a = ((f . f . f) . (f . f .f )) a
```
Hence, multiplication of 2 and 3 is "thrice f" or "three-fold" compositions, composed twice or composed "two-fold".

```haskell
-- Since a is common, we can drop it
Mult n2 n3 f = ( (f . f . f) . (f . f . f) ) 

-- But notice, n3 itself is a function applied three times, so
Mult n2 n3 f = (n3 f) . (n3 f)
-- hence the function (n3 . f) is being applied twice
-- which is exactly what n2 does, it applies a function twice.
Mult n2 n3 f = n2 (n3 f)
-- or
Mult n2 n3 f = (n2 . n3) f
-- f is common, so we can drop it.
Mult n2 n3 = (n2 . n3)
-- or in terms of the B combinator
Mult n2 n3 = B n2 n3
-- but now n2 and n3 are common, we can drop those.
Mult = B
```
-- **!!!! Hence Multiplication of two numbers is their Composition !!!!**

so 
`\n k f -> n(k f)`

#### Exponentiation

We know that...
```haskell
Pow n2 n3 = n2 X n2 X n2
-- But Mult is just B
Pow n2 n3 = n2 . n2 . n2
Pow n2 n3 = n3 n2  -- n2 applied 3 times.
--or
pow = \n k -> k n
-- Power takes two args and flips them around
```

This is called Thrush Combinator

#### Thrush Combinator

\ a f -> f a

Thrush Combinator can also be written as T = CI (Cardinal if Id)
