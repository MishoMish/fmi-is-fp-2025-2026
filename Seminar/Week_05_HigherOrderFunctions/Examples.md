# Седмица 5 — Примери

## Пример 1: Реимплементация на `map` и `filter`

```haskell
myMap :: (a -> b) -> [a] -> [b]
myMap _ []     = []
myMap f (x:xs) = f x : myMap f xs

myFilter :: (a -> Bool) -> [a] -> [a]
myFilter _ []     = []
myFilter p (x:xs)
  | p x       = x : myFilter p xs
  | otherwise = myFilter p xs
```

```haskell
ghci> myMap (*3) [1, 2, 3]
[3, 6, 9]
ghci> myFilter odd [1..10]
[1, 3, 5, 7, 9]
```

---

## Пример 2: Изразяване на функции чрез `foldr`

Много стандартни функции могат да се изразят чрез `foldr`:

```haskell
-- sum чрез foldr
mySum :: Num a => [a] -> a
mySum = foldr (+) 0

-- product чрез foldr
myProduct :: Num a => [a] -> a
myProduct = foldr (*) 1

-- length чрез foldr
myLength :: [a] -> Int
myLength = foldr (\_ acc -> acc + 1) 0

-- reverse чрез foldl
myReverse :: [a] -> [a]
myReverse = foldl (\acc x -> x : acc) []
-- или по-кратко:
-- myReverse = foldl (flip (:)) []

-- map чрез foldr
mapViaFoldr :: (a -> b) -> [a] -> [b]
mapViaFoldr f = foldr (\x acc -> f x : acc) []

-- filter чрез foldr
filterViaFoldr :: (a -> Bool) -> [a] -> [a]
filterViaFoldr p = foldr (\x acc -> if p x then x : acc else acc) []
```

```haskell
ghci> mySum [1..100]
5050
ghci> myLength "hello"
5
ghci> myReverse [1, 2, 3]
[3, 2, 1]
```

---

## Пример 3: Практическа обработка на данни

```haskell
type Student = (String, [Int])  -- (име, [оценки])

students :: [Student]
students =
  [ ("Иван",    [5, 6, 4, 5])
  , ("Мария",   [6, 6, 6, 5])
  , ("Петър",   [3, 4, 3, 2])
  , ("Елена",   [5, 5, 6, 6])
  ]

-- Средна оценка на студент
average :: [Int] -> Double
average xs = fromIntegral (sum xs) / fromIntegral (length xs)

-- Имена на студенти със средна оценка над 5.0
goodStudents :: [Student] -> [String]
goodStudents = map fst . filter (\(_, grades) -> average grades >= 5.0)

-- Максимална оценка на студент
maxGrade :: Student -> Int
maxGrade (_, grades) = maximum grades

-- Всички максимални оценки
allMaxGrades :: [Student] -> [Int]
allMaxGrades = map maxGrade
```

```haskell
ghci> goodStudents students
["Иван","Мария","Елена"]
ghci> allMaxGrades students
[6, 6, 4, 6]
ghci> map (\(name, gs) -> (name, average gs)) students
[("Иван",5.0),("Мария",5.75),("Петър",3.0),("Елена",5.5)]
```
