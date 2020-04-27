About book
-------

In nutshell, this book is like CCNA but for python. From the one hand, the book is basic enough, so everyone can handle it, from the other hand, the book considers all main topics which allow you to develop skill independently in the future. Python deep dive is not a goal of this book. The goal is to explain Python basics in plain language and provide understanding of necessary tools for practical usage. Everything in this book is focused on network equipment and interaction with it. It right away gives opportunity to use knowledge gained at the course in network engineers daily work. All shown examples are based on Cisco equipment but, of course, they could be applied to any other equipment.

Who is this book for?
~~~~~~~~~~~~~~~~~~

For network engineers with or without programming experience. All examples and homework will be formed with a focus on network equipment. This book will be useful for network engineers who want to automate their daily basis routine tasks and want start coding but don't know how to approach this.
Still haven't decided whether it worth reading this book? Read
:ref:`feedbacks <testimonials>`.

Why you need to learn programming?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Programming knowledge for network engineer could be compared with necessity of English knowledge. When you know English at least on level which allows to read technical documentation you expand your opportunities at once:

-  Much more literature, forums, blogs are available;
-  Easier to find solution for almost every question or issue if you ask Google.

Knowledge of programming is very similar in this. For instance, If you know Python at least on basic level  you open plenty of new opportunities. Also analogy to English fits here because you can be capable specialist without knowledge of English language. English gives you opportunity but it's not a mandatory requirement.


OS and Python requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All examples and terminal outputs in the book are shown on Debian Linux. Python 3.7 is used in this book but for the majority of examples Python 3.x will be enough. Only some examples requires Python version higher than 3.5. It always explicitly indicated and generally concerns some additional features.

Примеры
~~~~~~~

Все примеры, которые используются в книге, располагаются в
`репозитории <https://github.com/natenka/pyneng-examples-exercises>`__.
Примеры, которые рассматриваются в разделах книги, являются обучающими.
Это значит, что они не обязательно показывают лучший вариант решения
задачи, так как они основаны только на той информации, которая
рассматривалась в предыдущих главах книги. Кроме того, довольно часто
примеры, которые давались в разделах, развиваются в заданиях. То есть, в
заданиях вам нужно будет сделать лучшую, более универсальную, и, в
целом, более правильную версию кода. Если есть возможность, лучше
набирать код, который используется в книге, самостоятельно, или, как
минимум, скачать примеры и попробовать что-то в них изменить – так
информация будет лучше запоминаться. Если такой возможности нет,
например, когда вы читаете книгу в дороге, лучше повторить примеры
самостоятельно позже. В любом случае, обязательно нужно делать задания
вручную.

Tasks
~~~~~~~

Все задания и вспомогательные файлы можно скачать в
`репозитории <https://github.com/natenka/pyneng-examples-exercises>`__,
том же, где располагаются примеры кода. Если в заданиях раздела есть
задания с буквами (например, 5.2a), то нужно выполнить сначала задания
без букв, а затем с буквами. Задания с буквами, как правило, немного
сложнее заданий без букв и развивают идею в соответствующем задании без
буквы. Если получается, лучше делать задания по порядку. В книге
специально не приведены ответы на задания, так как, к сожалению, когда
есть ответы, очень часто вместо того, чтобы попытаться решить сложное
задание самостоятельно, подглядывают в них. Конечно, иногда возникает
ситуация, когда никак не получается решить задание – попробуйте отложить
его, задать вопрос в `Slack <https://join.slack.com/t/pyneng/shared_invite/enQtNzkyNTYwOTU5Njk5LWE4OGNjMmM1ZTlkNWQ0N2RhODExZDA0OTNhNDJjZDZlOTZhOGRiMzIyZjBhZWYzYzc3MTg3ZmQzODllYmQ4OWU>`__ и
сделать какое-либо другое.

.. note::
    На `Stack Overflow <https://stackoverflow.com>`__ есть ответы
    практически на любые вопросы. Так что, если Google отправил Вас на
    него, это, с большой вероятностью значит, что ответ найден. Запросы,
    конечно же, лучше писать на английском – по Python очень много
    материалов и, как правило, подсказку найти легко

Ответы могли бы показать, как ещё можно выполнить задание или же как
лучше это сделать. Но на этот счёт не следует переживать, так как,
скорее всего, в следующих разделах встретится пример, в котором будет
показано, как писать такой код.

Вопросы
~~~~~~~

Для части тем книги созданы вопросы:

-  `Типы данных. Часть 1 <https://goo.gl/forms/xKHX5xNM8Pv5sQDf2>`__
-  `Типы данных. Часть 2 <https://goo.gl/forms/igxR3ub3tQg3ycX53>`__
-  `Контроль хода программы. Часть
   1 <https://goo.gl/forms/2TmGcrhG11h2SdLn1>`__
-  `Контроль хода программы. Часть
   2 <https://goo.gl/forms/KZGaDquGlUmOz2kG3>`__
-  `Функции и модули. Часть
   1 <https://goo.gl/forms/M1DpbdD0brVbdp1G3>`__
-  `Функции и модули. Часть
   2 <https://goo.gl/forms/rNvdX9bHw8wLajJp2>`__
-  `Регулярные выражения. Часть
   1 <https://goo.gl/forms/5UpkJbm1dORqs4bP2>`__
-  `Регулярные выражения. Часть
   2 <https://goo.gl/forms/ltuOAO62yLlZkEmm1>`__
-  `Базы данных <https://goo.gl/forms/wtGgmWg0vow1Cyqo1>`__

Эти вопросы можно использовать как для проверки знаний, так и в роли
заданий. Очень полезно поотвечать на вопросы после прочтения соответствующей темы.
Они позволят вам вспомнить материал темы, а также увидеть на практике
разные аспекты работы с Python. Постарайтесь сначала ответить
самостоятельно, а затем подсмотреть ответы в IPython по тем вопросам, в
которых вы сомневаетесь.

Презентации
~~~~~~~~~~~

Для всех тем книги есть презентации в
`репозитории <https://github.com/natenka/pyneng-slides>`__. По ним
удобно быстро просматривать информацию и повторять. Если вы знаете
основы Python, то стоит их пролистать.

Скачать все презентации в формате PDF можно в специальном
`репозитории <https://github.com/natenka/pyneng-slides/tree/py3-pdf>`__

Форматы файлов книги
~~~~~~~~~~~~~~~~~~~~

Книгу можно скачать в двух форматах: PDF, Epub.
Они автоматически обновляются, поэтому всегда содержат одинаковую
информацию.


Обсуждение
~~~~~~~~~~

Для обсуждения книги, заданий, а также связанных вопросов используется
`Slack <https://pyneng-slack.herokuapp.com>`__. Все вопросы, предложения
и замечания по книге также пишите в
`Slack <https://pyneng-slack.herokuapp.com>`__.

