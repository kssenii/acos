# Терминлал

## Экранирование пробелов

```bash
mkdir ~/Program\ Files
mkdir ~/"Program Files (x86)"
```

## Потоки

Записать строку в файл (создать при отсутствии):

```bash
echo Hello, > hello.txt
echo World! >> hello.txt # append
```

Различные потоки вывода:

```bash
echo Smth    1> out.txt  # stdout
./error_prog 2> err.txt  # stderr
```

Поток ввода:

```bash
./count_words < in.txt
```

## Прерывание ввода и завершение

```text
Ctrl+D - принудительное закрытие входного потока
Ctrl+C - принудительное завершение программы (посылает сигнал)
Ctrl+Z - приостановка программы (SIGSTP), позже можно возобновить
```

## Команда `cat`

Выводит содержимое одного или нескольких файлов в стандартный поток вывода.

**Пример:**

```bash
cat file1.txt file2.txt  # на экран будет выведено содержимое сначала
                         # file1.txt, затем file2.txt
```

Эта команда часто используется для слияния нескольких файлов в один (в том
числе не только текстовых, но и бинарных):

```bash
cat header.txt body.txt footer.txt > document.txt
```

Если вызвать команду `cat` без аргументов, то в качестве входа испоьзуется
*стандартный поток ввода*. Таким способом можно создавать текстовые файлы,
не используя текстовый редактор:

```bash
cat > new_file.txt  # Будет осуществляться ввод с клавиатуры до нажатия Ctrl+D
```

## Команды `more` и `less` и перенаправление потока

Не все терминалы имеют возможность прокрутки содержимого, поэтому вывод командой
`cat` (или любой другой командой) содержимого большого текстового файла не
влезет на один экран.

Для того, чтобы организовать возможность прокрутки, предусмотрены команды
`more`, или `less`, которые принимают на вход текст из стандартного потока
ввода, и выводят его на экран постранично.

Команда `more` имеет возможность только прокрутки вперед с помощью клавиши
`ПРОБЕЛ`, команда `less` позволяет прокручивать текст как вперед, так и назад.

```bash
cat lorem_ipsum.txt | less
```

В данном примере программа `cat` выводит содержимое файла `lorem_ipsum.txt`
в стандартный поток вывода, а программа `less` отображает данные на экране из
стандартного потока ввода.

Оператор `|` между командами соединяет стандартный поток
вывода первой команды (`cat lorem_ipsum.txt`) со стандартным потоком
ввода второй команды (`less`), образуя *канал* передачи данных.

### `grep`

Выполняет фильрацию строк входного потока по *регулярному выражению* или
входжению некоторого текста и выводит результат в поток вывода.

Шаблон поиска является аргументом (заключается в кавычки, если содержит
пробельные символы или начинается с символа `-`). Часто используемые опции:

* `-F` - выполнять поиск простого текста, а не регулярного выражения
* `-v` - инвертировать условие поиска на противоположное
* `-q` - ничего не выводить на экран (если требуется только код возврата)

Если найдена хотя бы одна строка, то код вовзрата равен `0`, иначе `1`.
Код возврата, отличный от `0` и `1` означает ошибку.

**Пример использования:**

**Файл some_text.txt:**

```bash
Hello, World!
# Hello
# This is a comment
This is not a comment
```

**Вывести все строки, содержащие слово 'Hello':**

```bash
cat some_text.txt | grep -F Hello
```

**Вывести все строки, содержащие фразу 'a comment':**

```bash
cat some_text.txt | grep -F "a comment"
```

**Вывести все строки, начинающиеся с символа решетки:**

```bash
cat some_text.txt | grep ^#
  # символ ^ в регулярном выражении обозначает начало строки
```

**Вывести все строки, НЕ начинающиеся с символа решетки:**

```bash
cat some_text.txt | grep -v ^#
```

**Определить, есть ли в тексте строка, начинающаяся с символа решетки:**

```bash
cat some_text.txt | grep -q ^#
echo $?  # результат 0, если найдена хотя бы одна строка
```

## Основные команды, о которых нужно помнить всегда

* `man` - **самая нужная команда**; отображает справку по какой-либо команде
* `cd` - перейти в другой каталог
* `ls` - вывести содержимое каталога
* `mkdir` - создать каталог
* `cp` - скопировать файл или каталог
* `mv` - переименовать файл или каталог
* `rm` - удалить файл или каталог.

Для большинства команд предусмотрен аргумент `--help`, который отображает
короткую инструкцию по использованию команды.

### Команда `man`

`man РАЗДЕЛ ИМЯКОМАНДЫ` выводит справочную информацию по стандартным командам
UNIX, системным вызовам или функциям стандартной библиотеки Си.

Документация разбита на несколько разделов:

* `1` - команды оболочки
* `1p` - команды оболочки; выводится документация только на ту
функциональность, которая соответствует стандарту POSIX
* `2` - системные вызовы ядра операционной системы
* `3` - функции стандартной библиотеки Си
* `3p` - Си-функции POSIX API
* `4` - описания модулей ядра и системных устройств
* `5` - описания форматов конфигурационных файлов
* `6` - описания программ для X11
* `7` - различные руководства, не попадающие под какую-либо классификацию
* `8` - руководства по администрированию
* `n` - руководства Tcl.

Указывать раздел не обязательно. В этом случае будет отображен первый
найденный результат (как правило, руководство по команде). Некоторые
дистрибутивы, например openSUSE, сконфигурированы таким образом, что
команда `man` явно спрашивает номер раздела, если по запросу есть
несколько документов.

**Примеры использования**:

```bash
man 1p read  # отобразить справку по команде read
man 2 read   # отобразить справку по системному вызову read
man readline # отобразить справку по readline из <stdio.h>
man sudo     # отобразить справку по команде sudo
```

В последних двух случаях нужная страница присутствует только в одном
разделе справки, поэтому номер раздела не указан.

### Команда `ls`

Выводит в *стандартный поток вывода* (обычно это экран или окно терминала)
содержимое указанного каталога. Если каталог не указан в качестве аргумента,
то выводит содержимое текущего каталога.

```bash
ls ОПЦИИ КАТАЛОГ
```

Опции:
 * `-A` - отображать *скрытые файлы*, то есть те файлы, имена которых
 начинаются с точки
 * `-R` - рекурсивно пройти по всем подкаталогам и вывести их содержимое.


### Команда `mkdir`

Создает новый каталог. Опция `-p` указывает, что нужно создавать все
подкаталоги, которые не существуют в пути.

```
mkdir ~/new_dir  # новый каталог new_dir в домашнем каталоге
mkdir -p ~/new_dir/new_subdir1/new_subdir2  
                 # новый подкаталог new_subdir2 внутри
                 # ~/new_dir/new_subdir1, которые также создаются
```

### Команды `cp` и `mv`

Команда `cp` копирует файл, а команда `mv` перемещает (равнозначно
-- переименовывает) файл.

Первый аргумент - исходный файл или каталог; второй аргумент - новое имя.
Если копируется или перемещается обычный файл, а в качестве второго аргумента
указано имя каталога, то файл будет скопирован/перемещен в указанный каталог
с сохранением своего имени.

При указании опции `-R`, можно рекурсивно копировать каталоги.

### Команда `rm`

**Внимание!** Это деструктивная команда, используйте ее с осторожностью!

Команда `rm` удаляет файл или каталог. Для удаления каталога необходима
опция `-r`.

## Подстановка вывода команд

Вывод команд может быть направлен не только в файл, но и использован как
фрагмент другой команды. Для этого команда с необходимыми аргументами
заключается в *обратные одинарные кавычки* (этот символ находится на
PC-клавиатуре на одной клавише с буквой `Ё`).

**Пример.** Команда `date` выводит текущую дату и время. В качестве аргумента
ей можно передать формат вывода, например `date +%Y%m%d-%H%M` выведет дату
и время в формате `ГГГГММДД-ЧЧММ`. Допустим, нам требуется создать временный
каталог, в имени которого присутствует дата и время. Тогда можно использовать
вывод команды `date` для формирования аргумента команды `mkdir`:

```bash
mkdir temp-`date +%Y%m%d-%H%M`
```

## Синтаксис

Командная оболочка `bash` -- это не просто строка для ввода команд, а
полноценный интерпретатор командного shell-языка.

Последовательность команд, которая выполняется в командной строке, может
быть сохранена в текстовом файле для повторного выполнения.

**Пример.** Содержимое файла `myscript.sh`:

```bash
mkdir ~/new_dir
touch ~/new_dir/new_file.txt
```

Выполнение этой *последовательности команд* осуществляется командой:

```bash
bash myscript.sh
```

Если в начало файла `myscript.sh` добавить строку (которая с точки зрения
синтаксиса является комментарием) `#!/bin/sh`, и сделать этот файл выполняемым,
то запускается он еще проще:

```bash
./myscript.sh
```

**Замечание 1.** Суффикс имени `.sh` не обязателен и часто опускается.

**Замечание 2.** Обратите внимание, что первая строка - это `#!/bin/sh`, а
не `#!/bin/bash`. В большинстве дистрибутивов Linux, файл `/bin/sh` является
ссылкой на `/bin/bash`, но в некоторых UNIX-системах (например FreeBSD)
интерпретатор `bash` может быть не установлен по умолчанию, а в качестве
командного интерпретатора использоваться какой-либо другой, sh-совместимый.

### Операторы
 * **`;` (точка с запятой)** -- является разделителем между последовательно
 выполняемыми командами. Этот оператор эквивалентен переносу строки
 * **`&` (амперсанд, `Shift+7`)** -- ставится после команды (и ее аргументов);
 этот оператор означает, что команда будет выполнена в фоновом режиме, а
 интерпретатор продолжает свою работу, не дожидаясь завершения работы команды
 * **`&&` и `||`** -- аналогичны операторам в языке Си. Вызов каждой команды
 возвращает целое число -- код завершения. Логическому значению "истина"
 соотвтетствует код, равный `0`; все остальные коды завершения эквивалентны
 логическому значению "ложь"
 * **`>`, `>>` и `<`** -- операторы перенаправления потока вывода (`>`, `>>`)
 и ввода (`<`) в произвольный файл или из произвольного файла; оператор вывода
 в файл `>` отличается от оператора вывода `>>` тем, что предварительно
 очищает содержимое файла
 * **`|`** -- оператор перенаправления потока вывода из одной команды в
 поток ввода другой команды; ставится между командами.


### Переменные

#### Объявление переменных

Значения переменных объявляются присваиванием:

```bash
ИМЯ=ЗНАЧЕНИЕ
```

Обратите внимание, что вокруг символа `=` не допускаются пробелы!

Доступ к значениям переменных осуществляется конструкцией `$ИМЯ`.
Переменные обычно именуются заглавными буквами; при использовании в имени
символа подчеркивания необходимо для доступа использовать синтаксис
`${ИМЯ_С_ПОДЧЕРКИВАНИЕМ}` (фигурные скобки вокруг имени).


#### Экспорт значений переменных запускаемым процессам

Переменные доступны только в том сеансе командной строки, в котором они
объявлены (либо до конца выполняемого файла). Для того, чтобы значения
переменных были доступны дочерним запускаемым процессам, их необходимо
*экспортировать* с помощью оператора `export`:

```bash
export ИМЯ=ЗНАЧЕНИЕ
```

или

```bash
ИМЯ=ЗНАЧЕНИЕ
export ИМЯ
```

#### Специальные имена переменных

 * `$0` - имя команды текущего процесса
 * `$1`, `$2`, `$3` и т. д. - значения аргументов, переданных shell-скрипту
 * `$*` - список, состоящий из всех аргументов, передеанных shell-скрипту
 * `$#` - количество аргументов
 * `$?` - код возврата предыдущей выполненной команды

### Ключевые слова и конструкции shell-языка

#### Комментарии

Любой текст после `#` (решетка, `Shift+3`) до конца строки является
комментарием, который игнорируется интерпретатором.


#### Текстовый вывод дочернего процесса

Вызов команды, заключенной между символами обратных одинарных кавычек (в
английской раскладке PC-клавиатуры - на одной клавише с буквой "ё"),
возвращает текстовый вывод в виде

#### Проверка условия

**Синтаксис:**

```bash
if ВЫРАЖЕНИЕ 1
then
    НАБОР КОМАНД 1
elif ВЫРАЖЕНИЕ 2
    НАБОР КОМАНД 2
else
    НАБОР КОМАНД 3
fi
```

Обратите внимание на переносы строк (они могут быть заменены на оператор `;`).

Количество пробелов, в отличии от Python, не имеет никакого значения.

**Пример использования:**

```bash
#!/bin/sh

if mkdir some_dir/new_subdir
then
    echo "Subdirectory created"
else
    echo "Can't create"
fi
```


#### Выбор варианта

**Синтаксис:**

```bash
case ПЕРЕМЕННАЯ in
    ВАРИАНТ1|ВАРИАНТ2|ВАРИАНТ3)
       НАБОР КОММАНД 1
       ;;
    ВАРИАНТ4)
       НАБОР КОММАНД 2
       ;;
    *)
       НАБОР КОММАНД 3
       ;;
esac
```

Варианты выбора ограниичиваются закрывающей круглой скобкой; их может быть
несколько, в этом случае они разделяются вертикальной чертой.

Последовательность команд отделяется от следующего выбора двумя символами
точки с запятой `;;`.

**Пример использования:**

```bash
#!/bin/sh

case $1 in
    --help|-h)
    echo "Usage: $0 --help | --flag1 | --flag2"
    ;;
    --flag1|-f1)
    echo "Flag 1 set"
    ;;
    --flag2|-f2)
    echo "Flag 2 set"
    ;;
    *)
    echo "Unknown argument"
    ;;
esac

```


#### Цикл `while`

**Синтаксис:**

```bash
while УСЛОВИЕ
do
    НАБОР КОММАНД
done
```

**Пример использования:**

```bash
#!/bin/sh

echo "Press Ctrl+C to exit or type something"
while read USER_INPUT_LINE
do
    echo "Yout typed: ${USER_INPUT_LINE}"
done

```


#### Цикл `for`

**Синтаксис:**

```bash
for ИМЯ in СПИСОК
do
    НАБОР КОММАНД
done
```

**Пример использования:**

```bash
#!/bin/sh

echo "Program arguments are:"

for ARGUMENT in $*
do
    echo " ..... $ARGUMENT"
done

```

### Команда `[`

`[` - это *программа*, которая располагается в `/usr/bin`, предназначенная
для формирования логических выражений в виде `[ ВЫРАЖЕНИЕ ]`. В завимости
от результата проверки аргументов, возвращает нулевой либо ненулевой код
возврата, предназначенный для операторов `if` или `while`.

**Внимание!** В отличии от оператора присваивания, символ `=` и другие
символы сравнения являются аргументами команды `[`, и должны
отделяться пробелом.

#### Проверка строковых значений
 * `-n СТРОКА` - истина, если длина строки ненулевая
 * `-z СТРОКА` - истина, если строка пустая
 * `СТРОКА1 = СТРОКА2` - истина, если строки равны
 * `СТРОКА1 != СТРОКА2` - истина, если строки различные

#### Проверка численных значений
 * `ЧИСЛО1 -eq ЧИСЛО2` - числовое равенство
 * `ЧИСЛО1 -ne ЧИСЛО2` - числовое равенство
 * `ЧИСЛО1 -gt ЧИСЛО2` - ЧИСЛО1 > ЧИСЛО2
 * `ЧИСЛО1 -ge ЧИСЛО2` - ЧИСЛО1 >= ЧИСЛО2
 * `ЧИСЛО1 -lt ЧИСЛО2` - ЧИСЛО1 < ЧИСЛО2
 * `ЧИСЛО1 -le ЧИСЛО2` - ЧИСЛО1 <= ЧИСЛО2

#### Часто используемые проверки аттрибутов файлов
 * `-e ИМЯФАЙЛА` - указанный путь существует
 * `-d ИМЯФАЙЛА` - указанный путь существует и является каталогом
 * `-f ИМЯФАЙЛА` - указанный путь существует и является обычным файлом


### Команды `read` и `printf`

Для ввода-вывода данных (в основном - при написании скриптов) используются
команды `read` и `printf`

Команда `read` читает одну строку (до символа `\n`)из потока ввода,
и сохраняет ее значение в указанной переменной:

```bash
read SOME_VAR  # пользователь вводит строку
echo ${SOME_VAR}
```

Если поток данных завершился до ввода строки, то возвращается не нулевой
код возврата. Это можно использовать для огранизации цикла `while`.

Для вывода значений переменных, помимо команды `echo`, можно использовать
форматный вывод, аналогично Си:

```bash
A=123
B=3.14159
C=Text
D="Hello, World"  # D - содержит СПИСОК из двух слов, а не строку!
printf "%d -- %f\n%s\n%s\n" $A $B $C "$D"  # обратите внимание на кавычки
```


# Домашнее задание

Написать shell-скрипт, который выполняет очистку "мусорных" файлов в
соответствии с набором заданных шаблонов имен.
Скрипт должен принимать один аргумент: имя каталога, который
необходимо очистить; набор шаблонов передается скрипту в виде потока
ввода.

При реализации нужно использовать циклы и условия. Использование команды
`find` запрещено!

Шаблоны задаются в текстовом виде: каждый шаблон - на отдельной строке.
Входной поток может содержать "комментарии" - строки,
начинающиеся с символа `#`.


# Дополнительное домашнее задание

Пройти как можно больше уровней в игре Bandit [http://overthewire.org/wargames/bandit/].

# Пример простого скрипта тестирования на `bash`е

```bash
#!/bin/bash

program="$1"

echo "$program"

for file in tests/*.in
do
    resfile="${file/.in/.res}"
    outfile="${file/.in/.out}"
    echo "$file"

    if "$program" < "$file" > "$resfile"
    then
    echo success
    if diff "$resfile" "$outfile"
    then
        echo correct
    else
        echo incorrect
    fi
    else
    echo fail
    fi

done
```