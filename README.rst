Programming Guidelines
======================

My personal programming guidelines. **Please provide feedback**, tell me what's wrong and what's missing and create a new issue here: https://github.com/guettli/programming-guidelines/issues or write me a mail to guettliml@thomas-guettler.de

`1. Introduction <#1-introduction>`_

`2. Data structures <#2-data-structures>`_

`3. Dev <#3-dev>`_

`4. Remote APIs <#4-remote-apis>`_

`5. Op <#5-op>`_

`6. Networking <#6-networking>`_

`7. Monitoring <#7-monitoring>`_

`8. Communication with others <#8-communication-with-others>`_

`9. Epilog <#9-epilog>`_


1. Introduction
---------------

About this README
.................

I was born 1976. I started coding with basic and assembler when I was 13. Later turbo pascal. From 1996-2001 I studied computer science at HTW-Dresden (Germany). I learned Shell, Perl, Prolog, C, C++, Java, PHP and finally Python.


Sometimes I see young and talented programmers wasting time. There are two ways to learn: Make mistakes yourself, or read from the mistakes which were done by other people. 

This list summarises a lot of mistakes I did in the past. I wrote it, to help you, to avoid these mistakes.

It's my personal opinion and feeling. No facts, no single truth.

Relaxed focus on monitor
........................

Do not look at the keyboard while you type. Have a relaxed focus on your monitor.

I type with ten fingers. It's like flying if learned it. Your eyes can stay on the rubbish you type, and you don't need to move your eyes down (to keyboard) and up (to monitor) several hundred times per day. This saves a lot of energy. Avoid to switch between mouse and keyboard too much. 

I like lenovo keyboards with track point. If you want the track point to have more grip you can use sandpaper. Here are some images to illustrate what I use https://plus.google.com/108148237674350536526/posts/jF1Du1YwJwr?hl=de

Once I was fascinated by the copy+paste history of Emacs and PyCharm. But then I thought to myself: "I want more. 
I am hungry. I want a copy+paste history not only in one application, I want it for the whole desktop". The solution is
very simple, but somehow only few people use it. The solution is called clipboard manager. I use Diodon (Linux) and
CopyQ (for Windows). I use ctrl+alt+v to open the list of last copy+paste texts.

Avoid searching with your eyes. Search with the tools of your IDE. You should be able to use it "blind". You should be able to move the cursor to the matching position in your code without looking at your keyboard, without grabbing your mouse/touchpad/trackpoint and without looking up/down on your screen.



KISS
....

Keep it simple and stupid. The most boring and most obvious solution is often the best. Although it sometimes takes months until you know which solution it is.

From the book "Site Reliability Engineering" (O'Reilly Media 2016) https://landing.google.com/sre/book/chapters/simplicity.html

Quote:
 The Virtue of Boring 
 
 Unlike just about everything else in life, "boring" is actually a positive attribute when it comes to software! We don’t want our programs to be spontaneous and interesting; we want them to stick to the script and predictably accomplish their business goals.

Avoid redundancy
................

See heading.


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

If this is new to you, I will give you two examples:

One invoice has several invoice positions. For example you buy three books in one order,
the invoice will have three invoice positions. This is a 1:N relationship. The invoice position is
contained in exactly one invoice.

If you look at tags, for example at the Question+Answer site stackoverflow: One question can be related to several
tags/topics and of course a topic can be set on several questions. For example you have a strange UnicodeError in Python
then you can set the tags "python" and "unicode" on your question. This is an N:M relationship.

One more well know example of N:M is user and groups.


Conditionless Data Structures
.............................

If you have no conditions in your data structures, then the coding for the input/output of your data will be much easier.

Avoid nullable Foreign Keys
...........................

Imagine you have a table "meeting" and a table "place". The table "meeting" has a ForeignKey to table "place". In the beginning it might be not clear yet where the meeting will be. Most developers will make the ForeignKey optional (nullable). WAIT: This will create a condition in your data structure. There is a way easier solution: Create a place called "unknown". Use this as default, avoid nullable columns. This data structure (without a nullable ForeignKey) makes implementing the GUI much easier.

With other words: If there is no NULL in your data, then there will be no NullPointerException in your source code while processing the data :-)

Less conditions, less bugs.

Avoid nullable boolean columns
..............................

[True, False, Unknown] is not a nullable Bollean Column.

If you want to store a data in a SQL database which has three states (True, False, Unknown), then you might think a nullable boolean column (here "my_column") is the right choice. But I think it is not. Do you think the SQL statement "select * from my_table where my_column = %s" works? No, it won't work since "select * from my_table where my_column = NULL" will never ever return a single line. If you don't believe me, read: `Effect of NULL in WHERE clauses (Wikipedia) <https://en.wikipedia.org/wiki/Null_(SQL)#Effect_of_Unknown_in_WHERE_clauses>`_. If you like typing, you can work-around this in your application, but I prefer straight forward solutions with only few conditions.

If you want to store True, False, Unknown: Use text, integer or a new table and a foreign key.

Avoid nullable characters columns
.................................

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

Database transactions are atomic. If the transaction was sucessful, then it is \*D\*urable.

Imagine you have one outer-transaction, and two inner transaction.

#. Transaction OUTER starts
#. Transaction INNER1 starts
#. Transaction INNER1 commits
#. Transaction INNER2 starts
#. Transaction INNER2 raises an exception.

Is the result of INNER1 durable or not?

My conclusion: Transactions do not nest

Related: http://stackoverflow.com/questions/39719567/not-nesting-version-of-atomic-in-django



Roles vs Users+Groups
.....................

What is the difference between roles and groups?

There are several verbose and philosophical explanations. I like the way PostgreSQL handles it.

There is no more a distinction between a user and a group. A role can contain a role and a user is a role.

This makes some things easier and I whish I had choosen the role model and not the user+group model which
gets used by Django's auth app.

Imagine you have an issue tracking system. If you have the user+group model and you want to give the responsibility
of this issue to someone. You need two foreign keys if you use the user+group model: You can give the responsibility
to a particular user or you can give the responsibility to a group. Two FKs.

If you use the role model, then one FK is enough. Easier data structure, easier interface, less code, less bugs ...


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

Take a look at virtualization and containers (`Operating-system-level virtualization <https://en.wikipedia.org/wiki/Operating-system-level_virtualization>`_). There CRD gets used, not CRUD. Containers get created, then they execute, then they get deleted. You might use configuration management to set up a container. But this gets done exactly once. There is one update from vanilla container to your custom container. But this is like "create". No updates will follow once the container was created. This makes it easier and more predictable.



No Shell Scripting
..................

The shell is nice for interactive usage. But shell scripts are unreliable: Most scripts fail if filenames contain whitespaces. Shell-Gurus know how to work around this. But quoting can get really complicated. I use the shell for interactive stuff daily. But I stopped writing shell scripts.

Reasons:

* If a error happens in a shell script, the interpreter steps silently to the next line. Yes I know you can use "set -e". But  you don't get a stacktrace. Without stacktrace you waste a lot of time to analyze why this error happened.
* AFAIK you can't do object oriented programming in a shell. I like inheritance.
* AFAIK you can't raise exceptions in shell scripts.
* Shell-Scripts tend to call a lot of subprocesses. Every call to grep,head,tail,cut  creates a new process. This tends to get slow.
* I do this "find ... | xargs" daily, but only while using the shell interactively. But what happends if a filename contains a newline character? Yes, I know "find ... -print0 | xargs -r0", but now "find .. | grep | xargs" does not work any more .... It is dirty and will never get clean.
* Look at all the pitfalls: ` Bash Pitfalls <https://mywiki.wooledge.org/BashPitfalls>`_ My conclusion: I prefer to walk on solid ground, I don't write shell scripts any more.

Even Crontab lines are dangerous. Look at this:

    @weekly . ~/.bashrc && find $TMPDIR -mindepth 1 -maxdepth 1 -mtime +1 -print0 | xargs -r0 rm -rf


Do you spot the big risk? If TMPDIR is not set, then the `find` command will not fail. It will delete files in all sub directories!

Portable Shell Scripts
......................

I think writing portable shell scripts and avoiding bashism (shell scripts which use features which are only available in the bash) is a useless goal. It is wasting time. It feels productive, but it is not.

I think there are two environments: You either have /bin/bash, then use it. Or you are in an embedded environment where only a simple busybox shell is available. But in most cases /bin/bash is available - use it.

If I look at this page, which explains how to port shell scripts to /bin/dash I would like to laugh, but I can't because I think it is sad that young and talented people waste their precious time which this nonsense.

If you are not able to create a dependency to bash, then solve this issue. Use rpm/dpkg or configuration management to handle "my script foo.sh needs bash".

https://wiki.ubuntu.com/DashAsBinSh

I know that there are some edge cases where the bash is not available, but in most cases the time to get things done is far more important. Execution performance is not that important. First: get it done including automated tests.

Server without a shell is possible
..................................

In the past, it was unbelievable: A unix/linux server which does not execute a shell while doing its daily work.
The dream is true today.
These steps do not need a shell: operating system boots. Systemd starts. Systemd spawn daemons. For example a web
server. The web server spawns worker processes. A http request comes in and the worker process handles one web request
after the other. In the past the boot process and the start/stop scripts were shell scripts. I am very happy that
systemd exists.

 



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

The GPL license is much too long. I tried to read it twice, but I felt asleep. 
I don't like things which I don't understand.

Next argument: The GPL license is viral.

Avoid the GPL.

Loop in DB, not in your code
............................

Do the filtering in the database. In most cases it is faster then the
loops in your programming language. And if the DB is not fast enough,
then I guess there is just the matching index missing up to now.



Do permission checking via SQL
..............................

Imagine you have three models (users, groups and permissions) as tables in a relational database system.

Most systems do the permission checking via source code. Example: if user.is_admin then return True

Sooner or later you need the reverse: Show all users which have a given permission.

Now you write SQL (or use your ORM) to create a queryset which returns all users which satisfy the needed conditions.

Now you have two implementations. The first "if user.is_admin then return True" and one which uses set operations (SQL).

That's redundant.

I was told to avoid redundancy.

SQL is an API
.............

If you have an database driven application and a third party tool wants to send data to the application, then sometimes the easiest solution is to give the third party access to the database. 

Nitpickers will disagree: If the database schema changes, then the communication between both systems will break. Of course that's true. But in most cases this will be the same if you use a "real" API. If there is a change to the data structure, then the API needs to be changed, too.

I don't say that SQL is always the best solution. Of course http based APIs are better in general. But in some use cases doing more is not needed.

C is slow
.........

... looking at the time you need to get things implemented. Yes, the execution is fast, but the time to get the problem done takes "ages". I avoid C programming, if possible. If Python gets to slow, I can optimize the hotspots. But do this later. Don't start with the second step. First get it done and write tests. Then clean up the code (simplify it). Then optimize.


Version Control
...............

I like git.

Avoid long living branches
..........................

Avoid long living branches in your git repos. The
more time that passes, the less likely is that your work will ever get merged. For me two weeks are ok, but five weeks are too long.

Not one branch per customer
...........................

Some people use git branches to store the individual settings for customers or installations. Don't do this.
Create one git repo for every customer or installation.


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

My guideline (for commercial, closed source software) is to avoid internet access during both steps. During "build" dependencies get downloaded. Don't download them from the internet. Host your own repos for source code (git),
system packages (rpm/dpkg) and your language (pip for python).




Jenkins
.......

If you use Jenkins or an other GUI for continuous integration be sure to sure to keep it simple. Yes, modern tools like Jenkins can do a lot. With every new version they get even more turing complete (this was a joke, I hope you understood it). Please do speration of concerns. Jenkins is the GUI to start a job. Then the jobs runs, and then you can see the result of the job via Jenkins. If you do complex condition handling "if ... then ... else ..." inside Jenkins, then I think you are on the wrong track.

Jenkins calls a command line. To make it easy for debugging and development this job should be callable via the command line, too. With other word: Jenkins gets used to collect the arguments. Then a command line script gets called. Then Jenkins displays the result for you. I think it is wise to avoid a complex Jenkins setup. If you want to switch to a different tool (gitlab or travis), then this is easy if your logic is in scripts and not in jenkins configuration.

Avoid Threads and Async
.......................

Threads and Async are fascinating. BUT: It's hard to debug. You will need much longer than you initially estimated. Avoid it, if you want to get things done. It's different in your spare time: Do what you want and what is fascinating for you.

There is one tool and one concept that is rock solid, well known, easy to debug and available everywhere and it is great for parallel execution. The tool is called "operating system" and the concept is called "process". Why re-invent it? You think starting a new process is "expensive" ("it is too slow")? Just, do not start a new process for every small method you want to call in parallel. Use a `Task Queue <https://www.fullstackpython.com/task-queues.html>`_.
Let this tool handle the complicated async stuff and keep your own code simple like running in one process with one thread. It is all about IPO: Input-Processing-Output.

Don't waste time doing it "generic and reusable" if you don't need to
.....................................................................

If you are doing some kind of software project for the first time, then focus on getting it done. Don't waste time to do it perfect, reusable, fast or portable. You don't know the needs of the future today. One main goal: Try to make your code easy to understand without comments and make the customer happy. First get the basics working, then tests and CI, then listen to the new needs, wishes and dreams of your customers.

If you are developing web or server applications, don't waste time for making your code working on Linux and MS-Windows. Focus on Linux.


Use a modern IDE
................

Time for vi and emacs has passed. Use a modern IDE on modern hardware (SSD disk). For example PyCharm. I switched from Emacs to PyCharm in 2016. I used Emacs from 1997 until 2015 (18 years).


Easy to read code: Use guard clauses
....................................

Guard clauses help to avoid indentation. It makes code easier to read and understand. See http://programmers.stackexchange.com/a/101043/129077

Example::

    def my_method(my_model_instance):
        if my_model_instance.is_active:
            if my_model_instance.number > MyModel.MAX_NUMBER:
                if my_model_instance.foo:
                    ....
                    ....
                    ....
                    ....
                    ....
                    

    def my_method(my_model_instance):
        if not my_model_instance.is_active:
            return
        if not my_model_instance.number > MyModel.MAX_NUMBER:
            return
        if not my_model_instance.foo:
            return
        ....
        ....
        ....
        ....
        ....

Look at the actual code which does something. I used five lines with `....` points for it. I think more indendation, makes the code more complex. The "return" simplifies the code. For me the second version is much easier to read.
         

Source code generation is a stupid idea
.......................................

I guess every young programmer wants to write a tool which automatically creates source code.
Stop! Please think about it again. What do you gain?
Don't confuse data and code.
Imagine you have a source code generator which takes DATA as input and creates SOURCE as output.
What is the difference between the input (DATA) and the output (SOURCE)? What do you gain?
Even if you have some kind of artificial intelligence, you can't create new information if
your only input is DATA. It is just a different syntax.
Why not write a program which reads DATA and does the thing you want to do?

For the current context I see only two different things: **source code** for humans and
**generated code** for the machine.

If the TypeScript compiler creates JavaScript. Then the output is generated code
since the created JavaScript source is intended for the interpreter only. Not for the human.

With other words: source code gets created by humans
with the help of an editor or IDE.



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

I once gave a DB column the name "failed". It was a boolean indicating if the transmission of data to the next system was successful. The output as table in the GUI looked confusing for humans. The column heading was "failed". What should be visible in the cell for failed rows? Boolean usually get translated to "Yes/No" or "True/False". But if the human brain reads "Yes" or "True" it initially things "all right". But in this case "Yes" meant "Yes, it failed". The next time I will call the column "was_successful", then "Yes" means "Yes, it was successful". Some GUI toolkits render "True" as a green (meaning "everything is ok") hook and "False" as a red cross (meaning "it failed"). 

Love your docs
..............

I have seen it several times on github: If I provide a hint that the docs could be improved, a lot of maintainers don't care much. Just look at the README files on github. They starts with "Installing", then "Configuring" ... What is missing? An Introduction! Just some sentences what this great project is all about. Programmers love details. Dear programmers, learn to relax and look at the thing you create like a new comer. If you have this mind set "I do the important (programming) stuff. Someone else can care for the docs", then your open source project won't be successful.

If you write docs, then do it for new comers. Start with the introduction, define the important terms, then provide the simple use cases. Put details and special cases at the end.

If you write broken software, you will get feedback soon.

Tests fail or even worse customers will complain.

But if you write broken docs, no one will complain.

Even if someone reads your mistake, it is unlikely
that you get feedback. Unfortunately only few people take this serious and tell 
you that there is a mistake in your docs.


How to solve this?


Let someone else read your docs.

The quality of feedback you get depends on the type
of person you ask to read your docs.

If it is a programmer, it is likely that he does not read
your docs carefully. Most software developers do not
care for orthography and it is hard for them to read
the docs like a new comer. They already know
what's writen there, and they will say "it is ok".

My solution: resubmission: Read the text again 30 days later.

Canonical docs
..............

Look at the question concerning ssh options at the Q+A site serverfault. There is a lot of guessing. Something is wrong. Nobody knows where the canonical docs are. Easy linking to specific configuration is not possible. What happens? Redudant docs. Many blog posts try to explain stuff.... Don't write blog posts, improve the upstreams docs. Talk with the developers. Open an issue in the issue tracker if you think there is something missing in the docs. 

Open an issue if the docs start with the hairy details and don't start with an introduction/overview. Developers don't realize this, since they need to deal with the hairy details daily. Don't be shy: Help them to see the world through the eyes of a new comer.

I am unsure if I should love or hate "wiki.archlinux.org". On the one hand I found there valuable information about systemd and other linux related secrets. On the other hand it is redundant and since a lot of users take their knowledge from this resource, the canonical upstream docs get less love. First determine where the canonical upstream docs are. Then communicate with the maintainers. Avoid redundant docs.

Do not send long instructions to customers via mail
...................................................

If you send long instructions to customers via mail, then these docs in the mail are hidden magic. 
Only the customer who receives this mail knows the hidden magic.


Publish your docs in your app.
Send your customer a link to the online docs.

Despite all myth: There are users who read the docs.

And that's great, if the user has more knowledge.
Because this means you have less work. Less mails, less interrupts, 
less phone calls :-)


Care for newcomers
..................

In the year 1997 I was very thankful that there was a hint "If unsure choose ..." when I needed to compile a linux kernel. In these days you need to answer dozens question before you could compile the invention of Linus Torvalds.

I had no clue what most questions where about. But this small advice "If unsure choose ..." helped me get it done.

If you are managing a project: Care for new comers. Provide them with guide lines. But don't reinvent docs. Provide links to the relevant upstream docs, if you just use a piece of software.

Keep custom IDE configuration small
...................................

Imangine you lost your PC and you lost your development environment:

* IDE configuration
* Test data
* Test database

All that's left is your source code from version control, CI servers and deployment workflow.

How much would you lose? How much time would you waste to set up your personal development environment again?

Keep this time small. This is related to "care for new comers". If you need several hours to setup your development environment, then new team members would need even much more time.

Setting up a new development environment should be easy
.......................................................

This happened to me several times: I wanted to improve some software. Up to now I only used the software,
now I want to write a patch. If setting up a new development environment is too complicated or not documented,
then I will resign and won't provide a patch. These steps need to be simple for people starting from scratch:

* check out source from version control
* check that all tests are working (before modifying something)
* write patch and write test for patch
* check that all tests are working (after modifying something)




Passing around methods make things hard to debug
................................................

Even in C you can pass around method-pointers. It's very common in JavaScript and sometimes it gets done in Python, too. It is hard to debug. IDE's can't resolve the code: "Find usages" don't work.  I try to avoid it. I prefer OOP (Inheritance) and avoid passing around methods or treating them like variables.

I don't like `Closures (wikipedia) <https://en.wikipedia.org/wiki/Closure_(computer_programming)>`_

I like it simple: Input-Processing-Output.

Software Design Patterns are overrated
......................................

If you need several pages in a book to explain a software design pattern, then it is too complicated.
I think Software Design Patterns are overrated.

Why are so many books about software design patterns and nearly no books about database design patterns?

Time is too short for "git rebase" vs "git merge" discussions
.............................................................

What's the net result of "git rebase" vs "git merge" discussion? The result is source code. Who cares how source code got into the current state? Me, but only sometimes. Archeology is interesting .... but more interesting is the future, since you can influence it.

I hardly ever look at the graph of a git repository. But I love the "History for selection" feature of my favorite IDE. This way I can see the history of a part of the whole source code file.


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



For every method there is a corresponding test-method
.....................................................

You implemented the great method foo() and you implement a corresponding method called test_foo().
It does not matter if you write foo() first, and then test_foo() or the other way round.
But it makes sense to store both methods with one commit to one git repo.

Several months later you discover a bug in your code. Or worse: your customer discovers it.

If you fix foo() you need to extend test_foo() or write a new method test_foo_with_special_input(). Again both changes (production code and testing code) walk into the git repo like a pair of young lovers holding hands :-)

Testing: Checking if a method was called or not makes no sense
..............................................................

If you are testing something, then remeber it is all about: Input-Processing-Output.

If your input is x you might want the output to be y.

For me it does not make any sense to use the method  
`assert_called() <https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_called>`_
or the other assert_called_xxx() methods.

You treat your code like a black box. You provide input, and you check the output. If something was refactored and the
method was completely re-written from scratch, then your test still will work: Same input, same output. 

If a helper-method was called during the processing (which some people check with assert_called_xxx()).... who cares? I don't, as long as the desired output gets created.

Creating test data is much more important than you initial think
................................................................

Creating test data is very important. It can help you for several things:

1: It can help you to create a re-usable application: Imagine you have one customer in the beginning. You do everything the way the customer wants it to be. But the real benefit of software is its re-usabilty. Your code wants to get executed in different environments, for more than one customer.

2: It can help you to create presentation/demo systems

3: It can help you in automated tests.

Your tests should not run on real data from customers.

If you create test data this should be automated. This way you are able to fill a new database with useful data.
You should be able to create a demo system with one command (or one click).

Write the creation of test data once and use it for both: presentions and automated tests.

Do not use random data for testing. It just makes no sense: tests should be reproducable.

If your application is multi-tenant (support multible clients), then you need a demo tenant. All automated tests should use this tenant.

I don't see why a special library for creating test data is needed. If you use an ORM in your production code, then use the ORM to create your test data.

In Python/Django I use cached-properties and MyModel.objects.update_or_create(...) to create the test data.


This is untestable code
.......................

If you are new to software testing, then you might think ... "some parts of my code are *untestable*".

I don't think so. I guess your software uses the IPO pattern: https://en.wikipedia.org/wiki/IPO_model Input, Processing, Output. The question is: How to feed the input for testing to my code? Mocking, virtualization and automation are your friends.

The "untestable" code needs to be cared of. Code is always testable, there is no untestable code. Maybe your knowledge of testing is limited up to now. Finding untestable code and making it testable is the beginning of an interesting adventure.

Is config code or data?
.......................

This is a difficult question. At least at the beginning. For me most configuration is data, not code. That's why the config is in a **database**, not in a text or source code file in a version control system.

This has one major draw-back. All developers love their version control system. Most love git. At is such a secure place. Nothing can get lost or accidently modified. And if a change was wrong, you can always revert to an old version. It is like heaven. Isn't it?

No it is not. The customer can't change it. The customer needs to call you and you need to do stupid repeatable useless work. 

For me configuration should be in the database. This way you can provide a GUI for the customer to change the config.

The configuration and recipies for the configuration management is stored in git. But this is a different topic. If I speak about configuration management, then I speak mostly about configuring linux servers and networks. In my case this is nothing which my customer touches.


ForeignKey from code to DB
..........................

This code uses the ORM of django

.. code-block:: python

    if ....:
        issue.responsible_group=Group.objects.get(name='Leaders')

Above code is dirty because 'Leaders' is like a ForeignKey from code to a database row.

How to avoid this?

Create global config table in your database. This table has exactly one row. That's the global config. There you can create column called "Leaders" and there you store the ForeignKey to the matching group.

Testcode is conditionless
.........................

Testcode should not contain conditions (the keyword `if`). If you have loops (`for`, `while`) in your tests, then this looks strange, too.

Tests should be straight forward:

 #. Build environment: Data structures, ...
 #. Run the code which operates on the data structures
 #. Ensure that the output is like you want it to.

.. code-block:: python

    class MyTest(unittest.TestCase):
        def test_foo(self):
            foo=Foo()
            self.assertEqual(42, foo.find_answer())
        

Don't search the needle in a haystack. Inject dynamite and let it explode
.........................................................................

Imagine you have a huge code base which was written by a nerd which is gone since several months. 
Somewhere in the code a database a row gets updated. This update should not happen, 
and you can't find the relevant source code line during the first minutes. You can reproduce 
this failure in a test environment. What can you do? You can start a debugger and jump through 
the lines which get executed. Yes, this works. But this can take long, it is like 
"Searching the needle in a haystack". Here is a different way: Add a constraint or trigger
to your database which fires on the unwanted modification. Execute the code and BANG - 
you get the relevant code line with a nice stacktrace. This way you get the solution 
provided on a silver plattern with minimal effort :-)


With other words: Don't waste time with searching.

Sometimes you can't use a database constraint to find the relevant stacktrace, but often there are other ways.....

If you can't use a database constraint, maybe this helps: Raise Exception on unwanted syscall http://stackoverflow.com/a/42669844/633961

If you want to find the line where unwanted output in stdout gets emitted: http://stackoverflow.com/a/43210881/633961

If you have a library which logs a warning, but the warning does not help, since it is missing important information. And you have no clue where this warning comes from. You can use this solution: http://stackoverflow.com/a/43232091/633961

Avoid magic or uncommon things
..............................

* hard links in linux file systems.
* file system ACLs (Access control lists). Try to use as little as possible chmod/chown.
* git submodules (Please use configuration management, deployment tools, ...)
* gtk/qt/tk/tkinter: Avoid to write a native GUI. Use Django and its great admin web interface. If you are forced to use a native gui, then I would use QT (for example via PySide)

Learn one programming language, not ten.
........................................


Most young developers think you need to learn many programming languages to be a good developer.

Yes, sometimes it helps to know the programming language C.

My opinion: Learn Python, SQL and some JavaScript.

Then learn other topics: PostgreSQL, Configuration management, continuous integration, organizing, team work, learn to play a music instrument, long distance running, family

Learn "git bisect"
..................

"git bisect" is a great tool in conjunction with unittests. It is easy to find the commit, which introduced an error. Unfortunately it is not a one-liner up to now. You can use it like this:

.. code-block:: shell

    user@host> git bisect start HEAD HEAD~10 


    user@host> git bisect run py.test -k test_something
     ...
    c8bed9b56861ea626833637e11a216555d7e7414 is the first bad commit
    Author: ...



Conditional Breakpoints
.......................

Imagine, you are able to reproduce a bug in a test. But you could not fix it up to now. If you want to create a conditional breakpoint to find the root of the problem, then you could be on the wrong track. Rewrite the code first, to make it more fine-grained debuggable and testable.

Write a test where a normal (non-conditional) breakpoint is enough.

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

Errors (for example power outage) can happen in every millisecond. That's why you need to decide what you want:

* if the power outage happened, some jobs do not get executed. Cronjobs work this way. 
* if the power outage happened, some jobs do get executed twice to ensure they get done.


Further reading: http://docs.celeryproject.org/en/latest/userguide/tasks.html (I don't use celery, but I like this part of the docs)

https://en.wikipedia.org/wiki/Idempotence


File Locking is deprecated
..........................

In the past `File_Locking <https://en.wikipedia.org/wiki/File_locking>`_ was a very interesting and adventurous topic. Sometimes it worked, sometimes not, and you got interesting edge cases to solve again and again. It was fun. Only hard core experts know the difference between `fcntl`, `flock` and `lockf`.

.... But on the other hand: It's too complicated, too many edge cases, too much wasting time.

There will be chaos if there is no central dispatcher. 

I like tools like http://python-rq.org/ It is simple and robust.

BTW, the topic is called `Synchronization <https://en.wikipedia.org/wiki/Synchronization_(computer_science)>`_.

Further reading about "task queues": https://www.fullstackpython.com/task-queues.html

No nested directory trees
.........................

If you store files, then avoid nested directory trees. It is complicated and if you want to use a storage server like `S3 <https://en.wikipedia.org/wiki/Amazon_S3>`_ later, you are in trouble.

Most storage servers support containers and `blobs <https://en.wikipedia.org/wiki/Binary_large_object>`_ inside a container. Containers in containers are not supported, and that's good, since it makes the environment simpler.

Code doesn't call mkdir
.......................

Code runs in an environment. This environment was created with configuration management.
This means: source code usualy does not call mkdir. With other words: Creating directories
is the part of the configuration management. Setting up the environment and executing code in this environment are two distinct parts. If your software runs, the environment does already exist.
Code creating directories if they do not exist yet, should be cut into two parts. One part is creating the environment (gets executed only once) and the second part is the daily executing (which is 100% sure that the environment is like it is. With other words: the code can trust the environmen that the directory exists). These two distinct parts should be seperated.

How to create directories if I should not do it with my software? With automated configuration management (Ansible, Chef, ...) or during installation (RPM/DPKG).

Exception: You create a temporary directory which is only needed for some seconds. But since switching from subprocess/shell calling to using libraries (see "Avoid calling command line tools") temporary files get used much less.

Debugging Performance
.....................

I use two ways to debug slow performance:

 * Logging and profiling, if you have a particular reproducable use case
 * Django Debug Toolbar to see which SQL statements took long in a http request.
 * Statistics collected on production environments. For Python:  https://github.com/uber/pyflame or https://github.com/benfred/py-spy

You provide the GUI for configuring the system. Then the customer (not you) uses this GUI
.........................................................................................

I developed a workflow system for a customer. The customer gave me an excel sheet with steps, transitions and groups.

The coding was the difficult part.

Then I configured the system according to the excel sheet.

The code was bug free, but I made a mistake when I entered the values (from excel to the new web based workflow GUI).

The customer was upset, because the configuration contained mistakes.

I learned. Now I ask if it would be ok if I provide the GUI and the customer enters the configuration.
In most cases the customer likes to do this.

There is a big difference. The customer feels productive if he does something like this.
I hate it. I care for the database design and the code, but entering data with copy+paste
from the Excel sheet ... No I don't like this. Results will be better if you like what you do :-)

For detail lovers: No, it was not feasible to write a script which imported the excel sheet to the database. The excel sheet was not well structured.

*give a man a fish and you feed him for a day; teach a man to fish and you feed him for a lifetime*

Better error messages
.....................

If you have worked with Windows95, then you must have seen them: Empty error messages with just a red icon and a button labeled "OK". You had no clue what was wrong. On the one hand it was great fun, on the other hand it was very sad, since you wasted your precious time.

Do it better.

Imagine user "foo" wants to access data (lets call it "pam") which you only can see, if you are in the group "baywatch". Unfortunately user "foo" is not in the group. You could show him the simple message "permission denied". And no further information.

I don't like messages like this. They create extra work. The user will call the support and ask the question "Why am i not allowed to see the data?". The support needs to check the details.... and soon a half hour of two people is gone. 

Provide better error messages: In this particular case be explicit and let the code produce a message like: "to access the data you need to be in one of the following groups: baywatch, admin, ...".


Software security expert might disagree. I disagree their disagreement. Hiding the facts is just "Security through obscurity".



Avoid clever guessing
.....................

These days I needed to debug a well known Python library. It works fine, but you don't want to look under hood.

One method accepted a object with three different meanings types as first argument:

* case1: a string containing html markup
* case2: a string containing a file path. This file contained the html to work on.
* case3: a file descriptor with a read() method.
 
This looks convinient at the first sight. But in the long run it makes things complicated. This kind of guessing can always lead to false results. In my case I always used case1 (it contained a small html snippet). But, once the string was a accidently the name of an existing directory! This crashed, because the library thought this is was a file.... 

Conclusion: STOP GUESSING.

In Python you can use classmethods for alternative constructors.


.. code-block::

  # case 1
  obj = MyClass.from_string('.....')

  # case2
  obj = MyClass.from_file_name('/tmp/...')

  # case3
  with io.open('...') as fd:
      obj = MyClass.from_file_object(fd)

Don't stop with "permission denied"
...................................

In most non trivial projects there are several reasons why the permission was denied.

If you (the software developer) only return "permission denied", then the user/admin don't know the **reason**.

If you add a reason, then it is more likely that the user/admin can help themselves.

This means they don't call you, our a team mate, to solve this.

Less interrupts for your and happy customers, it's easy.

Or more general: Add enough information to error messages, to make it easier to understand the current situation.

For example you can add hyperlinks to docs/wiki/issue-tracker in you errors messages. 



OOP: Composition over inheritance
.................................

If unsure, then choose "has a" and not "is a".

https://en.wikipedia.org/wiki/Composition_over_inheritance

Cache for ever or don't cache at all
....................................

Avoid "maybe". If your http code returns a response you have two choices concering caching:

* the web client should cache this response for ever.
* the web client should not cache this response at all.

If you follow this guide you will get great performance since revalidation and ETag magic is not needed.

Avoid fiddling with ETag and If-Modified-Since http headers.

But you have to care for one thing: If you cache for ever, whenever you update your data, you need to give your resource a new URL. That's easy:

http://example.com/.../data-which-gets-cached-for-every?v=123456789

If the data of this URL gets changed, you need to update the v=123456789 to a new version.

Related: https://developer.yahoo.com/performance/rules.html


Avoid coding for one customer
.............................

Try to avoid to write software just for one customer. If you write code for one customer, you miss the great
benefit of software: You can write it once and make several customers happy. Of course every business starts small.
But try to create a re-usable product soon.


Misc
....

* `Release early, release often <https://en.wikipedia.org/wiki/Release_early,_release_often>`_
* `Rough consensus and running code. <https://en.wikipedia.org/wiki/Rough_consensus>`_

####################################################################################################

4. Remote APIs
--------------

Use http, avoid ftp/sftp/scp/rsync/smb/mail
...........................................

Use http for data transfer. Avoid the old ways (ftp/sftp/scp/rsync/smb/mail). 

If you want to transfer files via http from shell/cron you can use: `tbzuploader <https://github.com/guettli/tbzuploader>`_.

The next step is to avoid clever `inotify <https://en.wikipedia.org/wiki/Inotify>`_-daemons. You don't need this any more if you receive your data via http.

Why is http better? Because http can validate the data. If it is not valid, the data can be rejected. That's something you can't do with ftp/sftp/scp/rsync/smb/mail.

Avoid Polling
.............

Polling means checking for new data again and again. Avoid it, if possible. Try to find a way to "listen" for changes. In most databases you can execute a trigger if new data arrives.

Provide specific import directories, not one generic
....................................................

If you still receive files via ftp/scp since you have not switched to http-APIs yet, then be sure to provide specific input directories.

In the past I recevied files in a directory called "import". Several third party systems sent data to this directory. It looks easy in the first place. But sooner or later there will be chaos since you need to now where the data came from. Was it from third party system FOO or was the data from third party system BAR? You can't distinguish any more if you profide only one import directory.

Now we provide import-FOO, import-BAR, import-qwerty ...

Don't set up a SMTP daemon
..........................

If you can avoid it, then refuse to set up a SMTP daemon. If the application you write should import mails, then do it by using POP3 or IMAP. You will have much more trouble if you set up an SMTP daemon.


####################################################################################################

5. Op
-----

Operation. The last two characters of DevOp.

Configuration Management
........................

Use a configuration management tool like Ansible. 

Use CI here, too. Otherwise only few people dare to make changes.
And this means the speed of incremental evolution to a more efficent
way will decreases.

I do not use RPM/DPKG to configure a system.

Do you know why modern configuration management tools like Ansible use the term "`file.absent <https://docs.ansible.com/ansible/latest/modules/file_module.html>`_" and not "file.remove"?

`Google search for "Declarative vs Imperative" <https://www.google.com/search?q=Declarative+vs+Imperative>`_

Config Management: Change file vs put file
..........................................

Often there are two ways to do configuration management:


* change a part of a file: "replace", "append", "patch"
* put a whole file under configuration management.
 
You have far less trouble if you use "put a whole file". Example: Do not fiddle with the file `/etc/sudoers`. Put a whole file into `/etc/sudoers.d/`.

Config Management: No need for custom RPMs/DPKGs
................................................

In the past it was common to create a custom RPM or Debian package to install a file on a server.

For example a SSL cert.

If you have a configuration management tool, then this extra container (RPM/DPKG) does not make much sense.


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


Sooner or later you will make a typo. See this article from GitLab for a exciting report what happened during a denial of service: https://about.gitlab.com/2017/02/01/gitlab-dot-com-database-incident/ We are humans, and humans make mistakes. Automation helps to reduce the risk of data loss.


If you are doing "ssh production-server ... vi /etc/..." or "... apt install": Configuration management is much better. For example ansible.

If you are doing "ssh production-server .... less /var/log/...": No log-management yet? Get your logs to a central place.

If you are doing "ssh production-server ... rm ...": Please ask yourself what you are doing here. How can you automate this, to make this unneccessary in the future. 

Keep your directories clean
...........................

There are two kind of files in the context of backup: Files which should be in the backup and temporary files which should not be in the backup. Keep you directories clean. In a directory there should be either only files which should be in the backup xor only files which should not be in the backup. This will make live easier for you. The configuration of your backup is easier and cleaning temporary files is easier and looking at the directory makes more joy since it is clean.


Avoid logging to files
......................

I still do this, but I want to reduce it. Logs are endless streams. Files are a buch of bytes with fixed length.
Both concepts don't fit together. Sooner or later your logs get rotated. Now you are in trouble if you want to run a log checker for every line in your logfile. I mean the mathematically version of "every line". This gets really complicated if you want to check every line. Rotating logfiles needs to be done sooner or later. But how to rotate the file, if a process still write to it? This is one problem, which was solved several hundred times and each time different ...

In other words: Avoid logging to files and avoid logrotate. Logging is an endless stream.

Use Systemd
...........

It is available, don't reinvent. Don't do double-fork magic any more. Use a systemd service with Type=simple. See `Systemd makes many daemons obsolete <https://stackoverflow.com/a/30189540/633961>`_



If you do coding to implement backup ...
........................................

If you do coding/programming to implement your backup of data, then you are on the wrong track.

It is very likely that you will do it wrong, and this will be a big risk.

Why? Because you will notice your fault if you try to recover your data. 

**Use** a backup tool, even if you love to do programming. **Configure** it, but don't write it yourself.



Avoid re-inventing replication
.............................. 

That's what the customer wants from you to implement:

You should transfer data from database A to database B.
Every time there is an update in database A, data should get copied to
database B.

Slow down: What you are doing is replication. Replication creates
redundancy and redundancy needs to be avoided.

Why do you want redundancy in your data storage? The only reasons I can think
of are speed/performance and faul-toleranz (like DNS/LDAP).

If replication is really needed,
then take the replication tools the databases offer. Do not implement
replication yourself. This is not trival and experts with more knowledge than you and me
have solved this issue before.



6. Networking
-------------

No routing on servers
.....................

Imagine there are 20 servers in your network. Imagine there are two network routes. One route goes to a second internal network and the other route goes to the internet. All 20 servers should be able to access both networks. There are two ways to solve this:

* V1: Each of the 20 servers has the two routes configured.
* V2: There is one default gateway for the 20 servers. Every server has one route. (The common term is "default gateway")

Please choose V2. It is simpler, it is easier to understand, it is less error prone, it is more sane.

traceroute won't help you
.........................

If you have trouble with a tcp connection, then use tcptraceroute. Again \*tcp\*traceroute. It is the tool for tcp connection tests (http, https, ssh, smtp, pop3, imap, ...). Reason: normal traceroute uses UDP, not TCP.


7. Monitoring
-------------

Nagios Plugin API (0=ok, 1=warn ...)
....................................

Writing Nagios like checks is very simple. The exit status has this meaning:

* 0: ok
* 1: warn
* 2: error
* 3: unknown

Is this KISS (keep it simple and stupid)? Yes, I think it is **simple**. You can write a nagios plugin with any language you like. Often less then ten lines of source code are enough to implement a nagios check.

But on the other hand it is not **stupid**. The checks does two things: It collects some numbers (for example "How much disk space is left") and it does evaluate and judge ("only N MByte left, I think this is a warning"). That's not stupid this is some kind of intelligence. 

After writing and working with nagios checks for several years I think the evaluation of the data should not be done inside the check. Some data-collector should collect data. Then a different tool should evaluate the data and judge if this ok, warn or error.


Checks vs Logs
..............

Checks are for operators and logs are for developeres.

Since there are always some temporary network failures,
checks help more than logs do.

Example: 

#. yesterday night at 3:40 there was a temporary network failure and this results in log messages.

#. At 3:45 the network failure was gone.

#. You look at the log message at 9:15. You don't know: Is this message still valid?

Checks get executed again and again.

If a check fails at 3:41 it will be ok some minutes later.

Then you know immidiately that there was **temporary** failure.

Logs are important for developers for debugging.

But in this case, the developer can't do anything
usefull. Temporary network failures happen again and again. That's live.
Looking at the log which was created 
by a temporary network failure wastes the time of the developer.


Logs should contain the stacktrace and the local variables
of each frame in the stacktrace (a tool like sentry could be used), if real errors occur.


####################################################################################################


8. Communication with others
----------------------------

Avoid to get a nerd
...................


If you do "talk" with software to databases and APIs daily, your ability to communicate with humans might decrease.

You might start to think like a computer (at least a bit). 

The human mind works completly different, not just bits and bytes. It has `Emotions <https://en.wikipedia.org/wiki/Emotion>`_

Avoid to get a `Nerd https://en.wikipedia.org/wiki/Nerd`


Here some hints:

* Nerds like complaining. This book can help: "Rethinking Positive Thinking: Inside the New Science of Motivation" by Gabriele Oettingen. The method is called WOOP. 
* Nerds like to think at their problems first. `Nonviolent Communication <https://en.wikipedia.org/wiki/Nonviolent_Communication#Four_components>`_ can help.
* Meet with "normal" people. With "normal" I mean people who do not do IT stuff. 
* Raise a family.
* Do sport
* Relax

Avoid stress
............

Stress trigger your body’s “fight or flight” response. It pushes your blood into the muscles.
That's great if you need to jump onto the side walk because a fast red race car would hit you.
But in your daily life this "fight or flight" response is hardly needed. You need the energy
in your brain :-)

Avoid stress, relax daily.

On the other hand stress is fun: I like tennis and long distance running.

Care for both: brain and body.


Discussion, but no progress? V1, V2, V3, ...
............................................

This and the following parts are about "Requirement Engineering".

If a discussion brings not progress, then grab a pen. Start with V1. The letter V stands for "Solution Variant" or "One strategy of several to get to a goal". Find a term or short description of the first possible strategy. Write it down. Then: which other ways could be used? V2, V3, ... 

Rember, there is always the last variant: Leave things like they are today and think about this again N days later.

If you have found several solution variants, then look at them in detail. Most of the time it is useful to define the need sequence of steps. You can use the letter "S" for this: S1, S2, S3 ...

A simple example:

In the morning, you wake up.

* V1: Go to work now
* V2: Do some more sleeping
* V3: Try to remember what you dreamed, write it down
* V4: Do some sports
* V5: Play piano
* V6: Recall your personal goals, what is the next step?
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

Use a way to edit content (use cases, specs, ...) over the internet. Use an issue tracking system or wiki.

Don't waste time with UML tools. UML is like `esperanto <https://en.wikipedia.org/wiki/Esperanto>`_. It is (in theory) a great solution which solves a lot of problems. But somehow it does not work.

Write down the high level use case, the cardinality and the steps.
Sequence diagrams can be simplified to enumerations: first step, second step, third step ...

`Sketch <https://en.wikipedia.org/wiki/Sketch_(drawing)>`_ screenshots you want to build with your team with a pen. I avoid any digital device for this, since up to now paper or a whiteboard are far more real. If you need the result in digital format, just take a picture with your cell phone at the end.


Communication with Customers: Binary decision "do list" or "do later list"
...............................................................................

Define "done" with your customers. Humans like to be creative and if thing X gets changed,
then they have fancy ideas how to change thing Y.
Be friendly and listen: Write these fancy ideas down on the "do later list".

If the customer have new ideas, let them decide: Should this be on the "do list" or the "do later list".

If you don't have a definition of done/ready, then you should not start to write source code.
First define the goal, then choose a strategy to get to the goal.

Focus on a simple working solution first. Add optional stuff to the "do later" list.

Tell customers what they should test
....................................

I have seen it several times: Software gets developed. The customer was told to test and ... nothing happens.
That's not satisfying since software developers want to hear that their work does help.
If you (the developer) provide a checklist of things to test, then the likelihood to get feedback is bigger.

It is wise to create this checklist for testing as early as possible. It tells the developer the desired result.


Dare to say "Please wait, I want to take a note"
................................................

Most people can listen and write at once. I can't. And I guess a lot of developers have this problem.
I can only do one thing at a time. If you are telephoning with a customer and he has a lot of things to tell you,
don't fool yourself. You will only remember 4 of 5 issues. Dare to say "please wait, I want to take a note".
This way you can care for all issues, which results in happy customers.

Avoid Gossip
............

Gossip creates an atmosphere which promotes negativity (bad karma). Avoid to make jokes about other team mates
or customers. Yes, there are people who do strange stuff and who have strange attitudes.
Making jokes about them makes everything worse.
Please be aware that this guideline has a major drawback.
Sometimes all people around you are laughing about a customer or a team mate which is not here right now ...
and you are the only one who is not laughing. It is up to you how to react. Be patient.



####################################################################################################

9. Epilog
---------

It is always possible to make things more complicated
.....................................................


It is always possible to make things more complicated. The interesting adventure is to make things simpler and easier. 

It helps to talk
................

Most software developers do not talk much. Otherwise they would not have choosen this job. If you think about something too long, then you get blind for the obvious and easy solution. It helps to talk.

There is something called `Rubber duck debugging <https://en.wikipedia.org/wiki/Rubber_duck_debugging>`_. This might help, but talking to humans helps much more. If you find no solution in 30 minutes. Take a break. Do something different, talk to a team mate or friend, take a small walk outside.

Be curious
..........

There is always something you don't have understood up to now. Ask questions, even if you think you know the answer.
For one question, there are always several answers. If you know one answer,
then it is likely that someone has a better answer.

I like:

* https://stackoverflow.com/
* https://softwarerecs.stackexchange.com/
* https://serverfault.com/
* And some mailing lists.

Often I just write the question, and don't write about the solution I have on my mind. If you write about our solution, then the discussion is narrowed to a simple pro/contra of your idea. Ask the question like a newbee.

Creativity Management
.....................

A lot of ideas come to my mind, if I am far away from a laptop or pc. For example if I cylce from home to office or back.

I started with this way of creativity management some years go: I write a mail to myself.

If I cycle home on a friday evening, I want to keep my mind relaxed and focused on my family. All work related thoughts should be far away. I don't want to "carry" around work-related thoughts on the weekend. On the road from office to home I might have an idea what to do (how to hunt a strange bug, how to implement a cool feature which needs only a very little effort and time to implement, ...). I stop (that is great advantage of riding a bike - I can stop almost always immidiatley, and take my mobile phone). Then I write a mail to my business adress and now I am sure: This idea won't get lost. And I am free to have a nice weekend with my family.

The same happends when I drive from home to office: I have an idea related to my personal live? I stop and write a mail to my personal account.

That's how most of this guide-line was created: Most items came to my mind during cycling, walking, listening to music or laying in bathtub. Short mail to myself, and some days later I take the mail which contains just a handfull of words and I formulate it.

Cut bigger problems into smaller ones
.....................................

A lot of new comers have problems with this. Here is one example to illustrate the guideline "Cut bigger problems into smaller ones".

Imagine you are responsible for several servers and you should create graphs of their disk/cpu usage.

Cut the bigger problem into smaller ones:

* How to collect the data on one host
* How to transport the data from the host to a central place?
* How to store the data in a central database?
* How to generate the graphs?

BTW, why not use the PostgreSQL feature "Logical Replication"?


Read the Release Notes
......................

Read the release notes of the tools you use daily.

I like these release notes:

* https://www.postgresql.org/docs/devel/static/release.html The "Overview" links show the most important changes
* https://docs.djangoproject.com/en/dev/releases/
* Python
* PyCharm

Three Mail Accounts
...................

I have three mail accounts:

* personal mails (family, friends, ...)
* work related mails
* mailing lists


Clean up your desk
..................

Don't forget to clean your desk. I don't write this here because I do it often and with joy.
No, excat the opposite. I write it down since I want to push myself.

Don't look at all these things on your desk at once. Start on the left side take the first thing.
Where is the best place for this thing single thing? Unsure? Why not throw it in the trash can?
If you are unsure put it at least in box behind a closed cabinet door.
Some month later you might be able to throw it in the garbage.

Then wipe the dust.

If you have never time do this, then there is something wrong. Slow down.

Highlander, "There can be only one"
...................................

"Highlander" is a 1986 British-American adventure action fantasy film with tagline "There can be only one".
Thinking like this narrows your mind. There can be several thousand. Look how successfull ants and bees work. If someone is better or faster, then smile. Give applaud and say "wow".

`Don't be evil. <https://en.wikipedia.org/wiki/Don%27t_be_evil>`_ Don't waste time and mental energie. Applauding if the competitor is better, was new to me in 2017. I was at Rothenbaum and attended the German Open (Tennis). The coach of one player was applauding every time the opponent made a good shot. I was astonished. Why was the coach applauding the enemy? But this works. If you get angry, you waste energy and you start to think like a wild and stupid animal. Even if you have made a mistake or lost some how, no reason not to walk upright.


Don't waste your time with cheap hardeware
..........................................

Some people love the `Raspberry Pi <https://en.wikipedia.org/wiki/Raspberry_Pi>`_. I don't like it. It does not have enough computing power for my use cases. Yes, the device is cheap, but I prefer to spend some more money to have more performance. I don't like waiting.


Write a diary
.............

I think it helps to write a diary. Sitting down and writing about the last days help you to reflect the things you did.
I helps you to focus on your goals. Do you have goals? I found out that late (age of 40). A diary is fun to read several months later. I try to do it at least once a week.
I have three types of diaries.

One on facebook readable for everyone. It contains things from my daily life,
written in german. https://www.facebook.com/thomas.guttler.52


There is one on google-plus which contains IT topics
(open source, python, linux, PostgreSQL), written in english and readable by everyone. https://plus.google.com/112821159206665920618

And there is a private which I maintain with Anki. Anki is a flash card app. The front side
is the question and the back side is the answer. I use the first side for the date and one to three words,
and the back side contains the text.
This way I can ask myself what was on my mind these days. But all this should be fun, not a burden.

The Bus factor
..............

From Wikipedia: The bus factor is a measurement of the risk resulting from information and capabilities not being shared among team members

`Bus factor <https://en.wikipedia.org/wiki/Bus_factor>`_

Avoid to create secret knowledge which is only available to you. Share knowledge.

Avoid overspecialization of yourself. It will have drawbacks. Imagine there are some things which only you know.
Sooner or later you want to go on holiday and you want a relaxed holiday. You don't want to be called
on your mobile phone by your boss or a team mate. You want two weeks off without a single interrupt which
is related to your work.

I guess all people love it, if they are important. Everybody loves it, if someone needs them. But you will get
a burnout if no one else can do the things you do.

Avoid overspecialization of a team mate, too. If a team mates has secret knowledge and there is no one
else who has a clue: Talk. Try to reveal the things which only one person knows.
Tell him about your concerns (Bus factor). Maybe talk to his boss.

Imagine there is an action which needs to be done roughly twice a year. For example
setting up a new server. Up to now Bob did this everytime. Talk to your team mates. Explain that
every action should be known to at least two people. In practice this means: The next time Bob won't do it.
It needs to be done by someone else.

If you read above sentences and think "that's not my job, that's the
job of the team leader", then I think it is time stop acting like a dumb sleeping sheep.
Get resonsible. React relaxed if nobody is listening or understanding your concerns.
"The Best Path to Long-Term Change Is Slow, Simple and Boring."



Thank you
.........

* Robert C. Martin for the book "Clean Coder"
* Malcolm Tredinnick. Only few people listened like he did. With "listen" I mean "trying to understand the conversation partner".
* Linus Torvalds for the quote "Bad programmers worry about the code. Good programmers worry about data structures and their relationships.". 
* Bill Gates for the quote "I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it." 
* All people who contribute to open source software (Linux, Python, PostgreSQL, ...)
* All people who ask question and/or answers them at places like StackOverflow.
* People I met during study at HTW-Dresden
* My teammates at `tbz-pariv <http://www.tbz-pariv.de/>`_.
* https://chemnitzer.linux-tage.de/ All people involved in this great yearly event.
* Ionel Cristian Mărieș for the link to bash pitfalls.

.. Link in ReST: `text <http:....>`_



