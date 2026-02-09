# Седмица 6 — Примери

## Пример 1: Комбиниране на HOFs с ламбди и композиция

```haskell
-- Сума на квадратите на нечетните числа до n
sumOddSquares :: Int -> Int
sumOddSquares n = sum . map (^2) . filter odd $ [1..n]

-- Или чрез comprehension:
sumOddSquares' :: Int -> Int
sumOddSquares' n = sum [x^2 | x <- [1..n], odd x]

-- Брой думи с дължина > k
countLongWords :: Int -> String -> Int
countLongWords k = length . filter (> k) . map length . words

-- Или:
countLongWords' :: Int -> String -> Int
countLongWords' k s = length [w | w <- words s, length w > k]
```

```haskell
ghci> sumOddSquares 10
165
ghci> countLongWords 3 "the quick brown fox jumps"
3
```

---

## Пример 2: List comprehensions за комбинаторни задачи

```haskell
-- Всички Питагорови тройки до n
pythagorean :: Int -> [(Int, Int, Int)]
pythagorean n = [(a, b, c) | c <- [1..n]
                            , b <- [1..c]
                            , a <- [1..b]
                            , a^2 + b^2 == c^2]

-- Декартово произведение
cartesian :: [a] -> [b] -> [(a, b)]
cartesian xs ys = [(x, y) | x <- xs, y <- ys]

-- Пермутации на двуелементни подмножества
pairs :: [a] -> [(a, a)]
pairs xs = [(x, y) | (i, x) <- zip [0..] xs
                    , (j, y) <- zip [0..] xs
                    , i < j]
```

```haskell
ghci> pythagorean 20
[(3,4,5),(5,12,13),(6,8,10),(8,15,17),(9,12,15)]
ghci> cartesian [1,2] ["a","b"]
[(1,"a"),(1,"b"),(2,"a"),(2,"b")]
ghci> pairs [1,2,3,4]
[(1,2),(1,3),(1,4),(2,3),(2,4),(3,4)]
```

---

## Пример 3: Обработка на текстови данни

```haskell
import Data.Char (toUpper, toLower, isAlpha, isDigit)

-- Капитализиране на първата буква на всяка дума
capitalize :: String -> String
capitalize = unwords . map capitalizeWord . words
  where
    capitalizeWord []     = []
    capitalizeWord (c:cs) = toUpper c : map toLower cs

-- Броене на символи по вид
charStats :: String -> (Int, Int, Int)  -- (букви, цифри, други)
charStats s = (letters, digits, others)
  where
    letters = length $ filter isAlpha s
    digits  = length $ filter isDigit s
    others  = length s - letters - digits

-- Caesar cipher
caesarEncrypt :: Int -> String -> String
caesarEncrypt shift = map (\c -> if isAlpha c then shiftChar c else c)
  where
    shiftChar c
      | c >= 'a' && c <= 'z' = toEnum $ (fromEnum c - fromEnum 'a' + shift) `mod` 26 + fromEnum 'a'
      | c >= 'A' && c <= 'Z' = toEnum $ (fromEnum c - fromEnum 'A' + shift) `mod` 26 + fromEnum 'A'
      | otherwise             = c
```

```haskell
ghci> capitalize "hello world from haskell"
"Hello World From Haskell"
ghci> charStats "Hello, World! 123"
(10,3,4)
ghci> caesarEncrypt 3 "hello"
"khoor"
```
