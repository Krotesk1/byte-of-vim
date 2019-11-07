# Плагины

Как уже видели в предыдущей главе, мы можем использовать скрипты для расширения существующей функциональности Vim, чтобы делать больше вещей. Скрипты, которые расширяют или добавляют функциональность называются "плагины".

Существуют различные виды плагинов, которые могут быть написаны:

- vimrc
- глобальный плагин
- плагин файловых типов
- плагин подсветки синтаксиса
- плагин компилятора

Вы можете не только писать свои собственные плагины, но также загружать и [использовать плагины, написанные другими](http://www.vim.org/scripts/index.php).

## Персонализация с помощью vimrc

Когда я устанавливаю новый дистрибутив Linux или переустанавливаю Windows, первое, что я делаю после установки Vim - это извлекаю свой последний файл `vimrc` из моих резервных копий, а затем начинаю использовать Vim. Почему это так важно? Потому что `vimrc` содержит различные персонализации/установки, которые мне нравятся, что делает Vim более полезным и удобным для меня.

Есть два файла, которые вы можете создать, чтобы настроить Vim на свой вкус:

1. vimrc - для общих настроек
2. gvimrc - за специфических настроек интерфейса

Они хранятся в виде:

- `%HOME%/_vimrc` и `%HOME%/_gvimrc` в Windows
- `$HOME/.vimrc` и `$HOME/.gvimrc` в Linux/BSD/Mac OS X

Смотри `:help vimrc` о точном местоположении в своей системе.

Эти файлы vimrc и gvimrc могут содержать любые команды Vim. Соглашение заключается в использовании только простых настроек в файлах vimrc, а расширенные материалы получаются из плагинов.

Например, вот часть моего файла vimrc:

``` viml
" My Vimrc file
" Maintainer: www.swaroopch.com/contact/
" Reference: Initially based on http://dev.gentoo.org/~ciaranm/docs/vim-guide/
" License: www.opensource.org/licenses/bsd-license.php

" Use Vim settings, rather then Vi settings (much better!).
" This must be first, because it changes other options as a side effect.
set nocompatible


" Enable syntax highlighting.
syntax on

" Automatically indent when adding a curly bracket, etc.
set smartindent

" Tabs should be converted to a group of 4 spaces.
" This is the official Python convention
" http://www.python.org/dev/peps/pep-0008/
" I didn't find a good reason to not use it everywhere.
set shiftwidth=4
set tabstop=4
set expandtab
set smarttab

" Minimal number of screen lines to keep above and below the cursor.
set scrolloff=999

" Use UTF-8.
set encoding=utf-8

" Set color scheme that I like.
if has("gui_running")
  colorscheme desert
else
  colorscheme darkblue
endif

" Status line
set laststatus=2
set statusline=
set statusline+=%-3.3n\                      " buffer number
set statusline+=%f\                          " filename
set statusline+=%h%m%r%w                     " status flags
set statusline+=\[%{strlen(&ft)?&ft:'none'}] " file type
set statusline+=%=                           " right align remainder
set statusline+=0x%-8B                       " character value
set statusline+=%-14(%l,%c%V%)               " line, character
set statusline+=%<%P                         " file position

" Show line number, cursor position.
set ruler

" Display incomplete commands.
set showcmd

" To insert timestamp, press F3.
nmap <F3> a<C-R>=strftime("%Y-%m-%d %a %I:%M %p")<CR><Esc>
imap <F3> <C-R>=strftime("%Y-%m-%d %a %I:%M %p")<CR>

" To save, press ctrl-s.
nmap <c-s> :w<CR>
imap <c-s> <Esc>:w<CR>a

" Search as you type.
set incsearch

" Ignore case when searching.
set ignorecase

" Show autocomplete menus.
set wildmenu

" Show editing mode
set showmode

" Error bells are displayed visually.
set visualbell
```

Обратите внимание, что эти команды не имеют префикса двоеточия. Двоеточие является необязательным при написании скриптов в файлах, поскольку предполагается, что они являются командами нормального режима.

Если Вы хотите узнать подробное использование каждого параметра, упомянутого выше, обратитесь к `:help`.

Часть моего файла gvimrc:

``` viml
" Size of GVim window
set lines=35 columns=99

" Don't display the menu or toolbar. Just the screen.
set guioptions-=m
set guioptions-=T

" Font.
if has('win32') || has('win64')
  " set guifont=Monaco:h16
  " http://jeffmilner.com/index.php/2005/07/30/windows-vista-fonts-now-available/
  set guifont=Consolas:h12:cANSI
elseif has('unix')
  let &guifont="Monospace 10"
endif
```

Существуют различные примеры файлов vimrc, которые вы обязательно должны посмотреть и изучить различные виды настроек, которые возможно сделать, а затем выбрать те, которые понравятся именно Вам, и поместить их в свой собственный vimrc.

Несколько хороших, которые я нашходил ранее:

- [Распространение vim Стива Франсии](http://vim.spf13.com)
- [Конфигурация vim Стива Лоша](http://stevelosh.com/blog/2010/09/coming-home-to-vim/)
- [побрякушки dotvim](https://github.com/bling/dotvim)
- [Янус](https://github.com/carlhuda/janus)

Это известный факт, что vimrc человека обычно отражает, как долго этот человек использует Vim.

## Глобальные плагины

Глобальные плагины могут использоваться для обеспечения глобальной/общей функциональности.

Глобальные плагины могут храниться в двух местах:

1. `$VIMRUNTIME/plugin/` для стандартных плагинов, поставляемых Vim во время установки
2. Чтобы установить свои собственные плагины или загруженные плагины, вы можете использовать свой собственный каталог плагинов:

    - `$HOME/.vim/plugin/` на Linux/BSD/Mac ОS Х
    - `%HOME%/vimfiles/plugin/` в Windows
    - Смотрите `:help runtimepath` для получения подробной информации о ваших каталогах плагинов.

Давайте посмотрим, как использовать плагин.

Полезным плагином является ['highlight_current_line.vim'](http://www.vim.org/scripts/script.php?script_id=1652) от Ansuman Mohanty, который делает именно так, как следует из названия (подсветка текущей строки). Загрузите последнюю версию файла `highlight_current_line.vim` и поместите его в каталог плагинов (как упоминалось выше). Теперь перезапустите Vim и откройте любой файл. Обратите внимание, как выделена текущая строка по сравнению с другими строками в файле.

Но что, если вам это не нравится? Просто удалите файл `highlight_current_line.vim` и перезапустите Vim.

Точно так же мы можем установить наши собственные `related.vim` или `capitalize.vim` из главы Скриптинг в ваш каталог плагинов, и это позволит нам избежать проблем с использованием :source каждый раз. В конечном счете, любой плагин Vim, который вы пишете, должен оказаться где-то в вашем каталоге плагинов `.vim/vimfiles`.

## Плагины файловых типов

Плагины файловых типов написаны для работы с определенными типами файлов. Например, программы, написанные на языке программирования Си, могут иметь свой собственный стиль отступов, складывания, подсветки синтаксиса и даже отображения ошибок. Эти плагины не общего назначения, они работают для этих конкретных типов файлов.

### Использование плагина файловых типов

Давайте попробуем плагин файловых типов для XML. XML - это декларативный язык, который использует теги для описания структуры самого документа. Например, если у вас есть такой текст:

> Iron Gods <br>
> ---------  <br>
> <br>
> Ashok Banker's next book immediately following the Ramayana is said to be a novel tentatively titled "Iron Gods" scheduled to be published in 2007. A contemporary novel, it is an epic hard science fiction story about a war between the gods of different faiths. Weary of the constant infighting between religious sects and their deities, God (aka Allah, Yahweh, brahman, or whatever one chooses to call the Supreme Deity) wishes to destroy creation altogether. <br>
> <br>
> A representation of prophets and holy warriors led by Ganesa, the elephant-headed Hindu deity, randomly picks a sample of mortals, five of whom are the main protagonists of the book--an American Catholic, an Indian Hindu, a Pakistani Muslim, a Japanese Buddhist, and a Japanese Shinto follower. The mortal sampling, called a 'Palimpsest' is ferried aboard a vast Dyson's Sphere artifact termed The Jewel, which is built around the sun itself, contains retransplanted cities and landscapes brought from multiple parallel Earths and is the size of 12,000 Earths. It is also a spaceship travelling to the end of creation, where the Palimpsest is to present itself before God to plead clemency for all creation. <br>
> <br>
> Meanwhile, it is upto the five protagonists, aided by Ganesa and a few concerned individuals, including Lucifer Morningstar, Ali Abu Tarab, King David and his son Solomon, and others, to bring about peace among the myriad warring faiths. The question is whether or not they can do so before the audience with God, and if they can do so peacefully--for pressure is mounting to wage one final War of Wars to end all war itself.

(Отрывок взят из [Wikipedia](http://en.wikipedia.org/w/index.php?title=Ashok_Banker&oldid=86219280) под лицензией GNU Free Documentation)

Он может быть записан в виде XML (в частности, в формате "DocBook XML") как:
