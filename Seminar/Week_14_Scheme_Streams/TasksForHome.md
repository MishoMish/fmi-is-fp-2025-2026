# Седмица 14 - Задачи за домашна работа

## Задача 1: `stream-zipWith` и `stream-interleave`

```scheme
(define (stream-zipWith f s1 s2) ...)
(define (stream-interleave s1 s2) ...)
```

```scheme
(stream-take 5 (stream-zipWith + nats nats))    ; → (0 2 4 6 8)
(stream-take 8 (stream-interleave nats ones))    ; → (0 1 1 1 2 1 3 1)
```

---

## Задача 2: Поток на Фибоначи (три начина)

Реализирайте потока на Фибоначи по три различни начина:

**а)** С рекурсивна функция `(fib-from a b)`

**б)** С `stream-zipWith` (дефинирайте `fibs` чрез самия себе си)

**в)** С итератор: `(stream-iterate f init)`, който генерира `init, f(init), f(f(init)), ...`

---

## Задача 3: Банкова система

Реализирайте банкова система с множество сметки:

```scheme
(define bank (make-bank))
((bank 'create-account) "Ivan" 1000)
((bank 'create-account) "Maria" 500)
((bank 'deposit) "Ivan" 200)       ; → 1200
((bank 'withdraw) "Maria" 100)     ; → 400
((bank 'transfer) "Ivan" "Maria" 300)  ; → "Преведено"
((bank 'balance) "Ivan")           ; → 900
((bank 'balance) "Maria")          ; → 700
```

---

## Задача 4: Числови потоци

**а)** Дефинирайте поток от **частични суми**: `partial-sums s` генерира $s_0, s_0 + s_1, s_0 + s_1 + s_2, \ldots$

```scheme
(stream-take 5 (partial-sums nats))  ; → (0 1 3 6 10)
```

**б)** Приближение на $\pi$ чрез формулата на Лайбниц:

$$\frac{\pi}{4} = 1 - \frac{1}{3} + \frac{1}{5} - \frac{1}{7} + \ldots$$

```scheme
(define pi-stream ...)
(stream-take 5 pi-stream)
; → (4.0 2.666... 3.466... 2.895... 3.339...)
```

---

## Задача 5: Модел на средите - проследяване

Проследете стъпка по стъпка оценяването и начертайте диаграма на средите за:

```scheme
(define (make-counter)
  (let ((count 0))
    (define (increment) (set! count (+ count 1)) count)
    (define (decrement) (set! count (- count 1)) count)
    (define (reset) (set! count 0) count)
    (define (dispatch msg)
      (cond
        ((eq? msg 'inc) (increment))
        ((eq? msg 'dec) (decrement))
        ((eq? msg 'reset) (reset))
        ((eq? msg 'value) count)))
    dispatch))

(define c (make-counter))
(c 'inc)     ; → ?
(c 'inc)     ; → ?
(c 'dec)     ; → ?
(c 'value)   ; → ?
(c 'reset)   ; → ?
```

---

## 🌟 Бонус: Мини интерпретатор

Напишете интерпретатор за прост Scheme-подобен език, поддържащ:

- Числа и аритметика: `(+ 1 2)`, `(* 3 4)`
- `define`: `(define x 42)`
- `lambda`: `(lambda (x) (+ x 1))`
- `if`: `(if (> x 0) x (- x))`
- Среди: правилна реализация на обхват (scope)

```scheme
(define (my-eval expr env) ...)
(define (my-apply func args env) ...)
```
