# Седмица 11 — Примери

## Пример 1: Интерактивна програма — число за отгатване

```haskell
import System.Random (randomRIO)  -- нужен е пакет random

main :: IO ()
main = do
  putStrLn "Мисля си число от 1 до 100. Познай го!"
  secret <- randomRIO (1, 100) :: IO Int
  guessLoop secret 0

guessLoop :: Int -> Int -> IO ()
guessLoop secret attempts = do
  putStr "Твоят опит: "
  input <- getLine
  let guess = read input :: Int
  let attempts' = attempts + 1
  if guess == secret
    then putStrLn ("Браво! Позна от " ++ show attempts' ++ " опита!")
    else do
      if guess < secret
        then putStrLn "Твърде малко!"
        else putStrLn "Твърде голямо!"
      guessLoop secret attempts'
```

---

## Пример 2: Обработка на CSV файл

Имаме файл `students.csv`:
```
Иван,85,90,78
Мария,92,88,95
Петър,65,70,60
```

```haskell
import Data.List (intercalate)

type StudentRecord = (String, [Int])

-- Парсване на CSV ред
parseLine :: String -> StudentRecord
parseLine line = (name, grades)
  where
    parts  = splitOn ',' line
    name   = head parts
    grades = map read (tail parts)

splitOn :: Char -> String -> [String]
splitOn _ [] = [""]
splitOn sep (c:cs)
  | c == sep  = "" : splitOn sep cs
  | otherwise = let (x:xs) = splitOn sep cs in (c:x) : xs

-- Средна оценка
average :: [Int] -> Double
average xs = fromIntegral (sum xs) / fromIntegral (length xs)

-- Четене и анализ
analyzeStudents :: FilePath -> IO ()
analyzeStudents path = do
  content <- readFile path
  let records = map parseLine (lines content)
  
  putStrLn "=== Резултати ==="
  mapM_ printStudent records
  
  putStrLn ("\nСредна оценка на курса: " ++ show (courseAvg records))
  putStrLn ("Най-добър студент: " ++ bestStudent records)
  where
    printStudent (name, grades) =
      putStrLn (name ++ ": " ++ show grades ++ " (среден: " ++ show (average grades) ++ ")")
    
    courseAvg records = average (concatMap snd records)
    
    bestStudent records =
      fst $ foldl1 (\(n1,g1) (n2,g2) ->
        if average g1 >= average g2 then (n1,g1) else (n2,g2)) records

-- Записване на обработени данни
writeReport :: FilePath -> [StudentRecord] -> IO ()
writeReport path records = do
  let header = "Име,Средна оценка,Статус"
  let rows = map formatRecord records
  writeFile path (unlines (header : rows))
  putStrLn ("Записано в " ++ path)
  where
    formatRecord (name, grades) =
      let avg = average grades
          status = if avg >= 75 then "Издържал" else "Не е издържал"
      in name ++ "," ++ show avg ++ "," ++ status
```

---

## Пример 3: Четене на конфигурационен файл

Конфигурационен файл `config.txt`:
```
host=localhost
port=8080
debug=true
max_connections=100
```

```haskell
type Config = [(String, String)]

-- Парсване на конфигурация
parseConfig :: String -> Config
parseConfig = map parseLine . filter (not . null) . lines
  where
    parseLine line = case break (== '=') line of
      (key, '=':value) -> (strip key, strip value)
      (key, _)         -> (strip key, "")
    strip = reverse . dropWhile (== ' ') . reverse . dropWhile (== ' ')

-- Четене с валидация
readConfig :: FilePath -> IO (Either String Config)
readConfig path = do
  content <- readFile path
  let config = parseConfig content
  if null config
    then return (Left "Празен конфигурационен файл")
    else return (Right config)

-- Достъп до стойност с тип
configGetInt :: String -> Config -> Maybe Int
configGetInt key config = do
  val <- lookup key config
  case reads val of
    [(n, "")] -> Just n
    _         -> Nothing

configGetBool :: String -> Config -> Maybe Bool
configGetBool key config = do
  val <- lookup key config
  case val of
    "true"  -> Just True
    "false" -> Just False
    _       -> Nothing

-- Използване
main :: IO ()
main = do
  result <- readConfig "config.txt"
  case result of
    Left err     -> putStrLn ("Грешка: " ++ err)
    Right config -> do
      let host = maybe "localhost" id (lookup "host" config)
      let port = maybe 8080 id (configGetInt "port" config)
      let debug = maybe False id (configGetBool "debug" config)
      putStrLn ("Сървър: " ++ host ++ ":" ++ show port)
      putStrLn ("Debug: " ++ show debug)
```
