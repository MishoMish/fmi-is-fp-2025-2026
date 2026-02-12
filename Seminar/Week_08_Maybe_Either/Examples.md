# Седмица 8 - Примери

## Пример 1: Безопасен достъп до вложени данни

```haskell
type Config = [(String, String)]

-- Безопасно намиране на стойност
configGet :: String -> Config -> Maybe String
configGet _ [] = Nothing
configGet key ((k,v):rest)
  | key == k  = Just v
  | otherwise = configGet key rest

-- Безопасно парсване на Int
safeRead :: String -> Maybe Int
safeRead s = case reads s of
  [(n, "")] -> Just n
  _         -> Nothing

-- Верижно: вземи конфиг → парсни → валидирай
getPort :: Config -> Maybe Int
getPort config = case configGet "port" config of
  Nothing -> Nothing
  Just s  -> case safeRead s of
    Nothing -> Nothing
    Just n  -> if n > 0 && n < 65536
               then Just n
               else Nothing

-- Тестови конфигурации
config1 :: Config
config1 = [("host", "localhost"), ("port", "8080"), ("debug", "true")]

config2 :: Config
config2 = [("host", "localhost")]
```

```haskell
ghci> getPort config1
Just 8080
ghci> getPort config2
Nothing
ghci> configGet "host" config1
Just "localhost"
ghci> configGet "missing" config1
Nothing
```

---

## Пример 2: Валидиране с `Either`

```haskell
data RegistrationForm = RegForm
  { regName  :: String
  , regEmail :: String
  , regAge   :: Int
  } deriving (Show)

validateName :: String -> Either String String
validateName name
  | null name       = Left "Името не може да е празно"
  | length name < 2 = Left "Името трябва да е поне 2 символа"
  | otherwise       = Right name

validateEmail :: String -> Either String String
validateEmail email
  | '@' `notElem` email = Left "Имейлът трябва да съдържа '@'"
  | '.' `notElem` email = Left "Имейлът трябва да съдържа '.'"
  | otherwise           = Right email

validateAge :: Int -> Either String Int
validateAge age
  | age < 0   = Left "Възрастта не може да е отрицателна"
  | age > 150 = Left "Невалидна възраст"
  | age < 18  = Left "Трябва да сте поне на 18 години"
  | otherwise = Right age

-- Валидиране стъпка по стъпка
register :: String -> String -> Int -> Either String RegistrationForm
register name email age =
  case validateName name of
    Left err -> Left err
    Right validName ->
      case validateEmail email of
        Left err -> Left err
        Right validEmail ->
          case validateAge age of
            Left err -> Left err
            Right validAge -> Right (RegForm validName validEmail validAge)
```

```haskell
ghci> register "Иван" "ivan@mail.bg" 25
Right (RegForm {regName = "Иван", regEmail = "ivan@mail.bg", regAge = 25})
ghci> register "" "ivan@mail.bg" 25
Left "Името не може да е празно"
ghci> register "Иван" "invalid" 25
Left "Имейлът трябва да съдържа '@'"
ghci> register "Иван" "ivan@mail.bg" 15
Left "Трябва да сте поне на 18 години"
```

> 💡 Забележете колко вложен е кодът - монадите ще ни помогнат да го опростим!

---

## Пример 3: `mapMaybe` за обработка на данни

```haskell
import Data.Maybe (mapMaybe, catMaybes, fromMaybe)

-- Парсване на числа от списък от низове
parseNumbers :: [String] -> [Int]
parseNumbers = mapMaybe safeRead

-- Безопасно деление на всички елементи
safeDivAll :: [Double] -> Double -> [Maybe Double]
safeDivAll xs d = map (\x -> safeDiv x d) xs
  where
    safeDiv _ 0 = Nothing
    safeDiv x y = Just (x / y)

-- Средна стойност на успешно парснатите числа
averageValid :: [String] -> Maybe Double
averageValid strs =
  let nums = parseNumbers strs
  in if null nums
     then Nothing
     else Just (fromIntegral (sum nums) / fromIntegral (length nums))
```

```haskell
ghci> parseNumbers ["1", "abc", "3", "xyz", "5"]
[1, 3, 5]
ghci> catMaybes [Just 1, Nothing, Just 3, Nothing, Just 5]
[1, 3, 5]
ghci> averageValid ["10", "abc", "20", "30"]
Just 20.0
ghci> averageValid ["abc", "xyz"]
Nothing
```
