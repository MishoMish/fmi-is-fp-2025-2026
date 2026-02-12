# Седмица 4 - Задачи за в час

## Задача 1: `myReverse`

Реимплементирайте `reverse`:

```haskell
myReverse :: [a] -> [a]
```

```haskell
>>> myReverse [1, 2, 3]
[3, 2, 1]
```

---

## Задача 2: `myZip`

Реимплементирайте `zip`:

```haskell
myZip :: [a] -> [b] -> [(a, b)]
```

```haskell
>>> myZip [1, 2] ["a", "b", "c"]
[(1,"a"), (2,"b")]
```

---

## Задача 3: `myConcat`

Реимплементирайте `concat`:

```haskell
myConcat :: [[a]] -> [a]
```

```haskell
>>> myConcat [[1,2], [3], [4,5]]
[1,2,3,4,5]
```

---

## Задача 4: `isSorted`

Напишете `isSorted :: Ord a => [a] -> Bool`, която проверява дали списък е сортиран във възходящ ред.

```haskell
>>> isSorted [1, 3, 5]
True
>>> isSorted [1, 5, 3]
False
```

---

## Задача 5: `removeAt`

Напишете `removeAt :: Int -> [a] -> [a]`, която премахва елемента на позиция `n` (от 0).

```haskell
>>> removeAt 1 [1, 2, 3, 4]
[1, 3, 4]
```
