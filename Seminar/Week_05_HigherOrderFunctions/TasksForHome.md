# Седмица 5 — Задачи за домашна работа

## Задача 1: `mapFoldr` и `filterFoldr`

Реализирайте `map` и `filter` чрез `foldr`:

```haskell
mapFoldr :: (a -> b) -> [a] -> [b]
filterFoldr :: (a -> Bool) -> [a] -> [a]
```

---

## Задача 2: `zipWith'`

Реимплементирайте `zipWith`:

```haskell
zipWith' :: (a -> b -> c) -> [a] -> [b] -> [c]
```

```haskell
>>> zipWith' (+) [1,2,3] [10,20,30]
[11,22,33]
>>> zipWith' max [1,5,3] [4,2,6]
[4,5,6]
```

---

## Задача 3: `takeWhile'` и `dropWhile'`

```haskell
takeWhile' :: (a -> Bool) -> [a] -> [a]
dropWhile' :: (a -> Bool) -> [a] -> [a]
```

```haskell
>>> takeWhile' (<5) [1, 3, 5, 2, 4]
[1, 3]
>>> dropWhile' (<5) [1, 3, 5, 2, 4]
[5, 2, 4]
```

---

## Задача 4: `maximumBy'`

Напишете `maximumBy' :: (a -> a -> Ordering) -> [a] -> a`, която намира максимума спрямо дадена сравняваща функция.

```haskell
>>> maximumBy' compare [3, 1, 4, 1, 5]
5
>>> maximumBy' (\x y -> compare (length x) (length y)) ["hi", "hello", "yo"]
"hello"
```

---

## Задача 5: `groupBy'`

Напишете `groupBy' :: (a -> a -> Bool) -> [a] -> [[a]]`, която групира **последователни** елементи по даден предикат.

```haskell
>>> groupBy' (==) [1,1,2,2,2,3,3,1]
[[1,1],[2,2,2],[3,3],[1]]
>>> groupBy' (<) [1,3,5,2,4,6]
[[1,3,5],[2,4,6]]
```

---

## Задача 6: `any'`, `all'`

Реализирайте `any` и `all` чрез `foldr`:

```haskell
any' :: (a -> Bool) -> [a] -> Bool
all' :: (a -> Bool) -> [a] -> Bool
```

```haskell
>>> any' even [1, 3, 5, 4]
True
>>> all' even [2, 4, 6]
True
```

---

## 🌟 Бонус: `concatMap'`

Реализирайте `concatMap` чрез `foldr`:

```haskell
concatMap' :: (a -> [b]) -> [a] -> [b]
```

```haskell
>>> concatMap' (\x -> [x, x*10]) [1, 2, 3]
[1, 10, 2, 20, 3, 30]
```
