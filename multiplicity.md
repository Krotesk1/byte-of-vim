# Множественность

В этой главе мы рассмотрим как Vim помогает нам работать между различными частями файла, различными файлами, различными "окнами" и вкладками, чтобы помочь нам работать параллельно. В конце концов, важной частью хорошего редактирования является управление файлами.

## Использование сворачивания для нескольких разделов

Если вы редактируете большой документ, не было бы проще, если бы вы могли "закрыть" все разделы документа и сосредоточиться только на одном?

Это то, что мы называем *сворачиванием* в Vim.

Давайте возьмем пример, когда ваш документ структурирован таким образом, когда каждый уровень текста имеет отступ на один уровень выше, например, следующий фрагмент текста:

```
Book I
    The Shadow of the Past
        Three Rings for the Elven-kings under the sky,
        Seven for the Dwarf-lords in their halls of stone,
        Nine for Mortal Men doomed to die,
        One for the Dark Lord on his dark throne
        In the Land of Mordor where the Shadows lie.
        One Ring to rule them all, One Ring to find them,
        One Ring to bring them all and in the darkness bind them
        In the Land of Mordor where the Shadows lie.

    Three is Company

        The Road goes ever on and on
        Down from the door where it began.
        Now far ahead the Road has gone,
        And I must follow, if I can,
        Pursuing it with weary feet,
        Until it joins some larger way,
        Where many paths and errands meet.
        And whither then? I cannot say.
```

> Примечание: Этот текст был [взят с WikiQuote](http://en.wikiquote.org/wiki/The_Fellowship_of_the_Ring).

После написания этого текста запустите `:set foldmethod=indent`, поместите курсор на текст, который вы хотите отступить, нажмите `zc` и увидите как текст сворачивается. Нажмите кнопку `zo` для открытия складки.

Основные команды - `zo` и `zc`, где мы можем открыть и закрыть складку соответственно. Вы можете использовать `za` для чередования открытия и закрытия сгиба складки. Вы можете сделать это еще проще, используя пробел (в нормальном режиме), чтобы открыть и закрыть складку:

``` viml
:nmap <space> za
```

Сворачивание - это огромная тема сама по себе, с большим количеством способов (ручное, маркерное, экспрессивное) и различными способами открытия и закрытия иерархий складок и так далее. Подробности см. В разделе `:help folding`.

## Множество буферов

Предположим, вы хотите редактировать несколько файлов одновременно, используя один и тот же Vim, что вы делаете?

Помните, что файлы загружаются в буферы в Vim. Vim также может загружать несколько буферов одновременно. Таким образом, вы можете открывать одновременно несколько файлов и переключаться между ними.

Допустим, у вас есть два файла, `part1.txt` и `part2.txt`:

*part1.txt*

> I have coined a phrase for myself - 'CUT to the G': <br>
> 1. Concentrate <br>
> 2. Understand <br>
> 3. Think <br>
> 4. Get Things Done

*part2.txt*

>  Step 4 is eventually what gets you moving, but Steps 2 and 3 are equally important. As Abraham Lincoln once said "If I had eight hours to chop down a tree, I'd spend six hours sharpening my axe." And to get to this stage, you need to do Step 1 which boils down to one thing - It's all in the mind. That's why it's so hard.

А теперь запустим `:e part1.txt` и `:е part.txt` Обратите внимание, что второй файл теперь открыт для редактирования. Как вернуться к первому файлу? В этом конкретном случае вы можете просто запустить `:b 1`, 'b'uffer (буфер) номер '1'. Вы также можете запустить `:e part1.txt`, чтобы открыть существующий буфер для просмотра.

Вы можете видеть, какие буферы были загружены и, соответственно, какие файлы редактируются с помощью запуска `:buffers` или более короткой формы `:ls`, что означает 'l'i's't (список) буферов".

Буферы будут автоматически удалены при закрытии Vim, так что вам не нужно делать ничего особенного, кроме как убедиться, что вы сохранили свои файлы. Однако, если вы действительно хотите удалить буфер, например, чтобы использовать меньше памяти, то можете использовать `:bd 1` для 'd'elete (удаления) буфера с номером '1' и т.д.

Смотрите `:help buffer-list` о многочисленных вещах, которые вы можете сделать с буферами.

See `:help buffer-list` on the numerous things you can do with buffers.

## Множество окон

Мы видели, как редактировать несколько файлов одновременно, но что делать, если мы хотим просмотреть два разных файла одновременно. Например, вы хотите, чтобы были открыты две разные главы вашей книги для написания второй главы в соответствии с формулировками/описанием, приведенными в первой главе. Или просто хотите скопировать/вставить некоторые вещи из первого файла во второй.

В последнем разделе мы использовали одно и то же "представление" для редактирования нескольких буферов. Vim называет эти "представления" окнами. Этот термин "окно" *не* следует путать с окном вашего настольного приложения, которое обычно означает само приложение. Просто подумайте о "windows" как о "представлениях" разных файлов.

Давайте возьмем те же `part1.txt` и `part2.txt`, используемые в предыдущем разделе.

Во-первых, загрузите part1.txt используя `:e part1.txt` Теперь давайте откроем новый буфер, разделив окно, чтобы создать новое - запуском `:new`. Теперь вы можете выполнять любое обычное редактирование в новом буфере в новом окне, за исключением того, что не можете сохранить текст, потому что вы не связали имя файла с буфером. Для этого вы должны использовать `:w test.txt` для сохранения буфера.

![](img/multiple_windows.png)

Как переключаться между этими двумя окнами? Просто используя `ctrl-w <клавиша перемещения>` для переключения между окнами. Клавиши перемещения могут быть одной из `h`, `j`, `k`, `l` или даже любой из клавиш со стрелками (в этом примере имеют смысл только клавиши вверх и вниз). Помните, что операции `ctrl-w` работают на 'w'indows (окнах).

Как быстрый доступ, вы можете нажать `ctrl-w` и дважды, т.е. сочетание клавиш `ctrl-w ctrl-w` для цикличного переключения между всеми открытыми окнами.

Особая ситуация, когда нужны несколько окон, при случае когда вы хотите просмотреть две разные части одного и того же файла одновременно. Просто запустите `:sp` для создания 'sp'lit (разделенного) окна , а затем можете прокрутить каждое окно в другую позицию и продолжить редактирование. Поскольку они оба являются "окнами" в один и тот же буфер, изменения в одном окне будут немедленно отражены в другом окне. Вы также можете использовать `ctrl-w s` вместо `:sp`.

Чтобы создать вертикальное разделение, используйте `:vsp` или `ctrl-w v`. Чтобы закрыть окно, просто запустите `:q`.

Теперь, когда мы увидели, как открывать и использовать несколько окон, давайте посмотрим как ещё поиграться с дисплеем.

- Предположим, у вас есть два разделенных окна, но вы хотите изменить их так, что сможете сосредоточить своё внимание на нижней или верхней части экрана Вашего монитора в соответствии с вашими предпочтениями? Нажмите `ctrl-w r` чтобы 'r'otate (вращать) окна.
- Хотите переместить текущее окно в самую верхнюю позицию? Нажмите `ctrl-w K`.
- Хотите изменить размер окна, чтобы сделать его меньше или больше? Запустите `:resize 10` для отображения 10 строк дисплея и т.д.
- Хотите сделать текущее окно как можно больше, чтобы сосредоточиться на нем? Нажмите `ctrl-w _`. Подумайте о подчеркивании как о показателе того, что другие окна должны быть как можно меньше.
- Хотите снова сделать окна "равными" по высоте? Нажмите `ctrl-w =`.

Смотрите `:help windows` для дополнительных сведений о том, что можно сделать с окнами.

## Множество вкладок

In web browsers (such as Firefox, Google Chrome or Safari), you may have used the tabs feature which allows you to open multiple websites in a single window so that you can switch between them without having the headache of switching between multiple windows. Well, tabs work exactly the same way in Vim also. Except that they are called "tab pages."

Run `:tabnew` to open a new tab with a new buffer (analogous to `:new`). How do you switch between the tabs? By pressing `gt` to 'g'o to the next 't'ab and `gT` to 'g'o in the opposite direction i.e. the previous 't'ab.

![](img/multiple_tabs.png)

I personally prefer to use the keys `alt-j` and `alt-k` for the same operations analogous to how the `j` and `k` keys work for characters and how `ctrl-w j` and `ctrl-w k` work for (horizontally split) windows. To enable this, add the following lines to your vimrc file:

``` viml
" Shortcuts for moving between tabs.
" Alt-j to move to the tab to the left
noremap <A-j> gT
" Alt-k to move to the tab to the right
noremap <A-k> gt
```

To 'c'lose a 'tab', run `:tabc` or `:q`.

You can even open text that opens in a new window to open in a new tab instead. For example, `:help tabpage` opens the help in a horizontally split window. To view it in a new tab instead, use `:tab help tabpage`.

If you want to reorder the tabs, use `:tabmove`. For example, to move the current tab to the first position, use `:tabmove 0` and so on.

See `:help tabpage` for more details on tab pages and the other operations that you can run, such as `:tabdo` to operate on each of the tab pages which are open, and customizing the title of the tab pages (`:help setting-guitablabel`), etc.

## Резюме

Vim предоставляет несколько способов редактирования нескольких файлов одновременно - буферы, окна и вкладки. Использование этих функций зависит от вашей личной привычки. Например, использование нескольких вкладок может исключить использование нескольких окон. Важно использовать тот, который наиболее пригоден и удобен.
