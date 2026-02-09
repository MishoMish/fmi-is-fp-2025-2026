# Монади и do-нотация

## Въведение

Монадите са **шаблон за верижно свързване** на изчисления, всяко от които може да „обвива" резултата си в контекст (неуспех, странични ефекти, множество резултати и т.н.).

---

## 1. Проблемът: верижни `Maybe` операции

```haskell
-- От Седмица 8 — грозно вложено case:
lookup' :: String -> [(String, String)] -> Maybe String
lookup' _ [] = Nothing
lookup' k ((k',v):rest)
  | k == k'   = Just v
  | otherwise  = lookup' k rest

process :: [(String, String)] -> Maybe Int
process env = case lookup' "x" env of
  Nothing  -> Nothing
  Just val -> case safeRead val of
    Nothing -> Nothing
    Just n  -> if n > 0 then Just n else Nothing
```

Виждаме повтарящ се паттерн: „ако е `Nothing` — спри, ако е `Just` — продължи".

---

## 2. Операторът `>>=` (bind)

Точно този паттерн изразява `>>=`:

```haskell
(>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
Nothing >>= _ = Nothing
Just x  >>= f = f x
```

Сега `process` може да се напише:

```haskell
process :: [(String, String)] -> Maybe Int
process env = lookup' "x" env >>= safeRead >>= validate
  where
    validate n = if n > 0 then Just n else Nothing
```

> 💡 `>>=` се чете „bind" — свързва резултата от едно изчисление с функция, която продължава.

---

## 3. `>>` (then)

Когато не ни трябва резултатът от предишната стъпка:

```haskell
(>>) :: Maybe a -> Maybe b -> Maybe b
Nothing >> _ = Nothing
Just _  >> m = m
```

```haskell
ghci> Just 3 >> Just "hello"    -- Just "hello"
ghci> Nothing >> Just "hello"   -- Nothing
```

---

## 4. `return`

`return` поставя стойност в монадичен контекст:

```haskell
return :: a -> Maybe a
return x = Just x
```

> ⚠️ `return` в Haskell **НЕ** е като return в императивни езици! Не спира изпълнението.

---

## 5. `do`-нотация

`do`-нотацията е **синтактична захар** за верига от `>>=`:

```haskell
-- С >>=:
process env = lookup' "x" env >>= \val ->
              safeRead val >>= \n ->
              if n > 0 then Just n else Nothing

-- С do:
process env = do
  val <- lookup' "x" env
  n   <- safeRead val
  if n > 0 then Just n else Nothing
```

### Правила за `do`:
1. `x <- action` — изпълнява `action` и именува резултата `x`
2. `action` без `<-` — изпълнява `action`, игнорира резултата (като `>>`)
3. Последният ред е резултатът от целия `do` блок
4. `let x = expr` — обикновен `let` (без `in`!)

```haskell
example :: Maybe Int
example = do
  let x = 10
  y <- Just 5
  z <- Just 3
  return (x + y + z)  -- Just 18
```

---

## 6. `Maybe` като монада — примери

```haskell
-- Безопасно индексиране в матрица
type Matrix a = [[a]]

safeIndex :: [a] -> Int -> Maybe a
safeIndex xs i
  | i < 0 || i >= length xs = Nothing
  | otherwise                = Just (xs !! i)

matrixAt :: Matrix a -> Int -> Int -> Maybe a
matrixAt m row col = do
  r <- safeIndex m row
  safeIndex r col
```

```haskell
ghci> let m = [[1,2,3],[4,5,6],[7,8,9]]
ghci> matrixAt m 1 2
Just 6
ghci> matrixAt m 5 0
Nothing
```

---

## 7. `Either` като монада

```haskell
-- >>= за Either:
-- Left err >>= _ = Left err
-- Right x  >>= f = f x

validateAndProcess :: String -> String -> String -> Either String String
validateAndProcess name email age = do
  n <- validateName name
  e <- validateEmail email
  a <- validateAge age
  return ("Потребител: " ++ n ++ ", " ++ e ++ ", възраст " ++ a)
  where
    validateName  n = if null n then Left "Празно име" else Right n
    validateEmail e = if '@' `elem` e then Right e else Left "Невалиден имейл"
    validateAge   a = if all (`elem` "0123456789") a && not (null a) then Right a else Left "Невалидна възраст"
```

```haskell
ghci> validateAndProcess "Иван" "ivan@mail.bg" "25"
Right "Потребител: Иван, ivan@mail.bg, възраст 25"
ghci> validateAndProcess "" "ivan@mail.bg" "25"
Left "Празно име"
```

---

## 8. Списъкът като монада

`>>=` за списъци е `concatMap`:

```haskell
ghci> [1,2,3] >>= \x -> [x, x*10]
[1,10,2,20,3,30]

-- do нотация = list comprehension
pairs :: [(Int, Int)]
pairs = do
  x <- [1..3]
  y <- [x..3]
  return (x, y)
-- Еквивалентно на: [(x,y) | x <- [1..3], y <- [x..3]]
```

---

## 9. Монадичният типов клас

```haskell
class Monad m where
  return :: a -> m a
  (>>=)  :: m a -> (a -> m b) -> m b
```

| Монада | `return x` | `>>=` поведение |
|--------|-----------|----------------|
| `Maybe` | `Just x` | Спира при `Nothing` |
| `Either e` | `Right x` | Спира при `Left` |
| `[]` | `[x]` | `concatMap` |
| `IO` | Обвива в IO | Верижно изпълнение на ефекти |

---

## Обобщение

| Концепция | Описание | Пример |
|-----------|----------|--------|
| `>>=` (bind) | Верижно свързване | `Just 3 >>= \x -> Just (x+1)` |
| `>>` (then) | Свързване без резултат | `Just 3 >> Just "ok"` |
| `return` | Обвива стойност | `return 5 :: Maybe Int` |
| `do`-нотация | Синтактична захар за `>>=` | `do { x <- m; return (x+1) }` |
| `<-` | Извлича стойност в `do` | `x <- Just 5` |
