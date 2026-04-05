# Двоични дървета

## Въведение

Двоичното дърво е **индуктивна** (рекурсивна) структура от данни: или е празно, или съдържа стойност и две поддървета. В Haskell дефинирането и обработката на дървета е изключително естествено.

---

## 1. Дефиниция

```haskell
data Tree a = Empty
            | Node (Tree a) a (Tree a)
  deriving (Show, Eq)
```

- `Empty` - празно дърво (лист)
- `Node left value right` - възел със стойност и две поддървета

```haskell
--       5
--      / \
--     3   7
--    / \   \
--   1   4   9

exampleTree :: Tree Int
exampleTree = Node
  (Node (Node Empty 1 Empty) 3 (Node Empty 4 Empty))
  5
  (Node Empty 7 (Node Empty 9 Empty))
```

---

## 2. Основни операции

```haskell
-- Брой елементи
treeSize :: Tree a -> Int
treeSize Empty          = 0
treeSize (Node l _ r)   = 1 + treeSize l + treeSize r

-- Височина
treeHeight :: Tree a -> Int
treeHeight Empty        = 0
treeHeight (Node l _ r) = 1 + max (treeHeight l) (treeHeight r)

-- Сума (за числа)
treeSum :: Num a => Tree a -> a
treeSum Empty          = 0
treeSum (Node l x r)   = treeSum l + x + treeSum r

-- Търсене на елемент
treeElem :: Eq a => a -> Tree a -> Bool
treeElem _ Empty        = False
treeElem x (Node l v r) = x == v || treeElem x l || treeElem x r

-- Брой на листата
countLeaves :: Tree a -> Int
countLeaves Empty                = 0
countLeaves (Node Empty _ Empty) = 1
countLeaves (Node l _ r)         = countLeaves l + countLeaves r
```

```haskell
ghci> treeSize exampleTree
5
ghci> treeHeight exampleTree
3
ghci> treeElem 4 exampleTree
True
ghci> countLeaves exampleTree
3
```

---

## 3. Обхождания (Traversals)

```haskell
-- Inorder: ляво → корен → дясно
inorder :: Tree a -> [a]
inorder Empty          = []
inorder (Node l x r)   = inorder l ++ [x] ++ inorder r

-- Preorder: корен → ляво → дясно
preorder :: Tree a -> [a]
preorder Empty          = []
preorder (Node l x r)   = [x] ++ preorder l ++ preorder r

-- Postorder: ляво → дясно → корен
postorder :: Tree a -> [a]
postorder Empty          = []
postorder (Node l x r)   = postorder l ++ postorder r ++ [x]

-- Level order (BFS) - по нива
levelOrder :: Tree a -> [a]
levelOrder tree = go [tree]
  where
    go [] = []
    go (Empty : rest)      = go rest
    go (Node l x r : rest) = x : go (rest ++ [l, r])
```

```haskell
ghci> inorder exampleTree     -- [1,3,4,5,7,9]
ghci> preorder exampleTree    -- [5,3,1,4,7,9]
ghci> postorder exampleTree   -- [1,4,3,9,7,5]
ghci> levelOrder exampleTree  -- [5,3,7,1,4,9]
```

> 💡 За BST, `inorder` дава елементите в **сортиран ред**.

---

## 4. Двоично наредено дърво (BST)

BST (Binary Search Tree) поддържа инвариант: за всеки възел, лявото поддърво съдържа по-малки стойности, дясното - по-големи.

```haskell
-- Търсене в BST: O(log n) за балансирано дърво
bstSearch :: Ord a => a -> Tree a -> Bool
bstSearch _ Empty = False
bstSearch x (Node l v r)
  | x == v    = True
  | x < v     = bstSearch x l
  | otherwise  = bstSearch x r

-- Вмъкване в BST
bstInsert :: Ord a => a -> Tree a -> Tree a
bstInsert x Empty = Node Empty x Empty
bstInsert x (Node l v r)
  | x < v     = Node (bstInsert x l) v r
  | x > v     = Node l v (bstInsert x r)
  | otherwise  = Node l v r  -- вече съществува

-- Построяване на BST от списък
fromList :: Ord a => [a] -> Tree a
fromList = foldr bstInsert Empty
```

```haskell
ghci> let bst = fromList [5, 3, 7, 1, 4, 9]
ghci> inorder bst
[1,3,4,5,7,9]
ghci> bstSearch 4 bst
True
ghci> bstSearch 6 bst
False
```

---

## 5. Изтриване от BST

```haskell
-- Намира минималния елемент
bstMin :: Tree a -> a
bstMin (Node Empty x _) = x
bstMin (Node l _ _)     = bstMin l

-- Изтриване
bstDelete :: Ord a => a -> Tree a -> Tree a
bstDelete _ Empty = Empty
bstDelete x (Node l v r)
  | x < v     = Node (bstDelete x l) v r
  | x > v     = Node l v (bstDelete x r)
  | otherwise  = deleteNode (Node l v r)
  where
    deleteNode (Node Empty _ Empty) = Empty         -- лист
    deleteNode (Node Empty _ r')    = r'            -- само дясно дете
    deleteNode (Node l' _ Empty)    = l'            -- само ляво дете
    deleteNode (Node l' _ r')       =               -- две деца
      let minRight = bstMin r'
      in Node l' minRight (bstDelete minRight r')
```

Трите класически случая при изтриване:

- Лист (няма деца): Просто връщаме Empty. Възелът изчезва.
- Едно дете: Ако лявото е празно, връщаме дясното (и обратно). Детето "заема мястото" на изтрития възел, запазвайки връзката с горната част на дървото.
- Две деца (най-интересният случай):
  Не можем просто да изтрием възела, защото ще "скъсаме" дървото на две части.
  Стратегия: Намираме най-малкия елемент в дясното поддърво (minRight = bstMin r'). Той е идеалният заместник, защото е по-голям от всичко вляво и по-малък от всичко останало вдясно.
  Действие: Заменяме стойността на текущия възел с minRight и след това рекурсивно изтриваме оригиналния minRight от дясното поддърво, за да няма дублиране.

---

## 6. `map` и `fold` за дървета

```haskell
treeMap :: (a -> b) -> Tree a -> Tree b
treeMap _ Empty          = Empty
treeMap f (Node l x r)   = Node (treeMap f l) (f x) (treeMap f r)

treeFoldr :: (a -> b -> b) -> b -> Tree a -> b
treeFoldr _ acc Empty        = acc
treeFoldr f acc (Node l x r) = treeFoldr f (f x (treeFoldr f acc r)) l
```

```haskell
ghci> treeMap (*2) exampleTree   -- всички стойности × 2
ghci> treeFoldr (+) 0 exampleTree  -- сума = 29
ghci> treeFoldr (:) [] exampleTree -- inorder списък
```

---

## Обобщение

| Операция     | Сложност (BST баланс.) | Сложност (общо) |
| ------------ | ---------------------- | --------------- |
| `bstSearch`  | O(log n)               | O(n)            |
| `bstInsert`  | O(log n)               | O(n)            |
| `bstDelete`  | O(log n)               | O(n)            |
| `inorder`    | O(n)                   | O(n)            |
| `treeSize`   | O(n)                   | O(n)            |
| `treeHeight` | O(n)                   | O(n)            |

| Обхождане   | Ред                  | Приложение             |
| ----------- | -------------------- | ---------------------- |
| Inorder     | ляво → корен → дясно | Сортиран ред в BST     |
| Preorder    | корен → ляво → дясно | Копиране, сериализация |
| Postorder   | ляво → дясно → корен | Изтриване, eval        |
| Level order | по нива              | BFS, визуализация      |
