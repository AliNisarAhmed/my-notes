# Intuitions to remember

## Composition of Binary functions

- It acts a little strange, the first argument if passed to the first thing after (.), and then the result of that plus the second argument is passed to the second function.

```haskell

-- mapMaybe :: (a -> b) -> Maybe a -> Maybe b

bindMaybe :: (a -> Maybe b) -> Maybe a -> Maybe b
bindMaybe f Nothing = Nothing
bindMaybe f (Just x) = f x

applyMaybe :: Maybe (a -> b) -> Maybe a -> Maybe b
applyMaybe f a = bindMaybe (\f' -> mapMaybe f' a) f

twiceMaybe :: (a -> b -> c) -> Maybe a -> Maybe b -> Maybe c
twiceMaybe f = applyMaybe . mapMaybe f
-- which can also be written as
twiceMaye f a b = applyMaybe (mapMaybe f a) b

```

- So, above we can see that the composition has the following effect
  - first argument `a :: Maybe a`, get the function `f` mapped onto it, resulting in `x :: Maybe (b -> c)`.
  - then `x` and `b` both get supplied to applyMaybe
  - applyMaybe here will have the type `Maybe (b -> c) -> Maybe b -> Maybe c`

---

## Functor: map a constant value

- We can replace a value inside of a Functor with a const value by mapping with `const value` function, like this

```haskell

(<$) :: Functor k => b -> k a -> k b
(<$) v fx = (<$>) (const v) fx
-- or
(<$) v = (<$>) (const v)

--- or even more succintly

(<$) = (<$>) . const

--- >>>

--- 7 <$ (1 : 2 : 3 : []) =
--- [7, 7, 7]

```
