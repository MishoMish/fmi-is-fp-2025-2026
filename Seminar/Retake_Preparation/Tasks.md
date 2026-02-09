# Подготовка за поправителен изпит

## Обхват: Целият курс + типични грешки

---

## Чести грешки и съвети

### Haskell

1. **Забравяне на базов случай** в рекурсията → безкраен цикъл
2. **`head []` и `tail []`** → винаги проверявайте за празен списък
3. **`foldl` vs `foldr`** → `foldl` е стриктно ляво, `foldr` може да работи с безкрайни списъци
4. **Типови грешки при `div` vs `/`** → `div` за `Int`, `/` за `Double`
5. **`return` НЕ спира изпълнението** → просто обвива в монадичен контекст
6. **Pattern matching ред** → специфичните случаи отгоре, `_` отдолу

### Scheme

1. **Забравяне на скоби** → `(f x)` НЕ `f(x)`
2. **`'()` vs `()`** → `'()` е празен списък, `()` е грешка
3. **`eq?` vs `equal?`** → `eq?` за символи/числа, `equal?` за списъци
4. **`set!` не връща стойност** → `(begin (set! x 5) x)`
5. **Потоци: забравяне на delay** → без delay потоците не са мързеливи

---

## Задача 1: Haskell — Списъци и рекурсия

Напишете `spiral :: [[a]] -> [a]`, която обхожда матрица по спирала (от горния ляв ъгъл, по часовниковата стрелка):

```haskell
>>> spiral [[1,2,3],[4,5,6],[7,8,9]]
[1,2,3,6,9,8,7,4,5]
```

---

## Задача 2: Haskell — HOF и композиция

Напишете `pipeline :: [a -> a] -> a -> a`, която прилага списък от функции последователно:

```haskell
>>> pipeline [(+1), (*2), (subtract 3)] 5
9
```

Реализирайте и с `foldr`, и с `foldl`. Каква е разликата?

---

## Задача 3: Haskell — ADT

Дефинирайте тип за **файлова система**:

```haskell
data FSItem = File String String      -- име, съдържание
            | Dir String [FSItem]     -- име, деца
```

Напишете:
- `findFile :: String -> FSItem -> Maybe String` — търси файл по име, връща съдържание
- `totalSize :: FSItem -> Int` — общ размер (сума от `length` на всички файлове)
- `listAll :: FSItem -> [String]` — всички пътища

---

## Задача 4: Haskell — Монади

Напишете парсер за прости SQL заявки, използвайки `Either String`:

```haskell
data Query = Select [String] String (Maybe Condition)
data Condition = Eq String String | Gt String String

parseQuery :: String -> Either String Query
```

```haskell
>>> parseQuery "SELECT name, age FROM students WHERE age > 20"
Right (Select ["name","age"] "students" (Just (Gt "age" "20")))
```

---

## Задача 5: Haskell — Дървета

Напишете функция `treeZip :: Tree a -> Tree b -> Tree (a, b)`, която обединява две дървета:

```haskell
>>> treeZip (Node (leaf 1) 2 (leaf 3)) (Node (leaf "a") "b" (leaf "c"))
Node (leaf (1,"a")) (2,"b") (leaf (3,"c"))
```

---

## Задача 6: Scheme — Затваряния

Напишете `make-queue`, реализиращ опашка чрез две списъка и `set!`:

```scheme
(define q (make-queue))
((q 'enqueue) 1)
((q 'enqueue) 2)
((q 'enqueue) 3)
(q 'dequeue)     ; → 1
(q 'dequeue)     ; → 2
(q 'front)       ; → 3
```

---

## Задача 7: Scheme — Потоци

Напишете поток, който генерира редицата на Колац за дадено начално число:

```scheme
(define (collatz-stream n) ...)

(stream-take 10 (collatz-stream 6))
; → (6 3 10 5 16 8 4 2 1 1)
```

---

## Задача 8: Модел на средите

Проследете оценяването на:

```scheme
(define (make-object methods)
  (lambda (msg . args)
    (let ((method (assoc msg methods)))
      (if method
          (apply (cdr method) args)
          (error "Unknown method" msg)))))

(define counter
  (let ((count 0))
    (make-object
      (list
        (cons 'inc (lambda () (set! count (+ count 1)) count))
        (cons 'get (lambda () count))))))

(counter 'inc)
(counter 'inc)
(counter 'get)
```

Начертайте всички среди и рамки.
