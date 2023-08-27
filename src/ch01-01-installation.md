## Инсталация

<!-- The first step is to install Rust. We’ll download Rust through `rustup`, a
command line tool for managing Rust versions and associated tools. You’ll need
an internet connection for the download. -->
Първата стъпка е да инсталираме Rust. Ще изтеглим Rust чрез `rustup` -
инструмент на командния ред за управление на версиите на Rust и свързаните с
него инструменти. Ще Ви е нужна интернет връзка, за да изтегляте.

<!-- > Note: If you prefer not to use `rustup` for some reason, please see the
> [Other Rust Installation Methods page][otherinstall] for more options. -->
> Бележка: Ако предпочитате да не използвате `rustup` поради някаква причина,
> погледнете [Други методи за инсталиране на Rust][otherinstall] за повече
> опции.

<!-- The following steps install the latest stable version of the Rust compiler.
Rust’s stability guarantees ensure that all the examples in the book that
compile will continue to compile with newer Rust versions. The output might
differ slightly between versions because Rust often improves error messages and
warnings. In other words, any newer, stable version of Rust you install using
these steps should work as expected with the content of this book. -->
Стъпките описани по-долу са за инсталацията на най-новата стабилна версия на
компилатора на Rust. Гаранциите за стабилност на Rust подсигуряват всички пример
в книгата да се компилират и ще продължат да се компилират с нови версии на
Rust. Изходните съобщения може да имат малки разлики от версия на версия, защото
Rust често подобрява съобщенията за грешки и предупрежденията. С други думи,
която и да е по-нова и по-стабилна версия на Rust, която инсталирате с тези
стъпки би следвало да работи както се очаква от съдържанието на тази книга.

<!-- > ### Command Line Notation
>
> In this chapter and throughout the book, we’ll show some commands used in the
> terminal. Lines that you should enter in a terminal all start with `$`. You
> don’t need to type the `$` character; it’s the command line prompt shown to
> indicate the start of each command. Lines that don’t start with `$` typically
> show the output of the previous command. Additionally, PowerShell-specific
> examples will use `>` rather than `$`. -->

> ### Изписване на Командния Ред
>
> В тази глава, както и в цялата книга, ще показваме някои команди използвани
> в терминала. Редовете, които трябва да въведете в терминала започват с `$`.
> Не трябва да изписвате знака `$` - това е подкана от командния ред, която се
> показва, за да обозначи началото на всяка команда. Редовете, които не започват
> с `$` най-често показват изхода на последната команда. Освен това, примери
> специфични за PowerShell ще използват `>` вместо `$`.

<!-- ### Installing `rustup` on Linux or macOS -->
### Инсталиране на `rustup` на Linux или macOS

<!-- If you’re using Linux or macOS, open a terminal and enter the following command: -->
Ако използвате Linux или macOS, отворете терминал и въведете следната команда:

```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

<!-- The command downloads a script and starts the installation of the `rustup`
tool, which installs the latest stable version of Rust. You might be prompted
for your password. If the install is successful, the following line will appear: -->
Командата изтегля скрипт и стратира инсталацията на инструмента `rustup`, който
инсталира най-новата стабилна версия на Rust. Може да бъдете помолени да си
въведете паролата. Ако инсталацията е успешна, ще се бъде изписано следното:

```text
Rust is installed now. Great!
```

<!-- You will also need a *linker*, which is a program that Rust uses to join its
compiled outputs into one file. It is likely you already have one. If you get
linker errors, you should install a C compiler, which will typically include a
linker. A C compiler is also useful because some common Rust packages depend on
C code and will need a C compiler. -->
Също ще Ви е нужен *ликер*, което е програма, която Rust използва за да свърже
комилираните си изходи в един файл. Много е верятно вече да имате такъв. Ако
получите грешки с линкера, ще трябва да инсталирате компилатор за C, който
най-често има линкер. Компилатор за C е полезен, защото някои често използвани
пакети на Rust зависят от код на C и ще им е нужен компилатор за C.

<!-- On macOS, you can get a C compiler by running: -->
На macOS, можете да инсталирате компилатор за C като изпълните:

```console
$ xcode-select --install
```

<!-- Linux users should generally install GCC or Clang, according to their
distribution’s documentation. For example, if you use Ubuntu, you can install
the `build-essential` package. -->
Потремителите на Linux най-често трябва да инсталират GCC или Clang, според
документацията на тяхната дистрибуция. Например, ако използвате Ubuntu, можете
да инсталитаре пакета `build-essential`.

<!-- ### Installing `rustup` on Windows -->
### Инсталиране на `rustup` на Windows

<!-- On Windows, go to [https://www.rust-lang.org/tools/install][install] and follow
the instructions for installing Rust. At some point in the installation, you’ll
receive a message explaining that you’ll also need the MSVC build tools for
Visual Studio 2013 or later. -->
Ако сте на Windows, посетете [https://www.rust-lang.org/tools/install][install]
и следвайте инстукциите за инсталиране на Rust. В даден момент от инсталациятя
ще получите съобщение, което обяснява че съшо Ви трябват инструментите за
изграждане на MSVC за Visual Studio 2013 или по-нови.

<!-- To acquire the build tools, you’ll need to install [Visual Studio
2022][visualstudio]. When asked which workloads to install, include: -->
За да се сдобиете с инструментите за изграждане, ще трябва да инсталирате
[Visual Studio 2022][visualstudio]. Когато бъдете попитани кои натоварвания
(workloads) да инсталирате, включете:

* “Desktop Development with C++”
* The Windows 10 or 11 SDK
* Компонента за английски език, както и които и да е други езикови пакети по Ваш
  избор

<!-- The rest of this book uses commands that work in both *cmd.exe* and PowerShell.
If there are specific differences, we’ll explain which to use. -->
Останалата част от книгата използва команди, които ряботят и в *cmd.exe* и
PowerShell. Ако има специфични разлики, ще обясним кое да използвате.

### Отстраняване на Неизправности

<!-- To check whether you have Rust installed correctly, open a shell and enter this
line: -->
За да проверите дали сте инсталирали правилно Rust, отворете терминала и
въведете следния ред:

```console
$ rustc --version
```
<!-- 
You should see the version number, commit hash, and commit date for the latest
stable version that has been released, in the following format: -->
Трябва да видите номера на версията, commit hash-а, и дата на commit за
най-новата стабилна версия, която е публикувана, в следния формат:

```text
rustc x.y.z (abcabcabc yyyy-mm-dd)
```

<!-- If you see this information, you have installed Rust successfully! If you don’t
see this information, check that Rust is in your `%PATH%` system variable as
follows. -->
Ако виждате тази информация, Вие успешно сте инсталирали Rust! Ако не вижате
тази информация, проверете дали Rust е в системната променлива `%PATH%` на
Вашия компютър, както следва.

<!-- In Windows CMD, use: -->
В Windows CMD използвайте:

```console
> echo %PATH%
```

<!-- In PowerShell, use: -->
В PowerShell използвайте:

```powershell
> echo $env:Path
```

<!-- In Linux and macOS, use: -->
На Linux и macOS използвайте:

```console
$ echo $PATH
```

<!-- If that’s all correct and Rust still isn’t working, there are a number of
places you can get help. Find out how to get in touch with other Rustaceans (a
silly nickname we call ourselves) on [the community page][community]. -->
Ако всичко това е правилно и Rust все още не работи, има няколко места, където
можете да намерите помощ. Открийте как да се свържете с други
Rustaceans[^rustaceans] (глупав прякор с който назоваваме себе си) на
[страницата на общността][community].

<!-- ### Updating and Uninstalling -->
### Надграждане и Деинсталиране

<!-- Once Rust is installed via `rustup`, updating to a newly released version is
easy. From your shell, run the following update script: -->
След като Rust е инсталиран чрез `rustup`, надграждането до нови версии е лесно.
От терминала си, изпълнете следния скрипт за надграждане:

```console
$ rustup update
```

<!-- To uninstall Rust and `rustup`, run the following uninstall script from your
shell: -->
За да деинстралирате Rust и `rustup`, изпълнете следния скрипт за деинсталиране
от Вашия терминал.

```console
$ rustup self uninstall
```

<!-- ### Local Documentation -->
### Локална Документация

<!-- The installation of Rust also includes a local copy of the documentation so
that you can read it offline. Run `rustup doc` to open the local documentation
in your browser. -->
Инсталацията на Rust включва и локално копие на документацията, за да можете
да я чете офлайн. Изпълнете `rustup doc` за да отворите локалната документация
във Вашия браузър.

<!-- Any time a type or function is provided by the standard library and you’re not
sure what it does or how to use it, use the application programming interface
(API) documentation to find out! -->
Винаги когато дадени тип или функция са предоставени от стандартната библиотека
и не сте сигурен(на) какво прави или как се ползваа, отворете документацията на
проложно-програмния интерфейс (API) и разберете сами!

[otherinstall]: https://forge.rust-lang.org/infra/other-installation-methods.html
[install]: https://www.rust-lang.org/tools/install
[visualstudio]: https://visualstudio.microsoft.com/downloads/
[community]: https://www.rust-lang.org/community

[^rustaceans]: Идва от игра на думи на англисйки език: Rust + Crustacean
  (ракообразно) (бел. прев.)