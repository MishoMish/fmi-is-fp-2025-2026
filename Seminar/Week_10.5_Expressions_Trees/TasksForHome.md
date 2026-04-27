# Седмица 10.5 - Задачи за домашна работа

## Задача 1: Опростяване на изрази (Constant Folding)

Напишете функция `simplify :: Expression -> Expression`, която оптимизира аритметичното дърво.
Ако имаме операция над две константи `Op (Constant a) op (Constant b)`, те трябва да се изчислят предварително до `Constant (a op b)`.
Функцията трябва да опростява дървото рекурсивно от листата към корена.

_Пример:_
Изразът `1 + (2 * 3) + x`
Ще бъде представен като: `Op (Op (Constant 1) '+' (Op (Constant 2) '*' (Constant 3))) '+' (Var "x")`
Трябва да се опрости до `Op (Constant 7) '+' (Var "x")`.

<details>
<summary><b>Виж решението</b></summary>

```haskell
simplify :: Expression -> Expression
simplify (Constant n) = Constant n
simplify (Var v) = Var v
simplify (Op left op right) =
  let sLeft = simplify left
      sRight = simplify right
  in case (sLeft, sRight) of
       -- Ако и лявото, и дясното поддърво са константи, пресмятаме ги!
       (Constant c1, Constant c2) -> Constant (compute c1 op c2)
       -- Ако ли не, ги оставяме опростени.
       _ -> Op sLeft op sRight

-- Помощна функция
compute :: Int -> Char -> Int -> Int
compute a '+' b = a + b
compute a '-' b = a - b
compute a '*' b = a * b
compute _ _ _ = error "Unsupported operator"
```

</details>

## Задача 2: Пресичане на наредени дървета

Напишете функция `intersection :: IntTree -> IntTree -> IntTree`, която приема две двоични наредени дървета и връща ново двоично наредено дърво, съдържащо само елементите, които присъстват и в двете начални дървета.

<details>
<summary><b>Виж решението</b></summary>

```haskell
-- Решение чрез изравняване:
-- Правим едното дърво на списък и после филтрираме елементите му,
-- като проверяваме дали се съдържат в другото дърво. Накрая от списъка строим ново.

toList :: IntTree -> [Int]
toList Empty = []
toList (Node x left right) = toList left ++ [x] ++ toList right

member :: Int -> IntTree -> Bool
member _ Empty = False
member x (Node val left right)
  | x == val  = True
  | x < val   = member x left
  | otherwise = member x right

-- Добавяне от списък
fromList :: [Int] -> IntTree
fromList = foldl (flip insert) Empty

-- Моля, уверете се, че използвате функцията insert, която написахте в Task 3
insert :: Int -> IntTree -> IntTree
insert x Empty = Node x Empty Empty
insert x (Node val left right)
  | x == val  = Node val left right
  | x < val   = Node val (insert x left) right
  | otherwise = Node val left (insert x right)

intersection :: IntTree -> IntTree -> IntTree
intersection t1 t2 =
  let elemsT1 = toList t1
      commonElems = filter (`member` t2) elemsT1
  in fromList commonElems
```

</details>


## Задача 3: Проверка за еднакви дървета
Напишете функция `areEqual :: IntTree -> IntTree -> Bool`, която проверява дали две дървета съвпадат както по структура, така и по стойностите във възлите си.

<details>
<summary><b>Виж решението</b></summary>

```haskell
areEqual :: IntTree -> IntTree -> Bool
areEqual Empty Empty = True
areEqual (Node x1 l1 r1) (Node x2 l2 r2) =
    x1 == x2 && areEqual l1 l2 && areEqual r1 r2
areEqual _ _ = False
```

</details>

## Задача 4: Намиране на братя (siblings) в дървото

Напишете функция `siblings :: Int -> IntTree -> [Int]`, която връща списък (или единствен елемент, но в списък за удобство) от стойностите на братята (siblings) на дадена стойност `v`. Ако тя няма брат или възелът изобщо не се среща в дървото, да върне празен списък.

<details>
<summary><b>Виж решението</b></summary>

```haskell
-- Трябва да намерим възел, чиито деца (или поне едното) са равно на v,
-- след което връщаме стойността на другото дете, ако съществува.
siblings :: Int -> IntTree -> [Int]
siblings _ Empty = []
siblings v (Node _ left right) =
    case (left, right) of
      (Node lVal _ _, Node rVal _ _)
        | lVal == v -> rVal : searchFurther
        | rVal == v -> lVal : searchFurther
      _ -> searchFurther
  where
    searchFurther = siblings v left ++ siblings v right
```

</details>
