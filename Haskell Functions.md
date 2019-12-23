# Haskell Functions

```haskell
(++) :: [a] -> [a] -> [a]
concat :: [[a]] -> [a]
take :: Int -> [a] -> [a]

head :: [a] -> a  -- unsafe: Error on empty list
-- So keep in mind the diff b/w head and take
head [1, 2, 3] -- 1
take 1 [1, 2, 3]  -- [1]
drop 2 [1, 2, 3] -- [3]

takeWhile :: (a -> Bool) -> [a] -> [a]
dropWhile :: (a -> Bool) -> [a] -> [a]

-- takeWhile / DropWhile the condition is True, stop as soon as the condition is False

succ :: Enum a => a -> a  -- returns the successor
max :: Ord a => a -> a -> a  -- max 5 4 -> 5
min :: Ord a => a -> a -> a

[3, 2, 1] > [2, 1, 0]  -- True

tail [1, 2, 3, 4] -- [2, 3, 4]  -- unsafe

last [1, 2, 3] -- 3  -- unsafe

init [1, 2, 3, 4]  -- [1, 2, 3]  -- unsafe

length :: [a] -> Int
length [1, 2, 3]  -- 3

null :: [a] -> Bool
null []  -- True
null [2]  -- False

break :: (a -> Bool) -> [a] -> ([a], [a])  -- breaks exactly where the function first returns True
break even [1, 3, 5, 6, 8, 9, 10]  -- ([1, 3, 5], [6, 8, 9, 10])

span :: (a -> Bool) -> [a] -> ([a], [a])  -- opposite of break, just remember break


reverse [1, 2, 3]  -- [3, 2, 1]

maximum :: (Foldable t, Ord a) => t a -> a
maximum [1, 2, 3, 4, 5]  -- 5
minimum [1, 2, 3, 4, 5]  -- 1

sum [1, 2, 3]  -- 6
product [1, 2, 3, 4]  -- 24

elem :: a -> [a] -> Bool
elem 2 [1, 2, 3]  -- True
2 `elem` [1, 2, 3]  -- True

notElem :: a -> [a] -> Bool
notElem 2 [1, 3, 4] -- True

cycle [1, 2, 3]  -- [1, 2, 3, 1, 2, 3, 1, 2, 3...]
repeat 5  -- [5, 5, 5, 5, 5...]

replicate :: Int -> a -> [a]
replicate 3 10   -- [10, 10, 10]

zip :: [a] -> [b] -> [(a, b)]
zip [1, 2, 3] [4, 5, 6]  -- [(1, 4), (2, 5), (3, 6)]

intersperse :: a -> [a] -> [a]
intersperse '.' "MONKEY" -- "M.O.N.K.E.Y"

enumFromThenTo 1 3 10  -- [1, 3, 5, 7, 9]
enumFromTo 1 5 -- [1, 2, 3, 4, 5]
enumFrom 1  -- [1, 2, 3...] Infinite List possible

splitAt :: Int -> [a] -> ([a], [a])
splitAt 2 "samrah"  -- ("sa", "mrah")

foldr :: (a -> b -> b) -> b -> [a] -> b
-- or
foldr :: (x -> acc -> acc) -> acc(init) -> [x] -> acc

foldl :: (b -> a -> b) -> b -> [a] -> b
-- or
foldl :: (acc -> x -> acc) -> acc(init) -> [x] -> acc

all :: Foldable t => (a -> Bool) -> t a -> Bool
-- or
-- func applies to ALL elements of list == True
all :: (a -> Bool) -> [a] -> Bool

lines :: String -> [String]  -- separates on "\n" newline
words :: String -> [String]  -- separates on both spaces & "\n"

```
