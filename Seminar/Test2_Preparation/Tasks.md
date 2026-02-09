# Подготовка за Тест 2

## Обхват: Седмици 7 — 11

| Тема | Седмица | Ключови концепции |
|------|---------|-------------------|
| ADTs | 7 | `data`, конструктори, records, `deriving` |
| Maybe/Either | 8 | `Maybe a`, `Either e a`, `case`, `fromMaybe`, `catMaybes` |
| Типови класове | 9 | `class`, `instance`, `Eq`, `Ord`, `Show`, собствени |
| Монади и do | 10 | `>>=`, `return`, `do`-нотация, Maybe/Either/List като монади |
| IO и сериализация | 11 | `IO`, `getLine`, `putStrLn`, `readFile`, `writeFile`, CSV |

---

## Задача 1: ADT + Pattern matching

Дефинирайте тип `Expr` за аритметични изрази с числа, събиране, умножение и отрицание. Напишете `eval :: Expr -> Double` и `prettyPrint :: Expr -> String`.

---

## Задача 2: Maybe верижни операции

Напишете `safeOps :: [Int] -> Maybe Int`, която:
1. Взема първия елемент (безопасно)
2. Проверява дали е > 0 (иначе Nothing)
3. Дели 100 на него (безопасно)

Решете с `do`-нотация.

---

## Задача 3: Типови класове

Дефинирайте типов клас `Serializable` с метод `serialize :: a -> String` и `deserialize :: String -> Maybe a`.

Напишете инстанции за `Int`, `Bool` и `[a]` (ако `a` е `Serializable`).

---

## Задача 4: Binary Tree + Foldable

Дефинирайте `Tree a` и напишете:
- `treeToList :: Tree a -> [a]` (inorder)
- `treeFromList :: Ord a => [a] -> Tree a` (BST)
- `treeFoldr :: (a -> b -> b) -> b -> Tree a -> b`

---

## Задача 5: Either + do-нотация

Напишете мини-калкулатор, който парсва низ от вида `"3 + 4"` и пресмята резултата, като всяка стъпка може да върне `Either String Double`.

---

## Задача 6: IO задача

Опишете (без да изпълнявате) програма, която:
1. Чете CSV файл с колони `Име,Оценка`
2. Филтрира студентите с оценка ≥ 4.0
3. Записва резултата в нов файл

---

## Задача 7: Комплексна

Дефинирайте `data JSON = ...` и напишете:
- `jsonGet :: String -> JSON -> Maybe JSON`
- `jsonToString :: JSON -> String`
- `instance Eq JSON`
