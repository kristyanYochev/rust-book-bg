## Hello, Cargo!

<!-- Cargo is Rust’s build system and package manager. Most Rustaceans use this tool
to manage their Rust projects because Cargo handles a lot of tasks for you,
such as building your code, downloading the libraries your code depends on, and
building those libraries. (We call the libraries that your code needs
*dependencies*.) -->
Cargo е системата на Rust за изграждане и управление на пакетите. Повечето
Rustaceans използват този инструмент за да управляват техните проекти на Rust,
защото Cargo извършва много задачи вместо Вас, като изграждане на кода Ви,
изтегляне на библиотеките, от които Вашият код зависи и изграждането на тези
библиотеки (Наричаме библиотеките, от които кодът Ви зависи *зависимости* (*dependencies*)).

<!-- The simplest Rust programs, like the one we’ve written so far, don’t have any
dependencies. If we had built the “Hello, world!” project with Cargo, it would
only use the part of Cargo that handles building your code. As you write more
complex Rust programs, you’ll add dependencies, and if you start a project
using Cargo, adding dependencies will be much easier to do. -->
Най-простите програми на Rust, като тази която сте писали досега, нямат никакви
зависимости. Ако бяхме използвали Cargo, за да направим проекта "Hello, world!",
той би използвал само частта на Cargo, която се занимава с изграждането на кода
Ви. С писането на по-сложни програми на Rust ще добавяте зависимости и ако
стартирате проект, който използва Cargo, добавянето на зависмости ще е много
по-лесно.

<!-- Because the vast majority of Rust projects use Cargo, the rest of this book
assumes that you’re using Cargo too. Cargo comes installed with Rust if you
used the official installers discussed in the
[“Installation”][installation]<!-- ignore section. If you installed Rust
through some other means, check whether Cargo is installed by entering the
following in your terminal: -->
Тъй като почти всички проекти на Rust използват Cargo, останалата част от тази
книга предполага, че и Вие ползвате Cargo. Cargo е включен в инсталацията на
Rust, ако сте използвали официалните инсталационни програми описани в раздела за
[Инсталация][installation]<!-- ignore -->. Ако сте инсталирали Rust чрез други
пособи, проверете дали Cargo е инсталиран, като въведете следната команда в
терминала:

```console
$ cargo --version
```

<!-- If you see a version number, you have it! If you see an error, such as `command
not found`, look at the documentation for your method of installation to
determine how to install Cargo separately. -->
Ако видите номер на версията, значи го имате! Ако видите грешка подобна на
`command not found`, разгледайте документацията на Вашия метод за инсталация за
да разберете как да инсталирате Cargo отделно.

<!-- ### Creating a Project with Cargo -->
### Създаване на Проект с Cargo

<!-- Let’s create a new project using Cargo and look at how it differs from our
original “Hello, world!” project. Navigate back to your *projects* directory
(or wherever you decided to store your code). Then, on any operating system,
run the following: -->
Нека създадем нов проект чрез Cargo и видим как се различава от началния ни
проект "Hello, world!". Върнете се във Вашата директория *projects* (или където
сте решили да съхранявате кода си). След това, без значение от операционната
система, изпълнете следното:

```console
$ cargo new hello_cargo
$ cd hello_cargo
```

<!-- The first command creates a new directory and project called *hello_cargo*.
We’ve named our project *hello_cargo*, and Cargo creates its files in a
directory of the same name. -->
Първата команда създава нова директория и проект наречени *hello_cargo*.
Кръстили сме проекта *hello_cargo* и Cargo създава файловете си в директория
със същото име.

<!-- Go into the *hello_cargo* directory and list the files. You’ll see that Cargo
has generated two files and one directory for us: a *Cargo.toml* file and a
*src* directory with a *main.rs* file inside. -->
Влезте в директорията *hello_cargo* и избройте файловете. Ще видите, че Cargo е
генерирал два файла и една директория за нас: файл *Cargo.toml* и директория
*src* със файл *main.rs* вътре в нея.

<!-- It has also initialized a new Git repository along with a *.gitignore* file.
Git files won’t be generated if you run `cargo new` within an existing Git
repository; you can override this behavior by using `cargo new --vcs=git`. -->
Освен това е инициализирал ново Git хранилище заедно с файл *.gitignore*.
Файлове за Git няма да бъдат генерирани ако изпълните `cargo new` в съществуващо
Git хранилище; можете да промените това поведение ползвайки
`cargo new --vcs=git`.

<!-- > Note: Git is a common version control system. You can change `cargo new` to
> use a different version control system or no version control system by using
> the `--vcs` flag. Run `cargo new --help` to see the available options. -->
> Бележка: Git е широко разпространена система за управление на версиите. Можете
> Да промените `cargo new` така, че да използва друга система за управление на
> версиите или да не използва такава система като използвате флага `--vcs`.
> Изпълнете `cargo new --help` за да видите наличните опции.

<!-- Open *Cargo.toml* in your text editor of choice. It should look similar to the
code in Listing 1-2. -->
Отворете *Cargo.toml* в текстов редактор по ваш избор. Трябва да е подобен на
кода в разпечатка 1-2.

<span class="filename">Файл: Cargo.toml</span>

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

<span class="caption">Разпечатка 1-2: Съдържанието на *Cargo.toml* генериран от
`cargo new`</span>

<!-- This file is in the [*TOML*][toml]<!-- ignore (*Tom’s Obvious, Minimal
Language*) format, which is Cargo’s configuration format. -->
Този файл е във формата [*TOML*][toml]<!-- ignore -->(*Tom's Obvious, Minimal
Language*), който е формата за конфигуарции на Cargo.

<!-- The first line, `[package]`, is a section heading that indicates that the
following statements are configuring a package. As we add more information to
this file, we’ll add other sections. -->
Първият ред - `[package]` - е заглавие на раздел което показва, че по-долните
декларации са за конфигурация на пакет (*package*). Като добавяме още информация
в този файл, ще добавяме други раздели.

<!-- The next three lines set the configuration information Cargo needs to compile
your program: the name, the version, and the edition of Rust to use. We’ll talk
about the `edition` key in [Appendix E][appendix-e]ignore. -->
Следващите три реда задават конфигурационна информация, която е нужна на Cargo
за да компилира Вашата програма: името, версията и изданието на Rust, което да
ползва. Ще говорим за ключа `edition` (издание) в
[Приложение Д][appendix-e]<!-- ignore -->.

<!-- The last line, `[dependencies]`, is the start of a section for you to list any
of your project’s dependencies. In Rust, packages of code are referred to as
*crates*. We won’t need any other crates for this project, but we will in the
first project in Chapter 2, so we’ll use this dependencies section then. -->
Последният ред - `[dependencies]` - е началото на раздел, в който Вие изброявате
зависимостите на вашия проект. В Rust пакети от код се наричат *щайги*
(*crates*). Няма да са ни нужни други щайги в за този проект, но ще ни трябват
за първия проект във втора глава, така че ще използваме секцията dependencies
тогава.

<!-- Now open *src/main.rs* and take a look: -->
Сега отворете *src/main.rs* и погледнете:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

<!-- Cargo has generated a “Hello, world!” program for you, just like the one we
wrote in Listing 1-1! So far, the differences between our project and the
project Cargo generated are that Cargo placed the code in the *src* directory
and we have a *Cargo.toml* configuration file in the top directory. -->
Cargo е генерирал програма "Hello, world!" за Вас, досущ като тази, която
написахме в разпечатка 1-1! Дотук, разликите между нашия проект и проекта, който
Cargo е генерирал, са че Cargo постави кода в директория *src* и имаме
конфигурационен файл *Cargo.toml* в най-външната директория.

<!-- Cargo expects your source files to live inside the *src* directory. The
top-level project directory is just for README files, license information,
configuration files, and anything else not related to your code. Using Cargo
helps you organize your projects. There’s a place for everything, and
everything is in its place. -->
Cargo очаква Вашите изходни файлове да живеят в директорията *src*. Най-външната
директория на проекта е за README файлове, лицензна информация, конфигурационни
файлове и всичко останало несвързано с кода Ви. Ползването на Crago Ви помага да
организирате проектите си. Има място за всичко и всичко си е на място.

<!-- If you started a project that doesn’t use Cargo, as we did with the “Hello,
world!” project, you can convert it to a project that does use Cargo. Move the
project code into the *src* directory and create an appropriate *Cargo.toml*
file. -->
Ако сте започнали проект, който не използва Cargo, както направихме с проекта
"Hello, world!", можете да го преобразувате в проект, който използва Cargo.
Преместете кода на проекта си в директорията *src* и създайте подходящ файл
*Cargo.toml*.

<!-- ### Building and Running a Cargo Project -->
### Изграждане и Изпълнение на Проект с Cargo

<!-- Now let’s look at what’s different when we build and run the “Hello, world!”
program with Cargo! From your *hello_cargo* directory, build your project by
entering the following command: -->
Сега нека разгледаме какво е различното когато изграждаме и изпълняваме
програмата "Hello, world!" с Cargo! От Вашата *hello_cargo*, изградете проекта
си като въведете следната команда:

```console
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
```

<!-- This command creates an executable file in *target/debug/hello_cargo* (or
*target\debug\hello_cargo.exe* on Windows) rather than in your current
directory. Because the default build is a debug build, Cargo puts the binary in
a directory named *debug*. You can run the executable with this command: -->
Тази команда създава изпълним файл в *target/debug/hello_cargo* (или
*target\debug\hello_cargo.exe* на Windows) вместо в настоящата директория.
Поради причината, че по подразбиране се изгражда версия за дебъгване, Cargo
слага изпълнимия файл в директория на име *debug*. Можете да изпълните
програмата с тази команда:

```console
$ ./target/debug/hello_cargo # или .\target\debug\hello_cargo.exe on Windows
Hello, world!
```

<!-- If all goes well, `Hello, world!` should print to the terminal. Running `cargo
build` for the first time also causes Cargo to create a new file at the top
level: *Cargo.lock*. This file keeps track of the exact versions of
dependencies in your project. This project doesn’t have dependencies, so the
file is a bit sparse. You won’t ever need to change this file manually; Cargo
manages its contents for you. -->
Ако всичко е наред, `Hello, world!` би трябвало да бъде изведено в терминала.
Изпълняването на `cargo build` за пръв път кара Cargo да създаде нов файл в
най-външната директория: *Cargo.lock*. Този файл следи точните версии на
зависимостите във Вашия проект. Този проект няма зависимости, за това файла е
малко "рехав". Никога няма да ви се налага да променяте този файл на ръка -
Cargo управлява съдържанието му вместо Вас.

<!-- We just built a project with `cargo build` and ran it with
`./target/debug/hello_cargo`, but we can also use `cargo run` to compile the
code and then run the resultant executable all in one command: -->
Току-що изградихме проект с `cargo build` и го изпълнихме с
`./target/debug/hello_cargo`, но можем да използваме и `cargo run` за да
компилираме кода и изпълним програмата само с една команда:

```console
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

<!-- Using `cargo run` is more convenient than having to remember to run `cargo
build` and then use the whole path to the binary, so most developers use `cargo
run`. -->
Ползването на `cargo run` е по-удобно от това да трябва да се помни да се
изпълни `cargo build` и след това да се използва целия път до изпълнимия файл,
за това повечето разработчици използват `cargo run`.

<!-- Notice that this time we didn’t see output indicating that Cargo was compiling
`hello_cargo`. Cargo figured out that the files hadn’t changed, so it didn’t
rebuild but just ran the binary. If you had modified your source code, Cargo
would have rebuilt the project before running it, and you would have seen this
output: -->
Забележете, че този път не видяхме съобщения който да казват, че Cargo компилира
`hello_cargo`. Cargo разбра, че файловете не са променени, за това не ги изгради
наново, а просто изпълни прорамата. Ако сте променили изходния си код, Cargo ще
трябва да изгради проекта наново преди да го изпълни, а Вие щяхте да видите тези
съобщения:

```console
$ cargo run
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.33 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

<!-- Cargo also provides a command called `cargo check`. This command quickly checks
your code to make sure it compiles but doesn’t produce an executable: -->
Cargo също предоставя командата `cargo check`. Тази команда набързо проверява
дали Вашият код се компилира, но не създава изпълним файл.

```console
$ cargo check
   Checking hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
```

<!-- Why would you not want an executable? Often, `cargo check` is much faster than
`cargo build` because it skips the step of producing an executable. If you’re
continually checking your work while writing the code, using `cargo check` will
speed up the process of letting you know if your project is still compiling! As
such, many Rustaceans run `cargo check` periodically as they write their
program to make sure it compiles. Then they run `cargo build` when they’re
ready to use the executable. -->
Защо не бихте искали изпълним файл? Чсто `cargo check` е много по-бързо от
`cargo build`, защото прескача стъпката за създаване на изпълним файл. Ако
продължително проверявате работата си докато пишете кода си, ползването на
`cargo check` ще ускори процеса на уведомяването Ви дали кода Ви още може да се
компилира! Така Rustaceans изпълняват `cargo check` периодично докато пишат
програмите си, за да са сигурни, че се компилират. След това изпълняват `cargo
build` когато са готови да ползват изпълнимия файл.

<!-- Let’s recap what we’ve learned so far about Cargo: -->
Нека обобщим какво научихме за Cargo досега:

<!-- * We can create a project using `cargo new`.
* We can build a project using `cargo build`.
* We can build and run a project in one step using `cargo run`.
* We can build a project without producing a binary to check for errors using
  `cargo check`.
* Instead of saving the result of the build in the same directory as our code,
  Cargo stores it in the *target/debug* directory. -->
* Можем да създадем проект чрез `cargo new`
* Можем да изградим проект чрез `cargo build`
* Можем да изпълним проект в една стъпка чрез `cargo run`
* Можем да изградим проект без да създаваме изпълним файл за да проверим за
  грешки ползвайки `cargo check`.
* Вместо да запазва резултата от изграждането в същата директория като кода,
  Cargo го запазва в директорията *target/debug*.

<!-- An additional advantage of using Cargo is that the commands are the same no
matter which operating system you’re working on. So, at this point, we’ll no
longer provide specific instructions for Linux and macOS versus Windows. -->
Още едно предимство на Cargo е, че командите са еднакви без значение от
операционната система, на която работите. Заради това отсега нататък няма да
предоставяме специфични инструкции за Linux и macOS спрямо Windows.

<!-- ### Building for Release -->
### Изграждане за Издаване

<!-- When your project is finally ready for release, you can use `cargo build
--release` to compile it with optimizations. This command will create an
executable in *target/release* instead of *target/debug*. The optimizations
make your Rust code run faster, but turning them on lengthens the time it takes
for your program to compile. This is why there are two different profiles: one
for development, when you want to rebuild quickly and often, and another for
building the final program you’ll give to a user that won’t be rebuilt
repeatedly and that will run as fast as possible. If you’re benchmarking your
code’s running time, be sure to run `cargo build --release` and benchmark with
the executable in *target/release*. -->
Когато проектът Ви най-сетне е готов за издаване, можете да ползвате `cargo
build --release` за да го компилирате с оптимзации. Тази команда ще създаде
изпълним файл в *target/release* вместо *target/debug*. Оптимизациите правят
Вашия код на Rust по-бърз, но включването им удължава времето нужно за
компилацията на вашата програма. За това има два различни профила: един за
разработка, когато искате да изграждате нановто бързо и често, и друг за
изграждане на финаланата програма, която ще дадете на потребител, няма да бъде
изграждана наново често и ще е колкото може по-бърза. Ако бенчмарквате времето
за изпълнение на Вашата програма, уверете се, че ползвате `cargo build
--release` и бенчмарквайте изпълнимия файл в *target/release*.

<!-- ### Cargo as Convention -->
### Cargo като Конвенция

<!-- With simple projects, Cargo doesn’t provide a lot of value over just using
`rustc`, but it will prove its worth as your programs become more intricate.
Once programs grow to multiple files or need a dependency, it’s much easier to
let Cargo coordinate the build. -->
При прости проекти Cargo не предоставя много добавена стойност спрямо просто
ползване на `rustc`, но ще е все по-полезно с разширяването на вашите програми.
Когато програмите станат достатъчно големи за няколко файла или се нуждаят от
зависимости, много по-лесно е да оставите Cargo да координира изграждането.

<!-- Even though the `hello_cargo` project is simple, it now uses much of the real
tooling you’ll use in the rest of your Rust career. In fact, to work on any
existing projects, you can use the following commands to check out the code
using Git, change to that project’s directory, and build: -->
Въпреки че проектa `hello_cargo` е простичък, той сега използва голяма част от
истинската инструментация, която ще ползвате в остатъка от кариерата Ви с Rust.
Даже за да работите върху съществуващи проекти, можете да използвате следните
команди, за да разгледате кода чрез Git, влезете в директорията на този проект
и го изградите:

```console
$ git clone example.org/someproject
$ cd someproject
$ cargo build
```

<!-- For more information about Cargo, check out [its documentation][cargo]. -->
За повече информация за Cargo, вижте [документацията му][cargo].

<!-- ## Summary -->
## Обобщение

<!-- You’re already off to a great start on your Rust journey! In this chapter,
you’ve learned how to: -->
Вече имате чудесен старт за вашето пътешествие с Rust! В тази глава научихте как да:

<!-- * Install the latest stable version of Rust using `rustup`
* Update to a newer Rust version
* Open locally installed documentation
* Write and run a “Hello, world!” program using `rustc` directly
* Create and run a new project using the conventions of Cargo -->
* Инталирате най-новата стабилна версия на Rust ползвайки `rustup`
* Надграждате до нова версия на Rust
* Отваряте локално-инсталираната документация
* Пишете и изпълнявате програма "Hello, world!" директно ползвайки `rustc`
* Създавате и изпълнявате нов проект позлвайки конвенциите на Cargo

<!-- This is a great time to build a more substantial program to get used to reading
and writing Rust code. So, in Chapter 2, we’ll build a guessing game program.
If you would rather start by learning how common programming concepts work in
Rust, see Chapter 3 and then return to Chapter 2. -->
Сега е добър момент да изградим по-сериозна програма, за да свикнем да четем и
пишем код на Rust. За това във 2. глава ще изградим програма за игра на
отгатване. Ако бихте предпочели да започнете като научите как работят общи
концепции от програмирането в Rust, вижте 3. глава и после се върнете на 2.
глава.

[installation]: ch01-01-installation.html#installation
[toml]: https://toml.io
[appendix-e]: appendix-05-editions.html
[cargo]: https://doc.rust-lang.org/cargo/
