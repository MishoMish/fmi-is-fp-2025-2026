# Потоци, затваряния и модел на средите

## 1. Затваряния (Closures)

**Затваряне** е функция, която „запомня" средата, в която е създадена:

```scheme
(define (make-adder n)
  (lambda (x) (+ x n)))

(define add5 (make-adder 5))
(define add10 (make-adder 10))

(add5 3)    ; → 8
(add10 3)   ; → 13
```

> 💡 `add5` помни, че `n = 5`, дори след като `make-adder` е приключила. Това е затваряне.

---

## 2. `set!` — мутация

В Scheme можем да **променяме** стойности с `set!`:

```scheme
(define counter 0)
(set! counter (+ counter 1))
counter  ; → 1
```

### Брояч чрез затваряне

```scheme
(define (make-counter)
  (let ((count 0))
    (lambda ()
      (set! count (+ count 1))
      count)))

(define c1 (make-counter))
(define c2 (make-counter))

(c1)  ; → 1
(c1)  ; → 2
(c1)  ; → 3
(c2)  ; → 1  (отделен брояч!)
(c2)  ; → 2
```

> 💡 Всяко извикване на `make-counter` създава **нова среда** с отделно `count`.

---

## 3. Модел на средите

Средата (environment) е **верига от рамки** (frames), всяка съдържаща свързвания на имена със стойности.

```
Глобална среда: [+ → <proc>, * → <proc>, make-counter → <proc>, ...]
       ↑
Рамка от (make-counter): [count → 0]
       ↑
Рамка от (lambda ()): [<тяло>]
```

При оценяване на израз:
1. Търси се името в **текущата** рамка
2. Ако не е намерено → търси се в **обхващащата** среда
3. Продължава нагоре до глобалната среда

---

## 4. Потоци (Streams)

Потокът е **мързелив списък** — елементите се изчисляват при поискване.

### `delay` и `force`

```scheme
; delay обвива израз без да го оценява
; force оценява отложен израз

(define (my-delay expr) (lambda () expr))
(define (my-force promise) (promise))

; В Racket delay/force са вградени:
(define lazy-val (delay (begin (display "изчислявам!") 42)))
(force lazy-val)  ; отпечатва "изчислявам!" и връща 42
(force lazy-val)  ; директно 42 (кеширано)
```

### Дефиниция на поток

```scheme
; Потокът е двойка: (глава . отложена-опашка)
(define (stream-cons x lazy-tail) (cons x lazy-tail))
(define (stream-car s) (car s))
(define (stream-cdr s) (force (cdr s)))
(define stream-null '())
(define (stream-null? s) (null? s))
```

### С вградения `stream` на Racket:

```scheme
#lang racket

; Безкраен поток от единици
(define ones (stream-cons 1 ones))

; Натурални числа
(define (integers-from n)
  (stream-cons n (integers-from (+ n 1))))

(define nats (integers-from 0))

; Вземане на първите n елемента
(define (stream-take n s)
  (if (or (= n 0) (stream-empty? s))
      '()
      (cons (stream-first s)
            (stream-take (- n 1) (stream-rest s)))))

(stream-take 10 nats)  ; → (0 1 2 3 4 5 6 7 8 9)
```

---

## 5. Операции с потоци

```scheme
; map за потоци
(define (stream-map f s)
  (if (stream-empty? s)
      empty-stream
      (stream-cons (f (stream-first s))
                   (stream-map f (stream-rest s)))))

; filter за потоци
(define (stream-filter pred s)
  (cond
    ((stream-empty? s) empty-stream)
    ((pred (stream-first s))
     (stream-cons (stream-first s)
                  (stream-filter pred (stream-rest s))))
    (else (stream-filter pred (stream-rest s)))))

; Примери:
(define evens (stream-filter even? nats))
(define squares (stream-map (lambda (x) (* x x)) nats))

(stream-take 5 evens)    ; → (0 2 4 6 8)
(stream-take 5 squares)  ; → (0 1 4 9 16)
```

---

## 6. Класически примери с потоци

### Решето на Ератостен

```scheme
(define (sieve s)
  (let ((p (stream-first s)))
    (stream-cons p
      (sieve (stream-filter
               (lambda (x) (not (= 0 (modulo x p))))
               (stream-rest s))))))

(define primes (sieve (integers-from 2)))

(stream-take 10 primes)  ; → (2 3 5 7 11 13 17 19 23 29)
```

### Числа на Фибоначи

```scheme
(define (fib-stream a b)
  (stream-cons a (fib-stream b (+ a b))))

(define fibs (fib-stream 0 1))

(stream-take 10 fibs)  ; → (0 1 1 2 3 5 8 13 21 34)
```

### ZipWith за потоци

```scheme
(define (stream-zipWith f s1 s2)
  (if (or (stream-empty? s1) (stream-empty? s2))
      empty-stream
      (stream-cons (f (stream-first s1) (stream-first s2))
                   (stream-zipWith f (stream-rest s1) (stream-rest s2)))))

; Факториели чрез потоци
(define factorials
  (stream-cons 1 (stream-zipWith * factorials (integers-from 1))))

(stream-take 8 factorials)  ; → (1 1 2 6 24 120 720 5040)
```

---

## 7. Обекти чрез затваряния

Затварянията могат да симулират **обекти** с вътрешно състояние:

```scheme
(define (make-account balance)
  (define (deposit amount)
    (set! balance (+ balance amount))
    balance)
  (define (withdraw amount)
    (if (>= balance amount)
        (begin (set! balance (- balance amount)) balance)
        "Недостатъчен баланс"))
  (define (get-balance) balance)
  (define (dispatch msg)
    (cond
      ((eq? msg 'deposit) deposit)
      ((eq? msg 'withdraw) withdraw)
      ((eq? msg 'balance) (get-balance))
      (else (error "Непознато съобщение" msg))))
  dispatch)

(define acc (make-account 100))
((acc 'deposit) 50)   ; → 150
((acc 'withdraw) 30)  ; → 120
(acc 'balance)        ; → 120
```

---

## Обобщение

| Концепция | Описание | Пример |
|-----------|----------|--------|
| Затваряне | Функция + запомнена среда | `(make-adder 5)` |
| `set!` | Мутация на променлива | `(set! x (+ x 1))` |
| Модел на средите | Верига от рамки | Глобална → Локална → ... |
| `delay`/`force` | Отложено изчисление | `(delay expr)` |
| Поток | Мързелив безкраен списък | `(stream-cons 1 ones)` |
| `stream-map` | Map за потоци | `(stream-map f s)` |
| `stream-filter` | Filter за потоци | `(stream-filter pred s)` |
