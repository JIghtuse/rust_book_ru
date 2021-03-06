# Безразмерные типы

Большинство типов имеют определённый размер в байтах. Этот размер обычно
известен во время компиляции. Например, `i32` — это 32 бита, или 4 байта.
Однако, существуют некоторые полезные типы, которые не имеют определённого
размера. Они называются «безразмерными» или «типами динамического размера». Один
из примеров таких типов — это `[T]`. Этот тип представляет собой
последовательность из определённого числа элементов `T`. Но мы не знаем, как
много этих элементов, поэтому размер неизвестен.

Rust понимает несколько таких типов, но их использование несколько ограничено.
Есть три ограничения:

1. Мы можем работать с экземпляром безразмерного типа только с помощью
   указателя. `&[T]` будет работать, а `[T]` — нет.
2. Переменные и аргументы не могут иметь тип динамического размера.
3. Только последнее поле структуры может быть безразмерного типа; другие — нет.
   Варианты перечислений не могут содержать типы динамического размера в
   качестве данных.

А зачем это всё? Поскольку мы можем использовать `[T]` только через указатель,
если бы язык не поддерживал безразмерные типы, мы бы не смогли написать такой
код:

```rust,ignore
impl Foo for str {
```

или

```rust,ignore
impl<T> Foo for [T] {
```

Вместо этого, вам бы пришлось написать:

```rust,ignore
impl Foo for &str {
```

Таким образом, данная реализация работала бы только для [ссылок][ref], и не
поддерживала бы другие типы указателей. А реализацию для безразмерного типа
смогут использовать любые указатели, включая определённые пользователем умные
указатели (позже, когда будут исправлены некоторые ошибки).

[ref]: references-and-borrowing.html

# ?Sized

Если вы пишете функцию, принимающую тип динамического размера, вы можете
использовать специальное ограничение `?Sized`:

```rust
struct Foo<T: ?Sized> {
    f: T,
}
```

Этот `?` читается как «Т может быть размерным (`Sized`)». Он означает, что это
ограничение особенное: оно разрешает использование некоторых типов, которые не
могли бы быть использованы в его отсутствие. Таким образом, оно *расширяет*
множество подходящих типов, а не сужает его. Это можно представить себе как если
бы все типы `T` неявно были размерными (`T: Sized`), а `?` отменял это
ограничение по-умолчанию.
