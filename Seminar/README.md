# Семинар по Функционално програмиране

**Курс:** Функционално програмиране (ФП)
**Семестър:** Летен 2025–2026
**Факултет:** ФМИ, Софийски университет
**Специалност:** Информационни системи (ИС)

## За този семинар

Семинарът е организиран в 14 седмици и следва тематиката на лекциите. Всяка седмица съдържа:

1. **Теория** - Обяснение на концепциите с примери
2. **Примери** - Анотирани кодови примери в Haskell (и Scheme в последните седмици)
3. **Задачи за в час** - Решават се заедно по време на семинара
4. **Задачи за домашна работа** - Решават се самостоятелно след семинара

> ⚠️ Това е семинар с **практическа насоченост** за ИС студенти. Теорията е фундаментът, но задачите са повече от типичен семинар - разделени на работа в час и домашно.

---

## Седмичен план

### Част I - Основи на Haskell (Седмици 1–3)

<details>
<summary><h3>Седмица 1 - Въведение в Haskell</h3></summary>

#### Тема: GHCi, типове, прости функции, условни изрази

#### Теория (Theory.md)

- Какво е функционално програмиране?
- GHCi среда - инсталация и работа
- Основни типове: `Int`, `Integer`, `Double`, `Bool`, `Char`
- Аритметични операции и оператори
- `if-then-else`, `let`, `where`
- Дефиниране на прости функции
- Типови сигнатури

#### Примери (Examples.md)

- Аритметика в GHCi
- Дефиниране на функции стъпка по стъпка
- `let`/`where` за локални дефиниции

#### Задачи за в час (Tasks.md)

- Прости аритметични функции
- Условни изрази с `if-then-else`

#### Задачи за домашно (TasksForHome.md)

- Допълнителни упражнения с числа и типове

#### Файлове:

- [Theory.md](Week_01_Introduction/Theory.md)
- [Examples.md](Week_01_Introduction/Examples.md)
- [Tasks.md](Week_01_Introduction/Tasks.md)
- [TasksForHome.md](Week_01_Introduction/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 2 - Образци, гардове и кортежи</h3></summary>

#### Тема: Pattern matching, guards, tuples

#### Теория (Theory.md)

- Pattern matching - литерали, променливи, wildcards (`_`)
- Guards (`|` с условия, `otherwise`)
- `where` клаузи с деструктуриране
- Кортежи (tuples): `fst`, `snd`, pattern matching

#### Примери (Examples.md)

- Функции с guards
- Деструктуриране на кортежи
- Комбиниране на patterns и guards

#### Задачи за в час (Tasks.md)

- Функции с pattern matching
- Работа с кортежи

#### Задачи за домашно (TasksForHome.md)

- Комбиниране на guards и patterns

#### Файлове:

- [Theory.md](Week_02_Patterns_Guards/Theory.md)
- [Examples.md](Week_02_Patterns_Guards/Examples.md)
- [Tasks.md](Week_02_Patterns_Guards/Tasks.md)
- [TasksForHome.md](Week_02_Patterns_Guards/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 3 - Рекурсия</h3></summary>

#### Тема: Проста рекурсия, рекурсия по списъци, опашкова рекурсия

#### Теория (Theory.md)

- Рекурсия по числа - базов случай и стъпка
- Рекурсия по списъци
- Натрупващ параметър (акумулатор)
- Опашкова рекурсия (tail recursion)
- Итеративен стил с помощна функция

#### Примери (Examples.md)

- Факториел (обикновен и опашков)
- Сума на елементи (рекурсивно и с акумулатор)
- Обръщане на списък

#### Задачи за в час (Tasks.md)

- Рекурсивни функции върху числа
- Прости рекурсии по списъци

#### Задачи за домашно (TasksForHome.md)

- Опашкови версии на рекурсии
- По-сложни числови задачи

#### Файлове:

- [Theory.md](Week_03_Recursion/Theory.md)
- [Examples.md](Week_03_Recursion/Examples.md)
- [Tasks.md](Week_03_Recursion/Tasks.md)
- [TasksForHome.md](Week_03_Recursion/TasksForHome.md)

</details>

---

### Част II - Списъци и функции от по-висок ред (Седмици 4–6)

<details>
<summary><h3>Седмица 4 - Списъци</h3></summary>

#### Тема: Структура на списъци, операции, рекурсия по списъци

#### Теория (Theory.md)

- Списъци - свързана структура, хомогенност
- Конструкция: `[]`, `:` (cons), `++`
- Основни операции: `head`, `tail`, `last`, `init`, `length`, `null`, `reverse`
- `take`, `drop`, `!!`, `elem`
- Диапазони (ranges): `[1..10]`, `[1,3..20]`
- Низове като `[Char]`

#### Примери (Examples.md)

- Работа с основните функции за списъци
- Pattern matching на списъци: `[]`, `(x:xs)`, `(x:y:zs)`
- Реимплементация на стандартни функции

#### Задачи за в час (Tasks.md)

- Реимплементация на `length`, `elem`, `reverse`
- Прости трансформации на списъци

#### Задачи за домашно (TasksForHome.md)

- По-сложни операции: `zip`, `take`, `drop`, `++`
- Сортирана вмъкване и merge

#### Файлове:

- [Theory.md](Week_04_Lists/Theory.md)
- [Examples.md](Week_04_Lists/Examples.md)
- [Tasks.md](Week_04_Lists/Tasks.md)
- [TasksForHome.md](Week_04_Lists/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 5 - Функции от по-висок ред</h3></summary>

#### Тема: map, filter, foldr, foldl

#### Теория (Theory.md)

- Функции като аргументи
- `map` - трансформация на всеки елемент
- `filter` - филтриране по предикат
- `foldr` - сгъване отдясно
- `foldl` - сгъване отляво
- Реимплементации и разлики между `foldr`/`foldl`

#### Примери (Examples.md)

- Примери с `map`, `filter`, `foldr`, `foldl`
- Изразяване на `map`/`filter` чрез `foldr`
- Практически заявки върху данни

#### Задачи за в час (Tasks.md)

- Реимплементация на `map`, `filter`, `foldr`
- Прости трансформации с HOF

#### Задачи за домашно (TasksForHome.md)

- По-сложни fold задачи
- Комбиниране на map/filter/fold

#### Файлове:

- [Theory.md](Week_05_HigherOrderFunctions/Theory.md)
- [Examples.md](Week_05_HigherOrderFunctions/Examples.md)
- [Tasks.md](Week_05_HigherOrderFunctions/Tasks.md)
- [TasksForHome.md](Week_05_HigherOrderFunctions/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 6 - Ламбди, композиция и list comprehensions</h3></summary>

#### Тема: Lambda функции, (.), curry/uncurry, list comprehensions

#### Теория (Theory.md)

- Lambda синтаксис: `\x -> ...`
- Композиция на функции: `.`
- Currying и partial application
- `curry`, `uncurry`, `flip`
- List comprehensions: `[expr | x <- list, guard]`
- Множествени генератори

#### Примери (Examples.md)

- Ламбди с map/filter
- Композиция за pipeline обработка
- List comprehensions за различни задачи

#### Задачи за в час (Tasks.md)

- Пренаписване на функции с lambda
- List comprehension задачи

#### Задачи за домашно (TasksForHome.md)

- По-сложни comprehensions
- Комбиниране на композиция и HOF

#### Файлове:

- [Theory.md](Week_06_Lambdas_Comprehensions/Theory.md)
- [Examples.md](Week_06_Lambdas_Comprehensions/Examples.md)
- [Tasks.md](Week_06_Lambdas_Comprehensions/Tasks.md)
- [TasksForHome.md](Week_06_Lambdas_Comprehensions/TasksForHome.md)

</details>

---

### Подготовка за Тест 1

<details>
<summary><h3>Тест 1 - Обхват: Седмици 1–6</h3></summary>

Теми: Основи на Haskell, pattern matching, guards, рекурсия, списъци, HOF, ламбди, list comprehensions

- [Tasks.md](Test1_Preparation/Tasks.md)

</details>

---

### Част III - Типове и абстракция (Седмици 7–9)

<details>
<summary><h3>Седмица 7 - Алгебрични типове данни</h3></summary>

#### Тема: data, конструктори, записи, deriving

#### Теория (Theory.md)

- Ключовата дума `data`
- Типове-изброявания (enumerations)
- Конструктори с параметри
- Type synonyms (`type`) vs. data types
- Полиморфни типове
- Records (именувани полета)
- `deriving`: `Eq`, `Ord`, `Show`, `Read`, `Enum`

#### Примери (Examples.md)

- Дефиниране на `Shape`, `Color`, `Student`
- Pattern matching на конструктори
- Records с accessor функции

#### Задачи за в час (Tasks.md)

- Дефиниране на прости типове
- Операции върху custom типове

#### Задачи за домашно (TasksForHome.md)

- По-сложни типове с множество конструктори
- Рекурсивни типове (въведение)

#### Файлове:

- [Theory.md](Week_07_AlgebraicDataTypes/Theory.md)
- [Examples.md](Week_07_AlgebraicDataTypes/Examples.md)
- [Tasks.md](Week_07_AlgebraicDataTypes/Tasks.md)
- [TasksForHome.md](Week_07_AlgebraicDataTypes/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 8 - Maybe, Either и обработка на грешки</h3></summary>

#### Тема: Maybe, Either, case, безопасна обработка на грешки

#### Теория (Theory.md)

- `Maybe a` = `Nothing | Just a`
- Защо `Maybe` вместо `error`?
- Полезни функции: `fromMaybe`, `isJust`, `isNothing`, `catMaybes`, `mapMaybe`
- `Either a b` = `Left a | Right b`
- `case` изрази
- Практическа обработка на грешки

#### Примери (Examples.md)

- Безопасно деление с `Maybe`
- Верижна обработка с pattern matching
- `Either` за грешки с описание

#### Задачи за в час (Tasks.md)

- Рефакториране към `Maybe`
- Прости `Either` функции

#### Задачи за домашно (TasksForHome.md)

- `catMaybes`, `mapMaybe`, `partitionEithers`
- Валидация на входни данни

#### Файлове:

- [Theory.md](Week_08_Maybe_Either/Theory.md)
- [Examples.md](Week_08_Maybe_Either/Examples.md)
- [Tasks.md](Week_08_Maybe_Either/Tasks.md)
- [TasksForHome.md](Week_08_Maybe_Either/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 9 - Типови класове</h3></summary>

#### Тема: Type classes, instances, custom type classes

#### Теория (Theory.md)

- Какво е type class? (`Eq`, `Ord`, `Show`, `Read`, `Num`)
- `class` и `instance` дефиниции
- Дефиниране на собствени type classes
- Default implementations
- Йерархия на type classes

#### Примери (Examples.md)

- `instance Show` за custom тип
- `instance Eq` с custom логика
- Собствен type class `Printable`

#### Задачи за в час (Tasks.md)

- Инстанции за прости типове
- Custom type class

#### Задачи за домашно (TasksForHome.md)

- По-сложни инстанции
- `Semigroup` и `Monoid` въведение

#### Файлове:

- [Theory.md](Week_09_TypeClasses/Theory.md)
- [Examples.md](Week_09_TypeClasses/Examples.md)
- [Tasks.md](Week_09_TypeClasses/Tasks.md)
- [TasksForHome.md](Week_09_TypeClasses/TasksForHome.md)

</details>

---

### Част IV - Монади и IO (Седмици 10–11)

<details>
<summary><h3>Седмица 10 - Монади и do-нотация</h3></summary>

#### Тема: >>=, return, do-нотация, Maybe/Either като монади

#### Теория (Theory.md)

- Проблемът: верижни операции с `Maybe`
- Операторът `>>=` (bind)
- `return` в монаден контекст
- `do`-нотация - синтактична захар
- `let` vs `<-` в `do` блокове

#### Примери (Examples.md)

- Верижни Maybe операции с `>>=`
- Еквивалентност на `>>=` и `do`
- Either монада за обработка на грешки

#### Задачи за в час (Tasks.md)

- Пренаписване на код с `>>=`
- `do` блокове с `Maybe`

#### Задачи за домашно (TasksForHome.md)

- По-сложни монадни вериги
- Комбиниране на `Maybe`/`Either`

#### Файлове:

- [Theory.md](Week_10_Monads_Do/Theory.md)
- [Examples.md](Week_10_Monads_Do/Examples.md)
- [Tasks.md](Week_10_Monads_Do/Tasks.md)
- [TasksForHome.md](Week_10_Monads_Do/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 11 - IO и сериализация</h3></summary>

#### Тема: IO монада, файлов I/O, CSV сериализация

#### Теория (Theory.md)

- `IO` монада - странични ефекти в чист език
- `getLine`, `putStrLn`, `print`
- `readFile`, `writeFile`
- Парсване на текстов вход
- CSV формат - запис и четене
- `show` и `read` за сериализация

#### Примери (Examples.md)

- Интерактивна програма с `do`
- Четене и писане на файл
- CSV парсване на структурирани данни

#### Задачи за в час (Tasks.md)

- Прости IO програми
- Четене на файл и обработка

#### Задачи за домашно (TasksForHome.md)

- CSV сериализация на custom тип
- Интерактивно приложение (менюта)

#### Файлове:

- [Theory.md](Week_11_IO_Serialization/Theory.md)
- [Examples.md](Week_11_IO_Serialization/Examples.md)
- [Tasks.md](Week_11_IO_Serialization/Tasks.md)
- [TasksForHome.md](Week_11_IO_Serialization/TasksForHome.md)

</details>

---

### Подготовка за Тест 2

<details>
<summary><h3>Тест 2 - Обхват: Седмици 7–11</h3></summary>

Теми: Алгебрични типове, Maybe/Either, типови класове, монади, IO, сериализация

- [Tasks.md](Test2_Preparation/Tasks.md)

</details>

---

### Част V - Структури от данни (Седмица 12)

<details>
<summary><h3>Седмица 12 - Двоични дървета</h3></summary>

#### Тема: Индуктивна дефиниция, операции, BST

#### Теория (Theory.md)

- Индуктивна дефиниция: `data Tree a = Empty | Node a (Tree a) (Tree a)`
- Основни операции: `height`, `size`, `countLeaves`
- Обхождания: in-order, pre-order, post-order
- Двоично дърво за търсене (BST): `search`, `insert`
- `Functor` и `Foldable` за дървета

#### Примери (Examples.md)

- Конструиране на дърво
- Рекурсивни операции
- BST вмъкване и търсене

#### Задачи за в час (Tasks.md)

- Базови операции върху дървета
- BST search/insert

#### Задачи за домашно (TasksForHome.md)

- По-сложни дървесни задачи
- BST delete, обхождания

#### Файлове:

- [Theory.md](Week_12_BinaryTrees/Theory.md)
- [Examples.md](Week_12_BinaryTrees/Examples.md)
- [Tasks.md](Week_12_BinaryTrees/Tasks.md)
- [TasksForHome.md](Week_12_BinaryTrees/TasksForHome.md)

</details>

---

### Част VI - Scheme (Седмици 13–14)

<details>
<summary><h3>Седмица 13 - Въведение в Scheme</h3></summary>

#### Тема: Prefix нотация, define, lambda, cond, списъци

#### Теория (Theory.md)

- Scheme - диалект на Lisp
- S-изрази и prefix нотация
- `define`, `lambda`
- `if`, `cond`, `let`, `let*`
- Предикати: `even?`, `odd?`, `zero?`, `null?`
- Двойки и списъци: `cons`, `car`, `cdr`, `list`
- Quoting: `'`
- Рекурсия в Scheme

#### Примери (Examples.md)

- Аритметика и предикати
- Дефиниране на функции
- Работа със списъци

#### Задачи за в час (Tasks.md)

- Прости функции в Scheme
- Рекурсия и списъци

#### Задачи за домашно (TasksForHome.md)

- Функции от по-висок ред в Scheme
- `accumulate` задачи

#### Файлове:

- [Theory.md](Week_13_Scheme_Intro/Theory.md)
- [Examples.md](Week_13_Scheme_Intro/Examples.md)
- [Tasks.md](Week_13_Scheme_Intro/Tasks.md)
- [TasksForHome.md](Week_13_Scheme_Intro/TasksForHome.md)

</details>

<details>
<summary><h3>Седмица 14 - Scheme: потоци и модел на средите</h3></summary>

#### Тема: HOF в Scheme, потоци (streams), модел на средите

#### Теория (Theory.md)

- `map`, `filter`, `foldr` в Scheme
- Потоци (streams): `delay`, `force`, `cons-stream`
- Безкрайни потоци
- `set!` и мутация
- Модел на средите (Environment Model)
- Затваряния (closures) и фабрични функции

#### Примери (Examples.md)

- Потоци: безкрайни поредици
- Затваряния с `let` + `set!` + `lambda`
- Среди и рамки

#### Задачи за в час (Tasks.md)

- Работа с потоци
- Прости затваряния

#### Задачи за домашно (TasksForHome.md)

- По-сложни потоци
- Factory функции

#### Файлове:

- [Theory.md](Week_14_Scheme_Streams/Theory.md)
- [Examples.md](Week_14_Scheme_Streams/Examples.md)
- [Tasks.md](Week_14_Scheme_Streams/Tasks.md)
- [TasksForHome.md](Week_14_Scheme_Streams/TasksForHome.md)

</details>

---

### Подготовка за изпити

<details>
<summary><h3>Подготовка за Изпит</h3></summary>

Обхват: Целият курс (Седмици 1–14)

- [Tasks.md](Exam_Preparation/Tasks.md)

</details>

<details>
<summary><h3>Подготовка за Поправка</h3></summary>

Обхват: Целият курс + често срещани грешки

- [Tasks.md](Retake_Preparation/Tasks.md)

</details>

---

## Обобщение

Важно: Това разпределение е само примерно и може да търпи промени в зависимост от темпото на курса и нуждите на студентите. Целта е да се осигури балансирано покритие на теорията и практиката, като се акцентира върху разбирането и прилагането на концепциите.

| Седмица | Тема                         | Лекция   | Фокус                                     |
| ------- | ---------------------------- | -------- | ----------------------------------------- |
| 1       | Въведение в Haskell          | L00      | Типове, аритметика, прости функции        |
| 2       | Образци, гардове и кортежи   | L01      | Pattern matching, guards, tuples          |
| 3       | Рекурсия                     | L01, L05 | Числова/списъчна рекурсия, tail recursion |
| 4       | Списъци                      | L00      | Операции, ranges, pattern matching        |
| 5       | Функции от по-висок ред      | L04      | map, filter, foldr, foldl                 |
| 6       | Ламбди и list comprehensions | L04      | Lambda, (.), curry, comprehensions        |
| -       | **Тест 1**                   |          | Седмици 1–6                               |
| 7       | Алгебрични типове данни      | L02      | data, конструктори, records, deriving     |
| 8       | Maybe, Either, грешки        | L03      | Maybe/Either, case, error handling        |
| 9       | Типови класове               | L12      | class, instance, custom type classes      |
| 10      | Монади и do-нотация          | L03      | >>=, return, do, Maybe/Either монади      |
| 11      | IO и сериализация            | L03, L06 | IO монада, файлове, CSV                   |
| -       | **Тест 2**                   |          | Седмици 7–11                              |
| 12      | Двоични дървета              | L09      | Tree, BST, обхождания                     |
| 13      | Въведение в Scheme           | L14      | Prefix, define, lambda, списъци           |
| 14      | Scheme: потоци и среди       | L14, L15 | Streams, closures, env model              |

## Как да учите

1. **Прочетете теорията** - Разберете концепцията преди да пишете код
2. **Разгледайте примерите** - Проследете ги стъпка по стъпка в GHCi
3. **Решете задачите за в час** - Те затвърждават основните идеи
4. **Направете домашното** - То развива самостоятелност и дълбочина
5. **Експериментирайте** - GHCi е перфектен за бързо тестване

## Допълнителни ресурси

| Ресурс                                                                                   | Описание                     |
| ---------------------------------------------------------------------------------------- | ---------------------------- |
| [Learn You a Haskell](http://learnyouahaskell.com/)                                      | Безплатна книга за начинаещи |
| [Hoogle](https://hoogle.haskell.org/)                                                    | Търсачка по типова сигнатура |
| [Hackage](https://hackage.haskell.org/)                                                  | Документация на пакети       |
| [GHCup](https://www.haskell.org/ghcup/)                                                  | Инсталатор за Haskell        |
| [Видео лекции](https://www.youtube.com/playlist?list=PLED8hEwj9gdTVbVfWgOwPIYZroeNEJmWv) | Записи на лекциите           |

> **Забележка:** Тези материали са създадени за семинарите по ФП на ФМИ, СУ. Те следват и допълват лекционния материал.
