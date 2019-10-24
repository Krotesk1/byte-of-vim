# Режимы

В предыдущей главе мы впервые столкнулись с режимами. Теперь давайте рассмотрим эту концепцию дальше относительно типов доступных режимов и того, что мы можем сделать в каждом из них.

## Типы режимов

В Vim есть три основных режима:

1. Нормальный режим, где вы можете запускать команды. Это режим по умолчанию, в котором запускается Vim.
2. Режим вставки - в котором вы вставляете, т.е. пишете текст.
3. Визуальный режим - это когда вы визуально выделяете кучу текста, чтобы запустить команду/операцию только над этой частью текста.

## Нормальный режим

По умолчанию вы находитесь в обычном режиме. Давайте посмотрим, что мы можем сделать в этом режиме.

Введите `:echo "hello world"` и нажмите Enter. Вы должны увидеть знаменитые слова hello world эхом возвращенные к вам. То, что вы только что сделали, это запустили команду Vim под названием `:echo`, и вы предоставили ей текст, который был незамедлительно напечатан.

Введите `/ hello` и нажмите Enter. Vim будет искать эту фразу и перейдет к первому вхождению.

Это были всего лишь два простых примера команд, доступных в обычном режиме. В последующих главах мы увидим еще много таких команд.

## Как пользоваться справкой

Почти так же важно, как знать нормальный режим, знать как использовать команду `:help`. Здесь вы узнаете больше о командах, доступных в Vim.

Помните, что вам не нужно знать все команды, доступные в Vim, гораздо лучше знать где их найти когда они понадобятся. Например, в разделе `:help usr_toc` мы переходим к оглавлению справочного руководства. Вы можете посмотреть `:help index` для поиска конкретной темы, которая вас интересует, например, запустите `/insert mode`, чтобы увидеть соответствующую информацию о режиме вставки.

Если вы не можете вспомнить эти две темы справки, просто нажмите `F1` или запустите `:help`.

## Режим вставки

Когда Vim запускается в нормальном режиме, мы видели, как использовать i для перехода в режим вставки. Есть и другие способы переключения из нормального режима в режим вставки:

1. Запустите `:e dapping.txt`
2. Нажмите `i`
3. Введите следующий абзац (включая все опечатки и ошибки, мы исправим их позже):

    > means being determined about being determined and being passionate about being passionate

4. Нажать клавишу `<Esc>` для переключения в нормальный режим.
5. Запустите `:w`

Упс, мы, кажется, пропустили слово в начале строки, а курсор находится в конце строки, что нам теперь делать?

Каков был бы наиболее эффективный способ перейти к началу строки и вставить пропущенное слово? Должны ли мы использовать мышь, чтобы переместить курсор в начало строки? Должны ли мы использовать клавиши со стрелками, чтобы пройти весь путь до начала линии. Должны ли мы нажать клавишу home, а затем i, чтобы снова переключиться в режим вставки?

Оказывается, что наиболее эффективным способом будет нажать `I` (прописная буква I):

1. Нажмите `I`
2. Введите `Dappin`
3. Нажмите клавишу `<Esc>` для перключения обратно в нормальный режим.

Обратите внимание, что мы использовали другой ключ для переключения в режим вставки, его особенность заключается в том, что он перемещает курсор в начало строки, а затем переключается в режим вставки.

Также обратите внимание, как важно *переключиться обратно в нормальный режим, как только вы закончите вводить текст*. Выработка этой привычки будет полезна, потому что большая часть вашей работы (после написания начальной фазы) будет в нормальном режиме - вот где происходит все самое важное переписывание/редактирование/полировка.

Теперь давайте возьмем другой вариант команды `i`. Обратите внимание, что нажатие кнопки `i` помещает курсор перед текущей позицией, а затем переключается в режим вставки. Чтобы поместить курсор после текущей позиции, нажмите клавишу `a`

1. Нажмите `a`
2. Введите `g` (чтобы завершить слово как "Dapping")
3. Нажмите `<Esc>` чтобы вернуться в нормальный режим

Аналогично связи между клавишами `i` и `I`, существует связь между клавишами `a` и `A`- если вы хотите добавить текст в конце строки, нажмите `A`.

1. Нажмите `A`
2. Введите `.` (поставье точку чтобы правильно завершить предложение)
3. Нажмите `<Esc>` для возврата в нормальный режим

Подведем итог четырем ключам, которые мы узнали ранее:

| Команда | Действие                                      |
| ---     | ---                                           |
| `i`     | вставить текст непосредственно перед курсором |
| `I`     | вставить текст в начале строки                |
| `a`     | добавить текст сразу после курсора            |
| `A`     | добавить текст в конце строки                 |

Обратите внимание, что команды верхнего регистра являются "большими" версиями команд нижнего регистра.

Теперь, когда мы научились быстро перемещаться по текущей строке, давайте посмотрим, как перейти к новым строкам. Если вы хотите 'о'ткрыть новую строку, чтобы начать писать, нажмите клавишу `o`.
Now that we are proficient in quickly moving in the current line, let's see how to move to new lines. If you want to 'o'pen a new line to start writing, press the `o` key.

1. Нажмите `o`
2. Введите `I'm a rapper`.
3. Нажмите `<Esc>` для возврата в нормальный режим.

Хм, было бы более привлекательным, если бы это новое предложение, которое мы написали, было в абзаце само по себе.

1. Нажмите `O` (прописную 'O')
2. Нажмите `<Esc>` для возврата в нормальный режим.

Подведем итог двум новым ключам, которые мы только что узнали:

| Команда | Действие                   |
| ---     | ---                        |
| `o`     | откройте новую строку ниже |
| `O`     | открыть новую строку выше  |

Notice how the upper and lower case 'o' commands are opposite in the direction in which they open the line.

Was there something wrong in the text that we just wrote? Aah, it should be 'dapper', not 'rapper'! It's a single character that we have to change, what's the most efficient way to make this change?

We could press `i` to switch to insert mode, press `<Del>` key to delete the `r`, type `d` and then press `<Esc>` to switch back to the insert mode. But that is four steps for such a simple change! Is there something better? You can use the `s` key - s for 's'ubstitute.

1. Move the cursor to the character `r` (or simply press `b` to move 'b'ack to the start of the word)
2. Press `s`
3. Type `d`
4. Press `<Esc>` to switch back to the normal mode

Well, okay, it may not have saved us much right now, but imagine repeating such a process over and over again throughout the day! Making such a mundane operation as fast as possible is beneficial because it helps us focus our energies to more creative and interesting aspects. As Linus Torvalds says, *"it's not just doing things faster, but because it is so fast, the way you work dramatically changes."*

Again, there is a bigger version of the `s` key, `S` which substitutes the whole line instead of the current character.

1. Press `S`
2. Type `Be a sinner`.
3. Press `<Esc>` to switch back to normal mode.

| Command | Action |
| --- | --- |
| `s` | substitute the current character |
| `S` | substitute the current line |

Let's go back our last action... Can't we make it more efficient since we want to 'r'eplace just a single character? Yes, we can use the `r` key.

1. Move the cursor to the first character of the word `sinner`.
2. Press `r`
3. Type `d`

Notice we're already back in the normal mode and didn't need to press `<Esc>`.

There's a bigger version of `r` called `R` which will replace continuous characters.

1. Move the cursor to the 'i' in sinner.
2. Press `R`
3. Type `app` (the word now becomes 'dapper')
4. Press `<Esc>` to switch back to normal mode.

| Command | Action |
| --- | --- |
| `r` | replace the current character |
| `R` | replace continuous characters |

The text should now look like this:

> Dapping means being determined about being determined and being passionate about being passionate.
> <br>
> Be a dapper.

Phew. We have covered a lot in this chapter, but I guarantee that this is the only step that is the hardest. Once you understand this, you've pretty much understood the heart and soul of how Vim works, and all other functionality in Vim, is just icing on the cake.

To repeat, understanding how modes work and how switching between modes work is the key to becoming a Vimmer, so if you haven't digested the above examples yet, please feel free to read them again. Take all the time you need.

If you want to read more specific details about these commands, see `:help inserting` and `:help replacing`.

## Visual mode

Suppose that you want to select a bunch of words and replace them completely with some new text that you want to write. What do you do?

One way would be to use the mouse to click at the start of the text that you are interested in, hold down the left mouse button, drag the mouse till the end of the relevant text and then release the left mouse button. This seems like an awful lot of distraction.

We could use the `<Del>` or `<Backspace>` keys to delete all the characters, but this seems even worse in efficiency.

The most efficient way would be to position the cursor at the start of the text, press v to start the visual mode, use arrow keys or any text movement commands to the move to the end of the relevant text (for example, press `5e` to move to the end of the 5th word counted from the current cursor position) and then press `c` to 'c'hange the text. Notice the improvement in efficiency.

In this particular operation (the `c` command), you'll be put into insert mode after it is over, so press `<Esc>` to return to normal mode.

The `v` command works on a character basis. If you want to operate in terms of lines, use the upper case `V`.

## Summary

Here is a drawing of the relationship between the different modes:

```
+---------+  i,I,a,A,o,O,r,R,s,S  +----------+
| Normal  +---------->------------+ Insert   |
| mode    |                       | mode     |
|         +----------<------------+          |
+-+---+---+        <Esc>          +----------+
   |  |
   |  |
   |  |
   |  |
v, |  |
V  V  ^ <Esc>
   |  |
   |  |
   |  |
   |  |
+---+---+----+
| Visual     |
| mode       |
+------------+
```

> NOTE: This drawing was created using Vim and [Dr.Chip's DrawIt plugin](http://www.vim.org/scripts/script.php?script_id=40).

See `:help vim-modes-intro` and `:help mode-switching` for details on the various modes and how to switch between them respectively.

If you remain unconvinced about why the concept of modes is central to Vim's power and simplicity, do read the articles on ["Why Vi"](http://www.viemu.com/a-why-vi-vim.html) and about the [vi input model](http://blog.ngedit.com/2005/06/03/the-vi-input-model/) on why it is a better way of editing.
