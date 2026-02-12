# Седмица 14 - Примери

## Пример 1: Затваряния и генератори

```scheme
; Генератор на ID-та
(define (make-id-generator prefix)
  (let ((counter 0))
    (lambda ()
      (set! counter (+ counter 1))
      (string-append prefix (number->string counter)))))

(define gen-user (make-id-generator "USER-"))
(define gen-order (make-id-generator "ORD-"))

(gen-user)   ; → "USER-1"
(gen-user)   ; → "USER-2"
(gen-order)  ; → "ORD-1"
(gen-user)   ; → "USER-3"

; Мемоизация
(define (make-memoized f)
  (let ((cache '()))
    (lambda (x)
      (let ((cached (assoc x cache)))
        (if cached
            (cdr cached)
            (let ((result (f x)))
              (set! cache (cons (cons x result) cache))
              result))))))

(define memo-fib
  (make-memoized
    (lambda (n)
      (if (< n 2) n
          (+ (memo-fib (- n 1))
             (memo-fib (- n 2)))))))

(memo-fib 30)  ; → 832040 (бързо благодарение на мемоизацията!)
```

---

## Пример 2: Потоци от нулата (без вградени)

```scheme
; Ръчна реализация с cons + thunk
(define-syntax cons-stream
  (syntax-rules ()
    ((cons-stream head tail)
     (cons head (lambda () tail)))))

(define stream-car car)
(define (stream-cdr s) ((cdr s)))
(define the-empty-stream '())
(define (stream-null? s) (null? s))

; Основни операции
(define (stream-take n s)
  (if (or (= n 0) (stream-null? s))
      '()
      (cons (stream-car s)
            (stream-take (- n 1) (stream-cdr s)))))

(define (stream-map f s)
  (if (stream-null? s)
      the-empty-stream
      (cons-stream (f (stream-car s))
                   (stream-map f (stream-cdr s)))))

(define (stream-filter pred s)
  (cond
    ((stream-null? s) the-empty-stream)
    ((pred (stream-car s))
     (cons-stream (stream-car s)
                  (stream-filter pred (stream-cdr s))))
    (else (stream-filter pred (stream-cdr s)))))

; Безкрайни потоци
(define (integers-from n)
  (cons-stream n (integers-from (+ n 1))))

(define nats (integers-from 0))

(stream-take 10 nats)  ; → (0 1 2 3 4 5 6 7 8 9)

(stream-take 5 (stream-map (lambda (x) (* x x)) nats))
; → (0 1 4 9 16)

(stream-take 5 (stream-filter odd? nats))
; → (1 3 5 7 9)
```

---

## Пример 3: Модел на средите - стъпка по стъпка

Нека проследим оценяването на:

```scheme
(define (make-withdraw balance)
  (lambda (amount)
    (if (>= balance amount)
        (begin (set! balance (- balance amount))
               balance)
        "Недостатъчно")))

(define w1 (make-withdraw 100))
(define w2 (make-withdraw 50))
(w1 30)   ; → ?
(w2 20)   ; → ?
(w1 40)   ; → ?
```

**Стъпка 1**: Глобална среда

```
Global: [make-withdraw → <procedure>, ...]
```

**Стъпка 2**: `(define w1 (make-withdraw 100))`

```
Global: [make-withdraw → <proc>, w1 → <proc>, ...]
                                    ↑
E1: [balance → 100]  ← средата на w1
```

**Стъпка 3**: `(define w2 (make-withdraw 50))`

```
Global: [make-withdraw → <proc>, w1 → <proc>, w2 → <proc>]
                                    ↑                ↑
E1: [balance → 100]            E2: [balance → 50]
```

**Стъпка 4**: `(w1 30)` - създава рамка в E1

```
E1: [balance → 100]
      ↑
E3: [amount → 30]   →  balance >= 30? Да → balance = 100 - 30 = 70
```

Резултат: `70`. E1: `[balance → 70]`

**Стъпка 5**: `(w2 20)` - създава рамка в E2

```
E2: [balance → 50]
      ↑
E4: [amount → 20]   →  balance >= 20? Да → balance = 50 - 20 = 30
```

Резултат: `30`. E2: `[balance → 30]`

**Стъпка 6**: `(w1 40)` - създава нова рамка в E1

```
E1: [balance → 70]   (от стъпка 4)
      ↑
E5: [amount → 40]   →  balance >= 40? Да → balance = 70 - 40 = 30
```

Резултат: `30`. E1: `[balance → 30]`

> 💡 `w1` и `w2` имат **отделни среди** - промяната на `balance` в едната не засяга другата.
