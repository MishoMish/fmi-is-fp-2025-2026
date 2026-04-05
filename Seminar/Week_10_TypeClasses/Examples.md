# Седмица 9 - Примери

## Пример 1: Ръчни инстанции за геометрични фигури

```haskell
data Shape = Circle Double
           | Rectangle Double Double
           | Triangle Double Double Double

-- Ръчен Show: по-красив от автоматичния
instance Show Shape where
  show (Circle r)        = "Кръг с радиус " ++ show r
  show (Rectangle w h)   = "Правоъгълник " ++ show w ++ "x" ++ show h
  show (Triangle a b c)  = "Триъгълник (" ++ show a ++ ", " ++ show b ++ ", " ++ show c ++ ")"

-- Eq: две фигури са равни ако имат еднакво лице
instance Eq Shape where
  s1 == s2 = abs (area s1 - area s2) < 1e-9

-- Ord: сравняваме по лице
instance Ord Shape where
  compare s1 s2 = compare (area s1) (area s2)

area :: Shape -> Double
area (Circle r)        = pi * r ^ 2
area (Rectangle w h)   = w * h
area (Triangle a b c)  = sqrt (s * (s-a) * (s-b) * (s-c))
  where s = (a + b + c) / 2
```

```haskell
ghci> show (Circle 5)
"Кръг с радиус 5.0"
ghci> Circle 1 == Rectangle 1.7724538509055159 1  -- sqrt(pi) ≈ 1.772
True
ghci> import Data.List (sort)
ghci> sort [Rectangle 4 3, Circle 1, Triangle 3 4 5]
[Кръг с радиус 1.0, Триъгълник (3.0, 4.0, 5.0), Правоъгълник 4.0x3.0]
```

---

## Пример 2: Собствен типов клас `Describable`

```haskell
class Describable a where
  describe :: a -> String

data Animal = Cat String | Dog String | Fish

instance Describable Animal where
  describe (Cat name) = name ++ " е котка"
  describe (Dog name) = name ++ " е куче"
  describe Fish       = "Някаква рибка"

instance Describable Shape where
  describe s = show s ++ " с лице " ++ show (area s)

instance Describable Bool where
  describe True  = "Да"
  describe False = "Не"

-- Полиморфна функция, работеща с всеки Describable
describeAll :: Describable a => [a] -> String
describeAll = unlines . map describe
```

```haskell
ghci> describe (Cat "Мурка")
"Мурка е котка"
ghci> putStrLn $ describeAll [Cat "Мурка", Dog "Шаро", Fish]
Мурка е котка
Шаро е куче
Някаква рибка
```

---

## Пример 3: `Semigroup` и `Monoid` - практически пример

```haskell
-- Semigroup: тип с асоциативна операция (<>)
-- Monoid: Semigroup с неутрален елемент (mempty)

-- Вече инстанцирани:
-- [a]   : (<>) = (++), mempty = []
-- String: (<>) = (++), mempty = ""

-- Собствен: статистики, които могат да се комбинират
data Stats = Stats
  { count :: Int
  , total :: Double
  , maxVal :: Double
  , minVal :: Double
  } deriving (Show)

instance Semigroup Stats where
  s1 <> s2 = Stats
    { count  = count s1 + count s2
    , total  = total s1 + total s2
    , maxVal = max (maxVal s1) (maxVal s2)
    , minVal = min (minVal s1) (minVal s2)
    }

singleStat :: Double -> Stats
singleStat x = Stats 1 x x x

statsAverage :: Stats -> Double
statsAverage s = total s / fromIntegral (count s)

-- Изчисляване от списък
computeStats :: [Double] -> Stats
computeStats []     = Stats 0 0 0 0
computeStats (x:xs) = foldl (\acc v -> acc <> singleStat v) (singleStat x) xs
```

```haskell
ghci> let s = computeStats [3, 1, 4, 1, 5, 9]
ghci> count s
6
ghci> statsAverage s
3.8333333333333335
ghci> maxVal s
9.0
ghci> singleStat 3 <> singleStat 7
Stats {count = 2, total = 10.0, maxVal = 7.0, minVal = 3.0}
```
