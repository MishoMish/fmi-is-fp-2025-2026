# Maybe, Either и обработка на грешки

## Въведение

В Haskell няма `null` и няма изключения (exceptions) в класическия смисъл. Вместо тях използваме **типове**, за да изразим липса на стойност или грешка: `Maybe` и `Either`.

---

## 1. `Maybe` - може да има или да няма стойност

```haskell
data Maybe a = Nothing | Just a
```

- `Nothing` - липса на стойност (аналог на `null`)
- `Just x` - наличие на стойност `x`

```haskell
safeDiv :: Double -> Double -> Maybe Double
safeDiv _ 0 = Nothing
safeDiv x y = Just (x / y)

safeHead :: [a] -> Maybe a
safeHead []    = Nothing
safeHead (x:_) = Just x

safeLookup :: Eq a => a -> [(a, b)] -> Maybe b
safeLookup _ []          = Nothing
safeLookup key ((k,v):xs)
  | key == k  = Just v
  | otherwise = safeLookup key xs
```

```haskell
ghci> safeDiv 10 3
Just 3.3333333333333335
ghci> safeDiv 10 0
Nothing
ghci> safeHead [1,2,3]
Just 1
ghci> safeHead ([] :: [Int])
Nothing
```

---

## 2. Работа с `Maybe` стойности

### Pattern matching

```haskell
showResult :: Maybe Double -> String
showResult Nothing  = "Грешка!"
showResult (Just x) = "Резултат: " ++ show x
```

### `case` израз

```haskell
describeHead :: [Int] -> String
describeHead xs = case safeHead xs of
  Nothing -> "Празен списък"
  Just x  -> "Първият елемент е " ++ show x
```

### Полезни функции от `Data.Maybe`

```haskell
fromMaybe :: a -> Maybe a -> a
fromMaybe defaultVal Nothing  = defaultVal
fromMaybe _          (Just x) = x

isJust :: Maybe a -> Bool
isJust (Just _) = True
isJust Nothing  = False

isNothing :: Maybe a -> Bool
isNothing Nothing = True
isNothing _       = False

catMaybes :: [Maybe a] -> [a]
-- Премахва Nothing стойностите и разопакова Just

mapMaybe :: (a -> Maybe b) -> [a] -> [b]
-- map + филтриране на Nothing
```

```haskell
ghci> fromMaybe 0 (Just 5)       -- 5
ghci> fromMaybe 0 Nothing        -- 0
ghci> catMaybes [Just 1, Nothing, Just 3]  -- [1, 3]
ghci> mapMaybe safeHead [[1,2], [], [3]]   -- [1, 3]
```

---

## 3. Верижно свързване на `Maybe` операции

Когато имаме поредица от операции, всяка от които може да върне `Nothing`:

```haskell
-- Без верижно свързване (грозно!):
process :: [(String, String)] -> String -> Maybe Int
process env key = case safeLookup key env of
  Nothing  -> Nothing
  Just val -> case safeRead val of
    Nothing -> Nothing
    Just n  -> if n > 0 then Just n else Nothing

-- С помощна функция:
andThen :: Maybe a -> (a -> Maybe b) -> Maybe b
andThen Nothing  _ = Nothing
andThen (Just x) f = f x
```

> 💡 `andThen` всъщност е операторът `>>=` за `Maybe` - ще го разгледаме по-подробно при монадите.

---

## 4. `Either` - стойност или грешка с описание

```haskell
data Either a b = Left a | Right b
```

- `Left` - грешка (по конвенция) с описание
- `Right` - успешна стойност (мнемоника: "right" = "correct")

```haskell
safeDivE :: Double -> Double -> Either String Double
safeDivE _ 0 = Left "Деление на нула!"
safeDivE x y = Right (x / y)

parseAge :: String -> Either String Int
parseAge s
  | all isDigit s && not (null s) = let n = read s
                                     in if n >= 0 && n <= 150
                                        then Right n
                                        else Left ("Невалидна възраст: " ++ s)
  | otherwise = Left ("Не е число: " ++ s)
  where isDigit c = c >= '0' && c <= '9'
```

```haskell
ghci> safeDivE 10 3
Right 3.3333333333333335
ghci> safeDivE 10 0
Left "Деление на нула!"
ghci> parseAge "25"
Right 25
ghci> parseAge "abc"
Left "Не е число: abc"
ghci> parseAge "200"
Left "Невалидна възраст: 200"
```

---

## 5. Работа с `Either`

```haskell
-- Pattern matching
handleResult :: Either String Int -> String
handleResult (Left err) = "Грешка: " ++ err
handleResult (Right n)  = "Успех: " ++ show n

-- Верижно свързване
andThenE :: Either e a -> (a -> Either e b) -> Either e b
andThenE (Left err) _ = Left err
andThenE (Right x)  f = f x
```

### Полезни функции

```haskell
-- either :: (a -> c) -> (b -> c) -> Either a b -> c
ghci> either (\e -> "Error: " ++ e) show (Right 42)
"42"
ghci> either (\e -> "Error: " ++ e) show (Left "oops")
"Error: oops"

-- fromRight :: b -> Either a b -> b  (от Data.Either)
ghci> fromRight 0 (Right 5)  -- 5
ghci> fromRight 0 (Left "err")  -- 0
```

---

## 6. `Maybe` vs `Either`

| Свойство   | `Maybe a`                  | `Either e a`                    |
| ---------- | -------------------------- | ------------------------------- |
| При грешка | `Nothing` (без информация) | `Left e` (с описание)           |
| При успех  | `Just a`                   | `Right a`                       |
| Кога       | Резултатът може да липсва  | Искаме да знаем _защо_ е грешка |
| Пример     | `safeLookup`               | `parseAge`                      |

---

## Обобщение

| Концепция         | Описание                                       |
| ----------------- | ---------------------------------------------- |
| `Maybe a`         | `Nothing` или `Just a`                         |
| `Either e a`      | `Left e` (грешка) или `Right a` (успех)        |
| `fromMaybe`       | Стойност по подразбиране за `Maybe`            |
| `catMaybes`       | Филтрира `Nothing` от списък                   |
| `mapMaybe`        | `map` + филтриране на `Nothing`                |
| `case ... of`     | Pattern matching като израз                    |
| `andThen` / `>>=` | Верижно свързване на `Maybe`/`Either` операции |
