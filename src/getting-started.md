# C чего начать

Первый раздел книги рассказывает о том, как начать работать с Rust и его
инструментами. Сначала мы установим Rust, затем напишем классическую программу
«Привет, мир!», и, наконец, поговорим о Cargo, который представляет из себя
систему сборки и менеджер пакетов в Rust.

# Установка Rust

Первым шагом к использованию Rust является его установка. В этой главе нам
понадобится интернет соединение для выполнения команд, с помощью которых мы
загрузим Rust из интернета.

Мы воспользуемся несколькими командами в терминале, и они все будут начинаться
с `$`. Вам не нужно вводить `$`, они используются только для того чтобы
обозначить начало каждой команды. В интернете можно увидеть множество руководств
и примеров, которые следуют этому правилу: `$` обозначает команды, которые
выполняются с правами обычного пользователя и `#` для команд, которые
выполняются с правами администратора.

## Поддерживаемые платформы

Перечень платформ, на которых работает и для которых компилирует компилятор
Rust довольно большой, однако, не все платформы поддерживаются одинаково.
Уровни поддержки Rust разбиты на три уровня, у каждого из которых свой набор
гарантий.

Платформы идентифицируются по их "целевой тройке", которая является строкой,
сообщающей компилятору, какие выходные данные должны быть произведены. Столбцы
ниже указывают, работает ли соответствующий компонент на указанной платформе.

### Первый уровень

Первый уровень платформ может восприниматься как "гарантировано собирается и
работает". В частности, каждый из них удовлетворяет следующим требованиям:

 * Автоматические тесты обеспечивают тестирование этих платформ.
 * Изменения, принятые в ветку master репозитория `rust-lang/rust`, прошли
   тестирование.
 * Для этих платформ предоставляются официальные пакеты.
 * Доступна документация о том как собрать и использовать платформу.

|  Target                       | std |rustc|cargo| notes                      |
|-------------------------------|-----|-----|-----|----------------------------|
| `x86_64-pc-windows-msvc`      |  ✓  |  ✓  |  ✓  | 64-bit MSVC (Windows 7+)   |
| `i686-pc-windows-gnu`         |  ✓  |  ✓  |  ✓  | 32-bit MinGW (Windows 7+)  |
| `x86_64-pc-windows-gnu`       |  ✓  |  ✓  |  ✓  | 64-bit MinGW (Windows 7+)  |
| `i686-apple-darwin`           |  ✓  |  ✓  |  ✓  | 32-bit OSX (10.7+, Lion+)  |
| `x86_64-apple-darwin`         |  ✓  |  ✓  |  ✓  | 64-bit OSX (10.7+, Lion+)  |
| `i686-unknown-linux-gnu`      |  ✓  |  ✓  |  ✓  | 32-bit Linux (2.6.18+)     |
| `x86_64-unknown-linux-gnu`    |  ✓  |  ✓  |  ✓  | 64-bit Linux (2.6.18+)     |

### Второй уровень

Второй уровень платформ может восприниматься как "гарантировано собирается".
Автоматические тесты не поддерживаются и в связи с этим работоспособность сборки
не гарантируется. Но эти платформы обычно работают довольно хорошо, и
предложения по улучшению всегда приветствуются! В частности эти платформы
удовлетворяют следующим требованиям:

 * Настроена автоматическая сборка, но тестирования не происходит.
 * Изменения, принятые в ветку master репозитория `rust-lang/rust`, собираются
   для этих платформ. Имейте ввиду, что для некоторых платформ собирается только
   стандартная библиотека, но для остальных настроена полная раскрутка
   компилятора (bootstraping).
 * Для этих платформ предоставляются официальные пакеты.

|  Target                       | std |rustc|cargo| notes                      |
|-------------------------------|-----|-----|-----|----------------------------|
| `i686-pc-windows-msvc`        |  ✓  |  ✓  |  ✓  | 32-bit MSVC (Windows 7+)   |

### Третий уровень

Третий уровень платформ — это те, которые Rust поддерживает, но принятые
изменения автоматически не собираются и не тестируются. Для этих платформ
работоспособность сборки определятся степенью содействия сообщества. К тому же
официальные пакеты и установщики не предоставляются, но они могут быть
предоставлены сообществом.

|  Target                       | std |rustc|cargo| notes                      |
|-------------------------------|-----|-----|-----|----------------------------|
| `x86_64-unknown-linux-musl`   |  ✓  |     |     | 64-bit Linux with MUSL     |
| `arm-linux-androideabi`       |  ✓  |     |     | ARM Android                |
| `i686-linux-android`          |  ✓  |     |     | 32-bit x86 Android         |
| `aarch64-linux-android`       |  ✓  |     |     | ARM64 Android              |
| `arm-unknown-linux-gnueabi`   |  ✓  |  ✓  |     | ARM Linux (2.6.18+)        |
| `arm-unknown-linux-gnueabihf` |  ✓  |  ✓  |     | ARM Linux (2.6.18+)        |
| `aarch64-unknown-linux-gnu`   |  ✓  |     |     | ARM64 Linux (2.6.18+)      |
| `mips-unknown-linux-gnu`      |  ✓  |     |     | MIPS Linux (2.6.18+)       |
| `mipsel-unknown-linux-gnu`    |  ✓  |     |     | MIPS (LE) Linux (2.6.18+)  |
| `powerpc-unknown-linux-gnu`   |  ✓  |     |     | PowerPC Linux (2.6.18+)    |
| `i386-apple-ios`              |  ✓  |     |     | 32-bit x86 iOS             |
| `x86_64-apple-ios`            |  ✓  |     |     | 64-bit x86 iOS             |
| `armv7-apple-ios`             |  ✓  |     |     | ARM iOS                    |
| `armv7s-apple-ios`            |  ✓  |     |     | ARM iOS                    |
| `aarch64-apple-ios`           |  ✓  |     |     | ARM64 iOS                  |
| `i686-unknown-freebsd`        |  ✓  |  ✓  |     | 32-bit FreeBSD             |
| `x86_64-unknown-freebsd`      |  ✓  |  ✓  |     | 64-bit FreeBSD             |
| `x86_64-unknown-openbsd`      |  ✓  |  ✓  |     | 64-bit OpenBSD             |
| `x86_64-unknown-netbsd`       |  ✓  |  ✓  |     | 64-bit NetBSD              |
| `x86_64-unknown-bitrig`       |  ✓  |  ✓  |     | 64-bit Bitrig              |
| `x86_64-unknown-dragonfly`    |  ✓  |  ✓  |     | 64-bit DragonFlyBSD        |
| `x86_64-rumprun-netbsd`       |  ✓  |     |     | 64-bit NetBSD Rump Kernel  |
| `i686-pc-windows-msvc` (XP)   |  ✓  |     |     | Windows XP support         |
| `x86_64-pc-windows-msvc` (XP) |  ✓  |     |     | Windows XP support         |

Имейте ввиду, что эта таблица со временем может быть дополнена, это не
исчерпывающий набор третьего уровня!

## Установка на Linux или Mac

Если вы используете Linux или Mac, то всё что вам нужно сделать — это ввести
следующую команду в консоль:

```bash
$ curl -sSf https://static.rust-lang.org/rustup.sh | sh
```

Эта команда загрузит скрипт и начнёт установку. Если всё пройдёт успешно, то вы
увидите следующий текст:

```text
Welcome to Rust.

This script will download the Rust compiler and its package manager, Cargo, and
install them to /usr/local. You may install elsewhere by running this script
with the --prefix=<path> option.

The installer will run under ‘sudo’ and may ask you for your password. If you do
not want the script to run ‘sudo’ then pass it the --disable-sudo flag.

You may uninstall later by running /usr/local/lib/rustlib/uninstall.sh,
or by running this script again with the --uninstall flag.

Continue? (y/N)
```

Нажмите `y` для подтверждения и следуйте дальнейшим подсказкам.

## Установка на Windows

Если вы используете Windows, то скачайте подходящий [установщик][install-page].

[install-page]: https://www.rust-lang.org/install.html

## Удаление

Удалить Rust так же просто, как и установить его. На Linux или Mac нужно просто
запустить скрипт удаления:

```bash
$ sudo /usr/local/lib/rustlib/uninstall.sh
```

Если вы использовали установщик Windows, то просто повторно запустите `.msi`,
который предложит вам возможность удаления.

## Решение проблем

Если у вас установлен Rust, то можно открыть терминал и ввести:

```bash
$ rustc --version
```

Вы должны увидеть версию, хеш коммита и дату коммита.

Если это так, то теперь у вас есть установленный Rust! Поздравляем!

Если нет и вы пользователь Windows, то убедитесь в том, что Rust прописан в
вашей системной переменной %PATH%. Если это не так, то запустите установщик
снова, выберете "Change" на странице "Change, repair, or remove installation" и
убедитесь, что "Add to PATH" указывает на локальный жёсткий диск.

Существуют несколько мест, где вы можете получить помощь. Самый простой
вариант — [Канал #rust на irc.mozilla.org][irc], к которому вы можете
подключиться через [Mibbit][mibbit]. Нажмите на эту ссылку, и вы будете общаться
в чате с другими Rustaceans (это дурашливое прозвище, которым мы себя называем),
и мы поможем вам. Другие полезные ресурсы, посвящённые Rust: [форум
пользователей][users] и [Stack Overflow][stackoverflow].
Русскоязычные ресурсы: [сайт сообщества][rust-ru], [форум][forum-ru],
[Stack Overflow][stackoverflow-ru].

[irc]: irc://irc.mozilla.org/#rust
[mibbit]: http://chat.mibbit.com/?server=irc.mozilla.org&channel=%23rust
[users]: https://users.rust-lang.org/
[stackoverflow]: http://stackoverflow.com/questions/tagged/rust

[rust-ru]: http://rustycrate.ru
[forum-ru]: http://forum.rustycrate.ru
[stackoverflow-ru]: http://ru.stackoverflow.com/questions/tagged/rust

Установщик также устанавливает документацию, которая доступна без подключения к
сети. На UNIX системах она располагается в директории
`/usr/local/share/doc/rust`. В Windows используется директория `share/doc`,
относительно того куда вы установили Rust.

# Привет, мир!

Теперь, когда вы установили Rust, давайте напишем первую программу на Rust.
Традиционно при изучении нового языка программирования, первая написанная
программа просто выводит на экран «Привет, мир!», и мы следуем этой традиции.

Хорошо начинать с такой простой программы, поскольку можно убедиться, что ваш
компилятор не только установлен, но и работает правильно. Вывод информации на
экран будет замечательным способом проверить это.

> На самом деле это приводит к ещё одной проблеме, о которой мы должны
> предупредить: данное руководство предполагает, что у вас есть базовые навыки
> работы с командной строкой. Rust не выдвигает специфических требований к вашей
> среде разработки или тому, как вы храните свой код. Если вы предпочитаете
> использовать IDE, посмотрите на проект [SolidOak][solidoak], или на плагины к
> вашей любимой IDE. Есть множество расширений, разрабатываемых сообществом, а
> также [плагинов для разных редакторов][plugins], поддерживаемых командой Rust.
> Настройка вашего редактора или IDE выходит за пределы данного руководства.
> Посмотрите руководство по использованию выбранного вами плагина.

[solidoak]: https://github.com/oakes/SolidOak
[plugins]: https://github.com/rust-lang/rust/blob/master/src/etc/CONFIGS.md

## Создание проекта

Первое, с чего мы должны начать – создание файла для нашего кода. Для Rust не
имеет значения, где находится ваш код, но в рамках этого руководства мы
рекомендуем создать директорию *projects* в вашей домашней директории и хранить
там все ваши проекты. Откройте терминал и введите следующие команды чтобы
создать директорию для этого проекта:

```bash
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

> Если вы используете Windows и не используете PowerShell, ~ может не работать.
> Обратитесь к документации вашей оболочки для уточнения деталей.

## Написание и запуск программы на Rust

Теперь создадим новый файл для кода программы. Назовём наш файл *main.rs*.
Файлы с исходным кодом на Rust всегда имеют расширение *.rs*. Если вы
хотите использовать в имени вашего файла больше одного слова, разделяйте их
подчёркиванием; например *hello_world.rs*, а не *helloworld.rs*.

Теперь откройте только что созданный файл *main.rs* и добавьте в него следующий
код:

```rust
fn main() {
    println!("Привет, мир!");
}
```

Сохраните файл и вернитесь к вашему окну терминала. На Linux или OSX введите
следующие команды:

```bash
$ rustc main.rs
$ ./main
Привет, мир!
```

На Windows просто замените `main` на `main.exe`. В независимости от вашей ОС
вы должны увидеть строку `Привет, мир!` в терминале. Поздравляем! Вы написали
первую программу на Rust. Теперь вы Rust-разработчик! Добро пожаловать!

## Анатомия программ на Rust

Теперь давайте детально разберёмся, что происходит в программе "Привет, мир!".
Вот первый кусочек головоломки:

```rust
fn main() {

}
```

Эти строки объявляют «функцию» в Rust. Функция `main` особенна: это начало
каждой программы на Rust. Первая строка говорит: «Мы объявляем функцию,
именуемую `main`, которая не получает параметров и ничего не возвращает». Если
бы мы хотели передать в функцию параметры, то указали бы их в скобках (`(` и
`)`). Поскольку нам не надо ничего возвращать из этой функции, мы можем опустить
указание типа возвращаемого значения. Мы вернёмся к этому позже.

Вы должны были заметить, что функция обёрнута в фигурные скобки (`{` и `}`).
Rust требует оборачивать ими тело любой функции. Также хорошим стилем считается
ставить открывающую фигурную скобку на той же строке, что и объявление функции,
отделённую от него одним пробелом.

Теперь эта строка:

```rust
    println!("Привет, мир!");
```

Эта строка делает всю работу в нашей маленькой программе: выводит текст на
экран. Тут есть несколько нюансов, которые имеют существенное значение.
Во-первых, отступ в четыре пробела, а не табуляция.

Теперь разберёмся с `println!()`. Это вызов одного из [макросов][macro],
которыми представлено метапрограммирование в Rust. Если бы вместо макроса была
функция, это выглядело бы следующим образом: `println()` (без `!`). Позже мы
обсудим макросы Rust подробнее, а на данный момент все что вам нужно знать: если
вы видите `!`, то вызывается макрос вместо обычной функции.

[macro]: macros.html

Идём дальше. `"Привет, мир!"` — это «строка». Строки — это удивительно сложная
тема для системного языка программирования. Это [статически расположенная в
памяти][allocation] строка. Мы передаём строку в качестве аргумента в
`println!`, который выводит строки на экран. Достаточно просто!

[allocation]: the-stack-and-the-heap.html

Строка заканчивается точкой с запятой (`;`). Rust — [язык с ориентацией на
выражения][expression-oriented language], а это означает, что в нём большая
часть вещей является выражением. `;` используется для указания конца выражения и
начала следующего. Большинство строк кода на Rust заканчивается символом `;`.

[expression-oriented language]: glossary.html#expression-oriented-language

## Компиляция и запуск это отдельные шаги

В разделе "Написание и запуск программы на Rust" мы рассмотрели как запустить
только что созданную программу. Теперь мы разберём каждый шаг по отдельности.

Перед запуском программы её нужно скомпилировать. Вы можете воспользоваться
компилятором Rust с помощью команды `rustc` и передать ваш файл, как показано
здесь:

```bash
$ rustc main.rs
```

Если раньше вы программировали на С или С++, то заметите, что это напоминает
`gcc` или `clang`. После успешной компиляции Rust создаст двоичный исполняемый
файл. На Linux или OSX вы можете убедиться в этом с помощью команды `ls`:

```bash
$ ls
main  main.rs
```

Или в Windows:

```bash
$ dir
main.exe  main.rs
```

У нас есть два файла: файл с нашим исходным кодом, с расширением `.rs`, и
исполняемый файл (`main.exe` в Windows, `main` в остальных случаях). Все что
осталось сделать — это запустить `main` или `main.exe`:

```bash
$ ./main  # или main.exe на Windows
```
Мы вывели наш текст `"Привет, мир!"` в окне терминала.

Если раньше вы использовали динамические языки программирования вроде Ruby,
Python или JavaScript, то возможно разделение компиляции и запуска покажется вам
странным. Rust — это язык, на котором программы *компилируются перед
исполнением*. Это означает, что вы можете собрать программу, дать её кому-то
ещё, и ему не нужно устанавливать Rust для запуска этой программы. Если вы
передадите кому-нибудь `.rb`, `.py` или `.js` файл, им понадобится интерпретатор
Ruby, Python или JavaScript чтобы скомпилировать и запустить вашу программу (это
делается одной командой). В мире языков программирования много компромиссов, и
Rust сделал свой выбор.

Использовать `rustc` удобно лишь для небольших программ, но по мере роста
проекта, потребуется инструмент, который поможет управлять настройками проекта,
а также позволяет проще делиться кодом с другими людьми и проектами. Далее мы
познакомимся с новым инструментом `Cargo`, который используется для написания
настоящих программ на Rust.

# Привет, Cargo!

Cargo — это система сборки и пакетный менеджер для Rust, и Rustaceans используют
его для управления своими проектами на Rust. Cargo заботится о трёх вещах:
сборка кода, загрузка библиотек, от которых зависит ваш код, и сборка этих
библиотек. Библиотеки, которые нужны вашему коду, мы называем "зависимостями"
("dependencies"), поскольку ваш код зависит от них.

Поначалу вашей программе не понадобится никаких зависимостей, поэтому будем
использовать только первую часть его возможностей. Со временем нам понадобится
добавить несколько зависимостей, и нам не составит труда сделать это, используя
Cargo.

Подавляющее количество проектов на Rust используют Cargo, поэтому в рамках этой
книги мы будем исходить из того, что вы тоже делаете это. Если вы использовали
официальный установщик, то Cargo установился вместе с Rust. Если же вы
установили Rust каким-либо другим образом, то вы можете проверить, есть ли у вас
Cargo введя следующую команду в терминал:

```bash
$ cargo --version
```

Если вы увидели номер версии, то все в порядке. Если же вы увидели сообщение об
ошибке на подобии "`команда не найдена`", то вам нужно ознакомится с
документацией для системы, в которой вы установили Rust.

## Переход на Cargo

Давайте переведём наш проект "Привет, мир" на использование Cargo. Для перехода
на Cargo нужно сделать три вещи:

 1. Расположить файл с исходным кодом в правильной директории.
 2. Избавиться от старого исполняемого файла (`main.exe` или `main`) и сделать
    новый.
 3. Создать конфигурационный файл для Cargo.

Давайте сделаем это!

### Создание нового исполняемого файла и директории с исходным кодом

Для начала вернитесь к вашему терминалу, перейдите в вашу директорию
*hello_world* и введите следующие команды:

```bash
$ mkdir src
$ mv main.rs src/main.rs
$ rm main  # или 'del main.exe' для Windows
```

Cargo ожидает что ваши файлы с исходным кодом находятся в директории *src*.
Такой подход оставляет верхний уровень вашего проекта для вещей, вроде README,
файлов с текстом лицензии и других не относящихся к вашему коду. Cargo помогает
нам сохранять наши проекты красивыми и аккуратными. Всему есть своё место и все
находится на своих местах.

Теперь скопируйте *main.rs* в директорию *src* и удалите скомпилированный файл,
который вы создали с помощью `rustc`.

Отметим, что поскольку мы создаём исполняемый файл, то мы используем `main.rs`.
Если бы мы хотели создать библиотеку, то мы использовали бы lib.rs. Cargo
использует это соглашение для успешной компиляции вашего проекта, но вы можете
это изменить, если захотите.

### Создание конфигурационного файла

Теперь создайте новый файл внутри директории *hello_world* и назовите его
`Cargo.toml`.

Убедитесь в том, что имя правильное: вам нужна заглавная `C`! В противном случае
Cargo не найдёт конфигурационный файл.

Это файл в формате [TOML] (Tom's Obvious, Minimal Language). TOML это аналог
INI, но с некоторыми дополнениями, и он используется в конфигурационных файлах
для Cargo.

[TOML]: https://github.com/toml-lang/toml

Вставьте следующую информацию внутрь этого файла:

```toml
[package]

name = "hello_world"
version = "0.0.1"
authors = [ "Your name <you@example.com>" ]
```

Первая строка, `[package]`, говорит о том, что следующие параметры отвечают за
настройку пакета. Когда нам понадобится добавить больше информации в этот файл,
мы создадим другие разделы, но сейчас нам достаточно настроек пакета.

Другие три строчки устанавливают три значения конфигурации, которые необходимы
Cargo для компиляции вашей программы: имя, версия и автор.

После того как вы добавили эту информацию в *Cargo.toml*, сохраните изменения.
На этом создание конфигурационного файла завершено.

## Сборка и запуск Cargo проекта

Теперь, после создания файла `Carto.toml` в корневой директории, мы готовы
приступить к сборке и запуску нашего проекта. Чтобы сделать это, введите
следующие команды:

```bash
$ cargo build
   Compiling hello_world v0.0.1 (file:///home/yourname/projects/hello_world)
$ ./target/debug/hello_world
Привет, мир!
```

Та-да! Мы собрали наш проект вызвав `cargo build` и запустили его с помощью
`./target/debug/hello_world`. Мы можем сделать это в один шаг используя `cargo
run`:

```bash
$ cargo run
     Running `target/debug/hello_world`
Привет, мир!
```

Заметьте, что сейчас мы не пересобрали наш проект. Cargo понял, что мы не
изменили файл с исходным кодом и сразу запустил исполняемый файл. Если бы мы
изменили файл, мы бы увидели оба шага:

```bash
$ cargo run
   Compiling hello_world v0.0.1 (file:///home/yourname/projects/hello_world)
     Running `target/debug/hello_world`
Привет, мир!
```

На первый взгляд это кажется сложнее, по сравнению с более простым
использованием `rustc`, но давайте подумаем о будущем: если в нашем проекте
будет больше одного файла, мы должны будем вызывать rustc для каждого из них и
передавать кучу параметров, чтобы собрать их вместе. С Cargo, когда наш проект
вырастет, нам понадобится вызвать только команду `cargo build` и она всё сделает
за нас.

## Сборка релизной версии

Когда вы закончите работать над проектом, и он окончательно будет готов к
релизу, используйте команду `cargo build --release` для компиляции вашего
проекта с оптимизацией. Эти оптимизации делают ваш код на Rust быстрее, но
требуют больше времени на компиляцию. Именно из-за этого существует два разных
способа: один для разработки, другой для сборки финальной версии, которую вы
отдалите пользователям.

Также вы должны были заметить, что Cargo создал новый файл: `Cargo.lock`.

```toml
[root]
name = "hello_world"
version = "0.0.1"
```

Этот файл используется Cargo для отслеживания зависимостей в вашем приложении.
Прямо сейчас у нас нет ни одной, поэтому этот файл немного пустоват. Вам не
нужно редактировать этот файл самостоятельно, Cargo сам с ним разберётся.

Вот и все! Мы успешно собрали `hello_world` с помощью Cargo.

Несмотря на то, что наша программа проста, мы использовали большую часть
реальных инструментов, которые вы будете использовать в своём дальнейшем пути
Rust-программиста. Более того, вы можете рассчитывать, что практически все
проекты на Rust можно будет собрать с помощью вариации этих команд:

```bash
$ git clone someurl.com/foo
$ cd foo
$ cargo build
```

## Простой способ создать новый Cargo проект

Вам не нужно повторять вышеприведённые шаги каждый раз, когда вы хотите создать
новый проект! Cargo может создать директорию проекта, в которой вы сразу сможете
приступить к разработке.

Чтобы создать новый проект с помощью Cargo, нужно ввести команду `cargo new`:

```bash
$ cargo new hello_world --bin
```

Мы указываем аргумент `--bin`, так как хотим создать исполняемую программу. Если
мы не укажем этот аргумент, то Cargo создаст проект для библиотеки. Исполняемые
файлы частно называют *бинарниками* (поскольку обычно они находятся в
`/usr/bin`, если вы используете Unix систему).

Cargo сгенерировал два файла и одну директорию: `Cargo.toml` и директорию *src*
с файлом *main.rs*. Они должны выглядеть так же как те, что мы создали до этого.

Этого достаточно для того чтобы начать. Открыв `Cargo.toml`, вы должны увидеть
следующее:

```toml
[package]

name = "hello_world"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
```

Cargo наполнил этот файл значениями по умолчанию на основании переданных
аргументов и глобальной конфигурации `git`. Также он инициализировал
директорию `hello_world` как `git` репозитоий.

Вот что должно быть внутри `src/main.rs`:

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo создал «Hello World!» для нас и вы уже можете приступить к
программированию!

> У Cargo есть собственное [руководство][guide], в котором про него рассказано
> более детально.

[guide]: http://doc.crates.io/guide.html

# Заключение

Это основы, которые вы будете часто использовать на протяжении всего вашего
взаимодействия с Rust. Теперь давайте отложим инструментарий и узнаем больше о
самом языке.

У вас есть два пути: погрузиться в изучение реального проекта, открыв раздел
«[Изучение Rust][learnrust]», или начать с самого низа и постепенно продвигаться
наверх, начав с раздела «[Синтаксис и семантика][syntax]». Программисты, имеющие
опыт работы с системными языками, вероятно, предпочтут «Изучение Rust», в то
время как программисты, имеющие опыт работы с динамическими языками, скорее
всего захотят пойти по второму пути. Разные люди учатся по-разному! Выберите то,
что подходит именно вам.

[learnrust]: learn-rust.html
[syntax]: syntax-and-semantics.html
