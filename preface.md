# Предисловие

## О Vim

[Vim](http://www.vim.org) - это компьютерная программа, используемая для редактирования, предоставляющая ряд функций, которые помогут вам **писать лучше**.

## Почему Vim?

Давайте посмотрим правде в глаза, очень редко Вы делаете свою лучшую работу с первой попытки. Скорее всего, Вы будете часто редактировать её, пока она не станет 'хорошей'.

Как однажды сказал Луис Брандейс:

> Нет великой записи, только великое переписывание.

Выполнение этих многочисленных быстрых изменений было бы намного проще, если бы у нас был способный редактор, который поможет нам и *именно* в этом Vim блистает, он намного лучше большинства простых текстовых редакторов и богатых редакторов документов.

## Зачем написана эта книга?

Я использую редактор Vim с тех пор, как научился использовать старый редактор vi во время занятий с Unix в колледже. Vim является одним из немногих частей программного обеспечения, которые я использую в течение почти 10 часов в день. Я знал, что есть так много функций, о которых я не подозревал, но они могли быть потенциально полезными для меня, поэтому я начал понемногу изучать Vim.

Чтобы закрепить свое понимание и помочь другим также исследовать Vim, я начал писать этот сборник заметок и назвал его книгой.

Некоторые из принципов, которые я пытался запомнить при написании этих заметок:

1. Simple literature. The importance of this should be reinforced again and again.
2. Emphasis on examples and how-to.
3. The one-stop shop for readers to learn Vim - from getting started to learning advanced stuff.
4. Get the user to understand how to do things the Vim way - from modes to buffers to customization. Most people learn only the basic vi commands and do not attempt to learn anything beyond that. Learning such concepts is the tipping point, they become hardcore Vim users i.e. Vimmers, which means they extract the most out of Vim, which is the intent of this book.
5. A lot of things are documented and stored here as a reference for people such as how to use Vim as an IDE, etc. There are various ways of doing it and instead of the user struggling to figure out which plugins to try out, the book already has the basic background work already for the reader.
6. Just enough info to get you to understand and use, not everything required (Pareto principle)
7. Relatedly, the book shouldn't attempt to rewrite the reference manual. Where appropriate, it should simply point out the relevant parts. This way, there is no redundancy, the user learns to use the awesome built-in reference manual which is important, and the book can stand on its own strengths as well.

To summarize, the mantra is *Concepts. Examples. Pithy.*

## Статус книги

Книга была в стадии разработки и последний раз обновлялась в 2008 году. Восемь лет спустя (2016) я воссоздал книгу в [формате GitBook](http://www.gitbook.com) Итак, давайте скажем, что это была книга "1.0" в 2008 году :-)

Конструктивные предложения приветствуются. Пожалуйста, присылайте свои мысли и предложения [по электронной почте] ({{ book.contactUrl }}) или [вопросы github] ({{ book.sourceUrl }}).

## Официальный сайт

Официальный сайт книги {{ book.officialUrl }}. С сайта вы можете прочитать всю книгу онлайн или скачать последние версии книги, а также отправить мне отзыв.

## Некоторые мысли

> Книги не пишутся - они переписываются. В том числе и свои собственные. Это одна из самых трудных вещей для принятия, особенно после того, как седьмое переписывание ещё не сделало это.
> -- Майкл Крайтон

<!-- -->

> Совершенство достигается не тогда, когда больше нечего добавить, а когда уже нечего отнять.
> -- Антуан де Сант-Экзюпери
