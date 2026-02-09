# Седмица 10 — Задачи за домашна работа

## Задача 1: `mapM'` и `forM'`

```haskell
mapM'  :: (a -> Maybe b) -> [a] -> Maybe [b]
forM'  :: [a] -> (a -> Maybe b) -> Maybe [b]
```

```haskell
>>> mapM' (\x -> if even x then Just (x `div` 2) else Nothing) [2, 4, 6]
Just [1, 2, 3]
>>> mapM' (\x -> if even x then Just (x `div` 2) else Nothing) [2, 3, 6]
Nothing
```

---

## Задача 2: `filterM'`

```haskell
filterM' :: (a -> Maybe Bool) -> [a] -> Maybe [a]
```

Ако предикатът върне `Nothing` за **поне един** елемент, цялото филтриране е `Nothing`.

```haskell
>>> filterM' (\x -> if x > 0 then Just (even x) else Nothing) [2, 4, 3, 6]
Just [2, 4, 6]
>>> filterM' (\x -> if x > 0 then Just (even x) else Nothing) [2, -1, 6]
Nothing
```

---

## Задача 3: State-подобна монада

Реализирайте прост брояч с `Maybe`:

```haskell
type State s a = s -> Maybe (a, s)

get :: State s s
put :: s -> State s ()
modify :: (s -> s) -> State s ()
runState :: State s a -> s -> Maybe (a, s)

-- Използвайте за:
counter :: State Int [Int]
-- Увеличава брояча 5 пъти и връща историята на стойностите
```

---

## Задача 4: Парсер с `Either`

Напишете мини-парсер за аритметични изрази, използвайки `Either`:

```haskell
data Token = TNum Double | TPlus | TMinus | TMul | TDiv | TLParen | TRParen
  deriving (Show)

tokenize :: String -> Either String [Token]
```

```haskell
>>> tokenize "3 + 4.5 * (2 - 1)"
Right [TNum 3.0, TPlus, TNum 4.5, TMul, TLParen, TNum 2.0, TMinus, TNum 1.0, TRParen]
>>> tokenize "3 + @ 4"
Left "Неочакван символ: '@'"
```

---

## Задача 5: `Writer`-подобна монада

Реализирайте изчисления с логване:

```haskell
type Writer w a = (a, [w])  -- (резултат, лог)

tell :: w -> Writer w ()
tell msg = ((), [msg])

writerBind :: Writer w a -> (a -> Writer w b) -> Writer w b
writerBind (a, log1) f = let (b, log2) = f a in (b, log1 ++ log2)

-- Пример: факториел с логване на стъпки
factWithLog :: Int -> Writer String Int
```

```haskell
>>> factWithLog 5
(120, ["5 * fact(4)", "4 * fact(3)", "3 * fact(2)", "2 * fact(1)", "1 = 1"])
```

---

## 🌟 Бонус: `powerset` чрез List монада

Използвайте List монадата (`do`-нотация), за да генерирате степенното множество:

```haskell
powerset :: [a] -> [[a]]
```

```haskell
>>> powerset [1, 2, 3]
[[],[3],[2],[2,3],[1],[1,3],[1,2],[1,2,3]]
```

> 💡 Подсказка: За всеки елемент, решаваме дали да го включим или не — `[True, False]`.
