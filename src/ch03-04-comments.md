## Коментари

Всички програмисти се стремят да пишат код така, че да е лесен за разбиране, но
понякога има нужда от допълнителни обяснения. В тези случаи програмистите
оставят *коментари* в изходиния си код, които компилатора ще игнорира, но
хората, които четат кода, биха намерили за полезни.

Ето прост коментар:

```rust
// hello, world
```

В Rust, идиоматичния стил за коментари започва коментар с две наклонени черти и
коментара продължава до края на реда. За коментари, които са по-дълги от един
ред, ще трябва да добавяте `//` на всеки ред, по следния начин:

```rust
// So we’re doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what’s going on.
```

Коментарите могат също да се слагат на края на редове съдържащи код:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

Но по-често ще ги видите ползвани в следния формат, където коментарът е на
оъделен ред над кода, който обяснява:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

Rust има още един вид коментари - документационни коментари, които ще обсъдим в
раздела ["Публикуване на Щайга в Crates.io"][publishing]<!-- ignore --> на глава
14.

[publishing]: ch14-02-publishing-to-crates-io.html
