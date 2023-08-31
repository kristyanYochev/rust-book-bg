<!-- # Programming a Guessing Game -->
# Програмиране на Игра за Отгатване

<!-- Let’s jump into Rust by working through a hands-on project together! This
chapter introduces you to a few common Rust concepts by showing you how to use
them in a real program. You’ll learn about `let`, `match`, methods, associated
functions, external crates, and more! In the following chapters, we’ll explore
these ideas in more detail. In this chapter, you’ll just practice the
fundamentals. -->
Нека скочим в Rust като работим върху практически проект заедно! Тази глава Ви
въвежда в някои често срещани идеи в Rust, като Ви показва как да ги ползвате в
реална програма. Ще научите за `let`, `match`, методи, асоциирани функции,
външни щайги и още! В следващите глави ще разгледаме тези идеи в повече детайли.
В тази глава просто ще упражните основите.

<!-- We’ll implement a classic beginner programming problem: a guessing game. Here’s
how it works: the program will generate a random integer between 1 and 100. It
will then prompt the player to enter a guess. After a guess is entered, the
program will indicate whether the guess is too low or too high. If the guess is
correct, the game will print a congratulatory message and exit. -->
Ще имплементираме класическа задача за начинаещи: игра за отгатване. Ето как ще
работо: програмата генерира случайно число между 1 и 100. Тя ще помоли
потребителя да отгатне. След като предполагано число се въведе, програмата ще
каже дали числото е било твърде голямо или твърде малко. Ако числото е познато,
играта ще изведе поздравително съобщение и ще приключи изпълнение.

<!-- ## Setting Up a New Project -->
## Създаване на Нов Проект

<!-- To set up a new project, go to the *projects* directory that you created in
Chapter 1 and make a new project using Cargo, like so: -->
За да създадете нов проект, отидете в директорията *projects*, която направихте
в първа глава и създайте нов проект чрез Cargo по следния начин:

```console
$ cargo new guessing_game
$ cd guessing_game
```

<!-- The first command, `cargo new`, takes the name of the project (`guessing_game`)
as the first argument. The second command changes to the new project’s
directory. -->

Първата команда, `cargo new`, взима името на проекта (`guessing_game`) като
първи аргумент. Втората команда влиза в директорията на проекта.

<!-- Look at the generated *Cargo.toml* file: -->
Разгледайте генерирания файл *Cargo.toml*:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial
rm -rf no-listing-01-cargo-new
cargo new no-listing-01-cargo-new --name guessing_game
cd no-listing-01-cargo-new
cargo run > output.txt 2>&1
cd ../../..
-->

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/Cargo.toml}}
```

<!-- As you saw in Chapter 1, `cargo new` generates a “Hello, world!” program for
you. Check out the *src/main.rs* file: -->
Както видяхте в 1. глава, `cargo new` генерира програма "Hello, world!" за Вас.
Разгледайте файла *src/main.rs*:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/src/main.rs}}
```

<!-- Now let’s compile this “Hello, world!” program and run it in the same step
using the `cargo run` command: -->
Нека сега компилираме тази програма и я изпълним в същата стъпка чрез командата
`cargo run`:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/output.txt}}
```

<!-- The `run` command comes in handy when you need to rapidly iterate on a project,
as we’ll do in this game, quickly testing each iteration before moving on to
the next one. -->
Комндата `run` е много удобна, когато трябва бързо да итерирате върху даден
проект, както ще правим ние за тази игра тествайки бързичко всяка итерация преди
да продължим към следващата.

<!-- Reopen the *src/main.rs* file. You’ll be writing all the code in this file. -->
Отворете файла *src/main.rs* отново. Ще пишем всичкия код в този файл.

<!-- ## Processing a Guess -->
## Обработка на предположение

<!-- The first part of the guessing game program will ask for user input, process
that input, and check that the input is in the expected form. To start, we’ll
allow the player to input a guess. Enter the code in Listing 2-1 into
*src/main.rs*. -->
Първата част от прогарамата за играта за отгатване ще помоли потребителя да
въведе нещо, ще обработи въведеното и ще провери дали въведеното е в очаквания
формат. За да започнем, ще позволим на играча да въведе предположение. Въведете
кода от разпечатка 2-1 в *src/main.rs*.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:all}}
```

<span class="caption">Разпечатка 2-1: Код, който взима предположение от
потребителя и го извежда</span>

<!-- This code contains a lot of information, so let’s go over it line by line. To
obtain user input and then print the result as output, we need to bring the
`io` input/output library into scope. The `io` library comes from the standard
library, known as `std`: -->
Този код съдържа много информация, за това ще го разгледаме ред по ред. За да
вземем въведеното от потребителя и да го изведем, ни е нужна библиотеката `io`
за входно/изходни операции да е в обхвата (scope). Библиотеката `io` идва от
стандартната библиотека, позната като `std`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```

<!-- By default, Rust has a set of items defined in the standard library that it
brings into the scope of every program. This set is called the *prelude*, and
you can see everything in it [in the standard library documentation][prelude]. -->
По подразбиране, Rust има набор от елементи дефинирани в стандартната
библиотека, които вкарва в обхвата на всяка програма. Този набор се нарича
*prelude* и можете да видите всичко в него [в документацията на стандартната
библиотека][prelude].

<!-- If a type you want to use isn’t in the prelude, you have to bring that type
into scope explicitly with a `use` statement. Using the `std::io` library
provides you with a number of useful features, including the ability to accept
user input. -->
Ако изкате да ползвате някой тип и той не е в prelude, трябва явно да го
въведете в обхвата чрез декларацията `use`. Ползването на библиотеката `std::io`
Ви предоставя няколко полезни функионалности, включае възможността да четете
потребителски вход.

<!-- As you saw in Chapter 1, the `main` function is the entry point into the
program: -->
Както видяхте в 1. глава, функцията `main` е началната точка на програмата:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:main}}
```

<!-- The `fn` syntax declares a new function; the parentheses, `()`, indicate there
are no parameters; and the curly bracket, `{`, starts the body of the function. -->
Синтаксиса `fn` декларира нова функция; скобите `()` означават, че няма
параметри; и къдравата скоба - `{` - означава началото на тялото на функцията.

<!-- As you also learned in Chapter 1, `println!` is a macro that prints a string to
the screen: -->
Както също научихте в 1. глава, `println!` е макрос, който извежда низ на
екрана:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print}}
```

<!-- This code is printing a prompt stating what the game is and requesting input
from the user. -->
Този код извежда подкана, която обяснява каква е тази игра и моли потребителя
да въведе нещо.

<!-- ### Storing Values with Variables -->
### Съхранение на Стойности в Променливи

<!-- Next, we’ll create a *variable* to store the user input, like this: -->
Сега ще създадем *променлива* (*variable*) за да съхраним въведеното от
потребителя по следния начин:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:string}}
```

<!-- Now the program is getting interesting! There’s a lot going on in this little
line. We use the `let` statement to create the variable. Here’s another example: -->
Вече програмата започва да става интересна! Много неща се случват на този малък
ред. Използваме декларацията `let`, за да създадем променлива. Ето друг пример:

```rust,ignore
let apples = 5;
```

<!-- This line creates a new variable named `apples` and binds it to the value 5. In
Rust, variables are immutable by default, meaning once we give the variable a
value, the value won’t change. We’ll be discussing this concept in detail in
the [“Variables and Mutability”][variables-and-mutability]<!-- ignore
section in Chapter 3. To make a variable mutable, we add `mut` before the
variable name: -->
Този ред създава нова пролменлива на име `apples` и я обвързва със стойността 5.
В Rust, промнливите са непроменими (immutable) по подразбиране, което означава,
че след като дадем стойност на променливата, стойността няма да се променя. Ще
обсъждаме тази идея в детайли в раздела ["Променливи и променимост"]
[variables-and-mutability]<!-- ignore --> на 3. глава. За да направим
променливата променима, трябва да добавим `mut` преди името на променливата.

```rust,ignore
let apples = 5; // непроменима
let mut bananas = 5; // променима
```

<!-- > Note: The `//` syntax starts a comment that continues until the end of the
> line. Rust ignores everything in comments. We’ll discuss comments in more
> detail in [Chapter 3][comments]ignore. -->
> Бележка: Синтаксиса `//` дава начало на коментар, който продължава до края на
> реда. Rust игнорира всичко в коментарите. Ще обсъдим коментарите в повече
> детайли в [3. глава][comments]<!-- ignore -->.

<!-- Returning to the guessing game program, you now know that `let mut guess` will
introduce a mutable variable named `guess`. The equal sign (`=`) tells Rust we
want to bind something to the variable now. On the right of the equal sign is
the value that `guess` is bound to, which is the result of calling
`String::new`, a function that returns a new instance of a `String`.
[`String`][string]<!-- ignore is a string type provided by the standard
library that is a growable, UTF-8 encoded bit of text. -->
Връщайки се на програмата за отгатване вече знаете, че `let mut guess` ще
създаде променима променлива с името `guess`. Знакът равно (`=`) казва на Rust,
че сега искаме да обвържем нещо с променливата. От дясната страна на равното е
стойността която ще бъде обвързана с `guess`, която е резултата от извикването
на функцията `String::new`, която връща нова инстанция на `String`. [`String`]
[string]<!-- ignore --> е низов тип, предоставен от стандартната библиотека
представляващ растящ текст кодиран с UTF-8.

<!-- The `::` syntax in the `::new` line indicates that `new` is an associated
function of the `String` type. An *associated function* is a function that’s
implemented on a type, in this case `String`. This `new` function creates a
new, empty string. You’ll find a `new` function on many types because it’s a
common name for a function that makes a new value of some kind. -->
Синтаксиса `::` в реда `::new` показва, че `new` е асоциирана функция с типа
`String`. *Асоциирана функция* е функция, която имплементирана върху даден тип,
в този случай `String`. Тази функция `new` създава нов празен низ. Ще намерите
функция `new` на много типове, защото е често срещано име за функция която
създава някаква нова стойност.

<!-- In full, the `let mut guess = String::new();` line has created a mutable
variable that is currently bound to a new, empty instance of a `String`. Whew! -->
Като цяло редът `let mut guess = String::new();` създава променима променлива,
която в момента е обвързана с нова празна инстанция на `String`. Фюх!

<!-- ### Receiving User Input -->
### Получаване на Потребителски Вход

<!-- Recall that we included the input/output functionality from the standard
library with `use std::io;` on the first line of the program. Now we’ll call
the `stdin` function from the `io` module, which will allow us to handle user
input: -->
Припомняме, че сме включили входно/изходните функционалности от стандартната
библиотека чрез `use std::io;` на първия ред на програмата. Нега ще извикаме
функцията `stdin` от модула `io`, коет още ни позволи да обработим потребителски
вход:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

<!-- If we hadn’t imported the `io` library with `use std::io;` at the beginning of
the program, we could still use the function by writing this function call as
`std::io::stdin`. The `stdin` function returns an instance of
[`std::io::Stdin`][iostdin]<!-- ignore, which is a type that represents a
handle to the standard input for your terminal. -->
Ако не бяхме включили библиотеката `io` чрез `use std::io;` в началото на
програмата, пак бихме могли да ползваме тази функционалност пишейки това
извикване като `std::io::stdin`. Функцията `stdin` връща инстанция на
[`std::io::Stdin`][iostdin]<!-- ignore -->, което е тип, който представлява
"дръжка" (handle) към стандартния потребителски вход за Вашия терминал.

<!-- Next, the line `.read_line(&mut guess)` calls the [`read_line`][read_line]<!--
ignore method on the standard input handle to get input from the user.
We’re also passing `&mut guess` as the argument to `read_line` to tell it what
string to store the user input in. The full job of `read_line` is to take
whatever the user types into standard input and append that into a string
(without overwriting its contents), so we therefore pass that string as an
argument. The string argument needs to be mutable so the method can change the
string’s content. -->
След това, реда `.read_line(&mut guess)` извиква метода
[`read_line`][read_line]<!-- ignore --> на стандарната входна дръжка, за да
прочетем вход от потребителя. Освен това подаваме `&mut guess` като аргумент на
`read_line`, за да му кажем в кой низ да съхрани потребитлеския вход. Цялата
работа на `read_line` е да вземе това, което потребителя е въвел, и да го добави
накрая на низ (без да презаписва съдържанието му) и за това подаваме този низ
като аргумент. Низът трябва да е променим за да може метода да промени
съдържанието му.

<!-- The `&` indicates that this argument is a *reference*, which gives you a way to
let multiple parts of your code access one piece of data without needing to
copy that data into memory multiple times. References are a complex feature,
and one of Rust’s major advantages is how safe and easy it is to use
references. You don’t need to know a lot of those details to finish this
program. For now, all you need to know is that, like variables, references are
immutable by default. Hence, you need to write `&mut guess` rather than
`&guess` to make it mutable. (Chapter 4 will explain references more
thoroughly.) -->
Знакът `&` означава, че този аргумент е *препратка* (*reference*), което Ви дава
начин да дадете на няколко части на кода Ви да достъпят едни и съши данни без да
се налага да копирате тези данни няколко пъти в паметта. Препратките са зложна
функционалност и едно от големите предимства на Rust е безопасността и лекотата
на ползването на препратки. Не Ви е нужно да знаете голяма част от тези детайли,
за да довършите тази програма. Засега това, което трябва да знаете е, че като
променливите, препратките са непроменими по подразбиране. За това трявбва да
напишете `&mut guess` вместо `&guess`, за да я направите променима. (Глава 4 ще
обясни препратките по-подробно.)

<!-- Old heading. Do not remove or links may break. -->
<a id="handling-potential-failure-with-the-result-type"></a>

<!-- ### Handling Potential Failure with `Result` -->
### Обработване на Възможен Неуспех чрез `Result`

<!-- We’re still working on this line of code. We’re now discussing a third line of
text, but note that it’s still part of a single logical line of code. The next
part is this method: -->
Все още работим върху този ред код. Сега разглеждаме трети ред текст, но
забележете, че все още е част от един логически ред код. Следващата част е този
метод:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

<!-- We could have written this code as: -->
Можехме да напишем този код по следния начин:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

<!-- However, one long line is difficult to read, so it’s best to divide it. It’s
often wise to introduce a newline and other whitespace to help break up long
lines when you call a method with the `.method_name()` syntax. Now let’s
discuss what this line does. -->
Обаче един дълъг ред е труден за четене, така че е по-добре да го разделим.
Често е добре да слагаме нов ред или друго празно място, за да помагаме с
разделянето на дълги редове, когато извикваме метод със синтаксиса
`.method_name()`. Нека сега обсъдим какво прави този ред.

<!-- As mentioned earlier, `read_line` puts whatever the user enters into the string
we pass to it, but it also returns a `Result` value. [`Result`][result]<!--
ignore is an [*enumeration*][enums]<!-- ignore, often called an *enum*,
which is a type that can be in one of multiple possible states. We call each
possible state a *variant*. -->
Както споменахме по-горе, `read_line` слага това, което потребителя въведе, в
низа, който му подаваме, но освен това връща стойност `Result`.
[`Result`][result] <!-- ignore --> е [*енумерация*][enums]<!-- ignore -->
(enumeration), често наричана *енум* (enum), което е тип, който може да бъде в
едно от няколко възможни състояния. Наричаме отделните възможни състояния
*варианти* (*variant*).

<!-- [Chapter 6][enums]ignore will cover enums in more detail. The purpose
of these `Result` types is to encode error-handling information. -->
[Глава 6][enums]<!-- ignore --> ще разгледа енуми в повече детайли. Целта на
тези `Result` типове е да представляват информация за обработка на грешки.

<!-- `Result`’s variants are `Ok` and `Err`. The `Ok` variant indicates the
operation was successful, and inside `Ok` is the successfully generated value.
The `Err` variant means the operation failed, and `Err` contains information
about how or why the operation failed. -->
Вариантите на `Result` са `Ok` и `Err`. Варианта `Ok` означава, че операцията е
успешна и в `Ok` се съдържа успешно генерираната стойност. Варианта `Err`
означава, че операцията се е провалила и `Err` съдържа информация за това как
операцията се е провалила.

<!-- Values of the `Result` type, like values of any type, have methods defined on
them. An instance of `Result` has an [`expect` method][expect]<!-- ignore
that you can call. If this instance of `Result` is an `Err` value, `expect`
will cause the program to crash and display the message that you passed as an
argument to `expect`. If the `read_line` method returns an `Err`, it would
likely be the result of an error coming from the underlying operating system.
If this instance of `Result` is an `Ok` value, `expect` will take the return
value that `Ok` is holding and return just that value to you so you can use it.
In this case, that value is the number of bytes in the user’s input. -->
Стойностите от типа `Result`, като стойности от който и да е тип, имат
дефинирани методи върху тях. Инстанции на `Result` имат
[метод `expect`][expect]<!-- ignore -->, който можете да извикате. Ако тази
инстанция на `Result` е стойност `Err`, `expect` ще накара програмата да се
срине и да покаже съобщението, което сте подали като аргумент на `expect`. Ако
`read_line` върне `Err`, най-вероятно би било в резултат на грешка, която
произтича от операционната система. Ако тази инстанция на `Result` е стойност
`Ok`, то `expect` ще вземе върната стойност, която `Ok` държи и ще върне просто
тази стойност, за да можете да я ползвате. В този случай, стойността е броя
байтове във въведеното от потребителя.

<!-- If you don’t call `expect`, the program will compile, but you’ll get a warning: -->
Ако не извикате `expect`, програмата ще се компилира, но ще получите
предупреждение:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```

<!-- Rust warns that you haven’t used the `Result` value returned from `read_line`,
indicating that the program hasn’t handled a possible error. -->
Rust Ви предупреждава, че не сте използвали `Result`а, който Ви е върнал
`read_line`, което означава, че програмата не е обработила потенциална грешка.

<!-- The right way to suppress the warning is to actually write error-handling code,
but in our case we just want to crash this program when a problem occurs, so we
can use `expect`. You’ll learn about recovering from errors in [Chapter
9][recover]ignore. -->
Правилният начин да подтиснете предупреждението е реално да напишете код, който
да обработи грешката, в нашия случай просто искаме програмата да се срине когато
изникне проблем, за това можем да ползваме `expect`. Ще научите повече за
въстановяване от грешки в [Глава 9.][recover]<!-- ignore -->

<!-- ### Printing Values with `println!` Placeholders -->
### Извеждане на Стойности с `println!` Шаблони

<!-- Aside from the closing curly bracket, there’s only one more line to discuss in
the code so far: -->
Освен затварящата къдрава скоба, има само още един ред в кода, който трябва да
обсъдим:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

<!-- This line prints the string that now contains the user’s input. The `{}` set of
curly brackets is a placeholder: think of `{}` as little crab pincers that hold
a value in place. When printing the value of a variable, the variable name can
go inside the curly brackets. When printing the result of evaluating an
expression, place empty curly brackets in the format string, then follow the
format string with a comma-separated list of expressions to print in each empty
curly bracket placeholder in the same order. Printing a variable and the result
of an expression in one call to `println!` would look like this: -->
Този ред извежда низа, който вече съдържа въведеното от потребителя. Скобите
`{}` са заместител: представете си `{}` като малки ракови щипки които държат
стойността на място. Когато извеждате стойността на променлива, името ѝ може да
бъде между къдравите скоби. Когато извеждате резултата от изпълнението на израз,
сложете празни къдрави скоби в шаблонния низ, а след низа избройте изразите,
които искате да изведете в съответните празни къдрави скоби, разделени със
запетая. Извеждането на променлива и на стойността на израз в едно извикване на
`println!` би изглеждало така:

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

<!-- This code would print `x = 5 and y + 2 = 12`. -->
Този код би извел `x = 5 and y + 2 = 12`.

<!-- ### Testing the First Part -->
### Тестване на Първата Част

<!-- Let’s test the first part of the guessing game. Run it using `cargo run`: -->
Нека тестване първата част на играта за отгатване. Изпълнете я чрез `cargo run`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-01/
cargo clean
cargo run
input 6 -->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

<!-- At this point, the first part of the game is done: we’re getting input from the
keyboard and then printing it. -->
Вече първата част на играта е готова - получаваме вход от клавиатурата и го
извеждаме.

<!-- ## Generating a Secret Number -->
## Генериране на Тайно Число

<!-- Next, we need to generate a secret number that the user will try to guess. The
secret number should be different every time so the game is fun to play more
than once. We’ll use a random number between 1 and 100 so the game isn’t too
difficult. Rust doesn’t yet include random number functionality in its standard
library. However, the Rust team does provide a [`rand` crate][randcrate] with
said functionality. -->
Сега ще трябва да генерираме тайно число, което потребителя ще се опита да
отгатне. Тайното число трябва да е различно всеки път, за да бъде забавно да се
играе повече от веднъж. Ще използваме случайно число между 1 и 100, за да не е
много трудна играта. Rust все още не включва функционалност за случайни числа в
стандартната си библиотека. Обаче екипа на Rust предоставя
[щайгата rand][randcrate] с тази функционалност.

<!-- ### Using a Crate to Get More Functionality -->
### Ползване на Щайга за да Получите Повече Фунцкионалност

<!-- Remember that a crate is a collection of Rust source code files. The project
we’ve been building is a *binary crate*, which is an executable. The `rand`
crate is a *library crate*, which contains code that is intended to be used in
other programs and can’t be executed on its own. -->
Напомняме, че щайга е набор от файлове с Rust код. Проекта, който градим досега
е *двоична щайга* (*binary crate*), която е изпълнима. Щайгата `rand` е
*библиотечна щайга* (*library crate*), която съдържа код, който се очаква да се
ползва в други програми и не може да бъде изпълнен сам по себе си.

<!-- Cargo’s coordination of external crates is where Cargo really shines. Before we
can write code that uses `rand`, we need to modify the *Cargo.toml* file to
include the `rand` crate as a dependency. Open that file now and add the
following line to the bottom, beneath the `[dependencies]` section header that
Cargo created for you. Be sure to specify `rand` exactly as we have here, with
this version number, or the code examples in this tutorial may not work: -->
Координацията на външни щайги на Cargo е мястото, където Cargo наистина блясва.
Преди да можем да пишем код, който ползва `rand`, ни трябва да редактираме файла
*Cargo.toml*, за да добавим щайгата `rand` като зависимост. Сега отворете файла
и добавете следния ред най-долу, под заглавието на раздел `[dependencies]`,
което Cargo е създал за Вас. Уверете се, че сте сложили `rand` точно както ние
сме го направили тук, с този номер на версията, иначе примерите с код в този
наръчник може да не проработят:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:8:}}
```

<!-- In the *Cargo.toml* file, everything that follows a header is part of that
section that continues until another section starts. In `[dependencies]` you
tell Cargo which external crates your project depends on and which versions of
those crates you require. In this case, we specify the `rand` crate with the
semantic version specifier `0.8.5`. Cargo understands [Semantic
Versioning][semver]<!-- ignore (sometimes called *SemVer*), which is a
standard for writing version numbers. The specifier `0.8.5` is actually
shorthand for `^0.8.5`, which means any version that is at least 0.8.5 but
below 0.9.0. -->
Във файла *Crago.toml*, всичко след заглавието е част от съответния раздел,
който продължава до началото на следващия. В `[dependencies]` подавате на Cargo
кои от външни щайги зависи Вашият проект и кои техни версии са Ви нужни. В нашия
случай задаваме щайгата `rand` със семантична версия `0.8.5`. Cargo разбира от
[Семантични Версии][semver]<!-- ignore --> (понякога наричани *SemVer*), което
е страндарт за изписване на номера на версии. Спецификатора `0.8.5` е съкратено
от `^0.8.5`, което означава която и да е версия поне 0.8.5, но по-ниска от
0.9.0.

<!-- Cargo considers these versions to have public APIs compatible with version
0.8.5, and this specification ensures you’ll get the latest patch release that
will still compile with the code in this chapter. Any version 0.9.0 or greater
is not guaranteed to have the same API as what the following examples use. -->
Cargo приема, че тези версии имат публични API-ове, които са съвместими с версия
0.8.5 и тази спецификация Ви осигурява послената patch версия, която все още ще
се компилира с кода в тази глава. Всяка версия от 0.9.0 нагоре не гарантира да
има същото API като това, което следните примери изполват.

<!-- Now, without changing any of the code, let’s build the project, as shown in
Listing 2-2. -->
Сега, без да променяме никакъв код, нека изградим проекта както е показано в
разпечатка 2-2.

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
rm Cargo.lock
cargo clean
cargo build -->

```console
$ cargo build
    Updating crates.io index
  Downloaded rand v0.8.5
  Downloaded libc v0.2.127
  Downloaded getrandom v0.2.7
  Downloaded cfg-if v1.0.0
  Downloaded ppv-lite86 v0.2.16
  Downloaded rand_chacha v0.3.1
  Downloaded rand_core v0.6.3
   Compiling libc v0.2.127
   Compiling getrandom v0.2.7
   Compiling cfg-if v1.0.0
   Compiling ppv-lite86 v0.2.16
   Compiling rand_core v0.6.3
   Compiling rand_chacha v0.3.1
   Compiling rand v0.8.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
```

<span class="caption">Разпечатка 2-2: Изхода от изпълнението на `cargo build`
след добавянето на rand щанката като зависимост</span>

<!-- You may see different version numbers (but they will all be compatible with the
code, thanks to SemVer!) and different lines (depending on the operating
system), and the lines may be in a different order. -->
Можете да видите различни номера на версиите (но всички те ще са съвместими с
кода, благодарение на SemVer) и различни редове (в зависимост от операционната
система) и редовете може да са в различен ред.

<!-- When we include an external dependency, Cargo fetches the latest versions of
everything that dependency needs from the *registry*, which is a copy of data
from [Crates.io][cratesio]. Crates.io is where people in the Rust ecosystem
post their open source Rust projects for others to use. -->
Когато включваме външна зависимост Cargo изтегля последните версии на всичко,
от което тази зависимост се нуждае, от *регистъра*, което е копие на данните от
[Crates.io][cratesio]. Crates.io е мястото, където хората в екосистемата на Rust
публикуват техните проекти с отворен код на Rust, така че другите да ги видят.

<!-- After updating the registry, Cargo checks the `[dependencies]` section and
downloads any crates listed that aren’t already downloaded. In this case,
although we only listed `rand` as a dependency, Cargo also grabbed other crates
that `rand` depends on to work. After downloading the crates, Rust compiles
them and then compiles the project with the dependencies available. -->
След опресняване на регистъра, Cargo проверява раздела `[dependencies]` и
изтегля всички щайки, които са описани и не са вече изтеглени. В този случай,
въпреки че сме описали само `rand` като зависимост, Cargo изтегли и други щайги
нужни на `rand` за да работи. След изтеглянието на щайгите, Rust ги компилира и
после компилира проекта с наличните зависимости.

<!-- If you immediately run `cargo build` again without making any changes, you
won’t get any output aside from the `Finished` line. Cargo knows it has already
downloaded and compiled the dependencies, and you haven’t changed anything
about them in your *Cargo.toml* file. Cargo also knows that you haven’t changed
anything about your code, so it doesn’t recompile that either. With nothing to
do, it simply exits. -->
Ако веднага изпълните `cargo build` отново без да правите промени, няма да
видите нищо изведено освен реда `Finished`. Cargo знаке, че вече е изтеглил и
компилирал зависимостите и Вие не сте ги променяли по никакъв начин във Вашия
*Cargo.toml* файл. Cargo също знае, че не сте променяли нищо в кода си, затова
не го прекомпилира и него. Нямайки какво да прави, той просто приключва
изпълнение.

<!-- If you open the *src/main.rs* file, make a trivial change, and then save it and
build again, you’ll only see two lines of output: -->
Ако отворите файла *src/main.rs*, направите малка промяна, след което запазите
и изградите наново, ще видите изведени само два реда:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
touch src/main.rs
cargo build -->

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

<!-- These lines show that Cargo only updates the build with your tiny change to the
*src/main.rs* file. Your dependencies haven’t changed, so Cargo knows it can
reuse what it has already downloaded and compiled for those. -->
Тези редове показват, че Cargo само опреснява изградения файл с малката Ви 
промяна в *src/main.rs*. Вашите зависимости не са се сменили, затова Cargo знае,
че може да преизползва всичко, което вече е изтеглил и компилирал за тях.

<!-- #### Ensuring Reproducible Builds with the *Cargo.lock* File -->
#### Осигуряване на Повторими Изграждания с Файла *Cargo.lock*

<!-- Cargo has a mechanism that ensures you can rebuild the same artifact every time
you or anyone else builds your code: Cargo will use only the versions of the
dependencies you specified until you indicate otherwise. For example, say that
next week version 0.8.6 of the `rand` crate comes out, and that version
contains an important bug fix, but it also contains a regression that will
break your code. To handle this, Rust creates the *Cargo.lock* file the first
time you run `cargo build`, so we now have this in the *guessing_game*
directory. -->
Cargo има механизъм, за да подигури, че можете да преизградите същите артефакти
всеки път, когато Вие или някой друг изгражда Вашия код: Cargo ще използва само
версиите на зависиостите, които сте отбелязали, докато не смените с други.
Например, ако следващата седмица излезе версия 0.8.6 на щайгата `rand` и тази
версия съдържа важен bug fix, но съдържа и регресия, която ще счупи кода ви. За
да се справи с това, Rust създава файла *Cargo.lock* първия път като изпълните
`cargo build`, заради това сега го имаме в директорията *guessing_game*.

<!-- When you build a project for the first time, Cargo figures out all the versions
of the dependencies that fit the criteria and then writes them to the
*Cargo.lock* file. When you build your project in the future, Cargo will see
that the *Cargo.lock* file exists and will use the versions specified there
rather than doing all the work of figuring out versions again. This lets you
have a reproducible build automatically. In other words, your project will
remain at 0.8.5 until you explicitly upgrade, thanks to the *Cargo.lock* file.
Because the *Cargo.lock* file is important for reproducible builds, it’s often
checked into source control with the rest of the code in your project. -->
Когато изграждате проект за пръв път, Cargo преценява всички версии на 
зависимостите, които спазват критериите и ги записва във файла *Cargo.lock*.
Когато изграждате проекта си в бъдеще, Cargo ще види, че файла *Cargo.lock*
съществува и ще използва версиите, описани в него, вместо да ги изчислява
наново. Това автоматично Ви дава повторими изграждания. С други думи, Вашият
проект ще си остане с 0.8.5, докато изрично не надградите, благодарение на файла
*Cargo.lock*. Поради факта, че файла *Cargo.lock* е важен за повторими
изграждания, често е добавен в управлението на версиите заедно с останалия код
в проекта Ви.

<!-- #### Updating a Crate to Get a New Version -->
#### Надграждане на Щайга за да Получите Нова Версия

<!-- When you *do* want to update a crate, Cargo provides the command `update`,
which will ignore the *Cargo.lock* file and figure out all the latest versions
that fit your specifications in *Cargo.toml*. Cargo will then write those
versions to the *Cargo.lock* file. Otherwise, by default, Cargo will only look
for versions greater than 0.8.5 and less than 0.9.0. If the `rand` crate has
released the two new versions 0.8.6 and 0.9.0, you would see the following if
you ran `cargo update`: -->
Когато *искате* да надградите щайга, Cargo предоставя командата `update`, която
ще игнорира файла *Cargo.lock* и открие всичките нови версии, които отговарят
на изискванията Ви в *Cargo.toml*. Cargo после ще запише тези версии във файла
*Cargo.lock*. Иначе, по подразбиране Cargo само ще гледа за версии по-високи от
0.8.5 и по-малки от 0.9.0. Ако на щайгата `rand` са издадени 2 нови версии 0.8.6
и 0.9.0, ще видите следното като изпълните `cargo update`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
cargo update
assuming there is a new 0.8.x version of rand; otherwise use another update
as a guide to creating the hypothetical output shown here -->

```console
$ cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6
```

<!-- Cargo ignores the 0.9.0 release. At this point, you would also notice a change
in your *Cargo.lock* file noting that the version of the `rand` crate you are
now using is 0.8.6. To use `rand` version 0.9.0 or any version in the 0.9.*x*
series, you’d have to update the *Cargo.toml* file to look like this instead: -->
Cargo игнорира версията 0.9.0. Тогава също ще забележите промяна във Вашия файл
*Cargo.lock* показваща, че версията на щайгата `rand`, която ползвате, е 0.8.6.
За да използвате версия 0.9.0 на `rand` или която и да е версия в серията
0.9.*x*, трябва да обновите файла *Cargo.toml* за да изглежда по следния начин:

```toml
[dependencies]
rand = "0.9.0"
```

<!-- The next time you run `cargo build`, Cargo will update the registry of crates
available and reevaluate your `rand` requirements according to the new version
you have specified. -->
Следващият път като изпълните `cargo build`, Cargo ще обнови регистъра с налични
щайги и преразгледа изискванията Ви към `rand` спрямо новата версия, която сте
посочили.

<!-- There’s a lot more to say about [Cargo][doccargo]<!-- ignore and [its
ecosystem][doccratesio]<!-- ignore , which we’ll discuss in Chapter 14, but
for now, that’s all you need to know. Cargo makes it very easy to reuse
libraries, so Rustaceans are able to write smaller projects that are assembled
from a number of packages. -->
Има още много да се каже за [Cargo][doccargo]<!-- ignore --> и [екосистемата
му][doccratesio]<!-- ignore -->, които ще обсъдим в 14. глава. Засега това е
всичко, което Ви е нужно да знаете. Cargo прави изключително лесно
преизползването на библиотеки, за да могат Rustaceans да пишат по-малки проекти,
които са "сглобени" от множество пакети.

<!-- ### Generating a Random Number -->
### Генериране на Случайно Число

<!-- Let’s start using `rand` to generate a number to guess. The next step is to
update *src/main.rs*, as shown in Listing 2-3. -->
Нека започнем да ползваме `rand` за да генерираме случйни числа за познаване.
Следващата стъпка е да променим *src/main.rs* както е показано в разпечатка 2-3.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```

<span class="caption">Разпечатка 2-3: Добавяне на код за генериране на случайно
число</span>

<!-- First we add the line `use rand::Rng;`. The `Rng` trait defines methods that
random number generators implement, and this trait must be in scope for us to
use those methods. Chapter 10 will cover traits in detail. -->
Първо добавяме реда `use rand::Rng;`. Трейта `Rng` дефинира методи, които са
имплементирани от генератори на случайни числа и този трейт трябва да е в обсег,
за да ползваме тези методи. Глава 10. ще покрие трейтове в повече детайли.

<!-- Next, we’re adding two lines in the middle. In the first line, we call the
`rand::thread_rng` function that gives us the particular random number
generator we’re going to use: one that is local to the current thread of
execution and is seeded by the operating system. Then we call the `gen_range`
method on the random number generator. This method is defined by the `Rng`
trait that we brought into scope with the `use rand::Rng;` statement. The
`gen_range` method takes a range expression as an argument and generates a
random number in the range. The kind of range expression we’re using here takes
the form `start..=end` and is inclusive on the lower and upper bounds, so we
need to specify `1..=100` to request a number between 1 and 100. -->
След тива, добавяме два реда посредата. На първия ред, извикваме функцията
`rand::thread_rng`, която ни дава конкретния генератор на случайни числа, който
ще ползваме - такъв, който е локален за сегашната нижка на изпълнение и е
сийднат от операционната система. След това ще извикаме метода `gen_range` на
генератора на случайни числа. Този метод е дефиниран от трейта `Rng`, който
въведохме в обхват с декларацията `use rand::Rng;`. Метода `gen_range` приема
интервален израз като аргумент и генерира случайно число в интервала. Вида
интервален израз, който ползваме, е във формата `начало..=край` и е затворен и
в двата края, за това трябва да напишем `1..=100`, за да поискаме число между 1
и 100.

<!-- > Note: You won’t just know which traits to use and which methods and functions
> to call from a crate, so each crate has documentation with instructions for
> using it. Another neat feature of Cargo is that running the `cargo doc
> --open` command will build documentation provided by all your dependencies
> locally and open it in your browser. If you’re interested in other
> functionality in the `rand` crate, for example, run `cargo doc --open` and
> click `rand` in the sidebar on the left. -->
> Бележка: Няма как продто да знаете кои трейтове да използвате и кои методи и
> функции да извикате от щайга, затова всяка щайга има документация с инструкции
> за ползването ѝ. Друга полезна функционалност на Cargo е, че като изпълните
> командата `cargo doc --open`, той ще изгради докумнентацията на всичките Ви
> зависимости локално и ще я отвори във Вашия браузър. Ако се интересувате от
> други функционалности на щайгата `rand`, например, изпълнете `cargo doc
> --open` и цъкнете `rand` в менюто отляво.

<!-- The second new line prints the secret number. This is useful while we’re
developing the program to be able to test it, but we’ll delete it from the
final version. It’s not much of a game if the program prints the answer as soon
as it starts! -->
Вторият нов ред извежда тайното число. Това е полезно докато разработваме
програмата, за да можем да я тестваме, но ще го изтрием от финалната версия.
Няма да е никаква игра ако играта извежда отговора когато стартира.

<!-- Try running the program a few times: -->
Нека изпълним програмата няколко пъти:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-03/
cargo run
4
cargo run
5
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

<!-- You should get different random numbers, and they should all be numbers between
1 and 100. Great job! -->
Трябва да видите различни случайни числа и те трябва да са числа между 1 и 100.
Отлично!

<!-- ## Comparing the Guess to the Secret Number -->
## Сравняване на Предположението с Тайното Число

<!-- Now that we have user input and a random number, we can compare them. That step
is shown in Listing 2-4. Note that this code won’t compile just yet, as we will
explain. -->
Вече като имаме въведеното от потребителя и случайно число, можем да ги срваним.
Тази стъпка е показана в разпечатка 2-4. Забележете, че кода още няма да се
компилира, както ще обясним по-долу.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```

<!-- <span class="caption">Разпечатка 2-4: Handling the possible return values of
comparing two numbers</span> -->
<span class="caption">Разпечатка 2-4: Обработване на възможните резултати от
сравняването на две числа</span>

<!-- First we add another `use` statement, bringing a type called
`std::cmp::Ordering` into scope from the standard library. The `Ordering` type
is another enum and has the variants `Less`, `Greater`, and `Equal`. These are
the three outcomes that are possible when you compare two values. -->
Като за начало, добавяме още една декларация `use`, която въвежда тип, който се
казва `std::cmp::Ordering`, в обхвата от стандартната библиотека. Типа
`Ordering` е друг енум и има вариантите `Less`, `Greater` и `Equal`[^ordering].
Това са трите възможни резултата от сравняването на две стойости.

[^ordering]: `По-малко`, `По-голямо` и `Равно`, съответно. (бел. прев.)

<!-- Then we add five new lines at the bottom that use the `Ordering` type. The
`cmp` method compares two values and can be called on anything that can be
compared. It takes a reference to whatever you want to compare with: here it’s
comparing `guess` to `secret_number`. Then it returns a variant of the
`Ordering` enum we brought into scope with the `use` statement. We use a
[`match`][match]<!-- ignore expression to decide what to do next based on
which variant of `Ordering` was returned from the call to `cmp` with the values
in `guess` and `secret_number`. -->
След това добавяме пет нови реда най-отдолу, които използват типа `Ordering`.
Метода `cmp` стрявнява две стойности и може да бъде викан на всичко, което може
да се сравнява. Приема препратка към това, с което искате да сравнявате - в този
случай сравнява `guess` със `secret_number`. След това връща вариант на енума
`Ordering`, който въведохме в обхвата чрез декларацията `use`. Използваме израз
[`match`][match]<!-- ignore -->, за да решим какво ще се случва нататък въз
основа на това кой вариант на `Ordering` е върнат от извикването на `cmp` със
стойностите в `guess` и `secret_number`.

<!-- A `match` expression is made up of *arms*. An arm consists of a *pattern* to
match against, and the code that should be run if the value given to `match`
fits that arm’s pattern. Rust takes the value given to `match` and looks
through each arm’s pattern in turn. Patterns and the `match` construct are
powerful Rust features: they let you express a variety of situations your code
might encounter and they make sure you handle them all. These features will be
covered in detail in Chapter 6 and Chapter 18, respectively. -->
Израз `match` се състои от *разклонения*[^match-arms]. Разклоненията се състоят
от *шаблон*, с когото да бъде сравнена дадената стойност, и кода, който трябва
да се изпълни, ако стойността подадена на `match` пасва на шаблона на това
разклонение. Rust сравнява стойността подадена на `match` и разглежда шаблоните
последователно, един по един. Шаблоните и конструкцията `match` са силни
функционалности на Rust - помагат Ви да изразите множество истуации, с които
Вашият код може да се сблъска, и Ви уверява, че всичките биват обработени. Тези
функционалности ще бъдад разгледани по-обстойно съответно в глави 6 и 18.

[^match-arms]: *Arms* на английски, букв. "ръце" (бел. прев.)

<!-- Let’s walk through an example with the `match` expression we use here. Say that
the user has guessed 50 and the randomly generated secret number this time is
38. -->
Нека разгледаме пример с израза `match`. който използваме тук. Да приемем, че
потребителя е предположил 50 и случайно-генерираното тайно число е 38.

<!-- When the code compares 50 to 38, the `cmp` method will return
`Ordering::Greater` because 50 is greater than 38. The `match` expression gets
the `Ordering::Greater` value and starts checking each arm’s pattern. It looks
at the first arm’s pattern, `Ordering::Less`, and sees that the value
`Ordering::Greater` does not match `Ordering::Less`, so it ignores the code in
that arm and moves to the next arm. The next arm’s pattern is
`Ordering::Greater`, which *does* match `Ordering::Greater`! The associated
code in that arm will execute and print `Too big!` to the screen. The `match`
expression ends after the first successful match, so it won’t look at the last
arm in this scenario. -->
Когато кода сравнява 50 с 38, метода `cmp` ще върне `Ordering::Greater`, защото
50 е по-голямо от 38. Израза `match` взима стойността `Ordering::Greater` и
започва да проверява шаблона на всяко разклонение. Той разглежда шаблона на
първото разклонение - `Ordering::Less` - и вижда, че стойността
`Ordering::Greater` не съвпада с `Ordering::Less`, затова игнорира кода в това
разклонение и продължава към следващото. Шаблона на следващото разклонение е
`Ordering::Greater`, което *съвпада* с `Ordering::Greater`! Съответният код в
това разклонение ще се изпълни и изведе `Too big!` на екрана. Израза `match`
приключва след първото успешно съвпадение, затова няма да да разгледа последното
разклонение в този случай.

<!-- However, the code in Listing 2-4 won’t compile yet. Let’s try it: -->
Обаче кодът в разпечатка 2-4 все още няма да се компилира. Нека пробваме:

<!--
The error numbers in this output should be that of the code **WITHOUT** the
anchor or snip comments
-->

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

<!-- The core of the error states that there are *mismatched types*. Rust has a
strong, static type system. However, it also has type inference. When we wrote
`let mut guess = String::new()`, Rust was able to infer that `guess` should be
a `String` and didn’t make us write the type. The `secret_number`, on the other
hand, is a number type. A few of Rust’s number types can have a value between 1
and 100: `i32`, a 32-bit number; `u32`, an unsigned 32-bit number; `i64`, a
64-bit number; as well as others. Unless otherwise specified, Rust defaults to
an `i32`, which is the type of `secret_number` unless you add type information
elsewhere that would cause Rust to infer a different numerical type. The reason
for the error is that Rust cannot compare a string and a number type. -->
В основата си грешката гласи, че има *несъвпадащи типве*. Rust има силна
статична система за типове. Обаче има и подразбиране на типовете. Когато
написахме `let mut guess = String::new()`. Rust успя да реши, че `guess` трябва
да е `String` и не ни накара да напишем този тип. Тайното число `secret_number`
обаче e от числов тип. Някои от числовите типове на Rust могат да имат стойност
между 1 и 100: `i32` - 32-битово число; `u32` - 32-битово число без знак;
`i64` - 64-битово число; както и други. Ако не сме посочили друго, Rust използва
`i32` по подразвирабне, което е типа на `secret_number`, освен ако не добавим
информация за типа на друго място, което би накарало Rust за стигне до друг
числен тип. Причината за грешката е, че Rust не може да сравнява низ с числен
тип.

<!-- Ultimately, we want to convert the `String` the program reads as input into a
real number type so we can compare it numerically to the secret number. We do
so by adding this line to the `main` function body: -->
В крайна сметка искаме да преобразуваме `String`а, който програмата чете, в
реален числов тип, за да можем да ко сравним числено с тайното число. Постигаме
това като добавим този ред в тялото на функцията `main`:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

<!-- The line is: -->
Реда е:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

<!-- We create a variable named `guess`. But wait, doesn’t the program already have
a variable named `guess`? It does, but helpfully Rust allows us to shadow the
previous value of `guess` with a new one. *Shadowing* lets us reuse the `guess`
variable name rather than forcing us to create two unique variables, such as
`guess_str` and `guess`, for example. We’ll cover this in more detail in
[Chapter 3][shadowing]<!-- ignore, but for now, know that this feature is
often used when you want to convert a value from one type to another type. -->
Съдаваме променлива с името `guess`. Стойте малко, програмата няма ли вече
променлива с името `guess`? Да, има, но Rust услужливо ни помага да скрием
предишната стойност на `guess` с нова такава. *Скриването*[^shadowing] ни
помага да преизползваме имена на променливи, вместо да ни кара да правим две
уникални променливи, например `guess_str` и `guess`. Ще покрием това в повече
детайли в [глава 3][shadowing]<!-- ignore -->, но засега знайте, че тази
функционалност се ползва често когато искате да преобразувате дадена стойност
от един тип в друг.

[^shadowing]: *Shadowing* на аглийски, букв. "скриване в сянката", "засенчване"
  (бел. прев.)

<!-- We bind this new variable to the expression `guess.trim().parse()`. The `guess`
in the expression refers to the original `guess` variable that contained the
input as a string. The `trim` method on a `String` instance will eliminate any
whitespace at the beginning and end, which we must do to be able to compare the
string to the `u32`, which can only contain numerical data. The user must press
<span class="keystroke">enter</span> to satisfy `read_line` and input their
guess, which adds a newline character to the string. For example, if the user
types <span class="keystroke">5</span> and presses <span
class="keystroke">enter</span>, `guess` looks like this: `5\n`. The `\n`
represents “newline.” (On Windows, pressing <span
class="keystroke">enter</span> results in a carriage return and a newline,
`\r\n`.) The `trim` method eliminates `\n` or `\r\n`, resulting in just `5`. -->
Обвързваме тази нова променлива с израза `guess.trim().parse()`. `guess` в
израза се отнася към първата променлива `guess`, която съдържа въведеното като
низ. Метода `trim` на инстанция на `String` ще премахне всякакви прзни знаци в
началото и края, което трябва да направим, за да можем да срваним низа с `u32`,
който може да съдърза само числени данни. Потребителя трявбва да натисне <span
class="keystroke">enter</span>, за да удовлетвори `read_line` и въведе своето
предположение, което добавя знак за нов ред в низа. Например, ако потребителя
напише <span class="keystroke">5</span> и натисне <span class="keystroke">enter
</span>, `guess` изглежда по следния начин: `5\n`. `\n` представлява "нов ред".
(На Windows, натискането на <span class="keystroke">enter</span> добавя връщане
в изходно положение и нов ред - `\r\n`.) Метода `trim` премахва `\n` или `\r\n`,
оставяйки само `5`.

<!-- The [`parse` method on strings][parse]<!-- ignore converts a string to
another type. Here, we use it to convert from a string to a number. We need to
tell Rust the exact number type we want by using `let guess: u32`. The colon
(`:`) after `guess` tells Rust we’ll annotate the variable’s type. Rust has a
few built-in number types; the `u32` seen here is an unsigned, 32-bit integer.
It’s a good default choice for a small positive number. You’ll learn about
other number types in [Chapter 3][integers]ignore. -->
[Метода `parse` на низове][parse]<!-- ignore --> преобразува низ в друг тип. В
този случай го ползваме, за да преобразуваме низ в число. Трябва да кажем на
Rust конкретния числов тип, който искаме ползвайки `let guess: u32`. Двуеточието
(`:`) след `guess` казва на Rust, че ще анотираме типа на променливата. Rust има
няколко вградени числови типове; видяното тук `u32` е 32-битово цяло число без
знак. Добър избор по подразбиране за малко положително число. Ще научите за
други числени типове в [глава 3][integers]<!-- ignore -->.

<!-- Additionally, the `u32` annotation in this example program and the comparison
with `secret_number` means Rust will infer that `secret_number` should be a
`u32` as well. So now the comparison will be between two values of the same
type! -->
Освен това, анотацията `u32` в тази примерна програма и сравнението със
`secret_number` означават, че Rust ще заключи, че `secret_number` също трябва да
е `u32`. Така че сега сравнението ще е между две стойности от един и същи тип!

<!-- The `parse` method will only work on characters that can logically be converted
into numbers and so can easily cause errors. If, for example, the string
contained `A👍%`, there would be no way to convert that to a number. Because it
might fail, the `parse` method returns a `Result` type, much as the `read_line`
method does (discussed earlier in [“Handling Potential Failure with
`Result`”](#handling-potential-failure-with-result)<!-- ignore). We’ll treat
this `Result` the same way by using the `expect` method again. If `parse`
returns an `Err` `Result` variant because it couldn’t create a number from the
string, the `expect` call will crash the game and print the message we give it.
If `parse` can successfully convert the string to a number, it will return the
`Ok` variant of `Result`, and `expect` will return the number that we want from
the `Ok` value. -->
Метода `parse` ще работи само на символи, които логически могат да бъдат
преобразувани в числа, така че лесно може да възникнат грешки. Ако например
низът съдържаше `A👍%`, нямаше да има начин да се преобразува това в число.
Поради факта, че може да се провали, метода `parse` връще тип `Result`, както
метода `read_line` (както обсъдихме по-рано в [“Обработване на Възможен Неуспех
чрез `Result`”](#Обработване-на-Възможен-Неуспех-чрез-result)<!-- ignore -->).
Ще обработим този `Result` по същия начин ползвайки метода `expect` отново. Ако
`parse` върне `Err` вариант на `Result` защото не може да направи число от низа,
извикването на `expect` ще срине играта и изведе съобщението, което му дадем.
Ако `parse` може успешно да преобразува низа в число, ще върне `Ok` варианта на
`Result` и `expect` ще върне числото, което ни трябва от `Ok` стойността.

<!-- Let’s run the program now: -->
Нека изпълним програмата сега:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/
cargo run
  76
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

<!-- Nice! Even though spaces were added before the guess, the program still figured
out that the user guessed 76. Run the program a few times to verify the
different behavior with different kinds of input: guess the number correctly,
guess a number that is too high, and guess a number that is too low. -->
Прекрасно! Въпреки че имаше добавени знаци за отстояние преди числото,
програмата разбра, че потребителя е предположил 76. Изпълнете програмата няколко
пъти, за да потвърдите различното поведение при различни видове входни данни:
отгатнете числото, отгатнете по-голямо число и отгатнете по-малко число.

<!-- We have most of the game working now, but the user can make only one guess.
Let’s change that by adding a loop! -->
По-голямата част от играта работи вече, но потребителя може да направи само едно
предположение. Нека променим това като добавим цикъл!

<!-- ## Allowing Multiple Guesses with Looping -->
## Позволяване на Множество Отгатвания с Цикли

<!-- The `loop` keyword creates an infinite loop. We’ll add a loop to give users
more chances at guessing the number: -->
Ключовата дума `loop` създава безкраен цикъл. Ще добавим цикъл, за да дадем
повече шансове на потребителите да отгатнат числото:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

<!-- As you can see, we’ve moved everything from the guess input prompt onward into
a loop. Be sure to indent the lines inside the loop another four spaces each
and run the program again. The program will now ask for another guess forever,
which actually introduces a new problem. It doesn’t seem like the user can quit! -->
Както виждате, преместихме всичко от поканата за въвеждане на предположение
нататък в цикъл. Уверете се, че сте сложили редовете в цикъла с отстъп от още
четири знака за отстояние и изпълнете програмата наново. Програмата сега ще пита
за следващо предположение завинаги, което всъщност създава нов проблем.
Няма начин потребителя да излезе.

<!-- The user could always interrupt the program by using the keyboard shortcut
<span class="keystroke">ctrl-c</span>. But there’s another way to escape this
insatiable monster, as mentioned in the `parse` discussion in [“Comparing the
Guess to the Secret Number”](#comparing-the-guess-to-the-secret-number)<!--
ignore: if the user enters a non-number answer, the program will crash. We
can take advantage of that to allow the user to quit, as shown here: -->
Потребителя винаги може да прекъсне програмата позлвайки клавишната комбинация
<span class="keystroke">ctrl-c</span>. Но има друг начин да избегнем това
чудовище, както споменахме в дискусията за `parse` в [“Сравняване на
Предположението с Тайното Число
”](#Сравняване-на-Предположението-с-Тайното-Число)<!-- ignore -->: ако
протребителя не въведе нещо, което не е число, програмата ще се срине. Можем да
се възползваме от това за да позволим на потребителя да излезе, както е показано
тук:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-04-looping/
cargo run
(too small guess)
(too big guess)
(correct guess)
quit
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

<!-- Typing `quit` will quit the game, but as you’ll notice, so will entering any
other non-number input. This is suboptimal, to say the least; we want the game
to also stop when the correct number is guessed. -->
Изписването на `quit` ще излезе от играта, но както ще забележите, същото ще
направи въвеждането на всичко друго, което не е число. Най-малкото е
неоптимално; искаме играта да спира, когато числото е отгатнато.

<!-- ### Quitting After a Correct Guess -->
### Излизане След Правилно Отгатване

<!-- Let’s program the game to quit when the user wins by adding a `break` statement: -->
Нека накараме програмата да излезе, когато потребителя спечели, като добавим
команда `break`:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

<!-- Adding the `break` line after `You win!` makes the program exit the loop when
the user guesses the secret number correctly. Exiting the loop also means
exiting the program, because the loop is the last part of `main`. -->
Добавянето на реда `break` след `You win!ч` кара програмата за излезе от цикъла
когато потребителя отгатне правилно тайното число. Излизането от цикла означава
и излизане от цялата програма, защото цикъла е последната част на `main`.

<!-- ### Handling Invalid Input -->
### Обработване на Невалиден Вход

<!-- To further refine the game’s behavior, rather than crashing the program when
the user inputs a non-number, let’s make the game ignore a non-number so the
user can continue guessing. We can do that by altering the line where `guess`
is converted from a `String` to a `u32`, as shown in Listing 2-5. -->
За да ошлайфаме още поведението на играта, вместо да сриваме програмата когато
потребителя въведе нещо, което не е число, нека накараме играта да го игнорира,
за да може потребителя да продължи да отгатва. Можем да направим това като
променим реда, където `guess` бива преобразувана от `String` на `u32`, както е
показано в разпечатка 2-5.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```

<!-- <span class="caption">Listing 2-5: Ignoring a non-number guess and asking for
another guess instead of crashing the program</span> -->
<span class="caption">Разпечатка 2-5: Игнориране на нечислови предположения и
подканване на друго предположение вместо сриване на програмата</span>

<!-- We switch from an `expect` call to a `match` expression to move from crashing
on an error to handling the error. Remember that `parse` returns a `Result`
type and `Result` is an enum that has the variants `Ok` and `Err`. We’re using
a `match` expression here, as we did with the `Ordering` result of the `cmp`
method. -->
Подменяме извикване на `expect` с израз `match`, за да преминем от сриване при
грешка към обработване на грешката. Припомняме, че `parse` връща тип `Result` и
`Result` е енум, който има варианти `Ok` и `Err`. Ползваме израз `match`, както
го ползвахме за резултата `Ordering` на метода `cmp`.

<!-- If `parse` is able to successfully turn the string into a number, it will
return an `Ok` value that contains the resultant number. That `Ok` value will
match the first arm’s pattern, and the `match` expression will just return the
`num` value that `parse` produced and put inside the `Ok` value. That number
will end up right where we want it in the new `guess` variable we’re creating. -->
Ако `parse` може успешно да преобразува низа в число, ще върне стойност `Ok`,
която съдържа резултатното число. Тази стойност `Ok` ще съвпедне с шаблона на
първото разклонение и израза `match` просто ще върне стойността `num`, която
`parse` създаде и сложи в стойността `Ok`. Това число ще се озове точно там,
където го искаме, в новата променлива `guess`, която създаваме.

<!-- If `parse` is *not* able to turn the string into a number, it will return an
`Err` value that contains more information about the error. The `Err` value
does not match the `Ok(num)` pattern in the first `match` arm, but it does
match the `Err(_)` pattern in the second arm. The underscore, `_`, is a
catchall value; in this example, we’re saying we want to match all `Err`
values, no matter what information they have inside them. So the program will
execute the second arm’s code, `continue`, which tells the program to go to the
next iteration of the `loop` and ask for another guess. So, effectively, the
program ignores all errors that `parse` might encounter! -->
Ако `parse` *не* може да преобразува низа в число, ще върне стойност `Err`,
която съдържа повече информация за грешката. Стойността `Err` не съвпада с
шаблона `Ok(num)` в първото разклонение на `match`, но съвпада с шаблона
`Err(_)` във второто разклонение. Долната черта - `_` - е стойност, която
съвпада с всичко; в този пример казваме, че искаме да съвпадне с всички
стойности `Err`, без значение каква информация съдържат. Така програмата ще
изпълни кода на второто разклонение - `continue` - който кара програмата да
прескочи към следващата итерация на цикъла `loop` и да пита за друго
предположение. Така на практика програмата игнорира всички вЪзможни грешки от
`parse`!

<!-- Now everything in the program should work as expected. Let’s try it: -->
Сега всичко в програмата би трябвало да работи подобаващо. Нека пробваме:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-05/
cargo run
(too small guess)
(too big guess)
foo
(correct guess)
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 4.45s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

<!-- Awesome! With one tiny final tweak, we will finish the guessing game. Recall
that the program is still printing the secret number. That worked well for
testing, but it ruins the game. Let’s delete the `println!` that outputs the
secret number. Listing 2-6 shows the final code. -->
Супер! С една лека последна поправка ще завършим играта. Спомнете си, че
програмата все още извежда тайното число. Това работеше добре за тестване, но
разваля играта. Нека изтрием `println!`а, който извежда тайното число.
Разпечатка 2-6 показва финалния код.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

<!-- <span class="caption">Listing 2-6: Complete guessing game code</span> -->
<span class="caption">Разпечатка 2-6: Завършеният код на играта</span>

<!-- At this point, you’ve successfully built the guessing game. Congratulations! -->
Вече успешно направихте игра за отгатване. Поздравления!

<!-- ## Summary -->
## Обобщение

<!-- This project was a hands-on way to introduce you to many new Rust concepts:
`let`, `match`, functions, the use of external crates, and more. In the next
few chapters, you’ll learn about these concepts in more detail. Chapter 3
covers concepts that most programming languages have, such as variables, data
types, and functions, and shows how to use them in Rust. Chapter 4 explores
ownership, a feature that makes Rust different from other languages. Chapter 5
discusses structs and method syntax, and Chapter 6 explains how enums work. -->
Този проект беше практически начин да Ви запознаем с много нови идеи в Rust:
`let`, `match`, функции, ползата на външни щайги и още. В следващите няколко
глави ще научите за тях в повече детайли. Глава 3 покрива идеи, които
съществуват в повечето езици за програмиране като променливи, типове данни и
функции и показва как се ползват в Rust. Глава 4 разглежда притежание -
функционалност, която отличава Rust от другите езици. Глава 5 разглежда
структури и синтаксиса за методи, а глава 6 обяснява как работят енумите.

[prelude]: ../std/prelude/index.html
[variables-and-mutability]: ch03-01-variables-and-mutability.html#variables-and-mutability
[comments]: ch03-04-comments.html
[string]: ../std/string/struct.String.html
[iostdin]: ../std/io/struct.Stdin.html
[read_line]: ../std/io/struct.Stdin.html#method.read_line
[result]: ../std/result/enum.Result.html
[enums]: ch06-00-enums.html
[expect]: ../std/result/enum.Result.html#method.expect
[recover]: ch09-02-recoverable-errors-with-result.html
[randcrate]: https://crates.io/crates/rand
[semver]: http://semver.org
[cratesio]: https://crates.io/
[doccargo]: http://doc.crates.io
[doccratesio]: http://doc.crates.io/crates-io.html
[match]: ch06-02-match.html
[shadowing]: ch03-01-variables-and-mutability.html#shadowing
[parse]: ../std/primitive.str.html#method.parse
[integers]: ch03-02-data-types.html#integer-types
