## Какво Е Притежание?

*Притежание* е набор от правила, които определят как програма на Rust управлява
паметта. Всички програми трябва да управляват начина, по който полват паметта на
компютъра, докато се изпълняват. Някои езици имат garbage collection, което
редовно търси вече неползвана памет докато програмата се изпълнява; в други
езици програмиста трябва изрично да заделя и освобождава паметта. Rust използва
трети подход: паметта се управлява чрез система на притежание, което е набор от
правила, които компилатора проверява. Ако някое от тези правила е нарушено,
програмата няма да се компилира. Нито една от функционалностите на притежанието
ще забави програмаа Ви докато се изпълнява.

Понеже притежанието е нова идея за много програмисти, отнема малко време да ѝ
свикнете. Добрата новина е, че колко по-опитни ставате с Rust и правилата на
системата за притежание, толкова по-лесно ще намирате за естествена разработката
на безопасен и ефективен код. Дерзайте!

Когато разберете притежанието, ще имате добра основа да разберете
функционалностите, които правят Rust уникален. В тази глава ще научите за
притежанието като работите върху няколко примера, които се концентрират върху
много често срещана структура от данни: низове.

> ### Стекът и Хийпът
>
> Много езици за програмиране не изискват от Вас да мислите за стека и хийпа
> много често. Но в езици за системно програмиране като Rust, това дали дадена
> стойност е на стека или на хийпа променя поведението на езика и защо трябва да
> взимате някои решения. Части от притежанието ще бъдат описани по отношение на
> стека и хийпа по-късно в тази глава, така че ето бързо обяснение за
> подготовка.
>
> И стекът, и хийпът са част от паметта, която кода Ви може да достъпи по време
> на изпълнение, но са структурирани по различни начини. Стекът съхранява
> стойности в реда, в който ги получава, и ги премахва в обратен ред. Това се
> нарича *last in, first out*. Представете си чинии една върху друга: като
> добавяте още чинии, ги слагате отгоре на купа, а когато Ви трябва чиния, си я
> взимате отгоре. Добавянето и премахването на чинии отдолу няма да работи!
> Добавянето на данни се нарича *избутване на стека*, а премахването -
> *премахване от стека*[^stack-ops]. Всички данни на стека трябва да имат
> известен фиксиран размер. Данни с размер, който не се знае по време на
> компилация, заължително трябва да се съхраняват на хийпа.
>
> [^stack-ops]: *pushing onto the stack* и *popping off the stack*, съответно,
>   на английски (бел. прев.)
>
> Хийпът е по-неорганизиран: когато слагате данни там, изисквате дадено
> пространство. Заделителят на памет намира празно място в хийпа, което да е
> достатъчно голямо, отбелязва го като заето и връща *указател*, което е адреса
> на това място в паметта. Този процес се нарича *заделяне на хийпа*[^heap-ops]
> или понякога просто *заделяне* (избутване на стойности на стека не се води
> заделяне). Поради причината, че указателят към хийпа е с известен фиксиран
> размер, можете да съхранявате указател на стека, но когато Ви трябват самите
> данни, трябва да последвате указателя. Представете си, че сте в ресторант.
> Когато влезете, казвате колко човека сте и сервитьорът намира празна маса,
> която да побира всички и Ви води до нея. Ако някой от групата Ви закъснява,
> може да попита къде сте настанени, за да Ви намери.
>
> [^heap-ops]: *allocating on the heap*, или просто *allocating* на английски
>   (бел. прев.)
>
> Избутването на стека е по-бързо от заделяне на хийпа, защото заделителят не
> трябва да търси място за съхранение на новите данни; това място винаги е
> най-горе на стека. За сравнение, заделянето на хийпа изисква повече работа,
> защото заделителят трябва първо да намери достатъчно голямо място, за да
> съхрани данните и после трябва да "разтреби" като подготовка за следващата
> операция.
>
> Достъпът на данни на хийпа е по-бавен от достъпа на стека, защото трябва да
> следвате указател, за да стигнете до тях. Днешните процесори са по-бързи ако
> прескачат по-малко в паметта. Продължавайки аналога, представете си сервитьор
> в ресторант, който взима поръчки от много маси. По-ефективно е да вземе
> всичките поръчки от една маса преди да продължи към следващата. Взимането на
> поръчка от маса А, после от маса Б, после пак от маса А и после пак от маса Б
> би било много по-бавен процес. По същия начин, процесора може да си върши
> работата по-добре ако работи върху данни, които са близо една до друга (както
> е на стека), спрямо такива, които са по-далеч (както е на хийпа).
>
> Когато кодът Ви извиква функция, стойностите, които се подават на тази функция
> (включително, потенциално, указатели към данни на хийпа), и локалните
> променливи на функцията се избутват на стека. Когато функцията приключи, тези
> стойности се премахват от стека.
>
> Следенето на това кои части от кода ползват кои части от данните на хийпа,
> минимизирането на дубликатни данни на хийпа и зачистването на неизползваните
> данни на хийпа, за да не Ви свърши пространството, са проблеми, които
> притежанието решава. Веднъж разберете ли притежанието, няма да Ви се налага да
> мислите за стека и хийпа много често, но знаейки, че основната идея на
> притежанието е да управлява данни на хийпа, може да помогне защо работи по
> този начин.

### Правилата за Притежание

Първо нека разгледаме правилата за притежание. Помнете ги докато работим върху
примерите, които ги демонстрират:

* Всяка стойност в Rust има *притежател*.
* Може да има само по един притежател по което и да е време.
* Когато притежателя излезе от обхват, стойността се освобождава.

### Обхват на Променливите

Сега като знаете основния синтаксис на Rust, няма да слагаме всичкия код от
`fn main() {` в примерите, така че ако работите успоредно с книгата, уверете се,
че сте сложили примерите във функция `main` на ръка. Така нашите примери ще са
по-сбити, което ни позволява да се концентрираме върху реалните детайли вместо
върху "скелето".

Като пръв пример на притежание, ще разгледаме *обхвата* на някои променливи.
Обхват е частта от програмата, в която даден елемент е валиден. Вижте следната
променлива:

```rust
let s = "hello";
```

Променливата `s` се отнася към буквален низ, където стойността на низа е твърдо-
написана в програмния текст. Променливата е валидна от момента на декларация до
края на сегашния *обхват*. Разпечатка 4-1 показва програма с коментари, които
показват къде променливата `s` е валидна.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```

<span class="caption">Разпечатка 4-1: Променлива и обхватът, в който е валидна
</span>

С други думи, тук има два важни момента:

* Когато `s` *влиза в* обхват, тя е валидна.
* Остава валидна докато не *излезе от* обхват.

Засега, отношението между обхвати и кога променливите са валидни е подобно на
това е други езици за програмиране. Сега ще надградим на това разбиране като
въведем типа `String`.

### Типа `String`

За да демонстрираме правилата на притежанието, ни е нужен тип данни, който е
по-сложен от тези, които покрихмв в раздела ["Типове Данни
"][data-types]<!-- ignore --> в глава 3. Типовете, които покрихме там, са с
известен размер, могат да бъдат съхранявани на стека и премахвани от стека,
когато обхвата им свърши и могат бързо и лесно да бъдат копирани, за да се
направи нова независима инстанция, ако друга част от кода се нуждае от същата
стойност в различен обхват. Но ние искаме да разглеждаме данни на хийпа и
разглеадаме как Rust знае кога да разчисти тези данни, а типа `String` е
прекрасен пример.

Ще се концентрираме на частите на `String`, които се отнасят към притежанието,
Тези аспекти също са приложими върху други комплексни типове данни, независимо
дали са предоставени от стандартната библиотека или създадени от Вас. Ще обсъдим
`String` по-дълбоко в [глава 8][ch8]<!-- ignore -->.

Вече сме виждали буквални низове, където стойността на низа е твърдо закодирана
в програмата ни. Буквалните низове са удбни, но не стават за всяка ситуация, в
която бихме искали да ползваме текст. Една причина е, че са непроменими. Друга
е, че не всяка низова стойност може да е позната докато си пишем кода: например
ако искаме да вземем потребителски вход и да го съхраним? За тези ситуации Rust
има втори низов тип - `String`. Този тип управлява данни заделени на хийпаи като
такъв, може да държи количества текст, които са неузнаваеми по време на
компилация. Можете да създадете `String` от буквален низ ползвайки функцията
`from`, по следния начин:

```rust
let s = String::from("hello");
```

Оператора двойно двуеточие `::` ни позволява да извадим тази конкретна функция
`from` от пространството от имена под типа `String` вместо да ползваме някое име
от сорта на `string_from`. Ще обсъдим този синтаксис повече в раздела
["Синтаксис за Методи"][method-syntax]<!-- ignore --> на глава 5 и когато
говорим за пространства от имена с модули в ["Пътища за Достъп на Елемент
в Модулното Дърво"][paths-module-tree]<!-- ignore --> в глава 7.

Този вид низове *могат* да бъде променени:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

И, каква е разликата тук? Защо можем да променяме `String`, но не и буквални
низове? Разликата е как тези два типа управляват паметта си.

### Memory and Allocation

In the case of a string literal, we know the contents at compile time, so the
text is hardcoded directly into the final executable. This is why string
literals are fast and efficient. But these properties only come from the string
literal’s immutability. Unfortunately, we can’t put a blob of memory into the
binary for each piece of text whose size is unknown at compile time and whose
size might change while running the program.

With the `String` type, in order to support a mutable, growable piece of text,
we need to allocate an amount of memory on the heap, unknown at compile time,
to hold the contents. This means:

* The memory must be requested from the memory allocator at runtime.
* We need a way of returning this memory to the allocator when we’re done with
  our `String`.

That first part is done by us: when we call `String::from`, its implementation
requests the memory it needs. This is pretty much universal in programming
languages.

However, the second part is different. In languages with a *garbage collector
(GC)*, the GC keeps track of and cleans up memory that isn’t being used
anymore, and we don’t need to think about it. In most languages without a GC,
it’s our responsibility to identify when memory is no longer being used and to
call code to explicitly free it, just as we did to request it. Doing this
correctly has historically been a difficult programming problem. If we forget,
we’ll waste memory. If we do it too early, we’ll have an invalid variable. If
we do it twice, that’s a bug too. We need to pair exactly one `allocate` with
exactly one `free`.

Rust takes a different path: the memory is automatically returned once the
variable that owns it goes out of scope. Here’s a version of our scope example
from Listing 4-1 using a `String` instead of a string literal:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

There is a natural point at which we can return the memory our `String` needs
to the allocator: when `s` goes out of scope. When a variable goes out of
scope, Rust calls a special function for us. This function is called
[`drop`][drop]<!-- ignore -->, and it’s where the author of `String` can put
the code to return the memory. Rust calls `drop` automatically at the closing
curly bracket.

> Note: In C++, this pattern of deallocating resources at the end of an item’s
> lifetime is sometimes called *Resource Acquisition Is Initialization (RAII)*.
> The `drop` function in Rust will be familiar to you if you’ve used RAII
> patterns.

This pattern has a profound impact on the way Rust code is written. It may seem
simple right now, but the behavior of code can be unexpected in more
complicated situations when we want to have multiple variables use the data
we’ve allocated on the heap. Let’s explore some of those situations now.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-move"></a>

#### Variables and Data Interacting with Move

Multiple variables can interact with the same data in different ways in Rust.
Let’s look at an example using an integer in Listing 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

<span class="caption">Listing 4-2: Assigning the integer value of variable `x`
to `y`</span>

We can probably guess what this is doing: “bind the value `5` to `x`; then make
a copy of the value in `x` and bind it to `y`.” We now have two variables, `x`
and `y`, and both equal `5`. This is indeed what is happening, because integers
are simple values with a known, fixed size, and these two `5` values are pushed
onto the stack.

Now let’s look at the `String` version:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

This looks very similar, so we might assume that the way it works would be the
same: that is, the second line would make a copy of the value in `s1` and bind
it to `s2`. But this isn’t quite what happens.

Take a look at Figure 4-1 to see what is happening to `String` under the
covers. A `String` is made up of three parts, shown on the left: a pointer to
the memory that holds the contents of the string, a length, and a capacity.
This group of data is stored on the stack. On the right is the memory on the
heap that holds the contents.

<img alt="Two tables: the first table contains the representation of s1 on the
stack, consisting of its length (5), capacity (5), and a pointer to the first
value in the second table. The second table contains the representation of the
string data on the heap, byte by byte." src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Figure 4-1: Representation in memory of a `String`
holding the value `"hello"` bound to `s1`</span>

The length is how much memory, in bytes, the contents of the `String` are
currently using. The capacity is the total amount of memory, in bytes, that the
`String` has received from the allocator. The difference between length and
capacity matters, but not in this context, so for now, it’s fine to ignore the
capacity.

When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the
pointer, the length, and the capacity that are on the stack. We do not copy the
data on the heap that the pointer refers to. In other words, the data
representation in memory looks like Figure 4-2.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap."
src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-2: Representation in memory of the variable `s2`
that has a copy of the pointer, length, and capacity of `s1`</span>

The representation does *not* look like Figure 4-3, which is what memory would
look like if Rust instead copied the heap data as well. If Rust did this, the
operation `s2 = s1` could be very expensive in terms of runtime performance if
the data on the heap were large.

<img alt="Four tables: two tables representing the stack data for s1 and s2,
and each points to its own copy of string data on the heap."
src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-3: Another possibility for what `s2 = s1` might
do if Rust copied the heap data as well</span>

Earlier, we said that when a variable goes out of scope, Rust automatically
calls the `drop` function and cleans up the heap memory for that variable. But
Figure 4-2 shows both data pointers pointing to the same location. This is a
problem: when `s2` and `s1` go out of scope, they will both try to free the
same memory. This is known as a *double free* error and is one of the memory
safety bugs we mentioned previously. Freeing memory twice can lead to memory
corruption, which can potentially lead to security vulnerabilities.

To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as
no longer valid. Therefore, Rust doesn’t need to free anything when `s1` goes
out of scope. Check out what happens when you try to use `s1` after `s2` is
created; it won’t work:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

You’ll get an error like this because Rust prevents you from using the
invalidated reference:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/output.txt}}
```

If you’ve heard the terms *shallow copy* and *deep copy* while working with
other languages, the concept of copying the pointer, length, and capacity
without copying the data probably sounds like making a shallow copy. But
because Rust also invalidates the first variable, instead of being called a
shallow copy, it’s known as a *move*. In this example, we would say that `s1`
was *moved* into `s2`. So, what actually happens is shown in Figure 4-4.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap.
Table s1 is grayed out be-cause s1 is no longer valid; only s2 can be used to
access the heap data." src="img/trpl04-04.svg" class="center" style="width:
50%;" />

<span class="caption">Figure 4-4: Representation in memory after `s1` has been
invalidated</span>

That solves our problem! With only `s2` valid, when it goes out of scope it
alone will free the memory, and we’re done.

In addition, there’s a design choice that’s implied by this: Rust will never
automatically create “deep” copies of your data. Therefore, any *automatic*
copying can be assumed to be inexpensive in terms of runtime performance.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-clone"></a>

#### Variables and Data Interacting with Clone

If we *do* want to deeply copy the heap data of the `String`, not just the
stack data, we can use a common method called `clone`. We’ll discuss method
syntax in Chapter 5, but because methods are a common feature in many
programming languages, you’ve probably seen them before.

Here’s an example of the `clone` method in action:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

This works just fine and explicitly produces the behavior shown in Figure 4-3,
where the heap data *does* get copied.

When you see a call to `clone`, you know that some arbitrary code is being
executed and that code may be expensive. It’s a visual indicator that something
different is going on.

#### Stack-Only Data: Copy

There’s another wrinkle we haven’t talked about yet. This code using
integers—part of which was shown in Listing 4-2—works and is valid:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-06-copy/src/main.rs:here}}
```

But this code seems to contradict what we just learned: we don’t have a call to
`clone`, but `x` is still valid and wasn’t moved into `y`.

The reason is that types such as integers that have a known size at compile
time are stored entirely on the stack, so copies of the actual values are quick
to make. That means there’s no reason we would want to prevent `x` from being
valid after we create the variable `y`. In other words, there’s no difference
between deep and shallow copying here, so calling `clone` wouldn’t do anything
different from the usual shallow copying, and we can leave it out.

Rust has a special annotation called the `Copy` trait that we can place on
types that are stored on the stack, as integers are (we’ll talk more about
traits in [Chapter 10][traits]<!-- ignore -->). If a type implements the `Copy`
trait, variables that use it do not move, but rather are trivially copied,
making them still valid after assignment to another variable.

Rust won’t let us annotate a type with `Copy` if the type, or any of its parts,
has implemented the `Drop` trait. If the type needs something special to happen
when the value goes out of scope and we add the `Copy` annotation to that type,
we’ll get a compile-time error. To learn about how to add the `Copy` annotation
to your type to implement the trait, see [“Derivable
Traits”][derivable-traits]<!-- ignore --> in Appendix C.

So, what types implement the `Copy` trait? You can check the documentation for
the given type to be sure, but as a general rule, any group of simple scalar
values can implement `Copy`, and nothing that requires allocation or is some
form of resource can implement `Copy`. Here are some of the types that
implement `Copy`:

* All the integer types, such as `u32`.
* The Boolean type, `bool`, with values `true` and `false`.
* All the floating-point types, such as `f64`.
* The character type, `char`.
* Tuples, if they only contain types that also implement `Copy`. For example,
  `(i32, i32)` implements `Copy`, but `(i32, String)` does not.

### Ownership and Functions

The mechanics of passing a value to a function are similar to those when
assigning a value to a variable. Passing a variable to a function will move or
copy, just as assignment does. Listing 4-3 has an example with some annotations
showing where variables go into and out of scope.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

<span class="caption">Listing 4-3: Functions with ownership and scope
annotated</span>

If we tried to use `s` after the call to `takes_ownership`, Rust would throw a
compile-time error. These static checks protect us from mistakes. Try adding
code to `main` that uses `s` and `x` to see where you can use them and where
the ownership rules prevent you from doing so.

### Return Values and Scope

Returning values can also transfer ownership. Listing 4-4 shows an example of a
function that returns some value, with similar annotations as those in Listing
4-3.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

<span class="caption">Listing 4-4: Transferring ownership of return
values</span>

The ownership of a variable follows the same pattern every time: assigning a
value to another variable moves it. When a variable that includes data on the
heap goes out of scope, the value will be cleaned up by `drop` unless ownership
of the data has been moved to another variable.

While this works, taking ownership and then returning ownership with every
function is a bit tedious. What if we want to let a function use a value but
not take ownership? It’s quite annoying that anything we pass in also needs to
be passed back if we want to use it again, in addition to any data resulting
from the body of the function that we might want to return as well.

Rust does let us return multiple values using a tuple, as shown in Listing 4-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

<span class="caption">Listing 4-5: Returning ownership of parameters</span>

But this is too much ceremony and a lot of work for a concept that should be
common. Luckily for us, Rust has a feature for using a value without
transferring ownership, called *references*.

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
