# Седмица 7 - Задачи за домашна работа

## Задача 1: `MyList`

Дефинирайте свой тип за списък:

```haskell
data MyList a = Nil | Cons a (MyList a)
  deriving (Show)
```

Имплементирайте:

- `fromList :: [a] -> MyList a`
- `toList :: MyList a -> [a]`
- `myLength :: MyList a -> Int`
- `myMap :: (a -> b) -> MyList a -> MyList b`
- `myFilter :: (a -> Bool) -> MyList a -> MyList a`

---

## Задача 2: `Nat` (естествени числа)

```haskell
data Nat = Zero | Succ Nat
  deriving (Show)
```

Имплементирайте:

- `toNat :: Int -> Nat` (за неотрицателни числа)
- `fromNat :: Nat -> Int`
- `addNat :: Nat -> Nat -> Nat`
- `mulNat :: Nat -> Nat -> Nat`

```haskell
>>> fromNat (addNat (toNat 3) (toNat 4))
7
```

---

## Задача 3: `CardGame`

Моделирайте карти за игра:

```haskell
data Suit = Hearts | Diamonds | Clubs | Spades
  deriving (Show, Eq)

data Rank = Two | Three | Four | Five | Six | Seven | Eight
          | Nine | Ten | Jack | Queen | King | Ace
  deriving (Show, Eq, Ord, Enum, Bounded)

data Card = Card { rank :: Rank, suit :: Suit }
  deriving (Show, Eq)
```

Напишете:

- `fullDeck :: [Card]` - всички 52 карти
- `cardValue :: Card -> Int` - стойност (2-10, J/Q/K=10, A=11)
- `handValue :: [Card] -> Int` - сума на стойностите
- `isFlush :: [Card] -> Bool` - всички карти от една боя

---

## Задача 4: `JSON`

Дефинирайте тип за JSON стойности:

```haskell
data JSON = JNull
           | JBool Bool
           | JNum Double
           | JStr String
           | JArr [JSON]
           | JObj [(String, JSON)]
  deriving (Show, Eq)
```

Напишете:

- `prettyPrint :: JSON -> String` - форматиран текстов изход
- `jsonGet :: String -> JSON -> Maybe JSON` - достъп до поле на обект

```haskell
>>> jsonGet "name" (JObj [("name", JStr "Ivan"), ("age", JNum 20)])
Just (JStr "Ivan")
```

---

## 🌟 Бонус: `Expr` с променливи

Разширете типа `Expr` с поддръжка на променливи и let-изрази:

```haskell
data Expr = Lit Double
          | Var String
          | Add Expr Expr
          | Mul Expr Expr
          | Let String Expr Expr  -- let x = e1 in e2

type Env = [(String, Double)]

eval :: Env -> Expr -> Maybe Double
```

```haskell
>>> eval [] (Let "x" (Lit 3) (Add (Var "x") (Lit 2)))
Just 5.0
```
