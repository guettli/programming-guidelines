Programming Guidelines
======================

My personal programming guidelines. **Please provide feedback**, tell me what's wrong and what's missing and create a new issue here: https://github.com/guettli/programming-guidelines/issues

Introduction
------------

I was born 1976. I started coding with basic and assembler when I was 13. Later turbo pascal. From 1996-2001 I studied computer science at HTW-Dresden (Germany). I learned Shell, Perl, Prolog, C, C++, Java, PHP and finally Python.

No Shell Scripting
------------------

The shell is nice for interactive usage. But shell scripts are unreliable: Most scripts fail if filenames contain whitespaces. Shell-Gurus know how to work around this. But quoting can get really complicated. I use the shell for interactive stuff daily. But I stopped writing shell scripts.

Sometimes I see young and talented programmers wasting time. There are two ways to learn: Make mistakes yourself, or read from the mistakes which were done by other people. 

This list summarises a lot of mistakes I did in the past. I wrote it, to help you, to avoid these mistakes.

C is slow
---------

... looking at the time you need to get things implemented. Avoid it if possible.

SSH to remote-server
--------------------

I still do this (ssh remote-server ... vi /etc/...), but I want to reduce it. Configuration management is much better. For example salt-stack or ansible.

Logging to files
----------------
I still do this, but I want to reduce it. Logs are endless streams. Files are a buch of bytes with fixed length.
Both concepts don't fit together. Sooner or later your logs get rotated. Now you are in trouble if you want to run a log checker for every line in your logfile. I mean the mathematically version of "every line". This gets really complicated if you want to check every line. Rotating logfiles needs to be done sooner or later. But how to rotate the file, if a process still write to it? This is one problem, which was solved several hundred times and each time different ...

Use Systemd
------------

It is available, don't reinvent. Don't do double-fork magic any more. Use a systemd service with Type=simple.

Avoid Office Documents or UML-tools
-----------------------------------

Use a way to edit content (use cases, specs, ...) over the internet: Use wikis. Don't waste time with UML tools. Write down the high level use case, the cardinality and the steps. Sequence diagrams are not needed. Just: first, second, third ...


Focus on Data Structures
-----------------------

A relational database is a rock solid data storage. Use a tool to get schema migrations done (for example django). Use PostgreSQL.

Version Control
---------------

I like git.


Time is too short to run all tests before commit+push
-----------------------------------------------------
If the guideline of your team is: "Run all tests before commit+push", then there
is something wrong. Time is too short to watch tests running! Run only the tests of the code you touched (py.test -k my_keyword).

Conditionless Data Structures
----------------------------
Imagine you have a table "meeting" and a table "place". The table "meeting" has a ForeignKey to table "place". In the beginning it might be not clear yet where the meeting will be. Most developers will make the ForeignKey optional (nullable). WAIT: This will create a condition in your data structure. There is a way easier solution: Create a place called "unknown". Use this as default, avoid nullable columns. This data structure (without a nullable ForeignKey) makes implementing the GUI much easier.

With other words: If there is no NULL in your data, then there will be no NullPointerException in your source code while processing the data :-)

[True, False, Unknown] is not a nullable Bollean Column
-------------------------------------------------------
If you want to store a data in a SQL database which has three states (True, False, Unknown), then you might think a nullable boolean column (here "my_column") is the right choice. But I think it is not. Do you think the SQL statement "select * from my_table where my_column = %s" works? No, it won't work since "select * from my_table where my_column = NULL" will never ever return a single line. If you don't believe me, read: `Effect of NULL in WHERE clauses (Wikipedia) <https://en.wikipedia.org/wiki/Null_(SQL)#Effect_of_Unknown_in_WHERE_clauses>`_. If you like typing, you can work-around this in your application, but I prefer straight forward solutions with only few conditions.

If you want to store True, False, Unknown: Use text, integer or a new table and a foreign key.

CI
--
Use continuous integration. Only tested code is allowed to get deployed. This needs to be automated. Humans make more errors than automated processes.

Avoid Threads and Async
-----------------------
Threads and Async are fascinating. BUT: It's hard to debug. You will need much longer than you initially estimated. Avoid it, if you want to get things done. It's different in your spare time: Do what you want and what is fascinating for you.

Don't waste time doing it "generic and reusable" if you don't need to
----------------------------------------------------------------------
If you are doing some kind of software project for the first time, then focus on getting it done. Don't waste time to do it perfect, reusable, fast or portable. You don't know the needs of the future today. One main goal: Try to make your code easy to understand without comments. First get the basics working, then tests and CI, then listen to the needs, wishes and dreams of your customers.

Use all features PostgreSQL does offer
--------------------------------------

Use all features PostgreSQL does offer. Don't constrain yourself to use only the portable SQL features. It's ok if your code does work only with PostgreSQL and no other database. If there is the need to support other databases in the future, then handle this problem in the future, not today. PostgreSQL is great, and you waste time if you don't use its features.

Imagine there is be a a Meta-Programming-Language (AFAIK this does not exist) and it is an official standard created by the ISO (like SQL). You can compile this Meta-Programming-Language to Java, Python, C and other languages. But this Meta-Programming-Language would only support 70% of all features of the underlaying programming languages. Would it make sense to say "My code must be portable, you must not use implementation specific stuff!"?. No, I think it would make no sense.

My conclusion: Use all features PostgreSQL has. Don't make live more complicated than necessary and don't restrict yourself to use only portable SQL.

Use a modern IDE
----------------

Time for vi and emacs has passed. Use a modern IDE on modern hardware (SSD disk). For example PyCharm. I switched from Emacs to PyCharm in 2016. I used Emacs from 1997 until 2015 (18 years).

Type with ten fingers
---------------------
Learn to type with ten fingers. It's like flying if you do it. Your eyes can stay on the rubbish you type, and you don't need to move your eyes down (to keyboard) and up (to monitor) several hundred times per day. This saves a lot of energy. Avoid to switch between mouse and keyboard too much. I like lenovo keyboards with track point. Use a clipboard manager like Diodon.

Easy to read code: Use guard clauses
------------------------------------
Guard clauses help to avoid indentation. It makes code easier to read and understand. See http://programmers.stackexchange.com/a/101043/129077


Cardinality
-----------

It does not matter how you work with your data (struct in C, classes in OOP, tables in SQL, ...). Cardinality is very important. Using 0..* is often easier to implement than 0..1. The first can be handled by a simple loop. The second is often a nullable column/attribute. You need conditions (IFs) to handle nullable columns/attributes.

https://en.wikipedia.org/wiki/Cardinality_(data_modeling)

Communication with Customers: Tell customers what they should test
------------------------------------------------------------------
I have seen it several times: Software gets developed. The customer was told to test and ... nothing happens. That's not satisfying since software developers want to hear that their work does help. If you (the developer) provide a check-list of things to test, then the likelihood to get feedback soon is bigger.

Communication with Customers: Define "done"
-------------------------------------------
Define "done" with your customers. Humans like to be creative and if thing X gets changed, then they have fancy ideas how to change thing Y. Be friendly and listen: Write these fancy ideas down on the "do later" list or wiki page. If you don't have a definition of done/ready, then you should not start to write source code. First define the goal, then choose a strategy to get to the goal.

Dare to say "Please wait, I want to take a note"
------------------------------------------------

Most people can listen and write at once. I can't. And I guess a lot of programmers have this problem. I can only do one thing at a time. If you are telephoning with a customer and he has a lot of things to tell you, don't fool yourself. You will only remember 4 of 5 issues. Dare to day "please wait, I want to take a note". This way you can care for all issues, which results in happy customers.

Source code generation is a stupid idea
---------------------------------------

I guess every young programmer wants to write a tool which creates software (sooner or later). Stop! Please think about it again. What do you gain? Don't confuse data and code. Imagine you have a source code generator which takes DATA as input and creates SOURCE as output. What is the difference between DATA and SOURCE? What do you gain? Even if you have some kind of artificial intelligence, you can't create new (redundancy free) data if your only input is DATA. It is just a different syntax. Why not write a program which reads DATA and does the thing you want to do with SOURCE?

Exception1: If you have some sort of Interface Definition Language like (Corba or Protocol Buffers), then you can create stubs as source code. But this generated source should not contains conditions (IFs) or loops.

Regex are great - But it's like eating rubish
---------------------------------------------

Yes, I like regular expression. But slow down: What do I do, if I use a regex? I think it is "parsing". I remember to have read this some time ago: "Time is too short to rewrite parsers". Don't parse data! We live in the 21 century. Consume high level data structures like json, yaml or protcol buffers. If possible, refuse to accept CSV or custom text format as input data.

From time to time you need to do text processing. Unfortunately there are several regex flavors. My guide-line: Use PCRE. They are available in Python, Postfix and many other tools. Don't waste time with other regex flavors, if PCRE are available. 

Give booleans a "positive" name
-------------------------------
I once gave a DB column the name "failed". It was a boolean indicating if the transmission of data to the next system was successful. The output as table in the GUI looked confusing for humans. The column heading was "failed". What should be visible in the cell for failed rows? Boolean usually get translated to "Yes/No" or "True/False". But if the human brain reads "Yes" or "True" it initially things "all right". But in this case "Yes" meant "Yes, it failed". The next time I will call the column "was_successful", then "Yes" means "Yes, it was successful".

Love your docs
--------------
I have seen it several times on github: If I provide a hint that the docs could be improved, a lot of maintainers don't care much. Just look at the README files on github. They starts with "Installing", then "Configuring" ... What is missing? An Introduction! Just some sentences what this great project is all about. Programmers love details. Dear programmers, learn to relax and look at the thing you create like a new comer. If you have this mind set "I do the important (programming) stuff. Someone else can care for the docs", then your open source project won't be successful.

Passing around methods make things hard to debug
------------------------------------------------
Even in C you can pass around method-pointers. It's very common in JavaScript and sometimes it gets done in Python, too. It is hard to debug. IDE's can't resolve the code: "Find usages" don't work.  I try to avoid it. I prefer OOP (Inheritance) and avoid passing around methods or using them as variables.

Software Design Patterns are overrated
--------------------------------------

If you need several pages in a book to explain a software design pattern, then it is too complicated.
I think Software Design Patterns are overrated.

KISS
----

Keep it simple and stupid. The most boring and most obvious solution is often the best. Although it sometimes takes months until you know which solution it is.

Time is too short for "git rebase" vs "git merge" discussions
--------------------------------------------------------------
What's the net result of "git rebase" vs "git merge" discussion? The result is source code. Who cares how source code got into the current state? Me, but only sometimes. Archeology is intresting .... but more interesting is the future, since you can influence it.

traceroute won't help you
-------------------------
.... if you have trouble with a tcp connection. Use tcptraceroute for tcp connection tests (http, https, ssh, smtp, pop3, imap, ...). Reason: traceroute uses UDP, not TCP.

This is untestable code
-----------------------

If you are new to software unit testing, then you might think ... "some parts of my code are *untestable*".

I don't think so. I guess your software uses the IPO pattern: https://en.wikipedia.org/wiki/IPO_model Input, Processing, Output. The question is: How to feed the input for testing to my code? Mocking, virtualization and automation are your friends.

The "untestable" code needs to be cared of. Code is always testable, there is no untestable code. Maybe your knowledge of testing is limited up to now. Finding untestable code is the beginning of an interesting route to good code.

If you do coding to implement backup ...
----------------------------------------

If you do coding/programming to implement your backup of data, then you are on the wrong track.

It is very likely that you will do it wrong, and this will be a big risk, if your context is backing up data.

Why? Because you will notice your fault if you try to recover your data. 

Compare this to an gadet app for a mobile phone. If this app fails, it is likely that the fault does not lead to data loss.

**Use** a backup tool, even if you love to do programming. Configure it, but don't write it yourself.

ForeignKey from code to DB
--------------------------

This code is uses the ORM of django

.. code-block:: python

    if ....:
        ticket.responsible_group=Group.objects.get(name='Leaders')

Above code is dirty because 'Leaders' is like ForeignKey from code to a database row.

If you think this is better .....

.. code-block:: python

    if ....:
        ticket.responsible_group=Group.objects.get(name=constants_module.GROUP_NAME_OF_LEADERS)

.... then you did not understand what I tried to explain.


Testcode is conditionless
--------------------------

Testcode should not contain conditions (the keyword`if`). If you have loops (`for`, `while`) in your tests, then this looks strange, too.

Tests should be straight forward:

 #. Build environment: Data structures, ...
 #. Run the code which operates on the data structures
 #. Ensure that the output is like you want it to.

.. code-block:: python

    class MyTest(unittest.TestCase):
        def test_foo(self):
            foo=Foo()
            self.assertEqual(42, foo.find_answer_to_the_ultimate_question_of_life_the_universe_and_everything())
        

Don't search the needle in a haystack. Inject dynamite and let it explode
-------------------------------------------------------------------------

Imagine you have a huge code base which was written by a nerd which is gone since several months. Somewhere in the code a database row gets updated. This update should not happen, and you can't find the relevant source code line during the first minutes. You can reproduce this failure in a test environment. What can you do? You can start a debugger and jump through the lines which get executed. Yes, this works. But this is "Searching the needle in a haystack". This takes too long. I like solutions like this: Add a constraint trigger to your database which fires on modification. Execute the code and BANG. you get the relevant code line with a nice stacktrace. This way you get the solution provided on a silver plattern with minimal effort :-)

Avoid magic or uncommon things
------------------------------

* hard links
* file system ACLs (Access control lists)
* git submodules (Please use configuration management, deployment tools, ...)

Learn one programming language, not ten.
----------------------------------------

Most young developers think you need to learn many programming languages to be a good developer.

Yes, it does help sometimes to know how the programming language C works.

My opinion: Lear Python, JavaScript.

Then learn other topics: Database, Configuration management, continuous integration, organizing, team work, learn to play a music instrument.

Learn "git bisect"
------------------
"git bisect" is a great tool to find the commit, which introduced an error. Unfortunately there it is not a one-liner up to now, but you can use it like this:

.. code-block:: shell

    user@host> git bisect start HEAD HEAD~10 


    user@host> git bisect run py.test -k test_something
     ...
    c8bed9b56861ea626833637e11a216555d7e7414 is the first bad commit
    Author: ...


    # useless, but unfortunately needed
    user@host> git bisect reset


Thank you
---------
* Robert C. Martin for the book "Clean Coder"
* Linus Torvalds for the quote "Bad programmers worry about the code. Good programmers worry about data structures and their relationships."
* All people who contribute to open source software (Linux, Python, PostgreSQL, ...)
* All people who ask question and/or answers them at places like StackOverflow.
* People I meat during study at HTW-Dresden
* My teammates at TBZ
