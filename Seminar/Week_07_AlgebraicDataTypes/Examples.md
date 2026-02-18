# Седмица 7 - Примери

## Пример 1: Моделиране на геометрични фигури

```haskell
data Shape = Circle Double
           | Rectangle Double Double
           | Triangle Double Double Double
  deriving (Show)

area :: Shape -> Double
area (Circle r)        = pi * r ^ 2
area (Rectangle w h)   = w * h
area (Triangle a b c)  = sqrt (s * (s-a) * (s-b) * (s-c))
  where s = (a + b + c) / 2

isValid :: Shape -> Bool
isValid (Circle r)        = r > 0
isValid (Rectangle w h)   = w > 0 && h > 0
isValid (Triangle a b c)  = a > 0 && b > 0 && c > 0
                            && a + b > c && a + c > b && b + c > a

-- Мащабиране
scale :: Double -> Shape -> Shape
scale k (Circle r)        = Circle (r * k)
scale k (Rectangle w h)   = Rectangle (w * k) (h * k)
scale k (Triangle a b c)  = Triangle (a * k) (b * k) (c * k)

-- Най-голямата фигура от списък
largest :: [Shape] -> Shape
largest = foldl1 (\acc s -> if area s > area acc then s else acc)
```

```haskell
ghci> area (Circle 5)
78.53981633974483
ghci> isValid (Triangle 1 2 10)
False
ghci> scale 2 (Rectangle 3 4)
Rectangle 6.0 8.0
ghci> largest [Circle 1, Rectangle 3 4, Triangle 3 4 5]
Rectangle 3.0 4.0
```

---

## Пример 2: Свързан списък от нулата

```haskell
data MyList a = Nil | Cons a (MyList a)
  deriving (Show)

-- Конвертиране от/към стандартен списък
fromList :: [a] -> MyList a
fromList []     = Nil
fromList (x:xs) = Cons x (fromList xs)

toList :: MyList a -> [a]
toList Nil         = []
toList (Cons x xs) = x : toList xs

myLength :: MyList a -> Int
myLength Nil         = 0
myLength (Cons _ xs) = 1 + myLength xs

myAppend :: MyList a -> MyList a -> MyList a
myAppend Nil         ys = ys
myAppend (Cons x xs) ys = Cons x (myAppend xs ys)

myMap :: (a -> b) -> MyList a -> MyList b
myMap _ Nil         = Nil
myMap f (Cons x xs) = Cons (f x) (myMap f xs)
```

```haskell
ghci> let xs = fromList [1, 2, 3]
ghci> myLength xs
3
ghci> toList (myMap (*2) xs)
[2, 4, 6]
ghci> toList (myAppend (fromList [1,2]) (fromList [3,4]))
[1, 2, 3, 4]
```

---

## Пример 3: Records за студентска система

```haskell
data Student = Student
  { name  :: String
  , fn    :: Int
  , grade :: Double
  } deriving (Show, Eq)

-- Създаване
students :: [Student]
students =
  [ Student "Иван Петров"  12345 5.5
  , Student "Мария Иванова" 12346 6.0
  , Student "Петър Стоянов" 12347 3.5
  , Student "Елена Димитрова" 12348 5.0
  ]

-- Филтриране
passing :: [Student] -> [Student]
passing = filter (\s -> grade s >= 4.0)

-- Средна оценка
averageGrade :: [Student] -> Double
averageGrade ss = sum (map grade ss) / fromIntegral (length ss)

-- Намиране по ФН
findByFn :: Int -> [Student] -> Maybe Student
findByFn _ [] = Nothing
findByFn targetFn (s:ss)
  | fn s == targetFn = Just s
  | otherwise        = findByFn targetFn ss

-- Обновяване на оценка по ФН
updateGrade :: Int -> Double -> [Student] -> [Student]
updateGrade targetFn newGrade = map update
  where
    update s
      | fn s == targetFn = s { grade = newGrade }
      | otherwise        = s
```

```haskell
ghci> map name (passing students)
["Иван Петров","Мария Иванова","Елена Димитрова"]
ghci> averageGrade students
5.0
ghci> findByFn 12346 students
Just (Student {name = "Мария Иванова", fn = 12346, grade = 6.0})
```
