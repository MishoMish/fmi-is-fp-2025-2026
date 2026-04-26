# Примери от семинара

```haskell
module Examples where

-- === 1. AST Изрази ===

data Expression = Constant Int
                | Var String
                | Op Expression Char Expression
                deriving (Show, Eq)

type Environment = [(String, Int)]

-- Представя израза: (1 + x) * 3
-- Където x = 5
expr1 :: Expression
expr1 = Op (Op (Constant 1) '+' (Var "x")) '*' (Constant 3)

env1 :: Environment
env1 = [("x", 5), ("y", 10)]

-- Очаквана стойност при eval env1 expr1:
-- (1 + 5) * 3 = 18


-- === 2. Двоични наредени дървета ===

data IntTree = Empty | Node Int IntTree IntTree
             deriving (Show, Eq)

-- Създаваме дърво:
--        50
--      /    \
--    25     100
--   /  \       \
-- 10    30      120
--               /
--             110

bstTree :: IntTree
bstTree = Node 50
            (Node 25
                (Node 10 Empty Empty)
                (Node 30 Empty Empty))
            (Node 100
                Empty
                (Node 120
                    (Node 110 Empty Empty)
                    Empty))

-- Неправилно наредено дърво (полезно за тест на isBST):
notBstTree :: IntTree
notBstTree = Node 10
                (Node 5 Empty Empty)
                (Node 4 Empty Empty)
-- Не е BST, защото 4 < 10, но е в дясното поддърво (т.е. е по-голям)!


-- === 3. Дървета за общи задачи (симетрия, ротации и др.) ===

-- Симетрично дърво:
--        1
--      /   \
--     2     2
--    / \   / \
--   3   4 4   3
symmetricTree :: IntTree
symmetricTree = Node 1
                    (Node 2
                        (Node 3 Empty Empty)
                        (Node 4 Empty Empty))
                    (Node 2
                        (Node 4 Empty Empty)
                        (Node 3 Empty Empty))

-- Дърво, подходящо за ротация и пътища:
--        10
--       /  \
--      5    15
--          /  \
--         12   20
pathTree :: IntTree
pathTree = Node 10
            (Node 5 Empty Empty)
            (Node 15
                (Node 12 Empty Empty)
                (Node 20 Empty Empty))
```
