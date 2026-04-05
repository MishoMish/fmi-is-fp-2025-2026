# Седмица 9 - Задачи за в час

## Задача 1: `Eq` и `Show` за `Fraction`

Дефинирайте типа `Fraction` и напишете ръчни инстанции:

```haskell
data Fraction = Frac Int Int

instance Eq Fraction where ...    -- сравнение след опростяване
instance Show Fraction where ...  -- "3/4"
```

```haskell
>>> Frac 1 2 == Frac 2 4
True
>>> show (Frac 6 8)
"3/4"
```

---

## Задача 2: `Printable`

Дефинирайте типов клас `Printable` с метод `prettyShow :: a -> String`.

Напишете инстанции за:

- `Int` - число като текст
- `Bool` - `"yes"` / `"no"`
- `[a]` (ако `a` е `Printable`) - елементите, разделени със запетая

```haskell
>>> prettyShow (42 :: Int)
"42"
>>> prettyShow True
"yes"
>>> prettyShow [1 :: Int, 2, 3]
"1, 2, 3"
```

---

## Задача 3: `Ord` за `Card`

Дефинирайте ръчна `Ord` инстанция за карти за игра, където картите се сравняват **първо по ранг, после по боя**:

```haskell
data Suit = Clubs | Diamonds | Hearts | Spades
  deriving (Eq, Ord, Show)

data Rank = Two | Three | Four | Five | Six | Seven | Eight
          | Nine | Ten | Jack | Queen | King | Ace
  deriving (Eq, Ord, Show, Enum, Bounded)

data Card = Card Rank Suit
  deriving (Eq, Show)

instance Ord Card where ...
```

```haskell
>>> Card Ace Spades > Card King Hearts
True
>>> Card Ten Clubs < Card Ten Spades
True
>>> import Data.List (sort)
>>> sort [Card King Hearts, Card Ace Clubs, Card King Diamonds]
[Card King Diamonds, Card King Hearts, Card Ace Clubs]
```

---

## Задача 4: `Measurable`

Дефинирайте типов клас:

```haskell
class Measurable a where
  size :: a -> Int
```

Напишете инстанции за:

- `String` - дължина
- `[a]` - брой елементи
- `Maybe a` - 0 за `Nothing`, 1 за `Just`

---

## Задача 5: `Vector2D`

```haskell
data Vector2D = Vec2D Double Double
```

Напишете инстанции за `Eq`, `Show`, `Num`:

```haskell
>>> Vec2D 1 2 + Vec2D 3 4
(4.0, 6.0)
>>> Vec2D 1 2 * Vec2D 3 4   -- покомпонентно
(3.0, 8.0)
>>> negate (Vec2D 1 2)
(-1.0, -2.0)
```
