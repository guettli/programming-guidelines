Programming Guidelines
======================

My personal programming guidelines. **Please provide feedback**, tell me what's wrong and what's missing and create a new issue here: https://github.com/guettli/programming-guidelines/issues or write me a mail to guettliml@thomas-guettler.de

`1. Introduction <#1-introduction>`_

`2. Data structures <#2-data-structures>`_

`3. Dev <#3-dev>`_

`4. Op <#4-op>`_

`5. Monitoring <#5-monitoring>`_

`6. Communication with others <#6-communication-with-others>`_

`7. Epilog <#7-epilog>`_


1. Introduction
---------------

About this README
.................

I was born 1976. I started coding with basic and assembler when I was 13. Later turbo pascal. From 1996-2001 I studied computer science at HTW-Dresden (Germany). I learned Shell, Perl, Prolog, C, C++, Java, PHP and finally Python.


Sometimes I see young and talented programmers wasting time. There are two ways to learn: Make mistakes yourself, or read from the mistakes which were done by other people. 

This list summarises a lot of mistakes I did in the past. I wrote it, to help you, to avoid these mistakes.

It's my personal opinion and feeling. No facts, no single truth.

Type with ten fingers
.....................

Learn to type with ten fingers. It's like flying if you do it. Your eyes can stay on the rubbish you type, and you don't need to move your eyes down (to keyboard) and up (to monitor) several hundred times per day. This saves a lot of energy. Avoid to switch between mouse and keyboard too much. 

I like lenovo keyboards with track point. If you want the track point to have more grip you can use sandpaper. Here are some images to illustrate what I use https://plus.google.com/108148237674350536526/posts/jF1Du1YwJwr?hl=de

Use a clipboard manager like Diodon.

KISS
....

Keep it simple and stupid. The most boring and most obvious solution is often the best. Although it sometimes takes months until you know which solution it is.

From the book "Site Reliability Engineering" (O'Reilly Media 2016) https://landing.google.com/sre/book/chapters/simplicity.html

Quote:
 The Virtue of Boring 
 
 Unlike just about everything else in life, "boring" is actually a positive attribute when it comes to software! We don’t want our programs to be spontaneous and interesting; we want them to stick to the script and predictably accomplish their business goals.



####################################################################################################


2. Data structures
------------------

Introduction
............

"Bad programmers worry about the code. Good programmers worry about data structures and their relationships." -- Linus Torvalds (creator and developer of the Linux kernel and the version control system git)


Relational Database
...................

I know SQL is..... It is either obivious or incomprehensible. Yes, it is boring.

A relational database is a rock solid data storage. Use it.

Use a tool to get schema migrations done (for example django). 

I use PostgreSQL.

I don't like NoSQL, except for caching.


Cardinality
...........

It does not matter how you work with your data (struct in C, classes in OOP, tables in SQL, ...). Cardinality is very important. Using 0..* is often easier to implement than 0..1. The first can be handled by a simple loop. The second is often a nullable column/attribute. You need conditions (IFs) to handle nullable columns/attributes.

https://en.wikipedia.org/wiki/Cardinality_(data_modeling)


Conditionless Data Structures
.............................

Imagine you have a table "meeting" and a table "place". The table "meeting" has a ForeignKey to table "place". In the beginning it might be not clear yet where the meeting will be. Most developers will make the ForeignKey optional (nullable). WAIT: This will create a condition in your data structure. There is a way easier solution: Create a place called "unknown". Use this as default, avoid nullable columns. This data structure (without a nullable ForeignKey) makes implementing the GUI much easier.

With other words: If there is no NULL in your data, then there will be no NullPointerException in your source code while processing the data :-)

Less conditions, less bugs.

[True, False, Unknown] is not a nullable Bollean Column
.......................................................

If you want to store a data in a SQL database which has three states (True, False, Unknown), then you might think a nullable boolean column (here "my_column") is the right choice. But I think it is not. Do you think the SQL statement "select * from my_table where my_column = %s" works? No, it won't work since "select * from my_table where my_column = NULL" will never ever return a single line. If you don't believe me, read: `Effect of NULL in WHERE clauses (Wikipedia) <https://en.wikipedia.org/wiki/Null_(SQL)#Effect_of_Unknown_in_WHERE_clauses>`_. If you like typing, you can work-around this in your application, but I prefer straight forward solutions with only few conditions.

If you want to store True, False, Unknown: Use text, integer or a new table and a foreign key.

Avoid nullable characters columns in databases
..............................................

If you allow NULL in a character column, then you have two ways to express "empty":

* NULL
* empty string

Avoid it if possible. In most cases you just need one variant of "empty". Simplest solution: avoid that a column holding character data types is allowed to be null.

If you really think the character column should be allowed to be NULL, then consider a constraint: If the character string in the column is not NULL, then the string must not be empty. This way ensure that there are is only one variant of "empty".



Use all features PostgreSQL does offer
......................................

Use all features PostgreSQL does offer. Don't constrain yourself to use only the portable SQL features. It's ok if your code does work only with PostgreSQL and no other database. If there is the need to support other databases in the future, then handle this problem in the future, not today. PostgreSQL is great, and you waste time if you don't use its features.

Imagine there is be a a Meta-Programming-Language (AFAIK this does not exist) and it is an official standard created by the ISO (like SQL). You can compile this Meta-Programming-Language to Java, Python, C and other languages. But this Meta-Programming-Language would only support 70% of all features of the underlaying programming languages. Would it make sense to say "My code must be portable, you must not use implementation specific stuff!"?. No, I think it would make no sense.

My conclusion: Use all features PostgreSQL has. Don't make live more complicated than necessary and don't restrict yourself to use only portable SQL.

DB Constraints are great, but are sometimes a hint to redundancy
................................................................

Database constraints are great since you can fix the very important base of your fancy coding. But what does a constraint do? It ensures that data is valid. Sometimes it can be a hint that your data contains redundancy. If you need to keep column A and column B in sync, then why not put all information into one column? Then you don't need to keep both in sync. Maybe a simpler database layout would help and then you don't need a constraint. This pattern applies sometimes, not always. 

Here is a good example which explains that if you avoid redundancy, you can avoid complicated constraints: http://dba.stackexchange.com/a/168130/5705

Transactions do not nest
........................

I love nested function calls and recursion. This way you can write easy to read code. For example recursion in quicksort is great.

Nested transactions ... sounds great. But stop: What is `ACID <https://en.wikipedia.org/wiki/ACID>`_ about? This is about:

* Atomicity
* Consistency
* Isolation
* Durability

Database transactions are atomic. If the transaction was sucessful, then it is *D*urable.

Imagine you have one outer-transaction, and two inner transaction.

#. Transaction OUTER starts
#. Transaction INNER1 starts
#. Transaction INNER1 commits
#. Transaction INNER2 starts
#. Transaction INNER2 raises an exception.

Is the result of INNER1 durable or not?

My conclusion: Transactions do not nest

Related: http://stackoverflow.com/questions/39719567/not-nesting-version-of-atomic-in-django








####################################################################################################


3. Dev
------

Zen of Python
.............

`Zen of Python <https://www.python.org/dev/peps/pep-0020/>`_

* Beautiful is better than ugly.
* Explicit is better than implicit.
* Simple is better than complex.
* Complex is better than complicated.
* Flat is better than nested.
* Sparse is better than dense.
* Readability counts.
* Special cases aren't special enough to break the rules.
* Although practicality beats purity.
* Errors should never pass silently.
* Unless explicitly silenced.
* In the face of ambiguity, refuse the temptation to guess.
* There should be one-- and preferably only one --obvious way to do it.
* Although that way may not be obvious at first unless you're Dutch.
* Now is better than never.
* Although never is often better than *right* now.
* If the implementation is hard to explain, it's a bad idea.
* If the implementation is easy to explain, it may be a good idea.
* Namespaces are one honking great idea -- let's do more of those!


CRD
...

In most cases software does create, read, update, delete data. See `CRUD <https://en.wikipedia.org/wiki/Create,_read,_update_and_delete>`_

The "update" part is the most difficult one.

Sometimes CRD helps: Do not implement the update operation. Use delete+create.

Translating to SQL terms:

+-----------+-----------------------------------+
|CRUD Term  | SQL                               |
+===========+===================================+
| create    | insert into my_table values (...) |
+-----------+-----------------------------------+
| read      | select ... from my_table          |
+-----------+-----------------------------------+
| update    | update my_table set col1=...      | 
+-----------+-----------------------------------+
| delete    | delete from my_table where ...    |
+-----------+-----------------------------------+

Take a look at virtualization and containers (`Operating-system-level virtualization <https://en.wikipedia.org/wiki/Operating-system-level_virtualization>`_). There CRD gets used, not CRUD. Containers get created, then they execute, then they get deleted. Yes, you use configuration management to set up a container. But this gets done exactly once. There is no update. This makes it easier and more predictable.



No Shell Scripting
..................

The shell is nice for interactive usage. But shell scripts are unreliable: Most scripts fail if filenames contain whitespaces. Shell-Gurus know how to work around this. But quoting can get really complicated. I use the shell for interactive stuff daily. But I stopped writing shell scripts.

Reasons:

* If a error happens in a shell script, the interpreter steps silently to the next line. Yes I know you can use "set -e". But  you don't get a stacktrace. Without stacktrace you waste a lot of time to analyze why this error happened.
* AFAIK you can't do object oriented programming in a shell. I like inheritance.
* AFAIK you can't raise exceptions in shell scripts.
* Shell-Scripts tend to call a lot of subprocesses. Every call to grep,head,tail,cut  creates a new process. This tends to get slow.
* I do this "find ... | xargs" daily, but only while using the shell interactively. But what happends if a filename contains a newline character? Yes, I know "find ... -print0 | xargs -r0", but now "find .. | grep | xargs" does not work any more .... It is dirty and will never get clean.

Even Crontab lines are dangerous. Look at this:

    @weekly . ~/.bashrc && find $TMPDIR -mindepth 1 -maxdepth 1 -mtime +1 -print0 | xargs -r0 rm -rf


Do you spot the big risk? (Solution below)

Avoid calling command line tools
................................

I try to avoid calling a command line tool, if a library is available.

Example: You want to know how long a process is running (with Python). Yes, you could call `ps -p YOUR_PID -o lstart=` with the subprocess library. This works.

But why not use a library like `psutil <https://pypi.python.org/pypi/psutil>`_?

Why do you want to avoid a third party library?

Is there a feeling like "too much work, too complicated"? Installing a library is easy, do it.

Check the license of the library. If it is BSD, MIT or Apache like, then use the library.

Avoid GPL
.........

The GPL license is viral. Avoid it.


Do permission checking via SQL
..............................

Imagine you have three models (users, groups and permissions) as tables in a relational database system.

Most systems do the permission checking via source code. Example: if user.is_admin then return True

Sooner or later you need the reverse: Show all users which have a given permission.

Now you write SQL (or use your ORM) to create a queryset which returns all users which satisfy the needed conditions.

Now you have two implementations. The first "if user.is_admin then return True" and one which uses set operations (SQL).

That's redundant.

I was told to avoid redundancy.


C is slow
.........

... looking at the time you need to get things implemented. Yes, the execution is fast, but the time to get the problem done takes "ages". I avoid it, if possible. If Python/Ruby/... get to slow, you can optimize the hotspots. But do this later. Don't start with the second step. First get it done and write tests. Then optimize.


Version Control
...............

I like git.


Time is too short to run all tests before commit+push
.....................................................

If the guideline of your team is: "Run all tests before commit+push", then there
is something wrong. Time is too short to watch tests running! Run only the tests of the code you touched (py.test -k my_keyword).

It's the job of automated CI (Continuous Integration) to run all tests. That's not your job.


CI
..

Use continuous integration. Only tested code is allowed to get deployed. This needs to be automated. Humans make more errors than automated processes.

I documented how to set up github commit, travis CI, bumpversion, Upload to pypi: https://github.com/guettli/github-travis-bumpversion-pypi

All I need to do is to commit. All other steps are automated :-)

CI must not connect to the internet
...................................

If you do automated testing you usualy have these steps: build then test.

My guideline is to avoid internet access during both steps. During "build" dependencies
get downloaded. Don't download them from the internet. Host your own repos for source code (git),
system packages (rpm/dpkg) and your language (pip for python).




Jenkins
.......

If you use Jenkins or an other GUI for continuous integration be sure to sure to keep it simple. Yes, modern tools like Jenkins can do a lot. With every new version they get even more turing complete (this was a joke, I hope you understood it). Please do speration of concerns. Jenkins is the GUI to start a job. Then the jobs runs, and then you can see the result of the job via Jenkins. If you do complex condition handling "if ... then ... else ..." inside Jenkins, then I think you are on the wrong track.

Jenkins calls a command line. To make it easy for debugging and development this job should be callable via the command line, too. With other word: Jenkins gets used to collect the arguments. Then a command line script gets called. Then Jenkins displays the result for you. I think it is wise to avoid a complex Jenkins setup.

Avoid Threads and Async
.......................

Threads and Async are fascinating. BUT: It's hard to debug. You will need much longer than you initially estimated. Avoid it, if you want to get things done. It's different in your spare time: Do what you want and what is fascinating for you.

Don't waste time doing it "generic and reusable" if you don't need to
.....................................................................

If you are doing some kind of software project for the first time, then focus on getting it done. Don't waste time to do it perfect, reusable, fast or portable. You don't know the needs of the future today. One main goal: Try to make your code easy to understand without comments. First get the basics working, then tests and CI, then listen to the needs, wishes and dreams of your customers.

If you are developing web or server applications, don't waste time for making your code working on Linux and MS-Windows. Focus on Linux.


Use a modern IDE
................

Time for vi and emacs has passed. Use a modern IDE on modern hardware (SSD disk). For example PyCharm. I switched from Emacs to PyCharm in 2016. I used Emacs from 1997 until 2015 (18 years).


Easy to read code: Use guard clauses
....................................

Guard clauses help to avoid indentation. It makes code easier to read and understand. See http://programmers.stackexchange.com/a/101043/129077


Source code generation is a stupid idea
.......................................

I guess every young programmer wants to write a tool which creates software (sooner or later). Stop! Please think about it again. What do you gain? Don't confuse data and code. Imagine you have a source code generator which takes DATA as input and creates SOURCE as output. What is the difference between the input (DATA) and the output (SOURCE)? What do you gain? Even if you have some kind of artificial intelligence, you can't create new (redundancy free) data if your only input is DATA. It is just a different syntax. Why not write a program which reads DATA and does the thing you want to do with SOURCE?

According to wikipedia there are: source code, object code, byte code and machine code. For the current context I see only two different things: source code for humans and machine code for the machine (In this context it does not matter if it is object code, byte code or machine code).

If the TypeScript compiler creates JavaScript. Then the output is machine code (for me in this context) since the created JavaScript source is intended for the interpreter only. Not for the human.

With other words and my point of view: source code gets created by humans with the help of an editor or IDE. It makes no sense to automatically create software. You think it would be great if a robot could create software? Why should a robot create software? It makes no sense. The robot could do the things the created software should do imidiately without the superfluous step of creating source code.



Exception1: If you have some sort of Interface Definition Language like (Corba or Protocol Buffers), then you can create stubs as source code. But this generated source should not contains conditions (IFs) or loops.

Exception2: Compiling to JavaScript. Since there is no better solution available, creating JavaScript from (for example) TypeScript makes sense. But please, never ever edit the created JS :-)

Regex are great - But it's like eating rubish
.............................................

Yes, I like regular expression. But slow down: What do I do, if I use a regex? I think it is "parsing". I remember to have read this some time ago: "Time is too short to rewrite parsers". Don't parse data! We live in the 21 century. Consume high level data structures like json, yaml or protcol buffers. If possible, refuse to accept CSV or custom text format as input data.

From time to time you need to do text processing. Unfortunately there are several regex flavors. My guide-line: Use PCRE. They are available in Python, Postfix and many other tools. Don't waste time with other regex flavors, if PCRE are available.

Current Linux distributions ship with a grep versions which has the `-P` option to enable PCRE. AFAIK this is the only way to grep for special characters like the binary null: `How to grep for special character <https://superuser.com/a/612336/95878>`_ 

CSV - Comma-separated values
............................

CSV is not a data format. It is an illness.

If your customer sends you tabular data in Excel, read the excel directly. Do not convert it to CSV just because you think this is easier.

Use a library like: https://pypi.python.org/pypi/xlrd


Give booleans a "positive" name
...............................

I once gave a DB column the name "failed". It was a boolean indicating if the transmission of data to the next system was successful. The output as table in the GUI looked confusing for humans. The column heading was "failed". What should be visible in the cell for failed rows? Boolean usually get translated to "Yes/No" or "True/False". But if the human brain reads "Yes" or "True" it initially things "all right". But in this case "Yes" meant "Yes, it failed". The next time I will call the column "was_successful", then "Yes" means "Yes, it was successful".

Love your docs
..............

I have seen it several times on github: If I provide a hint that the docs could be improved, a lot of maintainers don't care much. Just look at the README files on github. They starts with "Installing", then "Configuring" ... What is missing? An Introduction! Just some sentences what this great project is all about. Programmers love details. Dear programmers, learn to relax and look at the thing you create like a new comer. If you have this mind set "I do the important (programming) stuff. Someone else can care for the docs", then your open source project won't be successful.

If you write docs, then do it for new comers. Start with the introduction, define the important terms, then provide the simple use cases. Put details and special cases at the end.

Canonical docs
..............

Look at the question concerning ssh options at the Q+A site serverfault. There is a lot of guessing. Something is wrong. Nobody knows where the canonical docs are. Easy linking to specific configuration is not possible. What happens? Redudant docs. Many blog posts try to explain stuff.... Don't write blog posts, improve the upstreams docs. Talk with the developers. Don't be shy.

I am unsure if I should love or hate "wiki.archlinux.org". On the one hand I found there valuable information about systemd and other linux related secrets. On the other hand it is redundant and since a lot of users take their knowledge from this resource, the canonical upstream docs get less love. That's https://en.wikipedia.org/wiki/Ambivalence - that's live.

Care for newcomers
..................

In the year 1997 I was very thankful that there was a hint "If unsure choose ..." when I need to compile a linux kernel. In these days you need to answer dozens question before you could compile the invention of Linus Torvalds.

I had no clue what most questions where about. But this small advice "If unsure choose ..." helped me get it done.

If you are managing a project: Care for newcomers. Provide them with guide lines. But don't reinvent docs. Provide links to the relevant upstream docs, if you just use a piece of software. Avoid redundant docs.

Keep custom IDE configuration small
...................................

Imangine you lost your PC and you lost:

* IDE configuration
* Test data
* Test database

All that's left is your source code from version control, CI servers and deployment workflow.

How much would you lose? How much time would you waste to set up your personal development environment again?

Keep this time small. This is related to "care for newcomers". If you need several hours to setup your development environment, then new team members would need even much more time.


Passing around methods make things hard to debug
................................................

Even in C you can pass around method-pointers. It's very common in JavaScript and sometimes it gets done in Python, too. It is hard to debug. IDE's can't resolve the code: "Find usages" don't work.  I try to avoid it. I prefer OOP (Inheritance) and avoid passing around methods or using them as variables.

Software Design Patterns are overrated
......................................

If you need several pages in a book to explain a software design pattern, then it is too complicated.
I think Software Design Patterns are overrated.



Time is too short for "git rebase" vs "git merge" discussions
.............................................................

What's the net result of "git rebase" vs "git merge" discussion? The result is source code. Who cares how source code got into the current state? Me, but only sometimes. Archeology is interesting .... but more interesting is the future, since you can influence it.


This is untestable code
.......................

If you are new to software unit testing, then you might think ... "some parts of my code are *untestable*".

I don't think so. I guess your software uses the IPO pattern: https://en.wikipedia.org/wiki/IPO_model Input, Processing, Output. The question is: How to feed the input for testing to my code? Mocking, virtualization and automation are your friends.

The "untestable" code needs to be cared of. Code is always testable, there is no untestable code. Maybe your knowledge of testing is limited up to now. Finding untestable code is the beginning of an interesting route to good code.


ForeignKey from code to DB
..........................

This code uses the ORM of django

.. code-block:: python

    if ....:
        issue.responsible_group=Group.objects.get(name='Leaders')

Above code is dirty because 'Leaders' is like ForeignKey from code to a database row.

If you think this is better .....

.. code-block:: python

    if ....:
        issue.responsible_group=Group.objects.get(name=constants_module.GROUP_NAME_OF_LEADERS)

.... then you did not understand what I tried to explain.


Testcode is conditionless
.........................

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
.........................................................................

Imagine you have a huge code base which was written by a nerd which is gone since several months. Somewhere in the code a database row gets updated. This update should not happen, and you can't find the relevant source code line during the first minutes. You can reproduce this failure in a test environment. What can you do? You can start a debugger and jump through the lines which get executed. Yes, this works. But this can take long, it is like "Searching the needle in a haystack". Here is a different way: Add a constraint trigger to your database which fires on the unwanted modification. Execute the code and BANG. you get the relevant code line with a nice stacktrace. This way you get the solution provided on a silver plattern with minimal effort :-)


With other words: Don't waste time with searching.

Sometimes you can't use a database constraint to find the relevant stacktrace, but often there are other ways.....

If you can't use a database constraing, maybe this helps: Raise Exception on unwanted syscall http://stackoverflow.com/a/42669844/633961

If you want to find the line where unwanted output in stdout gets emitted: http://stackoverflow.com/a/43210881/633961

If you have a library which logs a warning, but the warning does not help, since it is missing important information. And you have no clue where this warning comes from. You can use this solution: http://stackoverflow.com/a/43232091/633961

Avoid magic or uncommon things
..............................

* hard links
* file system ACLs (Access control lists). Try to use as little as possible chmod/chown.
* git submodules (Please use configuration management, deployment tools, ...)
* gtk/qt/tk/tkinter: Avoid to write a native GUI. Use Django and its great admin interface.

Learn one programming language, not ten.
........................................


Most young developers think you need to learn many programming languages to be a good developer.

Yes, sometimes it helps to know the programming language C.

My opinion: Learn Python and some JavaScript.

Then learn other topics: Database, Configuration management, continuous integration, organizing, team work, learn to play a music instrument.

Learn "git bisect"
..................

"git bisect" is a great tool to find the commit, which introduced an error. Unfortunately there it is not a one-liner up to now, but you can use it like this:

.. code-block:: shell

    user@host> git bisect start HEAD HEAD~10 


    user@host> git bisect run py.test -k test_something
     ...
    c8bed9b56861ea626833637e11a216555d7e7414 is the first bad commit
    Author: ...


    # useless, but unfortunately needed
    user@host> git bisect reset

Conditional Breakpoints
.......................

Imagine, you are able to reproduce a bug in a test. But you could not fix it up to now. If you want to create a conditional breakpoint to find the root of the problem, then you could be on the wrong track. Why not rewrite the code first, to make it more fine-grained testable?

Write a test where a normal breakpoint is enough.

It is very likely that this means you need to move the body of a loop into a new method.


.. code-block::

    # Old
    def my_method(...):
        for foo in get_foos():
            do_x(foo)
            do_y(foo)
            ...

.. code-block::

    # new
    def my_method(...):
        for foo in get_foos():
            my_method__foo(foo)

    def my_method__foo(foo):
        do_x(foo)
        do_y(foo)
        ...

Now you can call `my_method_foo()` in a test, and you don't need a conditional breakpoint any more.


Make a clear distinction between Authentication and Permission Checks
.....................................................................

It is important to understand the difference.

**Authentication** happens first: Is the user really Bob, or is there just someone who pretends to be Bob?

**Permission Checks** Is Bob allowed to do action "foo"? Here we already trust that the user is Bob and not someone else. I use the term "Permission Checks" on purpuse since the synonym "Authorization" sounds too similar to "Authentication". 


Related question: https://softwareengineering.stackexchange.com/questions/362350/synonym-for-authorization/363690#363690


Idempotence is great
....................

Idempotence is great, since it ensures, that it does not do harm if the method is called twice.

https://en.wikipedia.org/wiki/Idempotence

Further reading: http://docs.celeryproject.org/en/latest/userguide/tasks.html (although I don't use celery any more)

File Locking is deprecated
..........................

In the past `File_Locking <https://en.wikipedia.org/wiki/File_locking>`_ was a very interesting and adventurous topic. Sometimes it worked, sometimes not, and you got interesting edge cases to solve again and again. It was fun. Only hard core experts know the difference between `fcntl`, `flock` and `lockf`.

.... But on the other hand: It's too complicated, too many edge cases, too much wasting time.

There will be chaos if there is no central dispatcher. 

I like http://python-rq.org/ It is simple and robust.

BTW, the topic is called `Synchronization <https://en.wikipedia.org/wiki/Synchronization_(computer_science)>`_.

Further reading about "task queues": https://www.fullstackpython.com/task-queues.html

Test Driven Development
.......................

red, green, refactor. More verbose: make the test fail, make the test pass, refactor (simplify) code.

From bug to fix
...............

Imagine there is a bug in your method do_foo(). You see the mistake easily and you fix it. Done?

I think you are not done yet. I try to follow this guideline:

Before fixing the bug, search test_do_foo(). There is no test for this method up to now? Then write it.

Now you have test_do_foo(). 

You have two choices now: extend test_do_foo() or write test_do_foo__your_special_case(). I use the double underscore here.

Make the test fail (red)

Fix the code. Test is green now.

Slow down. Take a sip of tea. Look at your changes ("git diff" in your preferend IDE). Is there a way to simplify your patch? If yes, simplify it. 

Run the "surrounding tests". If do_foo() is inside the module "bar". Then run all tests for module "bar" (I use py.test -k bar). But if this would take more then three minutes, then leave the testing to the CI which happens after you commit+push (you have a CI, haven't you?)

Then commit+push. Let CI run all tests in background (don't waste time watching your unittests running and passing)

No nested directory trees
.........................

I you store files, then avoid nested directory trees. It is complicated and if you want to use a storage server like `S3 <https://en.wikipedia.org/wiki/Amazon_S3>`_ later, you are in trouble.

Most storage servers support containers and `blobs <https://en.wikipedia.org/wiki/Binary_large_object>`_ inside a container. Containers in containers are not supported, and that's good, since it is simple.


Debugging Performance
.....................

I use two ways to debug slow performance:

 * Logging and profiling, if you have a particular reproducable use case
 * Statistics collected on production environments. I use my own tool up to now: https://github.com/guettli/live-trace

You provide the GUI for configuring the system. Then the customer (not you) uses this GUI
.........................................................................................

I developed a workflow system for a customer. The customer gave me an excel sheet with steps, transitions and groups.

The coding was the difficult part.

Then I configured the system according to the excel sheet.

The code was bug free, but I made a mistake when I entered the values (from excel to the new web based workflow GUI).

The customer was upset, because the configuration contained mistakes.

I learned. Now I ask if it would be ok if the customer enters the mapping. In most cases the customer likes to do this. 

There is a big difference. The customer feels productive if he does something like this. I hate it. I care for the database design and the code, but entering data with copy+paste from the Excel sheet ... No I don't like this. Results will be better if you like what you do :-)

For detail lovers: No, it was not feasible to write a script which imported the excel sheet to the database. The excel sheet was not well structured.


Avoid clever guessing
.....................

These days I needed to debug a well known Python library. It works fine, but you don't want to look under hood.

One method accepted a object with three different meanings types as first argument:

* case1: a string containing html markup
* case2: a string containing a file path. This file contained the html to work on.
* case3: a file descriptor with a read() method.
 
This looks convinient at the first sight. But in the long run it makes things complicated. This kind of guessing can always lead to false results. In my case the string was a accidently the name of an existing directory. In my case all calls to the library used case1 "a string containing html markup". This failed because of the existing directory :-(

STOP GUESSING.

In Python you can use classmethods for alternative constructors.


.. code-block::

  # case 1
  obj = MyClass.from_string('.....')

  # case2
  obj = MyClass.from_file_name('/tmp/...')

  # case3
  with io.open('...') as fd:
      obj = MyClass.from_file_object(fd)


Misc
....

* `<https://en.wikipedia.org/wiki/Release_early,_release_often> Release early, release often`_


####################################################################################################

4. Op
-----

Operation. The last two characters of DevOp.

Configuration Management
........................

Use a configuration management tool like Salt or Ansible. 

Use CI here, too. Otherwise only few people dare to make changes. And this means the speed of incremental evolution to a more efficent way will decreases.

Do not use RPM/DPKG to configure a system.


Change file vs put file
.......................

Often there are two ways to do configuration management:


* change a part of a file: `replace <https://docs.saltstack.com/en/latest/ref/states/all/salt.states.file.html#salt.states.file.replace>`_ 
* put a whole file: `Manage file <https://docs.saltstack.com/en/latest/ref/states/all/salt.states.file.html#salt.states.file.managed>`_
 
You have far less trouble if you use "put a whole file". Example: Do not fiddle with the file `/etc/sudoers`. Put a whole file into `/etc/sudoers.d/`.



Use http, avoid ftp/sftp/scp/rsync/smb
......................................

Use http for data transfer. Avoid the old ways (ftp/sftp/scp/rsync/smb). 

If you want to transfer files via http from shell/cron you can use: `tbzuploader <https://github.com/guettli/tbzuploader>`_.

The next step is to avoid clever `inotify <https://en.wikipedia.org/wiki/Inotify>`_-daemons. You don't need this any more if you receive your data via http.




Provide specific import directories, not one generic
....................................................

If you still receive files via ftp/scp since you have not switched to http-APIs yet, then be sure to provide specific input directories.

In the past I recevied files in a directory called "import". Several third party systems sent data to this directory. It looks easy in the first place. But sooner or later there will be chaos since you need to now where the data came from. Was it from third party system FOO or was the data from third party system BAR? You can't distinguish any more if you profide only one import directory.

Now we provide import-FOO, import-BAR, import-qwerty ...

Cron Jobs
.........

A server exists to serve. If the server does not receive requests, why should the server do something? This results into my rule of thumb: Avoid cron jobs.

Sometimes you need to have a cron job for house keeping stuff.

Keep cron jobs simple. 

In general there are two ways to configure the arguments of a cron job:

* the command line arguments which are part of the crontab line
* additional source of configuration: config files or config from a database

Avoid mixing these two ways of configuring a cron job. I prefer to configure the cron job via the later of both ways. This keeps the cron job simple. My guide line: Do not configure the cron job via optional command line arguments. Only use required arguments. 


SSH to production-server
........................

I still do interactive logins to production remote-server (mostly via ssh). But I want to reduce it. 


Sooner or later you will make a typo. See this article from github for a exciting report what happened during a denial of service: https://about.gitlab.com/2017/02/01/gitlab-dot-com-database-incident/ We are humans, and humans make mistakes. Automation helps to reduce the risk of data loss.


If you are doing "ssh production-server ... vi /etc/..." or "... apt install": Configuration management is much better. For example salt-stack or ansible.

If you are doing "ssh production-server .... less /var/log/...": No log-management yet? Get your logs to a central place.

If you are doing "ssh production-server ... rm ...": Please ask yourself what you are doing here. How can you automate this, to make this unneccessary in the future. 

Keep you country clean
......................

There are two kind of files in the context of backup: Files which should be in the backup and temporary files which should not be in the backup. Keep you directories clean. In a directory there should be either only files which should be in the backup xor only files which should not be in the backup. This will make live easier for you. The configuration of your backup is easier and cleaning temporary files is easier and looking at the directory makes more fun since it is clean.


Logging to files
................

I still do this, but I want to reduce it. Logs are endless streams. Files are a buch of bytes with fixed length.
Both concepts don't fit together. Sooner or later your logs get rotated. Now you are in trouble if you want to run a log checker for every line in your logfile. I mean the mathematically version of "every line". This gets really complicated if you want to check every line. Rotating logfiles needs to be done sooner or later. But how to rotate the file, if a process still write to it? This is one problem, which was solved several hundred times and each time different ...

In other words: Avoid logrotate. Logging is an endless stream.

Use Systemd
...........

It is available, don't reinvent. Don't do double-fork magic any more. Use a systemd service with Type=simple. See `Systemd makes many daemons obsolete <https://stackoverflow.com/a/30189540/633961>`_

traceroute won't help you
.........................

.... if you have trouble with a tcp connection. Use tcptraceroute for tcp connection tests (http, https, ssh, smtp, pop3, imap, ...). Reason: traceroute uses UDP, not TCP.


If you do coding to implement backup ...
........................................

If you do coding/programming to implement your backup of data, then you are on the wrong track.

It is very likely that you will do it wrong, and this will be a big risk.

Why? Because you will notice your fault if you try to recover your data. 

**Use** a backup tool, even if you love to do programming. Configure it, but don't write it yourself.

Same for data replication. Do not write code to do this. Use existing tools.

Don't set up a SMTP daemon
..........................

If you can avoid it, then refuse to set up a SMTP daemon. If the application you write should import mails, then do it by using POP3 or IMAP. Use a tool like getmail (not fetchmail) which is a mail fetching client. You will have much more trouble if you set up an SMTP daemon.

5. Monitoring
----------------------------

Nagios Plugin API (0=ok, 1=warn ...)
....................................

Writing Nagios checks is very simple. The exit status has this meaning:

* 0: ok
* 1: warn
* 2: error
* 3: unknown

Is this KISS (keep it simple and stupid)? Yes, I think it is **simple**. You can write a nagios plugin with any language you like. Often less then ten lines of source code are enough to implement a nagios check.

But on the other hand it is not **stupid**. The checks does two things: It collects some numbers (for example "How much disk space is left") and it does evaluate and judge ("only N MByte left, I think this is a warning"). That's not stupid this is some kind of intelligence. 

After writing and working with nagios checks for several years I think the evaluation of the data should not be done inside the check. Some data-collector should collect data. Then a different tool should evaluate the data and judge if this ok, warn or error.






####################################################################################################


6. Communication with others
----------------------------

Avoid to get a nerd
...................


If you do "talk"  with software to databases and APIs daily, your ability to communicate with humans might decrease.

You might start to think like a computer (at least a bit). 

The human mind works completly different, not just bits and bytes. It has `Emotions <https://en.wikipedia.org/wiki/Emotion>`_

Avoid to get a `Nerd https://en.wikipedia.org/wiki/Nerd`

Here some hints:

* I like `Nonviolent Communication <https://en.wikipedia.org/wiki/Nonviolent_Communication#Four_components>`_ (In short, use this sequence: Facts, feelings, needs, request)
* Meet with "normal" people. With "normal" I mean people who do not do IT stuff.
* Do sport
* Relax
* Do not complain. Do something if you can. If you can't, then talk to friends. If they can't help, then cry and then seek new adventures.

Discussion, but no progress? V1, V2, V3, ...
............................................

If a discussion brings not progress, then grab a pen. Start with V1. The letter V stands for "Solution Variant" or "One strategy of several to get to a goal". Find a term or short description of the first possible strategy. Write it down. Then: which other ways could be used? V2, V3, ... 

Rember, there is always the last variant: Leave things like they are today and think about this again N days later.

If you have found several solution variants, then look at them in detail. Most of the time it is useful to define the need sequence of steps. You can use the letter "S" for this: S1, S2, S3 ...

A simple example:

In the morning, you wake up.

* V1: Go to work now
* V2: Do some more sleeping
* V3: Care for your family
* V4: Try to remember what you dreamed, write it down
* V5: Do some sports
* V6: Play piano
* V7: Remember your goals, what is the next step?
* ...

If you look at V1 in detail you get to a list of steps:

* S1: get up
* S2: make bed
* S3: wash yourself
* S4: put on clothings
* S5: eat
* S6: take bike and ride to work

I think the first letter (V, S) helps if you are brainstorming.


Avoid Office Documents or UML-tools
...................................

Use a way to edit content (use cases, specs, ...) over the internet: Use wikis. Don't waste time with UML tools. Write down the high level use case, the cardinality and the steps. Sequence diagrams are not needed. Just: first, second, third ...

`Sketch <https://en.wikipedia.org/wiki/Sketch_(drawing)>`_ screenshots you want to build with your team with a pen. I avoid any digital device for this, since up to now paper or a whiteboard are far more real. If you need the result in digital format, just take a picture with you cell phone at the end.


Communication with Customers: Tell customers what they should test
..................................................................

I have seen it several times: Software gets developed. The customer was told to test and ... nothing happens. That's not satisfying since software developers want to hear that their work does help. If you (the developer) provide a checklist of things to test, then the likelihood to get feedback is bigger.

It is wise to create this checklist for testing as early as possible. It tells the developer the desired result.


Communication with Customers: Define "done"
...........................................

Define "done" with your customers. Humans like to be creative and if thing X gets changed, then they have fancy ideas how to change thing Y. Be friendly and listen: Write these fancy ideas down on the "do later" list or wiki page. If you don't have a definition of done/ready, then you should not start to write source code. First define the goal, then choose a strategy to get to the goal.

Dare to say "Please wait, I want to take a note"
................................................

Most people can listen and write at once. I can't. And I guess a lot of programmers have this problem. I can only do one thing at a time. If you are telephoning with a customer and he has a lot of things to tell you, don't fool yourself. You will only remember 4 of 5 issues. Dare to day "please wait, I want to take a note". This way you can care for all issues, which results in happy customers.

Avoid Gossip
............

Gossip creates an atmosphere which promotes negativity (bad karma). Avoid to make jokes about other team mates or customers. Yes, there are people who do strange stuff and who have strange attitudes. Making jokes about them makes everything worse. Please be aware that this guideline has a major drawback. Sometimes all people around you are laughing about a customer or a team mate which is not here right now ... and you are the only one who is not laughing. It is up to you how to react. Be patient.



####################################################################################################

7. Epilog
---------

It is always possible to make things more complicated
.....................................................


It is always possible to make things more complicated. The interesting adventure is to make things simpler and easier. 


Be curious
..........

There is always something you don't have understood up to now. Ask questions, even if you think you now the answer. For one question, there are always several answers. If you know one answer, then it is likely that someone has a better answer. 

I like:

* https://stackoverflow.com/
* https://softwarerecs.stackexchange.com/
* https://serverfault.com/
* And some mailing lists.

Read the Release Notes
......................

I like these release notes:

* https://www.postgresql.org/docs/devel/static/release.html The "Overview" links show the most important changes
* https://docs.djangoproject.com/en/dev/releases/
* Python ... no, since I am still using Python 2.7


Three Mail Accounts
...................

I have three mail accounts:

* for personal mails (family, friends, ...)
* for work related mails
* for mailing lists


Clean up your desk
..................

Don't forget to clean your desk. I don't write this here because I do it often and with joy. No, excat the opposite. I write it down since I want to push myself. 

Don't look at all these things on your desk at once. Start on the left side take the first thing. Where is the best place for this thing single thing? Unsure? Why not throw in the garbage? If you are unsure put it at least in box behind a closed cabinet door. Some month later you might be able to throw it in the garbage.

Then wipe the dust.

If you have not time do this, then there is something wrong.

"Wer es eilig hat sollte sich setzen"

"Wer keinen Zeit hat ist ärmer als ein Bettler" TODO translate this.

Highlander, "There can be only one"
...................................

Highlander is a 1986 British-American adventure action fantasy film with tagline "There can be only one".
Thinking like this narrows your mind. There can be several thousand. Look how successfull ants and bees work. If someone is better or faster, then smile. Give applaud and say "wow".

`Don't be evil. <https://en.wikipedia.org/wiki/Don%27t_be_evil>`_ Don't waste time and mental energie. Applauding if the competitor is better, was new to me in 2017. I was at Rothenbaum and attended the German Open (Tennis). The coach of one player was applauding every time the opponent made a good shot. I was astonished. Why was the coach applauding the enemy? But this works. If you get angry, you waste energy and you start to think like a wild and stupid animal. Even if you have made a mistake or lost some how, no reason not to walk upright.


Don't waste your time with cheap hardeware
..........................................

Some people love the `Raspberry Pi <https://en.wikipedia.org/wiki/Raspberry_Pi>`_. I don't like it. It does not have enough computing power for my use cases. Yes, the device is cheap, but I prefer to spend some more money to have more performance. I don't like waiting.


Solutions
.........

* Big risk of "find $TMPDIR": If the variable $TMPDIR  is not set, then the `find` command does scan and delete all directories! 

Thank you
.........

* Robert C. Martin for the book "Clean Coder"
* Malcolm Tredinnick. (His quote "knows enough about stuff to be dangerous" and his sudden death opens a universe of phantasies for paranoid people). Only few people listened like he did. With "listen" I mean "trying to understand the conversation partner".
* Linus Torvalds for the quote "Bad programmers worry about the code. Good programmers worry about data structures and their relationships."
* Bill Gates for the quote "I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it." 
* All people who contribute to open source software (Linux, Python, PostgreSQL, ...)
* All people who ask question and/or answers them at places like StackOverflow.
* People I meat during study at HTW-Dresden
* My teammates at TBZ

.. Link in ReST: `text <http:....>`_
