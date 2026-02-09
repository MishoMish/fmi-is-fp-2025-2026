# Седмица 10 — Задачи за в час

## Задача 1: `andThen`

Реализирайте `>>=` за `Maybe` и `Either`:

```haskell
andThenMaybe :: Maybe a -> (a -> Maybe b) -> Maybe b
andThenEither :: Either e a -> (a -> Either e b) -> Either e b
```

Пренапишете `process` от теорията с `andThenMaybe`.

---

## Задача 2: `safePath`

Използвайте `do`-нотация с `Maybe`, за да реализирате:

```haskell
type FileSystem = [(String, Either String [(String, String)])]
-- (директория, Left файл_съдържание | Right поддиректории)

safePath :: FileSystem -> [String] -> Maybe String
```

`safePath` следва път от имена на директории/файлове и връща съдържанието на крайния файл.

---

## Задача 3: `collectResults`

```haskell
collectResults :: [Maybe a] -> Maybe [a]
```

Ако **всички** елементи са `Just`, събира ги в `Just [...]`. Ако **поне един** е `Nothing`, връща `Nothing`.

```haskell
>>> collectResults [Just 1, Just 2, Just 3]
Just [1, 2, 3]
>>> collectResults [Just 1, Nothing, Just 3]
Nothing
```

> 💡 Реализирайте с `do`-нотация и рекурсия.

---

## Задача 4: List monad

Използвайте `do`-нотация със списъци, за да генерирате:

**а)** Всички Питагорови тройки до n:
```haskell
pythTriples :: Int -> [(Int, Int, Int)]
```

**б)** Всички пермутации на списък:
```haskell
permutations' :: Eq a => [a] -> [[a]]
```

---

## Задача 5: Pipeline с `Either`

Дефинирайте pipeline от трансформации, всяка от които може да се провали:

```haskell
type Pipeline a = [a -> Either String a]

runPipeline :: Pipeline a -> a -> Either String a
```

```haskell
>>> let pipe = [\x -> if x > 0 then Right (x * 2) else Left "negative"
               ,\x -> if x < 100 then Right (x + 1) else Left "too large"]
>>> runPipeline pipe 5
Right 11
>>> runPipeline pipe (-3)
Left "negative"
>>> runPipeline pipe 60
Left "too large"
```
