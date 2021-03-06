# Введение

Добро пожаловать! Эта книга обучает основным принципам работы с языком
программирования [Rust][rust]. Rust — это системный язык программирования,
внимание которого сосредоточено на трёх задачах: безопасность, скорость и
параллелизм. Он решает эти задачи без сборщика мусора, что делает его полезным в
ряде случаев, когда использование других языков было бы нецелесообразно: при
встраивании в другие языки, при написании программ с особыми пространственными и
временными требованиями, при написании низкоуровневого кода, такого как драйверы
устройств и операционные системы. Во время компиляции Rust делает ряд проверок
безопасности. За счёт этого не возникает накладных расходов во время выполнения
приложения и устраняются все гонки данных.  Это даёт Rust преимущество над
другими языками программирования, имеющими аналогичную направленность. Rust
также направлен на достижение «абстракции с нулевой стоимостью». Хотя некоторые
из этих абстракций и ведут себя как в языках высокого уровня, но даже тогда Rust
по-прежнему обеспечивает точный контроль, как делал бы язык низкого уровня.

[rust]: http://rust-lang.org

Книга «Язык программирования Rust» делится на восемь разделов. Это введение
является первым из них. Затем идут:

* [C чего начать][gs] — Настройка компьютера для разработки на Rust.
* [Изучение Rust][lr] — Обучение программированию на Rust на примере небольших
проектов.
* [Синтаксис и семантика][ss] — Каждое понятие Rust разбивается на небольшие
кусочки.
* [Эффективное использование Rust][er] — Понятия более высокого уровня для
написания качественного кода на Rust.
* [Нестабильные возможности Rust][nr] — Передовые возможности, которые пока не
добавлены в стабильную сборку.
* [Глоссарий][gl] — Ссылки на термины, используемые в книге.
* [Библиография][bi] — Литература, которая оказала влияние на Rust.

[gs]: getting-started.html
[lr]: learn-rust.html
[er]: effective-rust.html
[ss]: syntax-and-semantics.html
[nr]: nightly-rust.html
[gl]: glossary.html
[bi]: bibliography.html

После прочтения этого введения, в зависимости от ваших предпочтений, вы можете
продолжить дальнейшее изучение либо в направлении «Изучение Rust», либо в
направлении «Синтаксис и семантика». Если вы предпочитаете изучить язык на
примере реального проекта, лучшим выбором будет раздел «Изучение Rust». Раздел
«Синтаксис и семантика» подойдёт тем, кто предпочитает тщательно изучить каждое
понятие языка отдельно, перед тем как двигаться дальше. Большое количество
перекрёстных ссылок соединяет эти части воедино.

### Содействие

Исходные файлы, из которых генерируется оригинал этой книги, могут быть найдены
на [GitHub][book].

[book]: https://github.com/rust-lang/rust/tree/master/src/doc/book

Исходные файлы перевода этой книги на русский язык также находятся на GitHub:
[https://github.com/ruRust/rust_book_ru](https://github.com/ruRust/rust_book_ru)
