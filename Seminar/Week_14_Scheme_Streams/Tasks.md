# Седмица 14 - Задачи за в час

## Задача 1: Затваряния

**а)** Напишете `make-accumulator`, който създава акумулатор:

```scheme
(define acc (make-accumulator 10))
(acc 5)    ; → 15
(acc 10)   ; → 25
(acc -3)   ; → 22
```

**б)** Напишете `make-monitored`, който обвива функция и брои извикванията:

```scheme
(define mf (make-monitored sqrt))
(mf 100)           ; → 10
(mf 25)            ; → 5
(mf 'how-many-calls?)  ; → 2
(mf 'reset-count)      ; → 0
```

---

## Задача 2: Потоци - основни

Реализирайте (ако не използвате Racket streams):

```scheme
(define (stream-take n s) ...)
(define (stream-map f s) ...)
(define (stream-filter pred s) ...)
```

Тествайте с `nats` и `(stream-take 10 (stream-filter even? nats))`.

---

## Задача 3: Безкрайни потоци

Дефинирайте:

```scheme
(define ones ...)            ; 1, 1, 1, ...
(define nats ...)            ; 0, 1, 2, 3, ...
(define powers-of-2 ...)     ; 1, 2, 4, 8, 16, ...
(define factorials ...)      ; 1, 1, 2, 6, 24, 120, ...
```

Проверете с `stream-take`.

---

## Задача 4: Модел на средите

Начертайте диаграма на средите за следния код:

```scheme
(define (make-pair x y)
  (define (dispatch msg)
    (cond
      ((eq? msg 'first) x)
      ((eq? msg 'second) y)
      ((eq? msg 'set-first!) (lambda (v) (set! x v)))
      ((eq? msg 'set-second!) (lambda (v) (set! y v)))))
  dispatch)

(define p (make-pair 3 4))
(p 'first)                   ; → ?
((p 'set-first!) 10)
(p 'first)                   ; → ?
```

---

## Задача 5: Решето на Ератостен

Реализирайте поток от прости числа чрез решетото на Ератостен:

```scheme
(define primes (sieve (integers-from 2)))
```

```scheme
(stream-take 15 primes)
; → (2 3 5 7 11 13 17 19 23 29 31 37 41 43 47)
```
