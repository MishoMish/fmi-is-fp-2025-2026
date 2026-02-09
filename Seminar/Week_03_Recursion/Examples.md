# Седмица 3 — Примери

## Пример 1: Рекурсия по числа

```haskell
-- Степенуване
power :: Int -> Int -> Int
power _ 0 = 1
power x n = x * power x (n - 1)

-- Бързо степенуване O(log n)
fastPow :: Int -> Int -> Int
fastPow _ 0 = 1
fastPow x n
  | even n    = half * half
  | otherwise = x * fastPow x (n - 1)
  where half = fastPow x (n `div` 2)

-- Сума на делителите
sumDivisors :: Int -> Int
sumDivisors n = helper 1
  where
    helper i
      | i > n        = 0
      | n `mod` i == 0 = i + helper (i + 1)
      | otherwise      = helper (i + 1)
```

```haskell
ghci> power 2 10
1024
ghci> fastPow 2 10
1024
ghci> sumDivisors 12
28
```

---

## Пример 2: Рекурсия по списъци

```haskell
-- Максимален елемент
myMaximum :: [Int] -> Int
myMaximum [x]    = x
myMaximum (x:xs) = max x (myMaximum xs)

-- Принадлежност
myElem :: Eq a => a -> [a] -> Bool
myElem _ []     = False
myElem e (x:xs) = e == x || myElem e xs

-- Обръщане (с акумулатор)
myReverse :: [a] -> [a]
myReverse xs = helper [] xs
  where
    helper acc []     = acc
    helper acc (x:xs) = helper (x : acc) xs
```

```haskell
ghci> myMaximum [3, 1, 7, 2]
7
ghci> myElem 3 [1, 2, 3, 4]
True
ghci> myReverse [1, 2, 3]
[3, 2, 1]
```

> 💡 `myReverse` е tail-recursive благодарение на акумулатора `acc`.

---

## Пример 3: Fibonacci — три варианта

```haskell
-- 1. Наивна рекурсия (експоненциална!)
fib :: Int -> Int
fib 0 = 0
fib 1 = 1
fib n = fib (n - 1) + fib (n - 2)

-- 2. С акумулатор (линейна, tail-recursive)
fibIter :: Int -> Int
fibIter n = helper 0 1 n
  where
    helper a _ 0 = a
    helper a b n = helper b (a + b) (n - 1)

-- 3. Чрез двойки
fibPair :: Int -> Int
fibPair n = fst (helper n)
  where
    helper 0 = (0, 1)
    helper n = let (a, b) = helper (n - 1)
               in (b, a + b)
```

```haskell
ghci> fibIter 30
832040
ghci> fib 30
832040  -- но МНОГО по-бавно!
```
