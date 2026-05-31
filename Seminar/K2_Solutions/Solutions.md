# Контролно 2 - Решения

## Задача 1: Двоично дърво

Тук се изисква създаването на полиморфно двоично дърво и няколко класически функции за работа с него.

```haskell
-- а) Дефиниция на полиморфно двоично дърво
data BinTree a = Empty
               | Node (BinTree a) a (BinTree a)
               deriving (Show, Eq)

-- б) Проекция на дървото (взема само втората компонента на възлите)
projectTree :: BinTree (a, b) -> BinTree b
projectTree Empty = Empty
projectTree (Node left (_, y) right) = Node (projectTree left) y (projectTree right)

-- в) Сума на първите компоненти. Използваме Num a =>, за да гарантираме оператора +
sumFirst :: Num a => BinTree (a, b) -> a
sumFirst Empty = 0
sumFirst (Node left (x, _) right) = x + sumFirst left + sumFirst right

-- г) Замяна на стойности от тип 'a' (Left) чрез функция f, запазване на 'b' (Right)
mapEither :: BinTree (Either a b) -> (a -> a) -> BinTree (Either a b)
mapEither Empty _ = Empty
mapEither (Node left val right) f = Node (mapEither left f) newVal (mapEither right f)
  where
    newVal = case val of
               Left x  -> Left (f x)  -- Прилагаме функцията, ако е Left
               Right y -> Right y     -- Оставяме непроменено, ако е Right
```

---

## Задача 2: Родословно дърво

Това е същинската и по-сложна част от контролното. Тъй като хората могат да имат произволен брой деца, ни трябва n-арно дърво

```haskell
-- а) Дефиниция на подходящи типове.
type Name = String
type Spouse = Maybe Name

-- Всеки възел съдържа име на кръвния роднина, евентуален съпруг и списък от деца.
data FamilyTree = PersonNode Name Spouse [FamilyTree]
                  deriving (Show, Eq)

-- Помощна функция: намира първия успешен резултат (Just) в списък от Maybe стойности
firstJust :: [Maybe a] -> Maybe a
firstJust [] = Nothing
firstJust (Just x : _) = Just x
firstJust (Nothing : xs) = firstJust xs

-- б) Добавяне на съпруг към неомъжен/неоженен член на рода
marry :: FamilyTree -> Name -> Name -> Maybe FamilyTree
marry (PersonNode n spouse children) target newSpouse
  -- Ако сме намерили търсения човек
  | n == target = case spouse of
      Nothing -> Just (PersonNode n (Just newSpouse) children) -- Успех
      Just _  -> Nothing -- Човекът вече има съпруг/а, връщаме грешка
  -- Ако не е този възел, търсим реално в децата
  | otherwise = do
      -- Обхождаме децата. Ако успеем да оженим някого в някое поддърво,
      -- сглобяваме обратно списъка с деца с обновеното поддърво.
      let updateChildren [] = Nothing
          updateChildren (c:cs) = case marry c target newSpouse of
                                    Just updatedChild -> Just (updatedChild : cs)
                                    Nothing           -> do
                                        rest <- updateChildren cs
                                        return (c : rest)
      updatedChildren <- updateChildren children
      return (PersonNode n spouse updatedChildren)


-- в) Намиране на братя и сестри по име
siblings :: FamilyTree -> Name -> Maybe [Name]
siblings (PersonNode _ _ children) target
  -- Проверяваме дали търсеният човек е сред децата на ТЕКУЩИЯ възел
  | target `elem` childNames = Just (filter (/= target) childNames)
  -- Ако не е тук, пускаме рекурсивно търсене надолу в децата
  | otherwise = firstJust (map (\c -> siblings c target) children)
  where
    childNames = [name | PersonNode name _ _ <- children]


-- г) Намиране на братовчеди (децата на братята и сестрите на родителя)
cousins :: FamilyTree -> Name -> Maybe [Name]
cousins root target = do
    parent <- findParent root target
    let PersonNode pName _ _ = parent
    parentSibs <- siblings root pName
    -- Намираме възлите на всички братя/сестри на родителя и извличаме децата им
    let sibNodes = [node | sib <- parentSibs, Just node <- [findNode root sib]]
    return [childName | PersonNode _ _ children <- sibNodes,
                        PersonNode childName _ _ <- children]

-- Помощна функция за намиране на родител на даден човек
findParent :: FamilyTree -> Name -> Maybe FamilyTree
findParent root@(PersonNode _ _ children) target
  | target `elem` [name | PersonNode name _ _ <- children] = Just root
  | otherwise = firstJust (map (\c -> findParent c target) children)

-- Помощна функция за намиране на самия възел по име
findNode :: FamilyTree -> Name -> Maybe FamilyTree
findNode root@(PersonNode n _ children) target
  | n == target = Just root
  | otherwise = firstJust (map (\c -> findNode c target) children)


-- д) Кръвна връзка "от кое коляно" (дължина на пътя между двама души)
-- Упътването подсказва намиране на пътя и използване на най-близкия общ предшественик (LCA).
bloodRelation :: FamilyTree -> Name -> Name -> Maybe Int
bloodRelation root name1 name2 = do
    p1 <- path root name1
    p2 <- path root name2
    -- Премахваме общата част от пътя (от корена до общия предшественик)
    -- Остатъкът от двата пътя формира разстоянието между тях.
    return (calculateDistance p1 p2)
  where
    calculateDistance (x:xs) (y:ys) | x == y = calculateDistance xs ys
    calculateDistance xs ys = length xs + length ys

-- Помощна функция: връща списък с имената по пътя от корена до търсения човек
path :: FamilyTree -> Name -> Maybe [Name]
path (PersonNode n _ children) target
  | n == target = Just [n]
  | otherwise = do
      -- Търсим пътя в някое от децата
      childPath <- firstJust (map (\c -> path c target) children)
      -- Ако го намерим, добавяме текущия възел отпред
      return (n : childPath)
```
