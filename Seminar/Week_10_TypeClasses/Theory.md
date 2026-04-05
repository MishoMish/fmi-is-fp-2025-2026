# Типови класове

## Въведение

**Типов клас** (type class) е механизъм за **ad-hoc полиморфизъм** в Haskell - позволява една и съща функция да работи по различен начин за различни типове. Аналогия: интерфейси в Java/C#.

---

## 1. Стандартни типови класове

### `Eq` - равенство

```haskell
class Eq a where
  (==) :: a -> a -> Bool
  (/=) :: a -> a -> Bool
  x /= y = not (x == y)    -- дефиниция по подразбиране
```

### `Ord` - наредба

```haskell
class Eq a => Ord a where
  compare :: a -> a -> Ordering  -- LT | EQ | GT
  (<), (<=), (>), (>=) :: a -> a -> Bool
  max, min :: a -> a -> a
```

> 💡 `Eq a => Ord a` означава: за да има `Ord`, трябва да има и `Eq` (суперклас).

### `Show` - преобразуване в `String`

```haskell
class Show a where
  show :: a -> String
```

### `Read` - четене от `String`

```haskell
class Read a where
  read :: String -> a    -- опростено
```

```haskell
ghci> show 42              -- "42"
ghci> show [1,2,3]         -- "[1,2,3]"
ghci> read "42" :: Int     -- 42
ghci> read "[1,2]" :: [Int]  -- [1,2]
```

---

## 2. Дефиниране на инстанция

За наш тип, не можем да използваме `==` автоматично - трябва да дефинираме инстанция:

```haskell
data Color = Red | Green | Blue

instance Eq Color where
  Red   == Red   = True
  Green == Green = True
  Blue  == Blue  = True
  _     == _     = False

instance Show Color where
  show Red   = "Червено"
  show Green = "Зелено"
  show Blue  = "Синьо"

instance Ord Color where
  compare Red   Red   = EQ
  compare Red   _     = LT
  compare _     Red   = GT
  compare Green Green = EQ
  compare Green Blue  = LT
  compare Blue  Green = GT
  compare Blue  Blue  = EQ
```

```haskell
ghci> Red == Blue    -- False
ghci> show Green     -- "Зелено"
ghci> Red < Blue     -- True
ghci> sort [Blue, Red, Green]  -- [Red, Green, Blue]
```

---

## 3. `deriving` - автоматично генериране

Вместо ръчно, Haskell може да генерира инстанции автоматично:

```haskell
data Color = Red | Green | Blue
  deriving (Eq, Ord, Show, Read, Enum, Bounded)
```

Автоматичното `Ord` сравнява по **реда на деклариране** (`Red < Green < Blue`).

---

## 4. Инстанции за типове с параметри

```haskell
data Pair a b = Pair a b

instance (Show a, Show b) => Show (Pair a b) where
  show (Pair x y) = "(" ++ show x ++ ", " ++ show y ++ ")"

instance (Eq a, Eq b) => Eq (Pair a b) where
  Pair a1 b1 == Pair a2 b2 = a1 == a2 && b1 == b2
```

> 💡 Контекстът `(Show a, Show b) =>` казва: за да покажем `Pair a b`, `a` и `b` трябва да са `Show`.

---

## 5. Дефиниране на собствен типов клас

```haskell
class Container f where
  empty  :: f a
  insert :: a -> f a -> f a
  toList :: f a -> [a]
```

```haskell
instance Container [] where
  empty   = []
  insert  = (:)
  toList  = id

data Stack a = Stack [a] deriving (Show)

instance Container Stack where
  empty              = Stack []
  insert x (Stack xs) = Stack (x:xs)
  toList (Stack xs)   = xs
```

---

## 6. Полезни типови класове от Prelude

| Клас         | Ключови функции                     | Предназначение          |
| ------------ | ----------------------------------- | ----------------------- |
| `Eq`         | `==`, `/=`                          | Сравняване за равенство |
| `Ord`        | `compare`, `<`, `>`, `max`, `min`   | Наредба                 |
| `Show`       | `show`                              | Преобразуване до String |
| `Read`       | `read`, `reads`                     | Четене от String        |
| `Num`        | `+`, `-`, `*`, `abs`, `fromInteger` | Числа                   |
| `Integral`   | `div`, `mod`, `toInteger`           | Цели числа              |
| `Fractional` | `/`, `fromRational`                 | Дробни числа            |
| `Enum`       | `succ`, `pred`, `[..]`              | Изброими типове         |
| `Bounded`    | `minBound`, `maxBound`              | Ограничени типове       |

### Йерархия

```
          Eq
          |
         Ord
        /   \
     Num    Enum
      |       |
  Integral  Bounded
      |
  Fractional
```

---

## 7. Ограничения в типови сигнатури

```haskell
-- Изисква Eq
elem' :: Eq a => a -> [a] -> Bool
elem' _ []     = False
elem' x (y:xs) = x == y || elem' x xs

-- Изисква Ord
sort :: Ord a => [a] -> [a]

-- Множество ограничения
showAndCompare :: (Show a, Ord a) => a -> a -> String
showAndCompare x y = show x ++ " vs " ++ show y ++ ": " ++ result
  where result = case compare x y of
                   LT -> "по-малко"
                   EQ -> "равно"
                   GT -> "по-голямо"
```

---

## Обобщение

| Концепция          | Синтаксис                   | Пример                               |
| ------------------ | --------------------------- | ------------------------------------ |
| Дефиниране на клас | `class C a where ...`       | `class Printable a where ...`        |
| Инстанция          | `instance C Type where ...` | `instance Eq Color where ...`        |
| Автоматично        | `deriving (...)`            | `deriving (Eq, Ord, Show)`           |
| Ограничение        | `C a =>`                    | `Eq a => a -> [a] -> Bool`           |
| Суперклас          | `class A a => B a`          | `class Eq a => Ord a`                |
| Контекст за инст.  | `instance (C a) => D (T a)` | `instance Show a => Show (MyList a)` |
