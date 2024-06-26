# Функциональное программирование: Пишем компилятор!

Цель этой лабораторной работы - придумать свой собственный функциональный язык программирования и разработать для него интерпретатор или компилятор.

Вы можете выполнить эту лабораторную работу в группе из 2 или 3 человек (или больше - но это требует одобрения преподавателя):

* 2 человека - компилятор/интерпретатор + примеры программ + краткая документация в README.md (вы можете заменить этот файл своей собственной документацией)
* 3 человека - компилятор/интерпретатор + примеры программ + более подробная документация на GitHub Pages
* 3 и более человек - помимо вышеуказанного, может включать следующее:
  - IDE в браузере
  - Поддержка Jupyter Notebook
  - Трансляция в JavaScript, чтобы можно было выполнять программу в браузере
  - Расширения для VS Code

## Задача

Ваша цель - изобрести и реализовать собственный функциональный язык программирования. Требования:

* Он должен тесно следовать парадигме функционального программирования, на основе либо [лямбда-исчисления](https://en.wikipedia.org/wiki/Lambda_calculus), либо [комбинаторной логики](https://en.wikipedia.org/wiki/Combinatory_logic).
* Он должен быть более или менее универсальным, т.е. реализовывать рекурсию. В идеале - полным по Тьюрингу.
* Как минимум, язык должен позволять запрограммировать функцию для расчета факториала.

> Имейте в виду, что написание парсеров - это утомительная задача, поэтому постарайтесь сделать синтаксис языка как можно проще.

Для вдохновения:

* Изучите [LISP](https://books.ifmo.ru/file/pdf/1918.pdf) - язык программирования с очень простым синтаксисом.
* Комбинаторные парсеры и библиотеку [fparsec](https://www.quanttec.com/fparsec/), если вы хотите реализовать язык с более сложным синтаксисом.
* [Top-Down Parser на F#](https://github.com/fholm/Vaughan).
* Интересный блог пост о [парсинге на F#](https://www.erikschierboom.com/2016/12/10/parsing-text-in-fsharp/).
* Парсинг с использованием инструментов [FsLex и FsYacc](https://realfiction.net/posts/lexing-and-parsing-in-f/) (не рекомендуется).
* Реализация [Scheme в F#](https://github.com/AshleyF/FScheme) - вы можете ознакомиться с этим проектом для вдохновения, но не заимствуйте код оттуда!

## Критерии оценки

* Универсальность
* Примеры программ (включая факториал, но не ограничиваясь им)
* Оригинальность и красота синтаксиса
* Документированность
* Красота реализации

Предпочтительный язык реализации - F#.

В документации явно укажите, какие функции языка вы реализовали:

* [+] Именованные переменные (`let`)
* [+] Рекурсия
* [+] Ленивое вычисление
* [+] Функции
* [+] Замыкания
* [+] Библиотечные функции: ввод-вывод файлов
* [+] Списки / Последовательности
* [ ] Библиотечные функции: списки/последовательности

## Репозиторий

Вам необходимо работать над кодом в репозитории GitHub Classroom. После завершения задачи **настоятельно рекомендуется** форкнуть этот код в свои собственные аккаунты GitHub, чтобы он служил вашим портфолио.

## Пошаговая работа

Поскольку проект довольно большой, его нужно делать поэтапно, загружая ваш код в GitHub на каждом этапе:

* Этап 1: Разработка абстрактного синтаксического дерева и парсера для вашего языка + одна примерная программа.
* Этап 2: Разработка интерпретатора/компилятора для вашего языка.
* Этап 3: Написание примеров программ и документации.

> Конечно, вы можете изменять язык на более поздних этапах, если посчитаете это нужным.

## Авторы

Не забудьте упомянуть свою команду в файле README.md, указав также, кто что делал. Также файл README.md должен включать краткое руководство по вашему языку и некоторые короткие примеры кода.

Имя | Роль в проекте
------------------|---------------------
Филенков Лев | Разработчик парсера
Кострюков Евгений | Разработчик интерпретатора, технический писатель
Шутов Дмитрий | Разработчик интерпретатора, технический писатель

Конечно, вот пример документации в формате Markdown для функционального языка программирования, который мы реализовали.

## Описание модулей

### 1. AST (Abstract Syntax Tree)

**Файл:** `AST.fs`

**Описание:** Этот модуль определяет абстрактное синтаксическое дерево (AST) для языка программирования.

**Типы выражений:**
- `Number of int`: числовые значения
- `Variable of string`: переменные
- `Lambda of string * Expr`: лямбда-выражения (функции)
- `Application of Expr * Expr`: применение функции к аргументу
- `Let of string * Expr * Expr`: именованные переменные (`let`)
- `If of Expr * Expr * Expr`: условные выражения (`if`)
- `Lazy of Expr`: ленивое вычисление
- `List of Expr list`: списки
- `FileRead of string`: чтение файла
- `FileWrite of string * Expr`: запись в файл
- `BinaryOp of string * Expr * Expr`: бинарные операции

### 2. Interpreter (Интерпретатор)

**Файл:** `Interpreter.fs`

**Описание:** Этот модуль реализует интерпретатор для выполнения выражений AST.

**Основные функции:**
- `eval (env: Env) (expr: Expr): Expr`: Рекурсивная функция, выполняющая выражение в заданном окружении.

**Особенности:**
- Поддержка вычислений выражений.
- Поддержка ленивого вычисления.
- Поддержка ввода-вывода файлов.
- Поддержка списков.

### 3. Parser (Парсер)

**Файл:** `Parser.fs`

**Описание:** Этот модуль содержит парсер для разбора строкового представления программ на нашем языке в AST.

**Основные функции:**
- `parse input`: Основная функция парсинга, которая принимает строку ввода и возвращает AST.

**Поддерживаемые конструкции:**
- `let` выражения
- `if` выражения
- `lazy` выражения
- `read` и `write` для ввода-вывода файлов
- Списки
- Лямбда-выражения
- Применение функций
- Переменные и числа

### 4. Main (Главный файл программы)

**Файл:** `Main.fs`

**Описание:** Этот модуль содержит основной код для запуска и тестирования различных программ, написанных на нашем языке.

**Основные функции:**
- `runExample input`: Функция для парсинга и выполнения программного кода.
- `main argv`: Точка входа, где выполняются примеры программ.

## Примеры программ

### Факториал

Программа для расчета факториала числа:

```
let rec fact = \n -> if n == 0 then 1 else n * (fact (n - 1)) in fact 5
```

### Ленивое вычисление

Программа, использующая ленивое вычисление:

```
let x = lazy (5 + 3) in x
```

### Ввод-вывод файлов

Программа, читающая из одного файла и записывающая в другой:

```
let content = read 'input.txt' in write 'output.txt' content
```

### Списки

Программа, создающая список:

```
[1; 2; 3]
```

## Использование

Чтобы запустить примерную программу, используйте следующий код в `Main.fs`:

```fsharp
open System
open AST
open SuperParser2004
open Interpreter

let factorialProgram = "let rec fact = \\n -> if n == 0 then 1 else n * (fact (n - 1)) in fact 5"
let lazyProgram = "let x = lazy (5 + 3) in x"
let ioProgram = "let content = read 'input.txt' in write 'output.txt' content"
let listProgram = "[1; 2; 3]"

let runExample input =
    let parsedExpr = parse input
    evalProgram parsedExpr

[<EntryPoint>]
let main argv =
    let result1 = runExample factorialProgram
    printfn "Factorial Result: %A" result1

    let result2 = runExample lazyProgram
    printfn "Lazy Evaluation Result: %A" result2

    let result3 = runExample ioProgram
    printfn "IO Result: %A" result3

    let result4 = runExample listProgram
    printfn "List Result: %A" result4

    0
```

Таким образом, можно использовать этот язык для написания и выполнения функциональных программ, используя все реализованные возможности.


> Если вы используете генеративный ИИ при написании этого кода (ChatGPT, GitHub Copilot и т.п.), вам необходимо упомянуть это здесь, и кратко описать подробности использования генеративного ИИ, а также как это повысило вашу продуктивность. *Использование генеративного ИИ без явного упоминания является нарушением академической этики!*
