# Седмица 13 — Задачи за в час

## Задача 1: Основни функции

Реализирайте:

```scheme
(define (my-length lst) ...)    ; дължина
(define (my-sum lst) ...)       ; сума
(define (my-last lst) ...)      ; последен елемент
(define (my-nth n lst) ...)     ; n-ти елемент (от 0)
```

```scheme
(my-length '(1 2 3))  ; → 3
(my-sum '(1 2 3 4))   ; → 10
(my-last '(1 2 3))    ; → 3
(my-nth 1 '(a b c))   ; → b
```

---

## Задача 2: `my-reverse`

```scheme
(define (my-reverse lst) ...)
```

Реализирайте с **опашкова рекурсия** (акумулатор).

```scheme
(my-reverse '(1 2 3 4))  ; → (4 3 2 1)
```

---

## Задача 3: `my-map` и `my-filter`

```scheme
(define (my-map f lst) ...)
(define (my-filter pred lst) ...)
```

```scheme
(my-map (lambda (x) (* x 2)) '(1 2 3))     ; → (2 4 6)
(my-filter (lambda (x) (> x 3)) '(1 5 2 8)) ; → (5 8)
```

---

## Задача 4: `my-zip`

```scheme
(define (my-zip lst1 lst2) ...)
```

```scheme
(my-zip '(1 2 3) '(a b c))  ; → ((1 . a) (2 . b) (3 . c))
(my-zip '(1 2) '(a b c))    ; → ((1 . a) (2 . b))
```

---

## Задача 5: Числови функции

```scheme
(define (factorial n) ...)
(define (fibonacci n) ...)
(define (is-prime? n) ...)
```

```scheme
(factorial 5)    ; → 120
(fibonacci 10)   ; → 55
(is-prime? 17)   ; → #t
(is-prime? 15)   ; → #f
```
