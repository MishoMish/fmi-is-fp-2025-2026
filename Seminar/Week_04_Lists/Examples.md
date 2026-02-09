# Седмица 4 — Примери

## Пример 1: Реимплементация на стандартни функции

```haskell
myLength :: [a] -> Int
myLength []     = 0
myLength (_:xs) = 1 + myLength xs

myReverse :: [a] -> [a]
myReverse []     = []
myReverse (x:xs) = myReverse xs ++ [x]

myConcat :: [[a]] -> [a]
myConcat []       = []
myConcat (xs:xss) = xs ++ myConcat xss

myTake :: Int -> [a] -> [a]
myTake _ []     = []
myTake 0 _      = []
myTake n (x:xs) = x : myTake (n - 1) xs

myDrop :: Int -> [a] -> [a]
myDrop _ []     = []
myDrop 0 xs     = xs
myDrop n (_:xs) = myDrop (n - 1) xs
```

```haskell
ghci> myReverse [1,2,3]
[3,2,1]
ghci> myConcat [[1,2],[3],[4,5]]
[1,2,3,4,5]
ghci> myTake 2 [10,20,30,40]
[10,20]
```

---

## Пример 2: Операции с проверки

```haskell
-- Проверка дали списък е сортиран
isSorted :: Ord a => [a] -> Bool
isSorted []       = True
isSorted [_]      = True
isSorted (x:y:xs) = x <= y && isSorted (y:xs)

-- Вмъкване в сортиран списък
sortedInsert :: Ord a => a -> [a] -> [a]
sortedInsert x [] = [x]
sortedInsert x (y:ys)
  | x <= y    = x : y : ys
  | otherwise = y : sortedInsert x ys

-- Insertion sort
insertionSort :: Ord a => [a] -> [a]
insertionSort []     = []
insertionSort (x:xs) = sortedInsert x (insertionSort xs)
```

```haskell
ghci> isSorted [1, 3, 5, 7]
True
ghci> sortedInsert 4 [1, 3, 5, 7]
[1,3,4,5,7]
ghci> insertionSort [5, 2, 8, 1, 4]
[1,2,4,5,8]
```

---

## Пример 3: Zip и работа с двойки

```haskell
-- Скаларно произведение
dotProduct :: [Double] -> [Double] -> Double
dotProduct xs ys = sum (zipWith (*) xs ys)

-- Индексиране (номериране) на елементи
enumerate :: [a] -> [(Int, a)]
enumerate xs = zip [0..] xs

-- Намиране на позицията на елемент
indexOf :: Eq a => a -> [a] -> Int
indexOf x xs = head [i | (i, y) <- enumerate xs, y == x]
```

```haskell
ghci> dotProduct [1, 2, 3] [4, 5, 6]
32.0
ghci> enumerate ["a", "b", "c"]
[(0,"a"),(1,"b"),(2,"c")]
ghci> indexOf 'l' "hello"
2
```

> 💡 `zip [0..] xs` — `[0..]` е безкраен списък, но `zip` спира когато по-късият списък свърши. Мързеливото оценяване в действие!
