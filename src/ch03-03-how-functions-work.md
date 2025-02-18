## Функции

Функциите са навсякъде в код на Rust. Вече сте видяли една от най-важните
функции в езика: функцията `main`, която е началото на много програми. Също сте
виждали ключовата дума `fn`, която Ви позволява да декларирате нови функции.

Код на Rust използва *snake case* като конвенционалния стил за имена на функции
и променливи, при който всички букви са малки и думте са разделени от долни
черти. Ето програма, която съдържа пример за декларция на функция:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

Дефинираме функция в Rust въвеждайки `fn`, последвано от името на функцията и
скоби. Къдравите скоби казват на компилатора къде започва и завършва тялото на
функцията.

Можем да извикаме която и да е функция, която сме дефинирали, като въведем името
ѝ и скоби. Поради причината че `another_function` е дефинирана в нашата
програма, може да бъде извикана от фунцкията `main`. Забележете, че сме
дефинирали `another_function` *след* `main` в кода; можехме да я дефинираме и
преди нея също. Rust не се интересува къде дефинирате функциите си, само да са
дефинирани някъде в обхват, който може да бъде видян от извикващия код.

Нека направим нов изпълним проект на име *funtions*, за да разгледаме още
функциите. Сложете примера с `another_function` в *src/main.rs* и го изпълнете.
Би трябвало да видите следния изход:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```

Редовете се изпълняват в реда, в който се виждат във функцията `main`. Първо се
извежда съобщението "Hello, world!", а после се извиква `another_function` и
нейното съобщение се извежда.

### Параметри

Можем да дефинираме функции с *параметри*, които са специални променливи, които
са част от сигнатурата на функцията. Когато функцията има параметри, можете да
ѝ предоставите конкретни стойности за тези параметри. Технически погледнато,
тези стойности се наричат *аргументи*, но разговорно хората използват думите
*параметър* и *аргумент* взаимозаменимо за променливите в дефиницията на
функцията или конкретните стойности, които са подадени когато извиквате функция.

В тази версия на `another_function` добавяме параметър:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

Пробвайте да изпълните тази програма; трябва да получите следния изход:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```

Декларацията на `another_function` има един параметър на име `x`. Типа на `x` е
зададен да е `i32`. Когато подадем `5` на `another_function`, макроса `println!`
слага `5` там, където къдравите скоби съдържащи `x` се намират в шаблонния низ.

В сигнатурите на функции *задължително* трябва да декларирате типа на всеки
параметър. Това е умишлено решение в дизайна на Rust: изискването на типови
анотации в дефинициите на функции означава, че на компилатора почти никога не му
е нужно да ги ползвате на друго място в кода, за да разбере какъв тип имате
впредвид. Компилатора също може да Ви даде по-полезни съобщения за грешка ако
знае кои типове очаква функцията.

Когато дефинирате няколко параметъра, разделете декларациите им със запетаи, ето
така:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

Този пример съзадава функция на име `print_labeled_measurement` с два
параметъра. Първия параметър се казва `value` и е `i32`. Втория се нарича
`unit_label` и е от тип `char`. Функцията след това извежда текст, който съдържа
и `value` и `unit_label`.

Нека пробваме да изпълним този код. Заменете програмата, която е сега във файла
*src/main.rs* на Вашия проект *functions*, с предишния пример и я изпълнете чрез
`cargo run`:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```

Поради причината, че извикахме функцията с `5` за стойността на `value` и `'h'`
за стойност на `unit_label`, изхода на програмата съдържа тази стойности.

### Команди и Изрази

Телата на функциите са съставени от поредица от команди и могат за завършат с
израз. Функциите, които сме покрил досега не са вкючвали завършващ израз, но сте
виждали изрази като част от команди. Поради факта, че Rust е език основан на
изрази, това е важна разлика, която трябва да разберете. Други езици нямат
такива разделение, за това нека разгледаме какво са команди и изрази и как
разликите им влияят на телата на функции.

* **Команди** са инстукции, които извършват някакво действие и не връщат
  резултат.
* **Изразите** се изчисляват то резултат. Нека разгледаме няколко примера.

Всъщност вече сме използвали команди и изрази. Съзадаването на променлива и
присвояването на стаойност към нея с ключовата дума `let` е
команда[^statements]. `let y = 6;` в разпечатка 3-1 е команда.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

<span class="caption">Разпечатка 3-1: Декларация на функция `main` съдържаща
една команда</span>

[^statements]: На английски има една обща дума за "команда" и "декларация",
  която се ползва в оригиналния текст - "statement". Поради тази причина, в този
  превод ще ползваме думите "команда" и "декларация" в значението им на
  "statement", като ползвам по-правилната там, където е възможно. В този случай,
  създаването на променлива с `let` е *декларация*, но съм го оставил "команда",
  за да създам връзката. (бел. прев.)

Дефинициите на функции също са команди; целия предходен пример е команда сам по
себе си.

Командите не връщат стойности. Следователно няма как да ги присвоите команда
`let` на друга променлива, както следният код се опитва да направи; ще получите
грешка:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

Когато изпълните програмата, грешката, която ще получите, изглежда така:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

Командата `let y = 6` не връща стойност, тоест няма какво да се обвърже с `x`.
Това е различно от това, което се случва в другите езици като C и Ruby, където
присвояването връща стойността за присвояване. В тези езици можете да напишете
`x = y = 6` и така и `x` и `y` да имат стойността `6`; това не е така в Rust.

Изразите се изчисляват до стойност и са голяма част от останалия код, който ще
пишете на Rust. Помислете си за математическа операция като `5 + 6`, която е
израз, който се изчислява до стойността `11`. Изразите могат да са част от
команди: в разпечатка 3-1, `6`цата в командата `let y = 6;` е израз, който се
изчислява до стойността `6`. Извикването на фунцкия е израз. Извикването на
макрос също е израз. Нов обхватен блок създаден с къдрави скоби е израз.
Например:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

Този израз:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

е блок, който в този случай се изчислява до `4`. Тази стойност се обвързва с `y`
като част от командата `let`. Забележете, че реда `x + 1` няма точка и запетая
накрая, което е различно от повечето редове, които сте видяли досега. Изразите
не включват точки и запетаи накрая. Ако добавите точка и запетая на края на
израз, Вие го превръщате в команда и стойността няма да бъде върната. Помнете
това докато разглеждате връщани стойности от функции и изрази.

### Функции, Които Връщат Стойности

Функциите могат да връщат стойности на кода, който ги е извикал. Не даваме име
на връщаните стойности, но трябва да декларираме типа им след стрелка (`->`). В
Rust стойността върната от функция е същата като финалния израз в блока в тялото
на функцията. Можете да върнете по-рано от функцията ползвайки ключовата дума
`return` и задавайки стойност, но повечето функции имплицитно връщат последия
израз. Ето пример за функция, която връща стойност:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

Няма извиквания на функции, макроси или дори команди `let` във функцията `five`,
а просто числото `5` само по себе си. Това е напълно валидна фунцкия в Rust.
Забележете, че типа, връщан от функцията също е деклариран като `-> i32`.
Опитайте се да изплните този код; изхода трябва да изглежда така:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

`5` във `five` е стойността, която връща функцията, което е причината връщания
тип да е `i32`. Нека разгледаме това в повече детайли. Има две важни части:
първо, реда `let x = five();` показва, че ползваме връщаната стойност на
функцията, за да инициализираме променлива. Този ред е еднакъв със следния,
защото функцията `five` връща `5`:

```rust
let x = 5;
```

Второ, функцията `five` няма параметри и дефинира типа на връщаната стойност, но
тялото на функцията е сам-самичко `5` без точка и запетая, защото е израз, чиято
стойност искаме да върнем.

Нека разгледаме още един пример:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

Ако изпълним този код, ще се изведе `The value of x is: 6`. Но ако сложим точка
и запетая на края на реда, който съдържа `x + 1`, променяйки го от израз на
команда ще получим грешка:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

Компилирането на този код предизвиква грешка като следната:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

Съобщението за грешка, `mismatched types`, разкрива основен проблем с този код.
Дефиницията на функцията `plus_one` казва, че ще върне `i32`, но командите не
се изчисляват до стойност, което е показано чрез `()` - единичния тип.
Следователно нищо не е върнато, което си противоречи с дефиницията на фунцкцията
и предизвиква грешка. В този изход, Rust предоставя съобщение, с което може да
помогне да се оправи проблема: предлага да махнете точката и запетаята, което би
поправило грешката.