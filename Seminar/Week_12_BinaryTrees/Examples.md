# Седмица 12 — Примери

## Пример 1: Визуализация на дърво

```haskell
data Tree a = Empty | Node (Tree a) a (Tree a)
  deriving (Eq)

-- Pretty-print на дърво (ротирано на 90°)
showTree :: Show a => Tree a -> String
showTree = go 0
  where
    go _ Empty = ""
    go depth (Node l x r) =
      go (depth + 1) r ++
      replicate (depth * 4) ' ' ++ show x ++ "\n" ++
      go (depth + 1) l

printTree :: Show a => Tree a -> IO ()
printTree = putStr . showTree

-- Пример:
--       5
--      / \
--     3   7
--    / \   \
--   1   4   9
exTree :: Tree Int
exTree = Node
  (Node (Node Empty 1 Empty) 3 (Node Empty 4 Empty))
  5
  (Node Empty 7 (Node Empty 9 Empty))
```

```haskell
ghci> printTree exTree
        9
    7
5
        4
    3
        1
```

---

## Пример 2: BST от нулата с всички операции

```haskell
-- Конструиране
bstInsert :: Ord a => a -> Tree a -> Tree a
bstInsert x Empty = Node Empty x Empty
bstInsert x (Node l v r)
  | x < v     = Node (bstInsert x l) v r
  | x > v     = Node l v (bstInsert x r)
  | otherwise  = Node l v r

fromList :: Ord a => [a] -> Tree a
fromList = foldl (flip bstInsert) Empty

-- Сортиране чрез BST
treeSort :: Ord a => [a] -> [a]
treeSort = inorder . fromList

inorder :: Tree a -> [a]
inorder Empty          = []
inorder (Node l x r)   = inorder l ++ [x] ++ inorder r

-- Намиране на k-тия най-малък елемент
kthSmallest :: Int -> Tree a -> Maybe a
kthSmallest k tree
  | k < 1 || k > length elems = Nothing
  | otherwise                  = Just (elems !! (k - 1))
  where elems = inorder tree

-- Проверка дали е валидно BST
isBST :: Ord a => Tree a -> Bool
isBST tree = isSorted (inorder tree)
  where
    isSorted []       = True
    isSorted [_]      = True
    isSorted (x:y:xs) = x < y && isSorted (y:xs)
```

```haskell
ghci> let bst = fromList [5, 3, 7, 1, 4, 9, 2, 8]
ghci> treeSort [5, 3, 7, 1, 4, 9, 2, 8]
[1,2,3,4,5,7,8,9]
ghci> kthSmallest 3 bst
Just 3
ghci> isBST bst
True
```

---

## Пример 3: Дървета на изрази

```haskell
data ExprTree = Lit Double
              | BinOp Char ExprTree ExprTree
  deriving (Show)

-- Оценяване
eval :: ExprTree -> Double
eval (Lit x) = x
eval (BinOp '+' l r) = eval l + eval r
eval (BinOp '-' l r) = eval l - eval r
eval (BinOp '*' l r) = eval l * eval r
eval (BinOp '/' l r) = eval l / eval r

-- Преобразуване в инфиксен запис
toInfix :: ExprTree -> String
toInfix (Lit x)        = show x
toInfix (BinOp op l r) = "(" ++ toInfix l ++ " " ++ [op] ++ " " ++ toInfix r ++ ")"

-- Преобразуване в постфиксен (RPN) запис
toPostfix :: ExprTree -> String
toPostfix (Lit x)        = show x
toPostfix (BinOp op l r) = toPostfix l ++ " " ++ toPostfix r ++ " " ++ [op]

-- Пример: (3 + 4) * (2 - 1)
exprTree :: ExprTree
exprTree = BinOp '*'
  (BinOp '+' (Lit 3) (Lit 4))
  (BinOp '-' (Lit 2) (Lit 1))
```

```haskell
ghci> eval exprTree
7.0
ghci> toInfix exprTree
"((3.0 + 4.0) * (2.0 - 1.0))"
ghci> toPostfix exprTree
"3.0 4.0 + 2.0 1.0 - *"
```
