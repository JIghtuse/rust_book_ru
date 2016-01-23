% Итераторы

Давайте поговорим о циклах.

Помните цикл `for` в Rust? Вот пример:

```rust
for x in 0..10 {
    println!("{}", x);
}
```

Теперь, когда вы знаете о Rust немного больше, мы можем детально обсудить, как
же это работает. Диапазоны (`0..10`) являются «итераторами». Итератор — это
сущность, для которой мы можем неоднократно вызвать метод `.next()`, в
результате чего мы получим последовательность элементов.

Как представлено ниже:

```rust
let mut range = 0..10;

loop {
    match range.next() {
        Some(x) => {
            println!("{}", x);
        },
        None => { break }
    }
}
```

Мы связываем с диапазоном изменяемое имя, которая и является нашим итератором.
Затем мы используем цикл `loop` с внутренней конструкцией `match`. Здесь `match`
применяется к результату `range.next()`, который выдает нам ссылку на следующее
значение итератора. В данном случае `next` возвращает `Option<i32>`, который
представляет собой `Some(i32)` когда у нас есть значение и `None` когда перебор
элементов закончен. Если мы получаем `Some(i32)`, то печатаем его, а если
`None`, то прекращаем выполнение цикла оператором `break`.

Этот пример, по большому счету, делает то же самое, что и пример с циклом `for`.
Цикл `for` — просто удобный способ записи конструкции `loop`/`match`/`break`.

Однако, цикл `for` не является единственной конструкцией, которая использует
итераторы. Написание своего собственного итератора заключается в реализации
типажа `Iterator`. Хотя эта тема и выходит за рамки данного руководства, Rust
предоставляет ряд полезных итераторов для выполнения различных задач. Прежде чем
мы поговорим о них, мы должны рассказать о плохой практике в Rust, связанной с
использованием диапазонов. Она продемонстрирована в примере ниже.

Вот, только что мы говорили о том, какие диапазоны крутые. Но диапазоны также и
очень примитивны. Например, если вам нужно перебрать содержимое вектора, у вас
может возникнуть желание написать так:

```rust
let nums = vec![1, 2, 3];

for i in 0..nums.len() {
    println!("{}", nums[i]);
}
```

Это намного хуже, чем если бы мы использовали итератор непосредственно. Вы
можете пройти по элементам векторов напрямую, как показано ниже:

```rust
let nums = vec![1, 2, 3];

for num in &nums {
    println!("{}", num);
}
```

Есть две причины предпочесть прямое использование итератора. Во-первых, это
яснее выражает наше намерение. Мы обходим элементы вектора, а не индексы с
последующей индексацией вектора. Во-вторых, эта версия является более
эффективной: первая версия будет выполнять дополнительные проверки границ,
потому что используется индексация, `nums[i]`. Во втором примере нет никаких
проверок границ, поскольку мы получаем ссылки на каждый элемент вектора, одну за
одной, по мере итерирования. Это очень распространенный прием работы с
итераторами: мы можем игнорировать ненужные проверки границ, но все еще быть
уверенными, что мы в безопасности.

Остается неясной еще одна деталь работы `println!`. На самом деле `num` имеет
тип `&i32`. То есть, это ссылка на `i32`, а не сам `i32`. `println!` выполняет
разыменование переменной за нас, поэтому мы не видим его в исходном коде. Этот
код также прекрасно работает:

```rust
let nums = vec![1, 2, 3];

for num in &nums {
    println!("{}", *num);
}
```

Здесь мы явно разыменовываем `num`. Почему `&nums` выдает нам ссылки? Во-первых,
потому что мы явно попросили его об этом с помощью `&`. Во-вторых, если он будет
выдавать нам сами данные, то мы должны быть их владельцем, что подразумевает
создание копии данных и выдачу этой копии нам. Со ссылками же мы просто
заимствуем ссылку на данные, и поэтому будет выдана просто ссылка, без
необходимости перемещать данные.

Теперь, когда мы установили, что зачастую диапазоны — это не то, что нужно,
давайте поговорим о том, что же можно использовать вместо диапазонов.

Есть три основных класса объектов, которые имеют отношение к данному вопросу:
*итераторы*, *адаптеры итераторов* и *потребители*. Вот некоторые определения:

* *итераторы* выдают последовательность значений;
* *адаптеры итераторов* применяются к итератору и выдают новый итератор с другой
  выходной последовательностью;
* *потребители* применяются к итератору, выдающему некоторый конечный набор
  значений.

Давайте сначала поговорим о потребителях, так как итераторы вы уже видели — это
диапазоны.

## Потребители

*Потребитель* применяется к итератору, возвращая какое-то значение или значения.
Наиболее распространенным потребителем является `collect()`. Этот код не
компилируется, но он показывает идею:

```rust,ignore
let one_to_one_hundred = (1..101).collect();
```

Как вы можете видеть, мы вызываем `collect()` для нашего итератора. `collect()`
принимает столько значений, сколько выдаст итератор, и возвращает коллекцию
результатов. Так почему же этот код не компилируется? Rust не может определить,
в какую коллекцию (например, вектор, список, и т.д.) вы хотите собрать элементы,
и поэтому тип необходимо указать явно. Вот версия, которая компилируется:

```rust
let one_to_one_hundred = (1..101).collect::<Vec<i32>>();
```

Если помните, синтаксис `::<>` позволяет задать подсказку типа. Поэтому в
приведенном примере мы указали, что хотим вектор целых чисел. Хотя не всегда
бывает нужно задавать весь тип целиком. Использование символа `_` позволит вам
задать частичную подсказку типа:

```rust
let one_to_one_hundred = (1..101).collect::<Vec<_>>();
```

Эта запись говорит компилятору Rust: «Пожалуйста, собери элементы в `Vec<T>`, а
вывод типа `T` сделай самостоятельно». По этой причине символ `_` иногда
называют «заполнителем типа».

`collect()` является наиболее распространенным из потребителей, но есть и
другие. Например `find()`:

```rust
let greater_than_forty_two = (0..100)
                             .find(|x| *x > 42);

match greater_than_forty_two {
    Some(_) => println!("У нас есть несколько чисел!"),
    None => println!("Числа не найдены :("),
}
```

`find` принимает замыкание, которое обрабатывает ссылку на каждый элемент
итератора. Замыкание возвращает `true`, если элемент является искомым элементом,
и `false` в противном случае. Так как нам не всегда удается найти
соответствующий элемент, `find` возвращает `Option`, а не сам элемент.

Еще один важный потребитель — `fold`. Вот как он выглядит:

```rust
let sum = (1..4).fold(0, |sum, x| sum + x);
```

`fold()` — это потребитель, который схематично можно представить в виде:
`fold(base, |accumulator, element| ...)`. Он принимает два аргумента: первый -
это элемент, называемый *базой*; второй — это замыкание, которое, в свою очередь,
само принимает два аргумента: первый называется *аккумулятор*, а второй -
*элемент*. На каждой итерации вызывается замыкание, результат выполнения
которого становится значением аккумулятора на следующей итерации. На первой
итерации значение аккумулятора равно базе.

Это немного запутанно. Давайте рассмотрим значения всех элементов итератора:

| база | аккумулятор | элемент | результат замыкания |
|------|-------------|---------|---------------------|
| 0    | 0           | 1       | 1                   |
| 0    | 1           | 2       | 3                   |
| 0    | 3           | 3       | 6                   |

Мы вызвали `fold()` с этими аргументами:

```rust
# (1..4)
.fold(0, |sum, x| sum + x);
```

Таким образом, `0` — это база, `sum` — это аккумулятор, а `x` — это элемент. На
первой итерации мы устанавливаем `sum` равной `0`, а `x` становится первым
элементом `nums`, `1`. Затем мы прибавляем `x` к `sum`, что дает нам `0 + 1 =
1`. На второй итерации это значение становится значением аккумулятора, `sum`, а
элемент становится вторым элементом массива, `2`. `1 + 2 = 3`, результат этого
выражения становится значением аккумулятора на последней итерации. На этой
итерации, `x` становится последним элементом, `3`, а значение выражения `3 + 3 =
6` является конечным значением нашей суммы. `1 + 2 + 3 = 6` — это результат,
который мы получили.

Вот так. `fold` может показаться немного странным, если вы используете его
впервые, но когда вы освоите его, то будете использовать его повсеместно. `fold`
подходит для случаев, когда у вас есть список элементов, а вам нужно получить
один единственный результат.

Потребители имеют очень большое значение в связи с одним свойством итераторов, о
котором мы еще не говорили: ленивость. Давайте ещё немного поговорим об
итераторах, и вы поймете, почему потребители так важны.

## Итераторы

Как мы уже говорили ранее, итератор являются сущностью, для которой мы можем
неоднократно вызвать метод `.next()`, в результате чего мы получим
последовательность элементов. Для получения каждого следующего элемента нужно
вызвать метод, а это означает, что итераторы *ленивы* — они не обязаны
создавать все значения заранее. Например, этот код на самом деле не генерирует
номера `1-99`, а просто создает значение, представляющее эту последовательность:

```rust
let nums = 1..100;
```

В этом примере мы никак не использовали диапазон, поэтому он и не создавал
последовательность. Давайте добавим потребителя:

```rust
let nums = (1..100).collect::<Vec<i32>>();
```

Теперь `collect()` потребует, чтобы диапазон выдавал ему какие-нибудь числа,
поэтому он сгенерирует последовательность.

Диапазоны — это один из двух основных типов итераторов. Другой часто
используемый итератор — `iter()`. `iter()` может преобразовать вектор в простой
итератор, который выдает вам каждый элемент по очереди:

```rust
let nums = vec![1, 2, 3];

for num in nums.iter() {
   println!("{}", num);
}
```

Эти два основных итератора хорошо послужат вам. Есть и более продвинутые
итераторы, в том числе и те, которые генерируют бесконечную последовательность.

Вот и все, что касается итераторов. Последнее понятие в этой теме, о котором мы
хотели бы рассказать — адаптеры итераторов. Давайте перейдем к нему!

## Адаптеры итераторов

*Адаптеры итераторов* получают итератор и изменяют его каким-то образом, выдавая
новый итератор. Простейший из них называется `map`:

```rust,ignore
(1..100).map(|x| x + 1);
```

`map` вызывается для итератора, и создает новый итератор, каждый элемент
которого получается в результате вызова замыкания, в качестве аргумента которому
передается ссылка на исходный элемент. Так что этот код выдаст нам числа
`2-100`. Ну, почти! Если вы скомпилируете пример, этот код выдаст
предупреждение:

```text
warning: unused result which must be used: iterator adaptors are lazy and
         do nothing unless consumed, #[warn(unused_must_use)] on by default
(1..100).map(|x| x + 1);
 ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

Причина этого — ленивость итераторов! То замыкание никогда не будет выполнено.
Пример ниже не напечатает ни одного значения:

```rust,ignore
(1..100).map(|x| println!("{}", x));
```

Если вы пытаетесь выполнить замыкание ради побочных эффектов (вроде печати), то
вместо этого просто используйте `for`.

Есть масса интересных адаптеров итераторов. `take(n)` вернет итератор,
представляющий следующие `n` элементов исходного итератора. Обратите внимание,
что это не оказывает никакого влияния на оригинальный итератор. Давайте
попробуем применить его для бесконечных итераторов, которые мы упоминали раньше:

```rust
# #![feature(step_by)]
for i in (1..).step_by(5).take(5) {
    println!("{}", i);
}
```

Этот код напечатает

```text
1
6
11
16
21
```

`filter()` представляет собой адаптер, который принимает замыкание в качестве
аргумента. Это замыкание возвращает `true` или `false`. Новый итератор,
полученный применением `filter()`, будет выдавать только те элементы, для
которых замыкание возвращает `true`:

```rust
for i in (1..100).filter(|&x| x % 2 == 0) {
    println!("{}", i);
}
```

Этот пример будет печатать все четные числа от одного до ста. (Обратите
внимание, что мы используем образец `&x`, чтобы извлечь само целое число. Это
необходимо, поскольку `filter` не потребляет элементы, которые выдаются во время
итерации, а лишь выдаёт ссылку.)

Вы можете соединить все три понятия вместе: начать с итератора, адаптировать его
несколько раз, а затем потребить результат. Например:

```rust
(1..)
    .filter(|&x| x % 2 == 0)
    .filter(|&x| x % 3 == 0)
    .take(5)
    .collect::<Vec<i32>>();
```

Этот код выдаст вектор, содержащий `6`, `12`, `18`, `24`, `30`.

Это просто небольшой обзор того, как итераторы, адаптеры итераторов и
потребители могут помочь вам. Уже написано множество действительно полезных
итераторов, и вы также можете написать свой собственный итератор. Итераторы
обеспечивают безопасный и эффективный способ работы со всеми видами списков.
Сперва работать с ними немного непривычно, но чем больше вы с ними
сталкиваетесь, тем больше они вас цепляют. Для получения полного списка
различных итераторов, адаптеров и потребителей смотрите
[документацию модуля iter](http://doc.rust-lang.org/std/iter/index.html).