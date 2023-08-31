<!-- # Common Programming Concepts -->
# Общи Идеи от Програмирането

<!-- This chapter covers concepts that appear in almost every programming language
and how they work in Rust. Many programming languages have much in common at
their core. None of the concepts presented in this chapter are unique to Rust,
but we’ll discuss them in the context of Rust and explain the conventions
around using these concepts. -->
Тази глава покрива идеи, които се появяват в почти всеки език за програмиране и
как те работят в Rust. Много езици за програмиране имат много общо в кода си.
Нито една от концепциите в тази глава са уникални за Rust, но ще ги разгледаме в
контакста на Rust и ще обясним конвнциите около ползването им.

<!-- Specifically, you’ll learn about variables, basic types, functions, comments,
and control flow. These foundations will be in every Rust program, and learning
them early will give you a strong core to start from. -->
По-точно, ще научите за променливи, основни типове, функции, коментари и
управление потока на изпълнението. Тези основи ще са в основата на всяка
програма на Rust и ранното им заучаване ще Ви даде добри основи, на които да
стъпите.

<!-- > #### Keywords
>
> The Rust language has a set of *keywords* that are reserved for use by the
> language only, much as in other languages. Keep in mind that you cannot use
> these words as names of variables or functions. Most of the keywords have
> special meanings, and you’ll be using them to do various tasks in your Rust
> programs; a few have no current functionality associated with them but have
> been reserved for functionality that might be added to Rust in the future. You
> can find a list of the keywords in [Appendix A][appendix_a]ignore. -->

> #### Ключови думи
>
> Езикът Rust има набор от *ключови думи*, които са запазени за ползване само
> от езика, както е и в други езици. Помнете, че не можете да ползвате тези думи
> като имена на променливи или функции. Повечето от тези думи имат специално
> значение и ще ги ползвате за да извършвате различни задачи във Вашите програми
> на Rust; някои понастоящем нямат функционалност обвързана с тях, но са
> запазени за функционалност, която може да бъде добавена в Rust в бъдещето. 
> Можете да намерите списък с ключовите думи в [Приложение
> А][appendix_a]<!-- ignore -->.

[appendix_a]: appendix-01-keywords.md
