# Алгебрични типове данни

## Въведение

В Haskell можем да дефинираме свои собствени типове чрез ключовата дума `data`. Тези типове се наричат **алгебрични типове данни** (Algebraic Data Types — ADTs).

---

## 1. Изброени типове (Enumerations)

```haskell
data Color = Red | Green | Blue

data Direction = North | South | East | West

data Day = Mon | Tue | Wed | Thu | Fri | Sat | Sun
```

> 💡 `Red`, `Green`, `Blue` са **конструктори на данни** (data constructors). Те са стойности от тип `Color`.

### Pattern matching

```haskell
colorToString :: Color -> String
colorToString Red   = "Червено"
colorToString Green = "Зелено"
colorToString Blue  = "Синьо"

isWeekend :: Day -> Bool
isWeekend Sat = True
isWeekend Sun = True
isWeekend _   = False
```

---

## 2. Конструктори с параметри

Конструкторите могат да носят данни:

```haskell
data Shape = Circle Double              -- радиус
           | Rectangle Double Double    -- ширина, височина
           | Triangle Double Double Double  -- три страни
```

```haskell
area :: Shape -> Double
area (Circle r)        = pi * r ^ 2
area (Rectangle w h)   = w * h
area (Triangle a b c)  = sqrt (s * (s-a) * (s-b) * (s-c))
  where s = (a + b + c) / 2

perimeter :: Shape -> Double
perimeter (Circle r)        = 2 * pi * r
perimeter (Rectangle w h)   = 2 * (w + h)
perimeter (Triangle a b c)  = a + b + c
```

```haskell
ghci> area (Circle 5)
78.53981633974483
ghci> area (Rectangle 3 4)
12.0
ghci> perimeter (Triangle 3 4 5)
12.0
```

---

## 3. Type synonyms (`type`)

`type` създава **алтернативно име** за съществуващ тип (не е нов тип):

```haskell
type Name   = String
type Age    = Int
type Point  = (Double, Double)
type Matrix = [[Double]]

distance :: Point -> Point -> Double
distance (x1, y1) (x2, y2) = sqrt ((x2-x1)^2 + (y2-y1)^2)
```

> ⚠️ `type` НЕ създава нов тип — `Name` и `String` са напълно взаимозаменяеми.

---

## 4. `newtype`

`newtype` създава **нов тип** с точно един конструктор и едно поле. По-ефективен е от `data`:

```haskell
newtype Meters = Meters Double
newtype Seconds = Seconds Double

speed :: Meters -> Seconds -> Double
speed (Meters m) (Seconds s) = m / s
```

> 💡 `newtype` е полезен за type safety — не можем случайно да смесим `Meters` и `Seconds`.

---

## 5. Record syntax

За типове с много полета, record syntax автоматично генерира getter функции:

```haskell
data Student = Student
  { studentName  :: String
  , studentAge   :: Int
  , studentGrade :: Double
  }

-- Автоматично генерирани:
-- studentName  :: Student -> String
-- studentAge   :: Student -> Int
-- studentGrade :: Student -> Double
```

```haskell
ivan :: Student
ivan = Student { studentName = "Иван", studentAge = 20, studentGrade = 5.5 }

-- Или позиционно:
maria :: Student
maria = Student "Мария" 21 6.0

-- Достъп до полета:
ghci> studentName ivan    -- "Иван"
ghci> studentGrade maria  -- 6.0

-- Обновяване с record update:
ivan' :: Student
ivan' = ivan { studentGrade = 5.75 }
```

---

## 6. Полиморфни типове

Типовете могат да имат **типови параметри**:

```haskell
data Pair a b = Pair a b

data List a = Empty | Cons a (List a)

data Maybe a = Nothing | Just a   -- вграден в Prelude

data Either a b = Left a | Right b  -- вграден в Prelude
```

```haskell
safeDivide :: Double -> Double -> Maybe Double
safeDivide _ 0 = Nothing
safeDivide x y = Just (x / y)
```

```haskell
ghci> safeDivide 10 3
Just 3.3333333333333335
ghci> safeDivide 10 0
Nothing
```

---

## 7. `deriving`

Haskell може автоматично да генерира инстанции на стандартни типови класове:

```haskell
data Color = Red | Green | Blue
  deriving (Show, Eq, Ord, Enum, Bounded)
```

```haskell
ghci> show Red       -- "Red"
ghci> Red == Blue    -- False
ghci> Red < Blue     -- True  (по реда на деклариране)
ghci> [Red ..]       -- [Red, Green, Blue]
ghci> minBound :: Color  -- Red
```

| Клас | Какво генерира |
|------|----------------|
| `Show` | Преобразуване до String |
| `Read` | Четене от String |
| `Eq` | `==` и `/=` |
| `Ord` | `<`, `>`, `compare` |
| `Enum` | `succ`, `pred`, `[..]` |
| `Bounded` | `minBound`, `maxBound` |

---

## Обобщение

| Концепция | Синтаксис | Пример |
|-----------|-----------|--------|
| Изброен тип | `data T = A \| B \| C` | `data Color = Red \| Green \| Blue` |
| Конструктор с данни | `data T = C Type1 Type2` | `data Shape = Circle Double` |
| Type synonym | `type T = ExistingType` | `type Name = String` |
| Newtype | `newtype T = C Type` | `newtype Meters = Meters Double` |
| Record | `data T = T { field :: Type }` | getter функции |
| Полиморфен тип | `data T a = ...` | `data Maybe a = Nothing \| Just a` |
| Deriving | `deriving (Show, Eq, ...)` | автоматични инстанции |
