# Седмица 2 - Задачи за в час

## Задача 1: `max3`

Напишете функция `max3 :: Int -> Int -> Int -> Int`, която връща максимума от три числа, използвайки **guards**.

```haskell
>>> max3 3 7 2
7
```

---

## Задача 2: `isTriangle`

Напишете функция `isTriangle :: Double -> Double -> Double -> Bool`, която проверява дали три отсечки могат да образуват триъгълник (неравенство на триъгълника).

```haskell
>>> isTriangle 3 4 5
True
>>> isTriangle 1 2 10
False
```

---

## Задача 3: `swap`

Напишете функция `swap :: (a, b) -> (b, a)`, която разменя елементите на двойка.

```haskell
>>> swap (1, 2)
(2, 1)
>>> swap ("hello", True)
(True, "hello")
```

---

## Задача 4: `matchDay`

Напишете функция `isWeekend :: Int -> Bool`, използвайки pattern matching (1 = понеделник, ..., 7 = неделя).

```haskell
>>> isWeekend 6
True
>>> isWeekend 3
False
```

---

## Задача 5: `signum'`

Реимплементирайте `signum` с **guards** (без `if-then-else`):

```haskell
signum' :: Int -> Int
```

```haskell
>>> signum' 5
1
>>> signum' 0
0
>>> signum' (-3)
-1
```
