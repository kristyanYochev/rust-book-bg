## Управление Потока на Изпълнение

Възможностите да изпълняваме даден код в зависимост от това дали някое условие е
`true` и да изпълняваме даден код отново и отново докато някое условие е `true`
са основни конструкции в повечето езици за програмиране. Най-често срещаните
конструкции, които Ви дават да управлявате потока на изпълнение на код на Rust
са изрази `if` и цикли.

### Изрази `if`

Израз `if` Ви позволява да разклонявате кода си спрямо дадени условия. Вие
предоставяте условието и после казвате "Ако това условие е постигнато, изпълни
този блок от код. Ако условието не е постигнато, не изпълнявай този блок код."

<!-- Create a new project called *branches* in your *projects* directory to explore
the `if` expression. In the *src/main.rs* file, input the following: -->
Създайте нов проект с име *branches* във Вашата директория *projects*, за да
разгледаме израза `if`. Въведете следното във файла *src/main.rs*:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

Всички изрази `if` започват с ключовата дума `if` последвана от условие. В този
случай условието проверява дали променливата `number` има стойност по-малка
от 5. Поставяме блока от код, който да се изпълни, ако условието е `true`,
веднага след условието в къдрави скоби. Блоковете код свързани с условията в
изрази `if` понякога се наричат *разклонения*, точно както разклоненията в
изрази `match`, които обсъдихме в раздела ["Сравняване на Предположението с
Тайното Число"][comparing-the-guess-to-the-secret-number] на глава 2.

По желание можем да добавим израз `else`, което избрахме да направим тук, за да
дадем на програмата друг блок, който да изпълни в случай, че условието се
изчисли до `false`. Ако не предоставите израз `else` и условеието е `false`,
програмата просто ще прескочи блока `if` и ще продължи нататък.

Пробвайте да изпълните този код; трябва да видите следния изход:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

Нека пробваме да сменим стойността на `number` към стойност, която прави
условието `false` и да видим какво ще стане:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

Изпълнете програмата отново и вижте изхода:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

Също трябва да се отбележи, че условието в кода *задължително* трябва да бъде
`bool`. Ако условието не е `bool`, ще получим грешка. Например провайте да
изпълните следния код:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

Условието на `if` се изчислява до стойността `3` този път и Rust изхвърля
грешка:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

Грешката показва, че Rust е очаквал `bool`, но е получил цяло число. За разлика
от езици като Ruby и JavaScript, Rust няма да се опита автоматично да
преобразува не-Булеви стойности в Булеви. Трябва да сте изрични и винаги да
предоставяте Булева стойност като условие за `if`. Например ако искаме блока на
`if` да се изпълни само когато дадено число не е равно на `0`, можем да променим
израза `if` по следния начин:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

Изпълнението на този код ще изведе `number was something other than zero`.

#### Обработка на Множество Условия с `else if`

Можете да ползвате няколко условия като комбинирате `if` и `else` в израз
`else if`. Например:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

Тази програма има четири възможни потока, по които може да мине. След изпълнение
трябва да виждате следния изход:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

Когато тази програма се изпълнява, тя проверява всеки израз `if` подред и
изпълнява първия блок, чието условие е `true`. Забележете, че въпреки че 6 се
дели на 2, не виждаме изхода `number is divisible by 2`, нито виждаме текста
`number is not divisible by 4, 3, or 2` от блока `else`. Това е защото Rust
изпълнява само блока с първото `true` условие и когато намери такъв, дори не
проверява останалите.

Ползването на твърде много изрази `else if` може да претрупа кода Ви, така че
ако имате повече от един, може би ще е добре да рефакторирате кода си. Глава 6
описва силна конструкция за разклонения в Rust, която се казва `match`, за тези
случаи.

#### Ползване на `if` в Декларация `let`

Понеже `if` е израз, можем да го ползваме отдясно на декларация `let`, за да
присвоим резултата на променлива като в разпечатка 3-2.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```

<span class="caption">Разпечатка 3-2: Присвояване на резултата от израз `if`
на променлива</span>

Променливата `number` ще бъде обвързана със стойност въз основа на резултата от
израза `if`. Изпълнете кода, за да видите какво с случва:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

Припомняме, че блокове от код се изчисляват до последния израз в тях и че
числата сами по себе си също са изрази. В този случай, стойността на целия израз
`if` зависи от това кой блок ще се изпълни. Това означава че стойности, които
могат да бъдат резултат от всяко разклонение на `if` задължително трябва да са
от един и същи тип - в разпечатка 3-2 резултатие и на разклонението `if` и на
разклонението `else` бяха цели числа `i32`. Ако типовете не съвпадаха, както е в
следния пример, ще получим грешка:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

Когато се опитаме да компилираме този код, ще получим грешка. Разклоненията `if`
и `else` имат типове, които са несъвместими и Rust показва къде точно да намерим
проблема в програмата:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```

Израза в блока `if` е цяло число, а израза в `else` блока е низ. Това няма да
работи, защото променливите задължително трябва да имат един тип и Rust трябва
да знае със сигурност от какъв тип е променливата `number` по време на
компилация. Знанието на типа на `number` позволява на компилатора да потвърди,
че типа е правилен навсякъде, където ползваме `number`. Rust не би могъл да го
направи ако типа на `number` се определяше по време на изпълнение; компилатора
щеше да е по-сложен и щеше да гарантира по-малко за кода ако трябваше да следи
няколко потенциални типове на която и да е променлива.

### Повторение чрез Цикли

Често е полезно да изпълним блок от код повече от веднъж. За тази задача, Rust
предоставя няколко *цикъла*, които ще изпълнят кода в тялото на цикъла докрай и
после ще започне пак отначало. За да експериментираме с циклите, нека направим
нов проект на име *loops*.

Rust има три вида цикъл: `loop`, `while` и `for`. Нека пробваме всеки един.

#### Повторение на Код с `loop`

Ключовата дума `loop` казва на Rust да изпълнява блок код отново и отново
завинаги или докато изрично не му кажете да спре.

Примерно сменете файла *src/main.rs* в директорията Ви *loops* за да изглежда
така:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

Когато изпълним тази програма, ще видим `again!` изведено отново и отново докато
не спрем програмата ръчно. Повечето терминали поддържат клавишната комбинация
<span class="keystroke">ctrl-c</span> за прекъсване на програма, която е
"заседнала" в продължителен цикъл. Пробвайте:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

Символа `^C` представлява къде сте натиснали <span
class="keystroke">ctrl-c</span>. Може да видите, а може и да не видите, думата
`again!` изведена след `^C`то, спрямо това къде е бил кода в цикъла когато е
получил сигнала за прекъсване.

За щастие, Rust също предоставя начин да излезете от цикъл чрез код. Можете да
сложите ключовата дума `break` в цикъла, за да кажете на програмата кога да спре
да изпълнява цикъла. Помните ли, че направихме това в играта в раздела
["Излизане След Правилно Отгатване"][quitting-after-a-correct-guess] <!-- ignore
--> на глава 2, за да излезем от програмата когато потребителя спечели играта
отгатвайки правилното число?

Също използвахме `continue` в играта, което казва на програмата да прескочи
останалия код на тази итерация и да отиде към следващата.

<!-- #### Returning Values from Loops -->
#### Връщане на Стойности от Цикли

Една от ползите на `loop` е да опитате отново операция, която знаете че може да
се провали като да проверите дали дадена нишка е свършила работата си. Може също
да трябва да предадете резултата на тази операция извън цикъла към останалия
код. За да направите това, може да добавите връщаната стойност след израза
`break`, който използвате, за да спрете цикъла; тази стойност ще бъде върната от
цикъла, за да можете да я ползвате, както е показано тук:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```

Преди цикъла декларираме променлива на име `counter` и я инициализираме с `0`.
После декларираме променлива `result`, която ще съдържа стойността върната от
цикъла. На всяка итерация на цикъла, добавяме `1` към променливата `counter` и
после проверяваме дали `counter` е равна на `10`. Ако е, ползваме ключовата дума
`break` със стойността `counter * 2`. След цикъла ползваме точка и запетая за да
завършим декларацията, което присвоява стойността на `result`. Последно
извеждаме стойността в `result`, която е `20` в този случай.

#### Етикети на Цикли за да Разграничим Между Няколко Цикъла

Ако имате цикли в цикли, `break` и `continue` важат за най-вътрешния цикъл,
който ги съдържа. Можете по желание да сложите *етикет на цикъла*, който можете
да ползвате с `break` или `continue`, за да покажете, че тези ключови думи важат
за цикъла с етикет вместо най-вътрешния цикъл. Етикетите за цикли задължително
започват с апостроф. Ето пример с два вложени цикъла:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

Външния цикъл има етикета `'counting_up` и ще брои от 0 до 2. Вътрешния цикъл
без етикет брои назад от 10 до 9. Първият `break`, който не посочва етикет ще
излезе само от вътрешния цикъл. Командата `break 'counting_up;` ще излезе от
външния цикъл. Този код извежда:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

#### Условни Цикли с `while`

Програмите често трябва да изчисляват условие в цикъл. Докато условието е
`true`, цикъла се изпълнява. Когато условието спре да е `true`, програмата
извиква `break`, спирайки цикъла. Възможно е да имплементирате подобно поведение
с комбинация от `loop`, `if`, `else` и `break`; можете да се пробвате сега в
програма, ако искате. Обаче този вид цикли е толкова често срещан, че Rust има
вградена конструкция в езцика за тях - нарича се цикъл `while`. В разпечатка 3-3
ползваме `while` за да изциклим програмата три пъти борейки назад всеки път и
след края на цикъла, извеждаме съобщение и излизаме:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```

<span class="caption">Разпечатка 3-3: Ползване на цикъл `while`, за да изпълним
код докато дадено условие е вярно</span>

Тази конструкция елиминира много влагане, което би било нужно ако ползвахте
`loop`, `if`, `else` и `break`, и е по-ясно. Докато условието е `true`, кода се
изпълнява; иначе излиза от цикъла.

#### Циклене през Колекция с `for`

<!-- You can choose to use the `while` construct to loop over the elements of a
collection, such as an array. For example, the loop in Listing 3-4 prints each
element in the array `a`. -->
Можете да изберете да ползвате конструкцията `while`, за да циклите през
елементите на колекция като масив. Например цикъла в разпечатка 3-4 извежда
всеки елемент в масива `a`.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```

<span class="caption">Разпечатка 3-4: Циклене през всеки елемент на колекция
чрез цикъл `while`</span>

Тук кода изброява елементите в масива. Започва с индекс `0` и после цикли докато
не стигне последния индекс в масива (тоест когато `index < 5` вече не е `true`).
Изпълнението на този код ще изведе всеки елемент в масива:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```

Всичките пет стойности в масива се извеждат в терминала, както очаквахме.
Въпреки че `index` ще достигне стойност `5` по някое време, цикъла спира
изпълнение преди да се опита да извлече шеста стойност от масива.

Обаче този подход е уязвим към грешки; можем да накараме програмата да влезе в
паника ако индексната стойност или условието са неправилни. Например, ако
промените дефиницията на масива `a` така, че да има четири елемента, но
забравите да обновите условието така, че да стане `while index < 4`, кода ще
влезе в паника. Освен това този подход е бавен, защото компилатора добавя код,
който да провери по време на изпълнение, дали индекса е в границите на масива на
всяка итерация на цикъла.

Като по-стегната алтернатива, можете да ползвате цикъл `for` и да изпълните
даден код за всеки елемент от колекцията. Цикъл `for` изглежда като кода в
разпечатка 3-5.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```

<span class="caption">Разпечатка 3-5: Циклене през всеки елемент в колекция чрез
цикъл `for`</span>

When we run this code, we’ll see the same output as in Listing 3-4. More
importantly, we’ve now increased the safety of the code and eliminated the
chance of bugs that might result from going beyond the end of the array or not
going far enough and missing some items.

Using the `for` loop, you wouldn’t need to remember to change any other code if
you changed the number of values in the array, as you would with the method
used in Listing 3-4.

The safety and conciseness of `for` loops make them the most commonly used loop
construct in Rust. Even in situations in which you want to run some code a
certain number of times, as in the countdown example that used a `while` loop
in Listing 3-3, most Rustaceans would use a `for` loop. The way to do that
would be to use a `Range`, provided by the standard library, which generates
all numbers in sequence starting from one number and ending before another
number.

Here’s what the countdown would look like using a `for` loop and another method
we’ve not yet talked about, `rev`, to reverse the range:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

This code is a bit nicer, isn’t it?

## Summary

You made it! This was a sizable chapter: you learned about variables, scalar
and compound data types, functions, comments, `if` expressions, and loops! To
practice with the concepts discussed in this chapter, try building programs to
do the following:

* Convert temperatures between Fahrenheit and Celsius.
* Generate the *n*th Fibonacci number.
* Print the lyrics to the Christmas carol “The Twelve Days of Christmas,”
  taking advantage of the repetition in the song.

When you’re ready to move on, we’ll talk about a concept in Rust that *doesn’t*
commonly exist in other programming languages: ownership.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#Сравняване-на-Предположението-с-Тайното-Число
[quitting-after-a-correct-guess]:
ch02-00-guessing-game-tutorial.html#Излизане-След-Правилно-Отгатване
