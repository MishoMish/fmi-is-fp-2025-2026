# IO и сериализация

## Въведение

В Haskell всички функции са **чисти** — нямат странични ефекти. Но реалните програми четат от клавиатурата, пишат на екрана и работят с файлове. `IO` монадата е „кутия" за изолиране на тези ефекти.

---

## 1. Типът `IO`

```haskell
-- IO a — действие, което при изпълнение може да има странични ефекти
--         и произвежда стойност от тип a

main :: IO ()       -- програма без резултат
getLine :: IO String    -- чете ред от stdin
putStrLn :: String -> IO ()  -- печата ред на stdout
```

> 💡 `IO ()` означава „действие с ефекти, което не връща интересен резултат" (аналог на `void`).

---

## 2. Основни IO операции

### Вход

```haskell
getLine  :: IO String          -- чете ред
getChar  :: IO Char            -- чете символ
readLn   :: Read a => IO a     -- чете и парсва
getContents :: IO String       -- чете целия stdin (мързеливо)
```

### Изход

```haskell
putStr    :: String -> IO ()       -- печата без нов ред
putStrLn  :: String -> IO ()       -- печата с нов ред
print     :: Show a => a -> IO ()  -- show + putStrLn
putChar   :: Char -> IO ()         -- печата символ
```

---

## 3. `do`-нотация за IO

```haskell
main :: IO ()
main = do
  putStrLn "Как се казваш?"
  name <- getLine
  putStrLn ("Здравей, " ++ name ++ "!")

  putStrLn "На колко години си?"
  ageStr <- getLine
  let age = read ageStr :: Int
  putStrLn ("След 10 години ще си на " ++ show (age + 10) ++ ".")
```

### Ключови разлики:
- `x <- action` — изпълнява IO действие, именува резултата
- `let x = expr` — обикновено свързване (без IO)
- `return x` — обвива чиста стойност в IO (НЕ спира изпълнението!)

---

## 4. `return` в IO

```haskell
-- return НЕ е като return в C/Java!
greet :: IO String
greet = do
  putStrLn "Име:"
  name <- getLine
  return ("Здравей, " ++ name)
  -- return просто обвива стойността в IO

main :: IO ()
main = do
  message <- greet    -- изпълняваме greet, получаваме String
  putStrLn message
```

---

## 5. Работа с файлове

```haskell
-- Четене
readFile :: FilePath -> IO String

-- Писане (презаписва!)
writeFile :: FilePath -> String -> IO ()

-- Добавяне
appendFile :: FilePath -> String -> IO ()
```

```haskell
-- Четене и обработка на файл
processFile :: FilePath -> IO ()
processFile path = do
  content <- readFile path
  let linesList = lines content
  let numbered = zipWith (\i l -> show i ++ ": " ++ l) [1..] linesList
  mapM_ putStrLn numbered
```

---

## 6. Полезни IO комбинатори

```haskell
mapM_   :: (a -> IO b) -> [a] -> IO ()     -- map + изпълняване, без резултат
mapM    :: (a -> IO b) -> [a] -> IO [b]    -- map + изпълняване, със резултат
forM_   :: [a] -> (a -> IO b) -> IO ()     -- обратен ред на аргументите
forM    :: [a] -> (a -> IO b) -> IO [b]
sequence_ :: [IO a] -> IO ()                -- изпълнява поредица
when     :: Bool -> IO () -> IO ()          -- условно изпълнение
unless   :: Bool -> IO () -> IO ()
```

```haskell
import Control.Monad (when, forM_)

main :: IO ()
main = do
  forM_ [1..5] $ \i ->
    putStrLn ("Стъпка " ++ show i)
  
  putStrLn "Продължи? (y/n)"
  answer <- getLine
  when (answer == "y") $ putStrLn "Продължаваме!"
```

---

## 7. CSV обработка

```haskell
-- Прост CSV парсер
parseCSV :: String -> [[String]]
parseCSV = map (splitOn ',') . lines

splitOn :: Char -> String -> [String]
splitOn _ [] = [""]
splitOn sep (c:cs)
  | c == sep  = "" : splitOn sep cs
  | otherwise = let (first:rest) = splitOn sep cs
                in (c:first) : rest

-- Четене и обработка на CSV файл
readCSV :: FilePath -> IO [[String]]
readCSV path = do
  content <- readFile path
  return (parseCSV content)

-- Писане на CSV
writeCSV :: FilePath -> [[String]] -> IO ()
writeCSV path rows = writeFile path csvContent
  where
    csvContent = unlines (map (intercalate ",") rows)
    intercalate sep xs = concat (intersperse sep xs)
    intersperse _ []     = []
    intersperse _ [x]    = [x]
    intersperse sep (x:xs) = x : sep : intersperse sep xs
```

---

## 8. Пълна програма

```haskell
import Data.Char (toUpper)

main :: IO ()
main = do
  putStrLn "=== Телефонен указател ==="
  loop []

type PhoneBook = [(String, String)]

loop :: PhoneBook -> IO ()
loop book = do
  putStrLn "\n1) Добави  2) Търси  3) Покажи  4) Запази  5) Изход"
  choice <- getLine
  case choice of
    "1" -> do
      putStr "Име: "
      name <- getLine
      putStr "Телефон: "
      phone <- getLine
      let book' = (name, phone) : book
      putStrLn "Добавено!"
      loop book'
    "2" -> do
      putStr "Търси име: "
      name <- getLine
      case lookup name book of
        Just phone -> putStrLn ("Телефон: " ++ phone)
        Nothing    -> putStrLn "Не е намерен."
      loop book
    "3" -> do
      mapM_ (\(n,p) -> putStrLn (n ++ ": " ++ p)) book
      loop book
    "4" -> do
      let content = unlines [n ++ "," ++ p | (n,p) <- book]
      writeFile "phonebook.csv" content
      putStrLn "Записано!"
      loop book
    "5" -> putStrLn "Довиждане!"
    _   -> do
      putStrLn "Невалиден избор."
      loop book
```

---

## Обобщение

| Функция | Тип | Описание |
|---------|-----|----------|
| `getLine` | `IO String` | Чете ред |
| `putStrLn` | `String -> IO ()` | Печата ред |
| `print` | `Show a => a -> IO ()` | `show` + `putStrLn` |
| `readFile` | `FilePath -> IO String` | Чете файл |
| `writeFile` | `FilePath -> String -> IO ()` | Пише файл |
| `return` | `a -> IO a` | Обвива стойност в IO |
| `mapM_` | `(a -> IO b) -> [a] -> IO ()` | `map` + изпълнение |
| `when` | `Bool -> IO () -> IO ()` | Условно изпълнение |
