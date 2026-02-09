# Седмица 13 — Задачи за домашна работа

## Задача 1: `flatten`

Напишете `flatten`, която „изравнява" вложен списък:

```scheme
(define (flatten lst) ...)
```

```scheme
(flatten '(1 (2 3) (4 (5 6))))  ; → (1 2 3 4 5 6)
(flatten '(1 2 3))               ; → (1 2 3)
(flatten '())                    ; → ()
```

> 💡 Подсказка: `(pair? x)` проверява дали `x` е двойка (списък).

---

## Задача 2: `my-foldr` и `my-foldl`

```scheme
(define (my-foldr f init lst) ...)
(define (my-foldl f acc lst) ...)
```

Изразете `my-map`, `my-filter` и `my-reverse` чрез `my-foldr`/`my-foldl`.

---

## Задача 3: `insertion-sort`

```scheme
(define (insertion-sort lst) ...)
```

```scheme
(insertion-sort '(5 2 8 1 3))  ; → (1 2 3 5 8)
```

---

## Задача 4: Матрици

Представете матрица като списък от списъци. Реализирайте:

```scheme
(define (matrix-ref m i j) ...)         ; елемент на позиция (i, j)
(define (matrix-transpose m) ...)       ; транспониране
(define (matrix-add m1 m2) ...)         ; събиране
(define (matrix-multiply m1 m2) ...)    ; умножение
```

```scheme
(define m '((1 2) (3 4)))
(matrix-ref m 0 1)         ; → 2
(matrix-transpose m)       ; → ((1 3) (2 4))
(matrix-add m m)           ; → ((2 4) (6 8))
```

---

## Задача 5: Множества

Реализирайте множества като списъци без повторения:

```scheme
(define (set-member? x s) ...)
(define (set-add x s) ...)
(define (set-union s1 s2) ...)
(define (set-intersection s1 s2) ...)
(define (set-difference s1 s2) ...)
(define (set-subset? s1 s2) ...)
```

```scheme
(set-union '(1 2 3) '(3 4 5))         ; → (1 2 3 4 5)
(set-intersection '(1 2 3) '(2 3 4))  ; → (2 3)
(set-difference '(1 2 3) '(2 3 4))    ; → (1)
```

---

## 🌟 Бонус: `eval-expr`

Напишете оценител на прости аритметични S-изрази:

```scheme
(define (eval-expr expr) ...)
```

```scheme
(eval-expr '(+ 3 4))           ; → 7
(eval-expr '(* (+ 1 2) (- 5 3)))  ; → 6
(eval-expr 42)                  ; → 42
```
