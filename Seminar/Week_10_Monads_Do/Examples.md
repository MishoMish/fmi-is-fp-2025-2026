# Седмица 10 — Примери

## Пример 1: Верижни `Maybe` операции

```haskell
type Inventory = [(String, Int)]     -- (продукт, наличност)
type PriceList = [(String, Double)]  -- (продукт, цена)

inventory :: Inventory
inventory = [("ябълки", 50), ("банани", 0), ("портокали", 30)]

prices :: PriceList
prices = [("ябълки", 1.20), ("банани", 0.80), ("портокали", 1.50)]

-- Намиране на продукт
findProduct :: String -> Inventory -> Maybe Int
findProduct = lookup

findPrice :: String -> PriceList -> Maybe Double
findPrice = lookup

-- Обща цена ако продуктът е наличен
totalPrice :: String -> Int -> Maybe Double
totalPrice product qty = do
  stock <- findProduct product inventory
  if stock < qty
    then Nothing
    else do
      price <- findPrice product prices
      return (fromIntegral qty * price)
```

```haskell
ghci> totalPrice "ябълки" 5
Just 6.0
ghci> totalPrice "ябълки" 100
Nothing
ghci> totalPrice "круши" 5
Nothing
```

---

## Пример 2: Навигация в JSON-подобна структура

```haskell
data JValue = JNull
            | JNum Double
            | JStr String
            | JBool Bool
            | JArr [JValue]
            | JObj [(String, JValue)]
  deriving (Show)

-- Безопасен достъп до поле
(.>) :: JValue -> String -> Maybe JValue
(JObj fields) .> key = lookup key fields
_             .> _   = Nothing

-- Безопасен достъп до елемент от масив
(!>) :: JValue -> Int -> Maybe JValue
(JArr xs) !> i
  | i >= 0 && i < length xs = Just (xs !! i)
  | otherwise                = Nothing
_         !> _               = Nothing

-- Извличане на String
asString :: JValue -> Maybe String
asString (JStr s) = Just s
asString _        = Nothing

-- Извличане на Number
asNum :: JValue -> Maybe Double
asNum (JNum n) = Just n
asNum _        = Nothing

-- Пример: JSON данни
userData :: JValue
userData = JObj
  [ ("users", JArr
      [ JObj [("name", JStr "Иван"), ("age", JNum 25)]
      , JObj [("name", JStr "Мария"), ("age", JNum 22)]
      ])
  ]

-- Вземи името на втория потребител
getSecondUserName :: JValue -> Maybe String
getSecondUserName json = do
  users <- json .> "users"
  user  <- users !> 1
  name  <- user .> "name"
  asString name
```

```haskell
ghci> getSecondUserName userData
Just "Мария"
```

---

## Пример 3: `do`-нотация с `Either`

```haskell
data Student = Student
  { stuName  :: String
  , stuAge   :: Int
  , stuEmail :: String
  } deriving (Show)

parseStudent :: String -> String -> String -> Either String Student
parseStudent name ageStr email = do
  validName  <- if null name then Left "Празно име" else Right name
  validAge   <- case reads ageStr :: [(Int, String)] of
                  [(n, "")] -> if n > 0 && n < 100
                               then Right n
                               else Left ("Невалидна възраст: " ++ ageStr)
                  _         -> Left ("Не е число: " ++ ageStr)
  validEmail <- if '@' `elem` email
                then Right email
                else Left ("Невалиден имейл: " ++ email)
  return (Student validName validAge validEmail)
```

```haskell
ghci> parseStudent "Иван" "25" "ivan@mail.bg"
Right (Student {stuName = "Иван", stuAge = 25, stuEmail = "ivan@mail.bg"})
ghci> parseStudent "Иван" "abc" "ivan@mail.bg"
Left "Не е число: abc"
ghci> parseStudent "Иван" "25" "invalid"
Left "Невалиден имейл: invalid"
```
