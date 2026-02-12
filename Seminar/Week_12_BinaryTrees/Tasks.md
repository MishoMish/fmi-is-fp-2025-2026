# Седмица 12 - Задачи за в час

## Задача 1: Основни операции

Дефинирайте `data Tree a = Empty | Node (Tree a) a (Tree a)` и реализирайте:

```haskell
treeSize    :: Tree a -> Int
treeHeight  :: Tree a -> Int
treeSum     :: Num a => Tree a -> a
countLeaves :: Tree a -> Int
```

---

## Задача 2: Обхождания

Реализирайте:

```haskell
inorder   :: Tree a -> [a]
preorder  :: Tree a -> [a]
postorder :: Tree a -> [a]
```

Тествайте с:

```haskell
--       5
--      / \
--     3   7
--    / \   \
--   1   4   9
```

```haskell
>>> inorder tree
[1,3,4,5,7,9]
```

---

## Задача 3: BST операции

Реализирайте:

```haskell
bstInsert :: Ord a => a -> Tree a -> Tree a
bstSearch :: Ord a => a -> Tree a -> Bool
fromList  :: Ord a => [a] -> Tree a
```

```haskell
>>> inorder (fromList [5, 3, 7, 1, 4])
[1,3,4,5,7]
>>> bstSearch 4 (fromList [5, 3, 7, 1, 4])
True
```

---

## Задача 4: `treeMap`

Реализирайте:

```haskell
treeMap :: (a -> b) -> Tree a -> Tree b
```

```haskell
>>> inorder (treeMap (*2) (fromList [1,2,3]))
[2,4,6]
>>> inorder (treeMap show (fromList [1,2,3]))
["1","2","3"]
```

---

## Задача 5: `treePaths`

Напишете функция `treePaths :: Tree a -> [[a]]`, която връща всички пътища от корена до листата:

```haskell
>>> treePaths (fromList [5, 3, 7, 1, 4, 9])
[[5,3,1],[5,3,4],[5,7,9]]
```
