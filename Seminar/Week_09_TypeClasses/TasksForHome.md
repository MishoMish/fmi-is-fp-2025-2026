# Седмица 9 - Задачи за домашна работа

## Задача 1: `MyMatrix`

```haskell
data MyMatrix = Mat [[Double]]
```

Напишете инстанции:

- `Show` - отпечатва матрицата ред по ред
- `Eq` - сравнява елемент по елемент
- `Num` - събиране, изваждане, умножение (матрично), `negate`, `abs` (поелементно), `fromInteger` (единична матрица с дадена размерност)

```haskell
>>> Mat [[1,2],[3,4]] + Mat [[5,6],[7,8]]
Mat [[6.0,8.0],[10.0,12.0]]
```

---

## Задача 2: `Convertible`

Дефинирайте типов клас за преобразуване между типове:

```haskell
class Convertible a b where
  convert :: a -> b
```

Напишете инстанции:

- `Convertible Int Double`
- `Convertible String [Char]` (тривиална)
- `Convertible Celsius Fahrenheit` и обратно

```haskell
newtype Celsius    = Celsius Double deriving (Show)
newtype Fahrenheit = Fahrenheit Double deriving (Show)
```

---

## Задача 3: `Collection`

```haskell
class Collection c where
  empty     :: c a
  insert    :: a -> c a -> c a
  member    :: Eq a => a -> c a -> Bool
  toList    :: c a -> [a]
  fromList  :: [a] -> c a
  fromList  = foldr insert empty
```

Напишете инстанции за `[]` и за `Set`:

```haskell
newtype Set a = Set [a] deriving (Show)
-- Set пази елементите уникални!
```

---

## Задача 4: `Functor`-подобен клас

Дефинирайте собствен `Mappable`:

```haskell
class Mappable f where
  myFmap :: (a -> b) -> f a -> f b
```

Напишете инстанции за:

- `[]`
- `Maybe`
- `Either e` (map-ва само `Right`)
- `Pair a` (map-ва втория елемент): `data Pair a b = Pair a b`

---

## Задача 5: `Priority Queue`

```haskell
data PQueue a = PQueue [(Int, a)]  -- (приоритет, стойност)
  deriving (Show)

class PriorityQueue pq where
  emptyPQ  :: pq a
  enqueue  :: Int -> a -> pq a -> pq a
  dequeue  :: pq a -> Maybe (a, pq a)  -- връща елемента с най-нисък приоритет
  peekPQ   :: pq a -> Maybe a
```

Напишете `PriorityQueue` инстанция за `PQueue`.

```haskell
>>> let pq = enqueue 3 "low" $ enqueue 1 "high" $ enqueue 2 "med" (emptyPQ :: PQueue String)
>>> peekPQ pq
Just "high"
>>> case dequeue pq of Just (v, rest) -> (v, peekPQ rest)
("high", Just "med")
```

---

## 🌟 Бонус: `Foldable'`

Дефинирайте типов клас `Foldable'` с метод `myFoldr`:

```haskell
class Foldable' f where
  myFoldr :: (a -> b -> b) -> b -> f a -> b
```

Напишете инстанции за `[]`, `Maybe` и `data Tree a = Leaf | Node (Tree a) a (Tree a)`.

Изразете `toList`, `sum'`, `length'` чрез `myFoldr`.
