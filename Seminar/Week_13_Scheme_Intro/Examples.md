# Седмица 13 - Примери

## Пример 1: Основни функции за списъци

```scheme
; Обръщане на списък
(define (my-reverse lst)
  (define (helper lst acc)
    (if (null? lst) acc
        (helper (cdr lst) (cons (car lst) acc))))
  (helper lst '()))

; Конкатенация
(define (my-append lst1 lst2)
  (if (null? lst1) lst2
      (cons (car lst1) (my-append (cdr lst1) lst2))))

; Принадлежност
(define (my-member x lst)
  (cond
    ((null? lst) #f)
    ((equal? x (car lst)) #t)
    (else (my-member x (cdr lst)))))

; Премахване на дубликати
(define (remove-duplicates lst)
  (cond
    ((null? lst) '())
    ((my-member (car lst) (cdr lst))
     (remove-duplicates (cdr lst)))
    (else (cons (car lst) (remove-duplicates (cdr lst))))))
```

```scheme
(my-reverse '(1 2 3 4))        ; → (4 3 2 1)
(my-append '(1 2) '(3 4 5))    ; → (1 2 3 4 5)
(my-member 3 '(1 2 3 4))       ; → #t
(remove-duplicates '(1 2 1 3 2)) ; → (1 3 2)
```

---

## Пример 2: Функции от по-висок ред от нулата

```scheme
; Наша версия на map
(define (my-map f lst)
  (if (null? lst) '()
      (cons (f (car lst))
            (my-map f (cdr lst)))))

; Наша версия на filter
(define (my-filter pred lst)
  (cond
    ((null? lst) '())
    ((pred (car lst))
     (cons (car lst) (my-filter pred (cdr lst))))
    (else (my-filter pred (cdr lst)))))

; foldr (reduce отдясно)
(define (my-foldr f init lst)
  (if (null? lst) init
      (f (car lst)
         (my-foldr f init (cdr lst)))))

; foldl (reduce отляво)
(define (my-foldl f acc lst)
  (if (null? lst) acc
      (my-foldl f (f acc (car lst)) (cdr lst))))
```

```scheme
(my-map (lambda (x) (* x x)) '(1 2 3 4 5))
; → (1 4 9 16 25)

(my-filter (lambda (x) (> x 3)) '(1 5 2 8 3))
; → (5 8)

(my-foldr + 0 '(1 2 3 4 5))       ; → 15
(my-foldr cons '() '(1 2 3))      ; → (1 2 3)
(my-foldl + 0 '(1 2 3 4 5))       ; → 15
(my-foldl (lambda (acc x) (cons x acc)) '() '(1 2 3))  ; → (3 2 1)
```

---

## Пример 3: Quicksort в Scheme

```scheme
(define (quicksort lst)
  (if (or (null? lst) (null? (cdr lst)))
      lst
      (let* ((pivot (car lst))
             (rest  (cdr lst))
             (less  (filter (lambda (x) (< x pivot)) rest))
             (equal (filter (lambda (x) (= x pivot)) rest))
             (greater (filter (lambda (x) (> x pivot)) rest)))
        (append (quicksort less)
                (cons pivot (append equal (quicksort greater)))))))
```

```scheme
(quicksort '(5 2 8 1 9 3))  ; → (1 2 3 5 8 9)
(quicksort '())               ; → ()
(quicksort '(1))              ; → (1)
```

---

## Пример 4: Асоциативен списък (речник)

```scheme
; Търсене по ключ
(define (assoc-get key alist)
  (cond
    ((null? alist) #f)
    ((equal? key (car (car alist)))
     (cdr (car alist)))
    (else (assoc-get key (cdr alist)))))

; Добавяне/обновяване
(define (assoc-set key value alist)
  (cons (cons key value)
        (filter (lambda (pair) (not (equal? key (car pair)))) alist)))

; Тест
(define phonebook
  (list (cons "Ivan" "0888123")
        (cons "Maria" "0899456")))

(assoc-get "Ivan" phonebook)       ; → "0888123"
(assoc-get "Pesho" phonebook)      ; → #f
(assoc-set "Pesho" "0877789" phonebook)
; → (("Pesho" . "0877789") ("Ivan" . "0888123") ("Maria" . "0899456"))
```
