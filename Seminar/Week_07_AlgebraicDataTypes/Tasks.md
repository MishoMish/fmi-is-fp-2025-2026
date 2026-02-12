# Седмица 7 - Задачи за в час

## Задача 1: `Shape`

Дефинирайте тип `Shape` с конструктори `Circle Double`, `Rectangle Double Double` и `Triangle Double Double Double`.

Напишете:

- `area :: Shape -> Double` - лице
- `perimeter :: Shape -> Double` - обиколка

```haskell
>>> area (Circle 5)
78.53981633974483
>>> perimeter (Rectangle 3 4)
14.0
```

---

## Задача 2: `safeHead` и `safeLast`

Напишете безопасни версии на `head` и `last`, които връщат `Maybe`:

```haskell
safeHead :: [a] -> Maybe a
safeLast :: [a] -> Maybe a
```

```haskell
>>> safeHead [1, 2, 3]
Just 1
>>> safeHead ([] :: [Int])
Nothing
```

---

## Задача 3: `Expr`

Дефинирайте тип за прости аритметични изрази:

```haskell
data Expr = Lit Double
          | Add Expr Expr
          | Mul Expr Expr
          | Neg Expr
```

Напишете `eval :: Expr -> Double`:

```haskell
>>> eval (Add (Lit 3) (Mul (Lit 2) (Lit 5)))
13.0
>>> eval (Neg (Add (Lit 1) (Lit 2)))
-3.0
```

---

## Задача 4: `Weekday`

Дефинирайте тип `Weekday` с `deriving (Show, Eq, Ord, Enum, Bounded)`.

Напишете:

- `isWeekend :: Weekday -> Bool`
- `nextDay :: Weekday -> Weekday` (неделя → понеделник)
- `daysUntilWeekend :: Weekday -> Int`

---

## Задача 5: `Fraction`

Дефинирайте тип `Fraction` за рационални числа:

```haskell
data Fraction = Frac Int Int  -- числител, знаменател
```

Напишете:

- `simplify :: Fraction -> Fraction`
- `addFrac :: Fraction -> Fraction -> Fraction`
- `mulFrac :: Fraction -> Fraction -> Fraction`

```haskell
>>> simplify (Frac 6 8)
Frac 3 4
>>> addFrac (Frac 1 3) (Frac 1 6)
Frac 1 2
```
