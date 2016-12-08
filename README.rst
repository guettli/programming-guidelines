Programming Guidelines
======================

My programming guidelines. Please provide feedback, tell me what's wrong and what's missing: https://github.com/guettli/programming-guidelines/issues

Introduction
------------

I was born 1076. I started coding with basic and assemlber when I was 13. Later turbo pascal. From 1996-2001 I studied computer science at HTW-Dresden (Germany). I learned Shell, Perl, Prolog, C, C++, Java, PHP and finally Python.

No Shell Scripting
------------------

The shell is nice for interactive usage. But shell scripts are unreliable: Most scripts fail if filenames contain whitespaces. Shell-Gurus know how to work around this. But quoting can get really complicated. I use the shell for interactive stuff daily. But I stopped writing shell scripts.

Sometimes I see young and talented programmers wasting time. There are two ways to learn: Make mistakes yourself, or read from the mistakes which were done by other people. 

This list summarizes a lot of mistakes I did in the past. I wrote it, to help you, to avoid these mistakes.

C is slow
---------

... looking at the time you need to get things implemented. Avoid it if possible.

SSH to remote-server
--------------------

I still do this, but I want to reduce it. Configuration management is much better. For example salt-stack

Logging to files
----------------
I still do this, but I want to reduce it. Logs are endless streams. Files are a buch of bytes with fixed length.
Both concepts don't fit together. Somer or later your logs get rotated. Now you are in trouble if you want to run a log checker for every line in your logfile. I mean the mathematicaly version of "every line". This gets really compliated if you want to check every line. Rotating logfiles needs to be done sooner or later. But how to rotate the file, if a process still write to it? This is one problem, which was solved several hundret times and each time different ...

Use Systemd
------------

It is available, don't reinvent. Don't do double-fork magic any more. Use a systemd service with Type=simple.

Avoid Office Documents or UML-tools
-----------------------------------

Use a way to edit content (use cases, specs, ...) over the internet: Use wikis. Don't waste time with UML tools. Write down the high level use case, the cardinaltiy and the steps. Sequence diagrams are not needed. Just: first, second, third ...


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

CI
--
Use continuous integration. Only tested code is allowed to get deployed. This needs to be automated. Humans make more errors than automated processes.

Avoid Threads and Async
-----------------------
Threads and Async are fascinating. BUT: It's hard to debug. You will need much longer than you initially estimated. Avoid it, if you want to get things done. It's different in your spare time: Do what you want and what is fascinating for you.

Don't waste time doing it "generic and reusable" if you don't need to
----------------------------------------------------------------------
If you are doing some kind of software project for the first time, then focus on getting it done. Don't waste time to do it perfect, reusable, fast or portable. You don't know the needs of the future today. One main goal: Try to make your code easy to understand without comments.

Use all features PostgreSQL does offer
--------------------------------------

Use all features PostgreSQL does offer. Don't constrain yourself to use only the portable features. It's ok if your code does work only with PostgreSQL and no other database. If there is the need to support other databases in the future, then handle this problem in the future, not today. PostgreSQL is great, and you waste time if you don't use its features.


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

Yes, I like regular expression. But slow down: What do I do, if I use a regex? I think it is "parsing". I remember to have read this some time ago: "Time is too short to rewrite parsers". Don't parse data! We live in the 21 century. Consume high level data structures like json, yaml or protcol buffers. Refuse to accept CSV or custom text based input data.

Give booleans a "positive" name
-------------------------------
I once gave a DB column the name "failed". It was a boolean indicating if the transmission of data to the next system was successful. The output as table in the GUI looked confusing for humans. The column heading was "failed". What should be visible in the cell for failed rows? Boolean usually get translated to "Yes/No" or "True/False". But if the human brain reads "Yes" or "True" it initially things "all right". But in this case "Yes" meant "Yes, it failed". The next time I will call the column "was_successful", then "Yes" means "Yes, it was successful".
