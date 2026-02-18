# Седмица 2 - Примери

## Пример 1: Pattern matching по числа

```haskell
fibonacci :: Int -> Int
fibonacci 0 = 0
fibonacci 1 = 1
fibonacci n = fibonacci (n - 1) + fibonacci (n - 2)

factorial :: Int -> Int
factorial 0 = 1
factorial n = n * factorial (n - 1)
```

```haskell
ghci> fibonacci 7
13
ghci> factorial 5
120
```

> 💡 `fibonacci 0` и `fibonacci 1` са базови случаи - без тях рекурсията никога няма да спре.

---

## Пример 2: Guards за класификация

```haskell
grade :: Int -> String
grade score
  | score >= 90 = "Отличен (6)"
  | score >= 75 = "Много добър (5)"
  | score >= 60 = "Добър (4)"
  | score >= 50 = "Среден (3)"
  | otherwise   = "Слаб (2)"

temperatureDesc :: Double -> String
temperatureDesc temp
  | temp < 0    = "Замръзване"
  | temp < 15   = "Студено"
  | temp < 25   = "Приятно"
  | temp < 35   = "Горещо"
  | otherwise   = "Екстремно горещо"
```

---

## Пример 3: Работа с кортежи

```haskell
-- Разстояние между две точки
distance :: (Double, Double) -> (Double, Double) -> Double
distance (x1, y1) (x2, y2) = sqrt ((x2 - x1)^2 + (y2 - y1)^2)

-- Средна точка
midpoint :: (Double, Double) -> (Double, Double) -> (Double, Double)
midpoint (x1, y1) (x2, y2) = ((x1 + x2) / 2, (y1 + y2) / 2)

-- Аритметика на двойки
addVec :: (Double, Double) -> (Double, Double) -> (Double, Double)
addVec (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)

scaleVec :: Double -> (Double, Double) -> (Double, Double)
scaleVec k (x, y) = (k * x, k * y)
```

```haskell
ghci> distance (0, 0) (3, 4)
5.0
ghci> midpoint (0, 0) (10, 10)
(5.0, 5.0)
ghci> scaleVec 3 (1, 2)
(3.0, 6.0)
```

---

## Пример 4: Комбиниране на patterns и guards

```haskell
-- Описание на число
describeNumber :: Int -> String
describeNumber 0 = "Нула"
describeNumber n
  | n > 0 && even n = "Положително четно"
  | n > 0           = "Положително нечетно"
  | even n           = "Отрицателно четно"
  | otherwise        = "Отрицателно нечетно"
```

```haskell
ghci> describeNumber 0
"Нула"
ghci> describeNumber 4
"Положително четно"
ghci> describeNumber (-3)
"Отрицателно нечетно"
```
