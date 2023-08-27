## Hello, World!

<!-- Now that you’ve installed Rust, it’s time to write your first Rust program.
It’s traditional when learning a new language to write a little program that
prints the text `Hello, world!` to the screen, so we’ll do the same here! -->
Сега, като сте инсталирали Rust, време е да напишете първата си програма на
Rust. Традиция е, когато учите нов език, да напишете малка програма, която
извежда текста `Hello, world!` на екрана, така че и ние ще направим същото!

<!-- > Note: This book assumes basic familiarity with the command line. Rust makes
> no specific demands about your editing or tooling or where your code lives, so
> if you prefer to use an integrated development environment (IDE) instead of
> the command line, feel free to use your favorite IDE. Many IDEs now have some
> degree of Rust support; check the IDE’s documentation for details. The Rust
> team has been focusing on enabling great IDE support via `rust-analyzer`. See
> [Appendix D][devtools]<!-- ignore for more details. -->
> Бележка: Тази книга предполада да сте поне малко запознати с командния ред.
> Rust няма специфични изисквания към Вашите инструменти за обработка или къде
> се съхранява кода Ви, така че ако предпочитате вградена среда за разработка
> (IDE) пред командния ред, не се притеснявайте да използвате любимото си IDE.
> Много IDE-та вече имат поддръжка за Rust до някаква степен; проверете
> документацията на IDE-то за повече информация. Екипът на Rust се е
> съсредоточил върху предоставяне на добра поддръжка на IDE-та чрез
> `rust-analyzer`. Вижте [Приложение Г][devtools] <!-- ignore --> за повече
> информация.

<!-- ### Creating a Project Directory -->
### Създаване на Директория за Проекта

<!-- You’ll start by making a directory to store your Rust code. It doesn’t matter
to Rust where your code lives, but for the exercises and projects in this book,
we suggest making a *projects* directory in your home directory and keeping all
your projects there. -->
Ще започнете като създадете директория, където ще съхранявате код на Rust. За
Rust няма значение къде "живее" вашия код, но за упражненията и проетите в тази
книга, препоръчваме да направите директория *projects* във вашата основна
директория и да държите всичките си проекти там.

<!-- Open a terminal and enter the following commands to make a *projects* directory
and a directory for the “Hello, world!” project within the *projects* directory. -->
Отворете термнал и въведете следните команди за да направите директория
*projects* и директория за проекта "Hello, world!" в директорията *projects*.

<!-- For Linux, macOS, and PowerShell on Windows, enter this: -->
За Linux, macOS и PowerShell на Windows, въведете това:

```console
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

<!-- For Windows CMD, enter this: -->
За Windows CMD, въвдете това:

```cmd
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

<!-- ### Writing and Running a Rust Program -->
### Писане и Изпъление на Програма на Rust

<!-- Next, make a new source file and call it *main.rs*. Rust files always end with
the *.rs* extension. If you’re using more than one word in your filename, the
convention is to use an underscore to separate them. For example, use
*hello_world.rs* rather than *helloworld.rs*. -->
След това направете нов файл за изходен код и го именувайте *main.rs*. Файловете
за Rust винаги имат разширение *.rs*. Ако използвате повече от една дума в името
на файла си, конвенцията повелява да използвате долна черта за да ги разделяте.
Например, използвайте *hello_world.rs* вместо *helloworld.rs*.

<!-- Now open the *main.rs* file you just created and enter the code in Listing 1-1. -->
Сега отворете файла *main.rs*, който току-що създадохте, и въведете кода на
разпечатка 1-1.

<span class="filename">Файл: main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

<span class="caption">Разпечатка 1-1: Програма, която извежда `Hello, world!`
</span>

<!-- Save the file and go back to your terminal window in the
*~/projects/hello_world* directory. On Linux or macOS, enter the following
commands to compile and run the file: -->
Запазете файла и се върнете в терминания Ви прозерец в директория
*~/projects/hello_world*. На Linux или macOS въведете следните команди, за да
компилирате и изпълните файла:

```console
$ rustc main.rs
$ ./main
Hello, world!
```

<!-- On Windows, enter the command `.\main.exe` instead of `./main`: -->
На Windows въведете командата `.\main.exe` вместо `./main`:

```powershell
> rustc main.rs
> .\main.exe
Hello, world!
```

<!-- Regardless of your operating system, the string `Hello, world!` should print to
the terminal. If you don’t see this output, refer back to the
[“Troubleshooting”][troubleshooting]<!-- ignore part of the Installation
section for ways to get help. -->
Без значение от операционната Ви система, низът `Hello, world!` трябва да се
изведе в терминала. Ако няма нищо изведено, прегледайте отново частта
["Отстраняване на Неизправности"][troubleshooting] <!-- ignore --> на подглавата
за инсталация, за да намерите помощ.

<!-- If `Hello, world!` did print, congratulations! You’ve officially written a Rust
program. That makes you a Rust programmer—welcome! -->
Ако е изведено `Hello, world!`, поздравления! Вече официално сте написали
програма на Rust. Това Ви прави програмист на Rust - добре дошли!

<!-- ### Anatomy of a Rust Program -->
### Анатомията на Програма на Rust

<!-- Let’s review this “Hello, world!” program in detail. Here’s the first piece of
the puzzle: -->
Нека разгледаме тази програма "Hello, world!" в детайли. Ето първото парче от
пъзела:

```rust
fn main() {

}
```

<!-- These lines define a function named `main`. The `main` function is special: it
is always the first code that runs in every executable Rust program. Here, the
first line declares a function named `main` that has no parameters and returns
nothing. If there were parameters, they would go inside the parentheses `()`. -->
Тези редове дефинират функция на име `main`. Функцията `main` е специална - тя
винаги е първото нещо, което се изпълнява във всяка изпълнима програма на Rust.
В този случай първият ред декларира функция на име `main`, която няма параметри
и не връща резултат. Ако имаше параметри, те щяха да са между скобите `()`.

<!-- The function body is wrapped in `{}`. Rust requires curly brackets around all
function bodies. It’s good style to place the opening curly bracket on the same
line as the function declaration, adding one space in between. -->
Тялото на функцията е оградено в `{}`. Rust изисква да има къдрави скоби около
всички тела на функции. Добър стил е да се слагга отварящата къдрава скоба на
същия ред като декларацията на функцията, оставяйки един знак за отстояние между
тях.

<!-- > Note: If you want to stick to a standard style across Rust projects, you can
> use an automatic formatter tool called `rustfmt` to format your code in a
> particular style (more on `rustfmt` in
> [Appendix D][devtools]<!-- ignore). The Rust team has included this tool
> with the standard Rust distribution, as `rustc` is, so it should already be
> installed on your computer! -->

> Бележка: Ако искате да се придържате към стандартен стил между различни
> проекти на Rust, може да използвате инструмент за автоматично оформление,
> който се казва `rustfmt` за да оформи кода Ви по даден стил (повече за
> `rustfmt` в [приложение Г][devtools]<!-- ignore -->). Екипът на Rust е включил
> този инструмент в стандартната дистрибуция на Rust, както е `rustc`, така че
> би трябвало вече да е инсталиран на Вашия компютър!

<!-- The body of the `main` function holds the following code: -->
В тялото на функцията `main` се съдържа следният код:

```rust
    println!("Hello, world!");
```

<!-- This line does all the work in this little program: it prints text to the
screen. There are four important details to notice here. -->
Този ред извършва всичката работа в тази малка програма: извежда текст на
екрана. Тук има четири важни детайла, на които трябва да се обърне внимание.

<!-- First, Rust style is to indent with four spaces, not a tab. -->
Първо, в стила на Rust е за отстъп да се използват 4 знака за отстояние, а не
знак за табулация.

<!-- Second, `println!` calls a Rust macro. If it had called a function instead, it
would be entered as `println` (without the `!`). We’ll discuss Rust macros in
more detail in Chapter 19. For now, you just need to know that using a `!`
means that you’re calling a macro instead of a normal function and that macros
don’t always follow the same rules as functions. -->
Второ, `println!` извиква макрос на Rust. Ако вместо това извикваше функция, то
щеше да бъде изписано `println` (без знака `!`). Ще разглегаме макросите в Rust
в повече детайли в 19. глава. Засега просто трябва да знаете, че използването на
`!` означава, че извиквате макрос вместо нормална функция и че макросите не
не винаги трябва да следват същите правила като функциите.

<!-- Third, you see the `"Hello, world!"` string. We pass this string as an argument
to `println!`, and the string is printed to the screen. -->
Трето, виждате низа `"Hello, world!"`. Подаваме този низ като аргумент на
`println!` и низът бива изведен на екрана.

<!-- Fourth, we end the line with a semicolon (`;`), which indicates that this
expression is over and the next one is ready to begin. Most lines of Rust code
end with a semicolon. -->
Четвърто, на края на реда слагаме точка и запетая (`;`), което показва че този
израз е приключил и следващият може да започва. Повечето редове в Rust заършват
с точка и запетая.

<!-- ### Compiling and Running Are Separate Steps -->
### Компилиране и Изпълнение са Отделни Стъпки

<!-- You’ve just run a newly created program, so let’s examine each step in the
process. -->
Току-що сте изпълнили новосъздадена програма, така че нека разгледаме всяка
стъпка от процеса.

<!-- Before running a Rust program, you must compile it using the Rust compiler by
entering the `rustc` command and passing it the name of your source file, like
this: -->
Преди да изпълните програма на Rust, трябва да я компилирате използвайки
компилатора като въведете командата `rustc` и ѝ подадете името на изходния Ви
файл по този начин:

```console
$ rustc main.rs
```

<!-- If you have a C or C++ background, you’ll notice that this is similar to `gcc`
or `clang`. After compiling successfully, Rust outputs a binary executable. -->
Ако имате опит със C или C++, ще забележите, че това е подобно на `gcc` или
`clang`. След като компилирате успешно, Rust извежда изпълним файл.

<!-- On Linux, macOS, and PowerShell on Windows, you can see the executable by
entering the `ls` command in your shell: -->
На Linux, macOS и PowerShell на Windows можете да видите изпълнимия файл като
въведете командата `ls` във Вашия терминал:

```console
$ ls
main  main.rs
```

<!-- On Linux and macOS, you’ll see two files. With PowerShell on Windows, you’ll
see the same three files that you would see using CMD. With CMD on Windows, you
would enter the following: -->
На Linux и macOS ще видите два файла. С PowerShell на Windows ще видите същите
три файла, които бихте видяли, ако ползвахте CMD. С CMD на Windows, бихте въвели
следното:

```cmd
> dir /B %= the /B option says to only show the file names =%
main.exe
main.pdb
main.rs
```

<!-- This shows the source code file with the *.rs* extension, the executable file
(*main.exe* on Windows, but *main* on all other platforms), and, when using
Windows, a file containing debugging information with the *.pdb* extension.
From here, you run the *main* or *main.exe* file, like this: -->
Това показва изходния файл с разширението *.rs*, изпълнимия файл (*main.exe*
на Windows, но *main* на всички други платформи) и когато ползвате Windows,
файл, който съдържда информация за дебъгване с разшрение *.pdb*. От тук
изпълнявате файла *main* или *main.exe* по следния начин:

```console
$ ./main # or .\main.exe on Windows
```

<!-- If your *main.rs* is your “Hello, world!” program, this line prints `Hello,
world!` to your terminal. -->
Ако Вашият файл *main.rs* е Вашата програма "Hello, world!", този ред извежда
`Hello, world!` във Вашия терминал.

<!-- If you’re more familiar with a dynamic language, such as Ruby, Python, or
JavaScript, you might not be used to compiling and running a program as
separate steps. Rust is an *ahead-of-time compiled* language, meaning you can
compile a program and give the executable to someone else, and they can run it
even without having Rust installed. If you give someone a *.rb*, *.py*, or
*.js* file, they need to have a Ruby, Python, or JavaScript implementation
installed (respectively). But in those languages, you only need one command to
compile and run your program. Everything is a trade-off in language design. -->
Ако сте запознати с динамичен език, като Ruby, Python или JavaScript, може да
не сте свикнали да компилирате и изпълнявате програми в отделни стъпки. Rust е
*предварително компилиран* език (*ahead-of-time compiled* language), което
означава, че можете да компилирате програма и да дадете изпълнимия файл на
някого и той може да я изпълни без да има инсталиран Rust. Ако дадете на
някого файл с разшрение *.rb*, *.py* или *.js*, ще му е нужно да имат
инсталирани имплементации съответно на Ruby, Python или JavaScript. Но в тези
езици е нужна само една команда за компилация и изълнение на Вашата програма.
Всичко в разработаката на езици за програмиране е компромис.

<!-- Just compiling with `rustc` is fine for simple programs, but as your project
grows, you’ll want to manage all the options and make it easy to share your
code. Next, we’ll introduce you to the Cargo tool, which will help you write
real-world Rust programs. -->
Просто компилиране с `rustc` е добре за прости програми, но с растежа на Вашия
проект ще искакте да управлявате всички опции и да направите така, че лесно да
споделяте Вашия код. В следващия раздел ще ви въведем в инструмента Cargo, който
ще Ви помага да пишете програми на Rust от реалния живот.

[troubleshooting]: ch01-01-installation.html#troubleshooting
[devtools]: appendix-04-useful-development-tools.md
