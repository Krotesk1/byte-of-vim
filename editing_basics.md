# Основы редактирования

Давайте изучим основные команды редактирования в Vim для чтения/записи файлов, вырезания/копирования/вставки, отмены/повтора и поиска.

## Чтение и редактирование файлов

### Буферы

При редактировании файла Vim переносит текст из файла на жестком диске в оперативную память компьютера. Это означает, что копия файла хранится в памяти компьютера и любые внесенные изменения производятся в памяти компьютера и отображаются немедленно. После того, как вы закончили редактирование файла, то можете сохранить файл, что означает, что Vim записывает обратно текст из памяти компьютера в файл на жестком диске. Память компьютера, используемая для временного хранения текста, называется "буфером". Обратите внимание, что эта же концепция является причиной, почему мы должны "сохранять" файлы во всех редакторах или текстовых процессорах, которые мы используем.

Теперь откройте Vim, напишите слова `Hello World` и сохраните их как файл `hello.txt`. Если Вам нужно напомнить как это сделать - обратитеь к [главе Первые шаги](./first_steps.md#write-file).

### Подкачка (swap)

Теперь вы заметите, что другой файл был создан в том же каталоге что и этот, файл будет называться что-то вроде `.hello.txt.swp`. Запустите `:swapname` , чтобы узнать точное имя файла.

Что это за файл? Vim поддерживает резервную копию буфера в файле, который он регулярно сохраняет на жесткий диск, так что в случае, если что-то пойдет не так (например, сбой компьютера или даже сбой Vim), у вас есть резервная копия изменений, внесенных с момента последнего сохранения исходного файла. Этот файл называется "файл подкачки (swap)", потому что Vim постоянно обменивается содержимым буфера в памяти компьютера с содержимым этого файла на жестком диске. Дополнительную информацию смотрите в разделе `:help swap-file`.

### Сохранить мой файл

Теперь, когда файл загружен, давайте сделаем небольшое редактирование. Нажмите клавишу ~ чтобы изменить регистр символа, на котором расположен курсор. Вы заметите, что Vim теперь отмечает файл, который был изменен (например, знак `+` отображается в строке заголовка графической версии Vim). Вы можете открыть фактический файл в другом редакторе и проверить что он еще не изменился, т.е. Vim только изменил буфер и еще не сохранил его на жестком диске.

Мы можем записать буфер обратно в исходный файл на жестком диске, выполнив команду `:write`.

> ПРИМЕЧАНИЕ: Чтобы упростить сохранинени - добавьте следующую строку в файл vimrc:
> <br>
> ``` viml
> " To save, ctrl-s.
> nmap <c-s> :w<CR>
> imap <c-s> <Esc>:w<CR>a
> ```
> <br>
> Теперь вы можете просто нажать `ctrl-s`, чтобы сохранить файл.

### Работа в моем каталоге

Vim запускается с вашим домашним каталогом в качестве каталога по умолчанию, и все операции будут выполняться в нем.

Для открытия файлов, расположенных в других директориях, можно использовать полные или относительные пути, такие как:

``` viml
:e ../tmp/test.txt
:e C:\\shopping\\monday.txt
```

Или вы можете переключить Vim на этот каталог:

``` viml
:cd ../tmp
```

`:cd` - это сокращение от 'c'hange 'd'irectory (смена каталога).

Чтобы узнать текущий каталог, в котором Vim ищет файлы:

``` viml
:pwd
```

`:pwd` - сокращение от 'p'rint 'w'orking 'd'irectory (напечатать рабочий каталог).

## Выразание, копирование и вставка

Как говорит Шон Коннери в фильме [Найти Форрестера](http://www.imdb.com/title/tt0181536/):

> No thinking - that comes later. You must write your first draft with your heart. You rewrite with your head. The first key to writing is... to write, not to think!
> Не думай - это придет позже. Ты должен написать свой первый черновик сердцем. Ты переписываешь с головой. Первый ключ к написанию - это... писать, а не думать!

Когда мы переписываем, мы часто меняем порядок абзацев или предложений, т.е. нам нужно иметь возможность вырезать/копировать/вставлять текст. В Vim мы используем несколько иную терминологию:

| Мир десктопа | Мир Vim | Операция |
| --- | --- | --- |
| cut (вырезать) | delete | `d` |
| copy (копировать)| yank | `y` |
| paste (вставить) | paste | `p` |

В обычной терминологии 'cut'ting (вырезать) текст означает удалить текст и поместить его в буфер обмена. Та же операция в Vim означает, что он удаляет текст из файлового буфера и сохраняет его в "регистре" (часть памяти компьютера). Поскольку мы можем выбрать регистр, в котором можем хранить текст, операция называется "удалить" (delete).

Аналогично, в обычной терминологии "копировать" текст означает, что копия текста помещается в буфер обмена. Vim делает то же самое, он "yanks" (дергает) текст и помещает его в регистр.

"Вставить" (paste) означает то же самое в обеих терминологиях.

Мы видели как вы можете выполнять операции вырезания/копирования/вставки в Vim. Но как вы определяете, с каким текстом должны работать эти операции? Мы уже исследовали это в предыдущем [разделе Части текста](./moving_around.md#text-objects).

Сочетание нотации операции и нотации объекта текста означает, что у нас есть бесчисленные способы манипулирования текстом. Давайте рассмотрим несколько примеров.

Напишите этот текст в Vim (точно так, как показано):

<!-- Combining the operation notation and the text object notation means we have innumerable ways of manipulating the text. Let's see a few examples. -->

> This is the rthe first paragraph. <br>
> This is the second line. <br>
> <br>
> This is the second paragraph. <br>

Поместите курсор в самую верхнюю левую позицию, нажав `1G` и `|`, которые перемещают в первую строку и первый столбец соответственно.

Давайте рассмотрим первый случай: мы набрали лишнюю 'r', которую теперь необходимо удалить. Нажмите `3w`, чтобы перейти вперед на 3 слова.

Теперь нам нужно удалить один символ в текущей позиции курсора.

Обратите внимание, что это состоит из двух частей:

| Операция | Текстовый объект / Движение |
| --- | --- |
| Удаление | Один символ от текущей позиции курсора |
| `d` | `l` |

Итак, мы должны просто нажать `dl` и удалить один символ! Обратите внимание, что мы можем использовать `l`, даже если это движение.

Теперь мы замечаем, что весь мир избыточен, потому что у нас есть "свое" дважды. Теперь подумайте хорошенько о том, какая должна быть самая быстрая комбинация клавиш, чтобы исправить это?

Потратьте некоторое время, чтобы подумать и выяснить это для себя. Не торопитесь. А теперь читайте дальше.

| Операция | Текстовый объект / Движение |
| --- | --- |
| Удаление | Слово |
| `d` | `w` |

Итак, нажав `dw` вы удалите слово. Вуаля! Так просто и так красиво. Красота заключается в том, что такие простые концепции могут быть объединены, для обеспечения такого богатого спектра возможностей.

Как мы достигаем такой же операции для строк? Ну, строки считаются особенными в Vim, потому что строки - это обычно то, как мы думаем о нашем тексте. Сокращенно, если вы повторите имя операции дважды, она будет работать на строке. Таким образом, `dd` удалит текущую строку, а `yy` будет дергать текущую строку.

Наш пример текста в Vim теперь должен выглядеть так:

> This is the first paragraph. <br>
> This is the second line. <br>
> <br>
> This is the second paragraph.

Перейдите ко второй строке, нажав `j`. Теперь нажмите `dd` и строка должна быть удалена. Теперь вы должны увидеть:

> This is the first paragraph. <br>
> <br>
> This is the second paragraph.

Давайте посмотрим на более крупный случай: как выдернуть текущий абзац?

| Операция | Текстовый объект / Движение |
| --- | --- |
| Yank | Абзац |
| `y` | `ap` |

Итак, `yap` скопирует текущий абзац.

Теперь, когда мы завершили копирование текста, как мы можем вставить его? Просто `p`.

Теперь вы должны увидеть:

> This is the first paragraph. <br>
> This is the first paragraph. <br>
> <br>
> <br>
> This is the second paragraph.

Обратите внимание, что пустая строка также копируется, когда мы выполняем `yap`, поэтому `p` добавляет и эту дополнительную пустую строку.

Существует два типа вставки, которые можно сделать точно так же, как два вида вставок, которые мы видели ранее:

| Клавиша | Действие |
| --- | --- |
| `p` | Вставка после текущей позиции курсора |
| `P` | Вставка перед текущей позицией курсора |

Развивая эту идею дальше, мы можем объединить их в более мощные способы.

Как поменять местами два символа? Нажмите `хр`.

- `x` &rarr; удалить один символ в текущей позиции курсора
- `p` &rarr; вставить после текущей позиции курсора

Как поменять местами два слова? Нажмите `dwwP`.

- `d` &rarr; удалить
- `w` &rarr; одно слово
- `w` &rarr; переместить на следующее слово
- `P` &rarr; вставить перед текущей позицией курсора

Важно *не* заучивать эти операции наизусть. Эти комбинации действий и текстовых объектов/движений должны быть автоматизированы для ваших пальцев, без раздумий. Это происходит, когда вы делаете их использование привычкой.

## Marking your territory

Вы пишете, и вы вдруг понимаете, что нужно изменить предложения в предыдущем разделе, чтобы дополнить то, что вы пишете в этом разделе. Проблема в том, что вы должны помнить, где находитесь прямо сейчас, чтобы вернуться сюда позже. Неужели Vim не может запомнить это для меня? Это может быть достигнуто с помощью меток.

You can create a mark by pressing m followed by the name of the mark which is a single character from `a-zA-Z`. For example, pressing `ma` creates the mark called 'a'.

Pressing `'a` returns the cursor to line of the mark. Pressing <code>`a</code> will take you to the exact line and column of the mark.

The best part is that you can jump to this position using these marks any time thereafter.

See `:help mark-motions` for more details.

## Time machine using undo/redo

Suppose you are rewriting a paragraph but you end up muddling up what you were trying to rewrite and you want to go back what you had written earlier. This is where we can "undo" what we just did. If we want to change back again to what we have now, we can "redo" the changes that we have made. Note that a change means some change to the text, it does not take into account cursor movements and other things not directly related to the text.

Suppose you have the text:

> I have coined a phrase for myself - 'CUT to the G': <br>
> 1. Concentrate <br>
> 2. Understand <br>
> 3. Think <br>
> 4. Get Things Done <br>
> <br>
> Step 4 is eventually what gets you moving, but Steps 2 and 3 are equally important. As Abraham Lincoln once said "If I had eight hours to chop down a tree, I'd spend six hours sharpening my axe." And to get to this stage, you need to do Step 1 which boils down to one thing - It's all in the mind. That's why it's so hard.

Now, start editing the first line:

1. Press `S` to 's'ubstitute the whole line.
2. Type the text `After much thought, I have coined a new phrase to help me solidify my approach:`.
3. Press `<esc>`.

Now think about the change that we just did. Is the sentence better? Hmm, was the text better before? How do we switch back and forth?

Press `u` to undo the last change and see what we had before. You will now see the text `I have coined a phrase for myself - 'CUT to the G':`. To come back to the latest change, press `ctrl-r` to now see the line `After much thought, I have coined a new
phrase to help me solidify my approach:`.

It is important to note that Vim gives unlimited history of undo/redo changes, but it is usually limited by the `undolevels` setting in Vim and the amount of memory in your computer.

Now, let's see some stuff that really shows off Vim's advanced undo/redo functionality, some thing that other editors will be jealous of: Vim is not only your editor, it also acts as a time machine.

For example, `:earlier 4m` will take you back by 4 minutes i.e. the state of the text 4 minutes "earlier".

The power here is that it is superior to all undoes and redoes. For example, if you make a change, then you undo it, and then continue editing, that change is actually never retrievable using simple u again. But it is possible in Vim using the `:earlier` command.

You can also go forward in time: `:later 45s` which will take you later by 45 seconds.

Or if you want the simpler approach of going back by 5 changes: `:undo 5`.

You can view the undo tree using `:undolist`.

See :help `:undolist` for the explanation of the output from this command.

See `:help undo-redo` and `:help usr_32.txt` for more details.

## A powerful search engine but not a dotcom

Vim has a powerful built-in search engine that you can use to find exactly what you are looking for. It takes a little getting used to the power it exposes, so let's get started.

Let's come back to our familiar example:

> I have coined a phrase for myself - 'CUT to the G': <br>
> 1. Concentrate <br>
> 2. Understand <br>
> 3. Think <br>
> 4. Get Things Done <br>
> <br>
> Step 4 is eventually what gets you moving, but Steps 2 and 3 are equally important. As Abraham Lincoln once said "If I had eight hours to chop down a tree, I'd spend six hours sharpening my axe." And to get to this stage, you need to do Step 1 which boils down to one thing - It's all in the mind. That's why it's so hard.

Suppose we want to search for the word "Step". In normal mode, type `/Step<cr>` (i.e. `/Step` followed by `enter` key). This will take you to the first occurrence of those set of characters. Press `n` to take you to the 'n'ext occurrence and `N` to go in the opposite direction i.e. the previous occurrence.

What if you knew only a part of the phrase or don't know the exact spelling? Wouldn't it be helpful if Vim could start searching as and when you type the search phrase? You can enable this by running:

``` viml
set incsearch
```

You can also tell Vim to ignore the case (whether lower or upper case) of the text that you
are searching for:

``` viml
set ignorecase
```

Or you can use:

``` viml
set smartcase
```

When you have `smartcase` on:

- If you are searching for `/step` i.e. the text you enter is in lower case, then it will search for any combination of upper and lower case text. For example, this will match all the following four - "Step", "Stephen", "stepbrother", "misstep."
- If you are searching for `/Step` i.e. the text you enter has an upper case, then it will search for **only** text that matches the exact case. For example, it will match "Step" and "Stephen", but not "stepbrother" or "misstep."

> NOTE: I recommend that you put these two lines in your vimrc file (explained later, but see `:help vimrc-intro` for a quick introduction) so that this is enabled always.

Now that we have understood the basics of searching, let's explore the real power of searching. The first thing to note that what you provide Vim can not only be a simple phrase, it can be a "expression". An expression allows you to specify the 'kinds' of text to search for, not just the exact text to look.

For example, you will notice that /step will take you to steps as well as step and even footstep if such a word is present. What if you wanted to look for the exact word step and not when it is part of any other word? Then you can search using `/\<step\>`. The `\<` and
`\>` indicate the start and end positions of a "word" respectively.

Similarly, what if you wanted to search for any number? Searching for `/\d` will look for a 'digit'. But a number is just a group of digits together. So we specify "one or more" digits together as `/\d\+`. If we were looking for zero or more characters, we can use the `*` instead of the `+`.

There are a variety of such magic stuff we can use in our search patterns. See `:help pattern` for details.

## Резюме

Мы рассмотрели некоторые из основных команд редактирования, которые будем использовать в нашем повседневном использовании Vim. *Очень важно, чтобы вы снова повторили эти понятия и сделали их привычкой.* Не нужно изучать каждый вариант или нюансы этих команд. Если вы знаете, как использовать команду и знаете, как найти больше о том, что вам необходимо, то вы настоящий Vim'мер.

Теперь, идите вперед и начните редактирование!
