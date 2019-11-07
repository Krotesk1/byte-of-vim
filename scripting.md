# Скриптинг

Если вы хотите настроить какое-либо программное обеспечение, скорее всего, вы измените различные настройки в программном обеспечении в соответствии с вашим вкусом и потребностями. А что, если Вы хотите сделать больше чем это? Например, чтобы проверить условия, такие как `если GUI-версии, то использовать эту цветовую схему, иначе использовать эту цветовую схему`? Вот где появляется "скриптинг". Скриптинг означает использование языка, на котором вы можете указать условия и действия, объединенные в "скрипт".

Существует два подхода к написанию скриптов в Vim - использование встроенного языка скриптинга Vim или использование полноценного языка программирования, такого как Python или Perl, которые имеют доступ к внутренним компонентам Vim через модули (при условии, что Vim был скомпилирован с включенными этими параметрами).

Эта глава требует некоторого знания программирования. Если у вас нет опыта программирования, вы все равно сможете понять, хотя это может показаться кратким. Если вы хотите научиться программированию, пожалуйста, обратитесь к моей другой бесплатной книге [A Byte of Python]({{ book.pythonBookUrl }}).

Существует два способа создания многоразовых функций в Vim - использование макросов и написание скриптов.

## Макрос

Используя макросы, мы можем записывать последовательности команд, а затем воспроизводить их в различных контекстах.

Например, предположим, что у вас есть такой текст:

```
tansen is the singer
daswant is the painter
todarmal is the financial wizard
abul fazl is the historian
birbal is the wazir
```

Здесь есть несколько вещей для исправления:

1. Изменить первую букву предложения на верхний регистр.
2. Изменить "is" на "was".
3. Изменить "the" на "a".
4. Закончить приговор словами "in Akbar's court."

Одним из способов было бы использовать ряд команд замены, таких как `:s/^\\w/\\u\\0/`, но для этого потребуется 4 команды замены, и это может быть непросто, если команда замены изменит части текста, которые мы не хотим изменять.

Лучшим способом было бы использовать макрос.

1. Поместите курсор на первую букву первой строки: `tanses is the singer`
2. Введите `qa` в нормальном режиме, чтобы начать запись макроса с именем `a`.
3. Введите `gUl` для перевода первой буквы в верхний регистр.
4. Введите `w` чтобы перейти к следующему слову.
5. Введите `cw` чтобы изменить слово.
6. Введите `was`.
7. Нажмите `<ESC>`.
8. Введите `w` для перемещения к следующему слову.
9. Введите `cw` чтобы изменить слово.
10. Введите `а`.
11. Нажмите `<ESC>`.
12. Введите `A` чтобы вставить текст в конце строки.
13. Введите `in Akbar's court`.
14. Нажмите `<ESC>`.
15. Введите `q` для остановки записи макроса.

Это выглядит как долгая процедура, но иногда, гораздо проще сделать так чем подготовить некоторые сложные команды замены!

В конце процедуры строка должна выглядеть так:

```
Tansen was a singer in Akbar's court.
```

Отлично. Теперь давайте перейдем к применению этого к другим строкам. Просто перейдите к первому символу второй строки и нажмите `@a`. Вуаля, строка должна измениться на:

```
Daswant was a painter in Akbar's court.
```

Это демонстрирует что макросы могут записывать сложные операции и могут быть легко воспроизведены. Это помогает пользователю воспроизвести сложное редактирование в нескольких местах. И это лишь один из видов повторного использования манипуляций, которые вы можете сделать с текстом. Далее мы увидим более формальные способы манипулирования текстом.

> ПРИМЕЧАНИЕ: Если вы хотите просто повторить последнее действие, а не последовательность действий, вам не нужно использовать макросы, просто нажмите `.` (точку).

## Основы скриптинга

Vim имеет встроенный скриптовый язык, с помощью которого вы можете писать свои собственные скрипты для принятия решений, "делать" вещи и манипулировать текстом.

### Действия

Как вы меняете тему, т.е. цвета, используемые Vim? Просто запустите:

``` viml
:colorscheme desert
```

Здесь я использую цветовую схему "desert", которая является моей любимой. Вы можете просмотреть другие доступные схемы, набрав `:colorscheme', а затем нажимать клавишу `<tab>` для циклического просмотра доступных схем.

Что делать, если вы хотите узнать, сколько символов в текущей строке?

``` viml
:echo strlen(getline("."))
```

Обратите внимание на имена "strlen" и "getline". Это "функции". *Функции* - это уже написанные фрагменты скриптов, которым дано имя, чтобы мы могли использовать их снова и снова. Например, функция getline извлекает строку, и мы указываем строку как `.` (точка), что означает текущую строку. Мы передаем результат, возвращенный функцией `getline` в функцию `strlen`, которая подсчитывает количество символов в тексте, а затем передает результат, возвращенный функцией `strlen` в команду `:echo`, которая просто отображает результат. Обратите внимание, как информация течет в этой команде.

`strlen(getline("."))` называется выражением. Мы можем хранить результаты таких выражений с помощью переменных. Переменные - это имена, указывающие на значения, и значение может быть любым, т.е. оно может меняться. Например, мы можем хранить длину как:

``` viml
:let len = strlen(getline("."))
:echo "We have" len "characters in this line."
```

Когда вы запустите эту строку на второй строке в тексте выше, то получите следующий вывод:

``` viml
We have 46 characters in this line.
```

Обратите внимание как мы можем использовать переменные в других "выражениях". Возможности, которые вы можете достичь с помощью этих переменных, выражений и команд бесконечны.

Vim имеет множество типов переменных, доступных через префиксы, такие как `$` для переменных среды, `&` для параметров и `@` для регистров:

``` viml
:echo $HOME
:echo &filetype
:echo @a
```

Вы можете создавать свои собственные функции, как хорошо:
Смотрите `:help function-list` для огромного списка доступных функций.

Вы также можете создавать свои собственные функции:

``` viml
:function CurrentLineLength()
: let len = strlen(getline("."))
: return len
:endfunction
```

Теперь поместите курсор на любую строку и выполните следующую команду:

``` viml
:echo CurrentLineLength()
```
Вы должны увидеть напечатанное число.

Имена функций должны начинаться с верхнего регистра. Это делается для того, чтобы отличить встроенные функции, начинающиеся с нижнего регистра, и пользовательские, начинающиеся с верхнего.

Если вы хотите просто "вызвать" функцию для запуска, но не отображать содержимое, то можете использовать `:call CurrentLineLength()`

## Решения

Предположим, вы хотите отобразить различные цветовые схемы, основанные на том, работает ли Vim в терминале или как графический интерфейс, т.е. вам нужен скрипт для принятия решений. Для этого вы можете использовать:

``` viml
:if has("gui_running")
: colorscheme desert
:else
: colorscheme darkblue
:endif
```

Как это работает:

- `has()` - это функция, используемая для определения, поддерживается ли указанная функция в Vim, установленном на текущем компьютере. Смотрите `:help feature-list` чтобы узнать, какие функции доступны в Vim.
- Оператор `if` проверяет данное условие. Если оно выполнено, мы предпринимаем определенные действия. "Else" (Иначе) мы принимаем альтернативное действие.
- Обратите внимание, что оператор `if` должен иметь соответствующий `endif`.
- Существует также `elseif`, если вы хотите объединить несколько условий и действий.

Циклические операторы "for" и "while" также доступны в Vim:

``` viml
:let i = 0
:while i < 5
: echo i
: let i += 1
:endwhile
```

Вывод:

```
0
1
2
3
4
```

Используя встроенные функции Vim, то же самое можно также записать как:

``` viml
:for i in range(5)
:    echo i
:endfor
```

- `range()` - это встроенная функция, используемая для генерации диапазона чисел. Смотрите `:help range()` для подробностей.
- Также доступны инструкции `continue` и `break`.

## Структуры данных

Vim scripting also has support for lists and dictionaries. Using these, you can build up complicated data structures and programs.

``` viml
:let fruits = ['apple', 'mango', 'coconut']

:echo fruits[0]
" apple

:echo len(fruits)
" 3

:call remove(fruits, 0)
:echo fruits
" ['mango', 'coconut']

:call sort(fruits)
:echo fruits
" ['coconut', 'mango']

:for fruit in fruits
: echo "I like" fruit
:endfor
" I like coconut
" I like mango
```

There are many functions available - see 'List manipulation' and 'Dictionary manipulation' sections in `:help function-list`.

## Writing a Vim script

We will now write a Vim script that can be loaded into Vim and then we can call its functionality whenever required. This is different from writing the script inline and running immediately as we have done all along.

Let us tackle a simple problem - how about capitalizing the first letter of each word in a selected range of lines? The use case is simple - When I write headings in a text document, they look better if they are capitalized, but I'm too lazy to do it myself. So, I can write the text in lower case, and then simply call the function to capitalize.

We will start with the basic template script. Save the following script as the file capitalize.vim:

``` viml
" Vim global plugin for capitalizing first letter of each word
"       in the current line.
" Last Change: 2008-11-21 Fri 08:23 AM IST
" Maintainer: www.swaroopch.com/contact/
" License: www.opensource.org/licenses/bsd-license.php

if exists("loaded_capitalize")
  finish
endif
let loaded_capitalize = 1

" TODO : The real functionality goes in here.
```

How It Works:

- The first line of the file should be a comment explaining what the file is about.
- There are 2-3 standard headers mentioned regarding the file such as 'Last Changed:' which explains how old the script is, the 'Maintainer:' info so that users of the script can contact the maintainer of the script regarding any problems or maybe even a note of thanks.
- The 'License:' header is optional, but highly recommended. A Vim script or plugin that you write may be useful for many other people as well, so you can specify a license for the script. Consequently, other people can improve your work and that it will in turn benefit you as well.
- A script may be loaded multiple times. For example, if you open two different files in the same Vim instance and both of them are `.html` files, then Vim opens the HTML syntax highlighting script for both of the files. To avoid running the same script twice and redefining things twice, we use a safeguard by checking for existence of the name 'loaded_capitalize' and closing if the script has been already loaded.

Now, let us proceed to write the actual functionality.

We can define a function to perform the transformation - capitalize the first letter of each word, so we can call the function as `Capitalize()`. Since the function is going to work on a range, we can specify that the function works on a range of lines.

``` viml
" Vim global plugin for capitalizing first letter of each word
"       in the current line
" Last Change: 2008-11-21 Fri 08:23 AM IST
" Maintainer: www.swaroopch.com/contact/
" License: www.opensource.org/licenses/bsd-license.php

" Make sure we run only once
if exists("loaded_capitalize") finish
endif
let loaded_capitalize = 1

" Capitalize the first letter of each word
function Capitalize() range
  for line_number in range(a:firstline, a:lastline)
    let line_content = getline(line_number)
    " Luckily, the Vim manual had the solution already!
    " Refer ":help s%" and see 'Examples' section
    let line_content = substitute(line_content, "\\w\\+", "\\u\\0", "g")
    call setline(line_number, line_content)
  endfor
endfunction
```

How It Works:

- The `a:firstline` and `a:lastline` represent the arguments to the function with correspond to the start and end of the range of lines respectively.
- We use a 'for' loop to process each line (fetched using `getline()`) in the range.
- We use the `substitute()` function to perform a regular expression search-and-replace on the string. Here we are specifying the function to look for words which is indicated by `\\w\\+` which means a word (i.e. a continuous set of characters that are part of words). Once such words are found, they are to be converted using `\\u\\0` - the `\\u` indicates that the first character following this sequence should be converted to upper case. The `\\0` indicates the match found by the `substitute()` function which corresponds to the words. In effect, we are converting the first letter of each word to upper case.
- We call the `setline()` function to replace the line in Vim with the modified string.

To run this command:

1. Open Vim and enter some random text such as 'this is a test'.
2. Run `:source capitalize.vim` - this 'sources' the file as if the commands were run in Vim inline as we have done before.
3. Run `:call Capitalize()`.
4. The line should now read 'This Is A Test'.

Running `:call Capitalize()` every time appears to be tedious, so we can assign a keyboard shortcut using leaders:

``` viml
" Vim global plugin for capitalizing first letter of each word
" in the current line
" Last Change: 2008-11-21 Fri 08:23 AM IST
" Maintainer: www.swaroopch.com/contact/
" License: www.opensource.org/licenses/bsd-license.php

" Make sure we run only once
if exists("loaded_capitalize")
  finish
endif
let loaded_capitalize = 1

" Refer ':help using-<Plug>'
if !hasmapto('<Plug>Capitalize')
  map <unique> <Leader>c <Plug>Capitalize
endif
noremap <unique> <script> <Plug>Capitalize <SID>Capitalize
noremap <SID>Capitalize :call <SID>Capitalize()<CR>

" Capitalize the first letter of each word
function s:Capitalize() range
  for line_number in range(a:firstline, a:lastline)
    let line_content = getline(line_number)
    " Luckily, the Vim manual had the solution already!
    " Refer ":help s%" and see 'Examples' section
    let line_content = substitute(line_content, "\\w\\+", "\\u\\0", "g")
    call setline(line_number, line_content)
  endfor
endfunction
```

- We have changed the name of the function from simply `Capitalize` to `s:Capitalize` - this is to indicate that the function is local to the script that it is defined in, and it shouldn't be available globally because we want to avoid interfering with other scripts.
- We use the `map` command to define a shortcut.
- The `<Leader>` key is usually backslash, `\`.
- We are now mapping `<Leader>c` (i.e. the leader key followed by the 'c' key) to some functionality.
- We are using `<Plug>Capitalize` to indicate the `Capitalize()` function described within a plugin i.e. our own script.
- Every script has an ID, which is indicated by `<SID>`. So, we can map the command `<SID>Capitalize` to a call of the local function `Capitalize()`.

So, now repeat the same steps mentioned previously to test this script, but you can now run `\c` to capitalize the line(s) instead of running `:call Capitalize()`.

This last set of steps may seem complicated, but it just goes to show that there's a wealth of possibilities in creating Vim scripts and they can do complex stuff.

If something does go wrong in your scripting, you can use `v:errmsg` to see the last error message which may give you clues to figure out what went wrong.

Note that you can use `:help` to find help on everything we have discussed above, from `:help \w` to `:help setline()`.

## Using external programming languages

Many people would not like to spend the time in learning Vim's scripting language and may prefer to use a programming language they already know and write plugins for Vim in that language. This is possible because Vim supports writing plugins in Python, Perl, Ruby and many other languages.

In this chapter, we will look at a simple plugin using the Python programming language, but we can easily use any other supported language as well.

As mentioned earlier, if you are interested in learning the Python language, you might be interested in my other free book [A Byte of Python]({{ book.pythonBookUrl }}).

First, we have to test if the support for the Python programming language is present.

``` viml
:echo has("python")
```

If this returns `1`, then we are good to go, otherwise you might want to install Python on your machine and try again.

Suppose you are writing a blog post. A blogger usually wants to get as many people to read his/her blog as possible. One of the ways people find such blog posts is by querying a search engine. So, if you're going to write on a topic (say 'C V Raman', the famous Indian physicist who has won a Nobel Prize for his work on the scattering of light), you might want to use important phrases that helps more people find your blog when they search for this topic. For example, if people are searching for 'c v raman', they might also search for the 'raman effect', so you may want to mention that in your blog post or article.

How do we find such related phrases? It turns out that the solution is quite simple, thanks to Yahoo! Search.

First, let us figure out how to use Python to access the current text, which we will use to generate the related phrases.

``` viml
" Vim plugin for looking up popular search queries related
"       to the current line
" Last Updated: 2008-11-21 Fri 08:36 AM IST
" Maintainer: www.swaroopch.com/contact/
" License: www.opensource.org/licenses/bsd-license.php

" Make sure we run only once
if exists("loaded_related")
  finish
endif
let loaded_related = 1

" Look up Yahoo Search and show results to the user
function Related()
python <<EOF

import vim

print 'Length of the current line is', len(vim.current.line)
EOF
endfunction
```

The main approach to writing plugins in interfaced languages is same as that for the built-in scripting language.

The key difference is that we have to pass on the code that we have written in the Python language to the Python interpreter. This is achieved by using the EOF as shown above - all the text from the `python <<EOF` command to the subsequent `EOF` is passed to the Python interpreter.

You can test this program, by opening Vim again separately and running `:source related.vim`, and then run `:call Related()`. This should display something like `Length of the current line is 54`.

Now, let us get down the actual functionality of the program. Yahoo! Search has something called a [RelatedSuggestion query](http://developer.yahoo.com/search/web/V1/relatedSuggestion.html) which we can access using a web service. This web service can be accessed by using a Python API provided by Yahoo! Search [pYsearch](http://pysearch.sourceforge.net):

``` viml
" Vim plugin for looking up popular search queries related
" to the current line
" Last Updated: 2008-11-21 Fri 08:36 AM IST
" Maintainer: www.swaroopch.com/contact/
" License: www.opensource.org/licenses/bsd-license.php

" Make sure we run only once
if exists("loaded_related")
  finish
endif
let loaded_related = 1

" Look up Yahoo Search and show results to the user
function Related()
python <<EOF

import vim

from yahoo.search.web import RelatedSuggestion

search = RelatedSuggestion(app_id='vimsearch', query=vim.current.line)
results = search.parse_results()

msg = 'Related popular searches are:\n'
i= 1
for result in results:
    msg += '%d. %s\n' % (i, result)
    i += 1
print msg

EOF
endfunction
```

Notice that we use the current line in Vim as the current text we are interested in, you can change this to any text that you want, such as the current word, etc.

We use the `yahoo.search.web.RelatedSuggestion` class to query Yahoo! Search for phrases related to the query that we specify. We get back the results by calling `parse_results()` on the result object. We then loop over the results and display it to the user.

1. Run `:source related.vim`
2. Type the text `c v raman`.
3. Run `:call Related()`
4. The output should look something like this:

```
Related popular searches are:
1. raman effect
2. c v raman india
3. raman research institute
4. chandrasekhara venkata raman
```

## Summary

We have explored scripting using the Vim's own scripting language as well as using external scripting/programming languages. This is important because the functionality of Vim can be extended in infinite ways.

See `:help eval`, `:help python-commands`, `:help perl-using` and `:help ruby-commands` for details.

To dive deep into this topic, see [Learn VimScript The Hard Way by Steve Losh](http://learnvimscriptthehardway.stevelosh.com).
