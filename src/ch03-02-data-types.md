## Типове Данни

Всяка стойност в Rust има някакъв *тип данни*, което казва на Rust какъв вид
данни се ползва, за да знае как да работи с тези данни. Ще разгледаме две
подмножества на типовете данни: скаларни и съставни.

Помнете, че Rust е *статично типизиран* език, което означава, че задължително
трябва да знае типовете на всички променливи по време на компилация. Компилатора
често може да заключи какъв тип искаме въз основа на стойността и как я
ползваме. В случите, когато е възможно да са много типове, както когато
преобразувахме `String` в числов тип чрез `parse` в раздела ["Сравняване на
Предположението с Тайното
Число"][comparing-the-guess-to-the-secret-number]<!-- ignore --> във 2. глава,
задължително трябва да добавим анотация за типа по следния начин:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Ако не добавим типовата анотация `: u32`, която е показана в по-горния код, Rust
ще покаже следната грешка, която означава, че компилатора се нуждае от повече
информация от нас, за да знае кой тип искаме да използваме.

```console
{{#include ../listings/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```

Ще видите различни типови анотации за различини типове данни.

### Скаларни Типове

*Скаларен* тип представлява една стойност. Rust има четири основни скаларни
типа: цели числа, числа с плаваща запетая, Булеви стойности и символи. Можете да
разпознаете тези от други езици за програмиране. Нека влезем в работата им в
Rust.

#### Целочислени Типове

*Цяло число* е число, което няма дробна част. Използвахме един целочислен тип в
глава 2. - типа `u32`. Тази декларация на тип показва, че стойността с която е
обвързана трябва да е цяло число без знак (цели числа със знак започват с `i`
вместо с `u`), което заема пространство от 32 бита. Таблица 3-1 показва
вградените целочислени типове в Rust. Можем да ползваме който и да е от тези
варианти за да декларираме типа на целочислена стойност.

<span class="caption">Таблица 3-1: Целочислени Типове в Rust</span>

| Дължина | Със знак | Без знак |
|---------|----------|----------|
| 8 бита  | `i8`     | `u8`     |
| 16 бита | `i16`    | `u16`    |
| 32 бита | `i32`    | `u32`    |
| 64 бита | `i64`    | `u64`    |
| 128 бита| `i128`   | `u128`   |
| архитектурно | `isize`  | `usize`  |

Всеки вариант може да бъде със или без знак и има изричен размер. *Със знак* и
*без знак* се отнасят към това дали е възможно числото да бъде отрицателно. С
други думи, дали числото трябва да има знак със себе си или ще бъде само
положително и следователно може да бъде представено без знак. Като писането на
числа на хартия е: когато знакът има значение, числото се пише с плюс или минус;
обаче когато може да се предположи, че числото е положително, се пише без знак.
Числата със знак се съхраняват ползвайки [комплемента на
двойката][twos-complement]<!-- ignore -->.

Всеки вариант със знак може да съхранява числа от -(2<sup>n - 1</sup>) до
2<sup>n - 1</sup> включително, където *n* е броя битове, които ползва този
вариант. Тоест `i8` може да съхранява числа от -(2<sup>7</sup>) до
2<sup>7</sup> - 1, което е равно на -128 до 127. Вариантите без знак могат да
съхраняват числа от 0 до 2<sup>n</sup> - 1, така че `u8` може да съхранява числа
от 0 до 2<sup>8</sup> - 1, което е равно на 0 до 255.

Освен това типовете `isize` и `usize` зависят от аргитектурата на компютъра, на
който се изпълнява Вашата програма, което е показано в таблицата като
"архитектурно": 64 бита ако сте на 64 битова архитектура и 32 бита ако сте на 32
битова архитектура.

Можете да пишете целочислени буквални стойности в която и да е от формите
показани в таблица 3-2. Забележете, че буквалните стойности, които могат да
бъдат повече от един тип, позволяват за типова наставка, например `57u8` за да
се определи типа. Буквални числа също могат да ползват `_` като визуален
разделител, за да направи числата по-лесни за четене, например `1_000`, което би
имало същата стойност каквато би била, ако бяхте написали `1000`.

<span class="caption">Таблица 3-2: Буквални Числа в Rust</span>

| Буквални числа   | Пример        |
|------------------|---------------|
| Десетични        | `98_222`      |
| Шестнадесетични  | `0xff`        |
| Осмични          | `0o77`        |
| Двоични          | `0b1111_0000` |
| Байт (само `u8`) | `b'A'`        |

Как знаете кой тип да ползвате? Ако не сте сигурни, типовете по подразбиране на
Rust са добро начало: целочислените типове са `i32` по подразбиране. Основните
ситуации, където бихте ползвали `isize` или `usize` е когато индексирате някакво
множество.

> ##### Целочислено Преливане[^integer-overflow]
>
> Нека кажем, че имате промнлива от тип `u8`, която може да съхранява стойности
> между 0 и 255. Ако се опитате да промените стойността ѝ с такава извън този
> диапазон, например 256, ще се случи *целочислено преливане*, което може да
> предизвика едно от две поведения. Когато компилирате в режим за дебъгване,
> Rust добавя проверки за целочислено преливане, които карат програмата да влезе
> в *паника* по време на изпълнение, ако това се случи. Rust използва термина
> *panicking* когато програма приключва изпълнение с грешка; ще обсъдим паники
> в по-дълбоко в раздела ["Невъзстановими грешки с
> `panic!`"][unrecoverable-errors-with-panic]<!-- ignore --> в глава 9.
>
> Когато компилирате в режим за издаване с флага `--release`, Rust *не* добавя
> проверки за целочислено преливане, които да предизвикат паника. Вместо това,
> ако се случи преливане, Rust извършва *обгръщане с комплемента на двойката*.
> Накратко, стойностите, които са по-големи от максималната стойност могат да
> се "обгърнат" към минималната, която този тип може да съхранява. В случая на
> `u8`, стойността 256 става 0, 257 става 1 и т.н. Програмата няма да влезе в
> паника, но стойността, която ще има променливата най-вероятно няма да е
> каквото очаквате. Зависенето от обгръщащото поведение на преливането се смята
> за грешка.
>
> За да изрично се обработи възможността за преливане, можете да ползвате тези
> семейства от методи, предоставени от стандартната библиотека за примитивни
> числени типове:
>
> * Обгръщайте във всички режими с методите `wrapping_*`, като `wrapping_add`.
> * Върнете стойността `None` когато настъпи преливане с методите `checked_*`.
> * Върнете стойността и булева стойност, показваща дали е настъпило преливане с
>   методите `overflowing_*`.
> * Наситете в минимума или максимума с методите `saturating_*`.
>
> [^integer-overflow]: "Integer overflow" на английски (бел. прев.)

#### Числа с Плаваща Запетая

Rust също има два примитивни типа за *числа с плаваща запетая*, които са числа
с десетична запетая. Типовете с плаваща запетая в Rust са `f32` и `f64` и са
съответно с размер 32 и 64 бита. Типа по подразбиране е `f64`, поради факта че
на днешните процесори е горе-долу толкова бърз колкото `f32`, но има възможонст
за повече прецизност. Всички типове с плаваща запетая имат знак.

Ето пример, който показва числа с плаваща запетая в действие:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

Числата с плаваща запетая са представени съобразно стандарта IEEE-754. Типа
`f32` е единично-прецизно число с плаваща запетая, а `f64` е двойно-прецизно.

#### Числени Операции

<!-- Rust supports the basic mathematical operations you’d expect for all the number
types: addition, subtraction, multiplication, division, and remainder. Integer
division truncates toward zero to the nearest integer. The following code shows
how you’d use each numeric operation in a `let` statement: -->
Rust поддърже основните математически операции, които очаквате, за всички
числени типове: събиране, изваждане, умножение, деление и остатък. Деленето на
цели числа съкращаа към нула до най-близкото цяло число. Следният код показва
как бихте ползвали всяка числена операция в декларация `let`:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```

Всеки израз в тези декларации използва математически оператор и се изчислява до
една стойност, която после се обвързва с променлива. [Приложение
Б][appendix_b]<!-- ignore --> съдържа списък с всичките оператори, които Rust
предоставя.

#### Булевия Тип

<!-- As in most other programming languages, a Boolean type in Rust has two possible
values: `true` and `false`. Booleans are one byte in size. The Boolean type in
Rust is specified using `bool`. For example: -->
Както в повечето други езици за програмине, Булевия тип в Rust има две възможни
стойности: `true` и `false`. Булевите са само един байт по размер. Булевия тип
в Rust се нарича `bool`. Например:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```

Основния начин да ползваме Булеви стойности е чрез условни оператори, като израз
`if`. Ще обясним как работят изразите `if` в Rust в раздела ["Управление Потока
на Изпълнение"][control-flow]<!-- ignore -->.

#### Знаковия Тип

<!-- Rust’s `char` type is the language’s most primitive alphabetic type. Here are
some examples of declaring `char` values: -->
Типа `char` в Rust е най-примитивния азбучен тип на езика. Ето няколко примера
за деклариране на `char` стойности:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```

Забележете, че изписваме буквални знаци с единични кавички, за разлика от
буквални низове, които ползват двойни кавички. Типа `char` на Rust е четири
байта по размер и представлява скаларна Unicode стойност, което означава, че
може да представлява много повече от просто ASCII. Ударени букви; китайски,
японски и корейски знаци; емоджи; и отстояния с нулева широчина са валидни
стойности `char` в Rust. Скаларни Unicode стойности варират от `U+0000` до
`U+D7FF` и от `U+E000` до `U+10FFF` включително. Обаче "знак" не е концепция в
Unicode, така че човешката Ви интуиция за това какво е "знак" може да не съвпада
с това какво е `char` в Rust. Ще обсъдим тази тема в детайли в ["Съхранение на
Текст Кодиран с UTF-8 с Низове"][strings]<!-- ignore --> в глава 8.

### Съставни Типове

*Съставните типове* могат да групират няколко стойности в един тип. Rust има два
примитивни съставни типа: кортежи (n-торки) и масиви.

### Кортежния Тип

*Кортеж* е общ начин за групиране на няколко стойности с разнообразни типове в
един съставен тип. Кортежите имат фиксирана дължина: веднъж декларирани, не
могат да растат или намалят размера си.

Създаваме кортеж като изписваме списък от стойности между скоби, разделени със
запетаи. Всяка позиция в кортежа си има тип и типовете на различните стойности
в кортежа не трябва да са едни и същи. Добавили сме незадължителни анотации на
типовете в този пример:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

Променливата `tup` се обвързва с целия кортеж, защото кортежите се разглеждат
като единични съставни елементи. За да вземем отделните стойности в кортеж,
можем да ползваме шаблони да деструктурираме кортежната стойност, по следния
начин:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

Тази програма първо създава кортеж и го обвързва с променливата `tup`. После
ползва шаблони с `let`, за да вземе `tup` и да го раздели на три отделни
променливи - `x`, `y` и `z`. Това се нарича *деструктуриране*, защото разлага
единичния шаблон на три части. Най-накрая, програмата извежда стойността на `y`,
която е `6.4`.

Можем също да достъпим елемент на кортежа директно чрез точка (`.`) следвана от
индекса на стойността, който искаме да достъпим. Например:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

Тази програма създава кортежа `x` и после достъпва всеки негов елемент със
съответния му индекс. Както при повечето езици за програмиране, първият индекс
на кортеж е 0.

Кортежа без стойности има специално има, *единичен*[^unit]. Тази стойност и
съответстващия ѝ тип се изписват `()` и представляват празна стойност или празен
връщан тип. Изразите имплицитно връщат единичната стойност ако не връщат друга
стойност.

[^unit]: "Unit type" на английски. (бел. прев.)

#### Типа Масив

Друг начин да имаме колекция от няколко стойности е с *масив*. За разлика от
кортежите, всеки елемент на масива трябва да има един и същи тип. За разлика от
масивите в други езици, масивите в Rust са с фиксирана дължина.

Пишем стойностите в масив като списък разделен със запетаи, в квадратни скоби:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

Масивите са полезни, когато искате данните Ви да са заделени на стека вместо на
хийпа (ще обсъдим още стека и хийпа в [глава 4][stack-and-heap]<!-- ignore -->)
или искате да подсигурите че винаги имате фиксиран брой елементи. Обаче масивът
не е толкова гъвкав колкото е векторния тип. *Вектор* е подобен тип колекции
предоставен от стандартната библиотека, който *може* да расте или намаля размера
си. Ако не сте сигурни дали да ползвате масив или вектор, най-вероятно трябва да
ползвате вектор. В [глава 8][vectors]<!-- ignore --> обсъждаме векторите в
повече детайли.

Обаче масивите са по-полезни когато знаете, че броя елементи няма да се налага
да се променя. Например, ако ползвате имената на месеците в програма,
най-вероятно бихте ползвали масив вместо вектор, защото знаете, че винаги ще
съдържа 12 елемента:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

Изписваме типа на масив полвайки квадратни скоби с типа на всеки елемент в
квадратни скоби, точка и запетая и после броя на елементите в масива, по следния
начин:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Тук `i32` е типа на всеки елемент. След точката и запетаята, числото `5`
показва, че масива съдържа 5 елемента.

Също можете да инициализирате масив да съдържа една и съща стойност за всеки
елемент като определите началната стойност, последвана от точка и запетая и
после дължината на масива в квадратни скоби, както е показано тук:

```rust
let a = [3; 5];
```

Масивът на име `a` ще съдържа `5` елемента, които ще бъдат инициализирани със
стойността `3`. Това е същото като да напишете `let a = [3, 3, 3, 3, 3];`, но
по по-сбит начин.

##### Достъпване Елементите на Масив

Масивът е едно парче от памет с познат, фиксиран размер, което може да бъде
заделено на стека. Можете да достъпите елементите му чрез индексиране.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

В този пример променливата `first` ще получи стойността `1`, защото това е
стойността на индекс `[0]` в масива. Променливата `second` ще получи стойността
`2` от индекс `[1]` в масива.

##### Невалиден Достъп до Елементи на Масив

Нека видим какво се случва, ако се опитате да достъпите елемент от масив, който
е след края му. Нека кажем, че изпълните този код подобен на играта на отгатване
в глава 2, за да получите индекс в масива от потребителя:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

Кода се компилира успешно. Ако изпълните този код с `cargo run` и въведете `0`,
`1`, `2`, `3` или `4`, програмата ще изведе съответната стойност на този индекс
в масива. Ако вместо това въведете число след края на масива, например `10`, ще
видите изход подобен на този:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Програмата получи грешка *по време на изпълнение* там, където използвахме
невалидната стойност при операцията индексиране. Програмата излезе със съобщение
за грешка и не изпълни последната команда `println!`. Когато се опитвате да
достъпите елемент чрез индексиране, Rust ще провери дали индекса, който сте
подали е по-малък от дължината на масива. Ако е по-голям или равен на дължината,
Rust ще влезе в паника. Тази проверка трябва да се случи по време на изпълнение
особено в този случай, защото компилатора няма как да знае каква стойност ще
въведе потребителя когато изпълнят кода по-късно.

Това е пример за принципте за безопасност на паметта на Rust в действие. При
много езици от ниско ниво, този вид проверка не се извършва, а когато дадете
неправилен индекс, се достъпва невалидна памет. Rust Ви защитава от тези грешки
като веднага излиза вместо да позволи достъпа на паметта и да продължи. В глава
9 разглеждаме още начина, по който Rust обработва грешки и как можете да пишете
четим и безопасен код който не влиза в паника и не позволява неправилен достъп
в паметта.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#Сравняване-на-Предположението-с-Тайното-Число
[twos-complement]: https://en.wikipedia.org/wiki/Two%27s_complement
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[appendix_b]: appendix-02-operators.md
