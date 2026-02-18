# Седмица 8 - Задачи за в час

## Задача 1: `safeIndex`

Напишете `safeIndex :: [a] -> Int -> Maybe a`, която безопасно връща елемент по индекс.

```haskell
>>> safeIndex [10, 20, 30] 1
Just 20
>>> safeIndex [10, 20, 30] 5
Nothing
>>> safeIndex [10, 20, 30] (-1)
Nothing
```

---

## Задача 2: `safeMax` и `safeMin`

```haskell
safeMax :: Ord a => [a] -> Maybe a
safeMin :: Ord a => [a] -> Maybe a
```

```haskell
>>> safeMax [3, 1, 4, 1, 5]
Just 5
>>> safeMax ([] :: [Int])
Nothing
```

---

## Задача 3: `firstJust`

Напишете `firstJust :: [Maybe a] -> Maybe a`, която връща **първата** `Just` стойност от списък.

```haskell
>>> firstJust [Nothing, Just 3, Nothing, Just 5]
Just 3
>>> firstJust [Nothing, Nothing]
Nothing
```

---

## Задача 4: Валидиране на парола

Напишете `validatePassword :: String -> Either String String`:

- Ако дължината е < 8: `Left "Паролата е твърде кратка"`
- Ако няма цифра: `Left "Паролата трябва да съдържа цифра"`
- Ако няма главна буква: `Left "Паролата трябва да съдържа главна буква"`
- Иначе: `Right password`

```haskell
>>> validatePassword "abc"
Left "Паролата е твърде кратка"
>>> validatePassword "abcdefgh"
Left "Паролата трябва да съдържа цифра"
>>> validatePassword "Abcdefg1"
Right "Abcdefg1"
```

---

## Задача 5: `chainMaybe`

Напишете `chainMaybe :: Maybe a -> (a -> Maybe b) -> Maybe b`.

```haskell
>>> chainMaybe (Just 10) (\x -> if x > 0 then Just (x * 2) else Nothing)
Just 20
>>> chainMaybe Nothing (\x -> Just (x * 2))
Nothing
```

Използвайте я за: `safeHead xs `chainMaybe` (\x -> safeDiv 100 x)`
