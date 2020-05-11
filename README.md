# Programming Guidelines

My personal programming guidelines. **Please provide feedback**, tell me
what's wrong and what's missing and create a new issue here:
<https://github.com/guettli/programming-guidelines/issues> or write me a
mail to <guettliml@thomas-guettler.de>

[1. Introduction](#1-introduction)

[2. Data structures](#2-data-structures)

[3. Dev](#3-dev)

[4. Remote APIs](#4-remote-apis)

[5. Op](#5-op)

[6. Networking](#6-networking)

[7. Monitoring](#7-monitoring)

[8. Communication with others](#8-communication-with-others)

[9. Epilog](#9-epilog)

## 1. Introduction

### About this README

I was born 1976. I started coding with basic and assembler when I was
13. Later turbo pascal. From 1996-2001 I studied computer science at
HTW-Dresden (Germany). I learned Shell, Perl, Prolog, C, C++, Java, PHP
and finally Python.

Sometimes I see young and talented programmers wasting time. There are
two ways to learn: Make mistakes yourself, or read from the mistakes
which were done by other people.

This list summarises a lot of mistakes I did in the past. I wrote it, to
help you, to avoid these mistakes.

It's my personal opinion and feeling. No facts, no single truth.

### Relaxed focus on monitor

Do not look at the keyboard while you type. Have a relaxed focus on your
monitor.

I type with ten fingers. It's like flying if you learned it. Your eyes
can stay on the rubbish you type, and you don't need to move your eyes
down (to keyboard) and up (to monitor) several hundred times per day.
This saves a lot of energy. This is simple tool to help you to learn touch typing:
[tipp10](https://www.tipp10.com/en/)

Avoid to switch between mouse and keyboard too much.

I like lenovo keyboards with track point. If you want the track point to
have more grip you can use sandpaper. Here is an images to illustrate
what I use [sandpaper-sticked-on-track-point.jpg](https://raw.githubusercontent.com/guettli/programming-guidelines/master/sandpaper-sticked-on-track-point.jpg)

Once I was fascinated by the copy+paste history of Emacs and PyCharm.
But then I thought to myself: "I want more. I am hungry. I want a
copy+paste history not only in one application, I want it for the whole
desktop". The solution is very simple, but somehow only few people use
it. The solution is called clipboard manager. I use Diodon (Linux) and
CopyQ (for Windows). I use ctrl+alt+v to open the list of last
copy+paste texts.

### Avoid searching with your eyes

Avoid searching with your eyes. Search with the tools of your IDE. You
should be able to use it "blind". You should be able to move the cursor
to the matching position in your code without looking at your keyboard,
without grabbing your mouse/touchpad/trackpoint and without looking
up/down on your screen.

Compare two files with a diff tool, otherwise you might get this ugly skeptical frown.

### KISS

Keep it simple and stupid. The most boring and most obvious solution is
often the best. Although it sometimes takes months until you know which
solution it is.

From the book "Site Reliability Engineering" (O'Reilly Media 2016)
<https://landing.google.com/sre/book/chapters/simplicity.html>

Quote:

:   The Virtue of Boring

    Unlike just about everything else in life, "boring" is actually a
    positive attribute when it comes to software! We don’t want our
    programs to be spontaneous and interesting; we want them to stick to
    the script and predictably accomplish their business goals.

### Avoid redundancy

See heading.

### Premature optimization is the root of all evil.

The famous quote "premature optimization is the root of all evil." is true.
You can read more about this here [When to optimize](https://en.wikipedia.org/wiki/Program_optimization#When_to_optimize).

First get a [MVP (minimum valuable product)](https://en.wikipedia.org/wiki/Minimum_viable_product) to your customer, and then listen to their feedback. Care for their needs, not for your own vision of a super performant application.

------------------------------------------------------------------------

## 2. Data structures

### Introduction

"Bad programmers worry about the code. Good programmers worry about data
structures and their relationships." -- Linus Torvalds (creator and
developer of the Linux kernel and the version control system git)

### Relational Database

I know SQL is..... It is either obivious or incomprehensible. Yes, it is
boring.

A relational database is a rock solid data storage. Use it.

Use a tool to get schema migrations done (for example django).

I use PostgreSQL.

I don't like NoSQL, except for caching.

But maybe I just have not enough experience with NoSQL up to now.

### Cardinality

It does not matter how you work with your data (struct in C, classes in
OOP, tables in SQL, ...). Cardinality is very important. Using 0..\* is
often easier to implement than 0..1. The first can be handled by a
simple loop. The second is often a nullable column/attribute. You need
conditions (IFs) to handle nullable columns/attributes.

<https://en.wikipedia.org/wiki/Cardinality_(data_modeling)>

If this is new to you, I will give you two examples:

-   1:N --&gt; One invoice has several invoice positions. For example
    you buy three books in one order, the invoice will have three
    invoice positions. This is a 1:N relationship. The invoice position
    is contained in exactly one invoice.
-   N:M --&gt; If you look at tags, for example at the Question+Answer
    site stackoverflow: One question can be related to several
    tags/topics and of course a topic can be set on several questions.
    For example you have a strange UnicodeError in Python then you can
    set the tags "python" and "unicode" on your question. This is an N:M
    relationship. One more well know example of N:M is user and groups.

### Conditionless Data Structures

If you have no conditions in your data structures, then the coding for
the input/output of your data will be much easier.

### Avoid nullable Foreign Keys

Imagine you have a table "meeting" and a table "place". The table
"meeting" has a ForeignKey to table "place". In the beginning it might
be not clear yet where the meeting will be. Most developers will make
the ForeignKey optional (nullable). WAIT: This will create a condition
in your data structure. There is a way easier solution: Create a place
called "unknown". Use this [senitel value](https://en.wikipedia.org/wiki/Sentinel_value) as default. This data
structure (without a nullable ForeignKey) makes implementing the GUI
much easier.

With other words: If there is no NULL in your data, then there will be
less NullPointerException in your source code while processing the data
:-)

Less conditions, less bugs.

### Avoid nullable boolean columns

\[True, False, Unknown\] is not a nullable Bollean Column.

If you want to store a data in a SQL database which has three states
(True, False, Unknown), then you might think a nullable boolean column
(here "my\_column") is the right choice. But I think it is not. Do you
think the SQL statement "select \* from my\_table where my\_column = %s"
works? No, it won't work since "select \* from my\_table where
my\_column = NULL" will never ever return a single line. If you don't
believe me, read: [Effect of NULL in WHERE clauses
(Wikipedia)](https://en.wikipedia.org/wiki/Null_(SQL)#Effect_of_Unknown_in_WHERE_clauses).
If you like typing, you can work-around this in your application, but I
prefer straight forward solutions with only few conditions.

If you want to store True, False, Unknown: Use text, integer or a new
table and a foreign key.

### Avoid nullable characters columns

If you allow NULL in a character column, then you have two ways to
express "empty":

-   NULL
-   empty string

Avoid it if possible. In most cases you just need one variant of
"empty". Simplest solution: avoid that a column holding character data
types is allowed to be null.

If you really think the character column should be allowed to be NULL,
then consider a constraint: If the character string in the column is not
NULL, then the string must not be empty. This way ensure that there are
is only one variant of "empty".

### SQL: I prefer subqueries to joins

I most cases, I use a ORM to access data, and don't write SQL by hand.

If I do write SQL by hand, then I often prefer [SQL Subqueries](https://en.wikipedia.org/wiki/SQL_syntax#Subqueries)
to SQL Joins. 

Have a look at this example:
```
SELECT id, name
FROM products
WHERE category_id IN
   (SELECT id
    FROM categories
    WHERE expired = True)
```
I can translate this to human language easily: Select all products, which
belong to a category which has expired.


### Use all features PostgreSQL does offer

If you want to store structured data, then PostgreSQL is a save default
choice. It fits in most cases. Use all features PostgreSQL does offer.
Don't constrain yourself to use only the portable SQL features. It's ok
if your code does work only with PostgreSQL and no other database, if
this will solve your current needs. If there is the need to support
other databases in the future, then handle this problem in the future,
not today. PostgreSQL is great, and you waste time if you don't use its
features.

Imagine there is be a a Meta-Programming-Language META (AFAIK this does
not exist) and it is an official standard created by the ISO (like SQL).
You can compile this Meta-Programming-Language to Java, Python, C and
other languages. But this Meta-Programming-Language would only support
70% of all features of the underlaying programming languages. Would it
make sense to say "My code must be portable, you must use META, you must
not use implementation specific stuff!"?. No, I think it would make no
sense.

My conclusion: Use all features PostgreSQL has. Don't make live more
complicated than necessary and don't restrict yourself to use only
portable SQL.

### Where to not use PostgreSQL?

-   For embedded systems SQLite may fit better
    \* Prefer SQLite if there will only be one process accessing the
    database at a time. As soon as there are multiple users/connections,
    you need to consider going elsewhere
-   TB-scale full text search systems.
-   Scientific number crunching:
    [hdf5](https://en.wikipedia.org/wiki/Hierarchical_Data_Format)
-   Caching or high performance job queues: Redis fits better.
-   Go with the flow: If you are wearing the admin hat (instead of the
    dev hat), and you should install (instead of develop) a product,
    then try the default db (sometimes MySQL) first.

Source: PostgreSQL general mailing list:
<https://www.postgresql.org/message-id/5ded060e-866e-6c70-1754-349767234bbd%40thomas-guettler.de>

### Transactions do not nest

I love nested function calls and recursion. This way you can write easy
to read code. For example recursion in quicksort is great.

Nested transactions ... sounds great. But stop: What is
[ACID](https://en.wikipedia.org/wiki/ACID) about? This is about:

-   Atomicity
-   Consistency
-   Isolation
-   Durability

Database transactions are atomic. If the transaction was successful,
then it is \*D\*urable.

Imagine you have one outer-transaction, and two inner transaction.

1.  Transaction OUTER starts
2.  Transaction INNER1 starts
3.  Transaction INNER1 commits
4.  Transaction INNER2 starts
5.  Transaction INNER2 raises an exception.

Is the result of INNER1 durable or not?

Conclusion: Transactions do not nest

Related:
<http://stackoverflow.com/questions/39719567/not-nesting-version-of-atomic-in-django>

The "partial transaction" concept in PostgreSQL is called savepoints.
<https://www.postgresql.org/docs/devel/sql-savepoint.html> They capture
linear portions of a transaction's work. Your use of them may be able to
express a hierarchical expression of updates that may be preserved or
rolled back, but the concept in PostgreSQL is not itself hierarchical.

------------------------------------------------------------------------

## 3. Dev

### Less code, less bugs

-   Not existing code is the best: Less code, less bugs
-   Code maintained by a reliable upstream (like Python, PostgreSQL,
    Django, Linux, Node.js, Typescript, ...) is more reliable than own
    code.

### Zen of Python

[Zen of Python](https://www.python.org/dev/peps/pep-0020/) (Written by
Tim Peters in the year 1999)

-   Beautiful is better than ugly.
-   Explicit is better than implicit.
-   Simple is better than complex.
-   Complex is better than complicated.
-   Flat is better than nested.
-   Sparse is better than dense.
-   Readability counts.
-   Special cases aren't special enough to break the rules.
-   Although practicality beats purity.
-   Errors should never pass silently.
-   Unless explicitly silenced.
-   In the face of ambiguity, refuse the temptation to guess.
-   There should be one-- and preferably only one --obvious way to do
    it.
-   Although that way may not be obvious at first unless you're Dutch.
-   Now is better than never.
-   Although never is often better than *right* now.
-   If the implementation is hard to explain, it's a bad idea.
-   If the implementation is easy to explain, it may be a good idea.
-   Namespaces are one honking great idea -- let's do more of those!

In the year 2001 I knew these programming languages: Basic, Pascal,
Assembler, C, C++, Prolog, Lisp, Visual Basic, Java, JavaScript, tcl/tk,
Perl.

I was unhappy with all of them and looked for a new language. I narrowed
down the languages I was interested in and there were two choices left.
One was ruby, the other was python. I choose Python. It looked simpler,
like executable pseudo-code. Since 2001 I use it nearly every work-day.
I like it, and up to now no other language attracts me.

I am not married with Pyhon. I am willing to change. But the next
language needs to be better. Up to now I see no alternative.

JavaScript has the big benefit, that it can be executed in the browser.
But I don't like it. Why I don't like it? I don't know. Sometimes
feelings are more important than facts.

### CRUD --&gt; CRD

In most cases software does create, read, update, delete data. See
[CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)

The "update" part is the most difficult one.

Sometimes CRD helps: Do not implement the update operation. Use
delete+create. But be sure to use transactions to avoid data loss, if your
data stroage supports this:
"BEGIN; DELETE ...; INSERT ...; COMMIT;"

Translating to SQL terms:

|  CRUD Term   |SQL                                |
|  ------------|-----------------------------------|
|  create      |insert into my\_table values (...) |
|  read        |select ... from my\_table          |
|  update      |update my\_table set col1=...      |
|  delete      |delete from my\_table where ...    |

Take a look at virtualization and containers ([Operating-system-level
virtualization](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)).
There CRD gets used, not CRUD. Containers get created, then they
execute, then they get deleted. You might use configuration management
to set up a container. But this gets done exactly once. There is one
update from vanilla container to your custom container. But this is like
"create". No updates will follow once the container was created. This
makes it easier and more predictable.

The same is true for operating on data-structures in memory. In most cases you should not alter the data structure which are iterating. Create a new data structure while iterating the input data. With other words: no in-place editing.

### Stateless

When I was a student I was excited and fascinated by [CORBA (Common Object Request Broker Architecture)](https://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture). I thought this is the future of machine to machine communication. Today I smile about how childish I was 19 years ago. CORBA is dead, stateless [http](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) has won.

Things are much easier to implement and predict if you just have one method call. One request and one response. You don't have an open connection and a reference to a remote object which executes on a remote server.

Look at all the dated protocols which are like a human conversation between a client and a server: SMTP, IMAP, FTP, ... Nobody wants the client and the server to have a chatty dialog like this: 

```
Client: My name is Bob
Server: Hi Bob, nice to meet you.
Server: But are you really Bob?
Server: Please prove to me that you're Bob. You can use method foo, bar, blu for authentication
Client: I choose method "blu"
Server: Ok, then please tell send the magic blu token
Client: Here it is xyuasdusd8.. I hope you like it.
Server: Fine, I accept this. Now I trust you. Now I know you are Bob
Client: Please show me the first message
Server: here it is:
Server: ....
Client: looks like spam. Please delete this message
Server: Now I know that you want to delete this message. 
Server: But won't delete it now. Please send me EXPUNGE to execute the delete.
Client: grrrr, this is complicated. I already told you that I want the message to be deleted.
Client: EXPUNGE
...
```

Of course roughly the same needs to be done with http. But http you can cut the task into several smaller http requests. This gives the service the chance of delegating request-1 to server-a and request-2 to server-b. In the cloud environment containers get created and destroyed in seconds. It is easier without long living connection.

In above case (IMAP protocol) the EXPUNGE is like a COMMIT in relational databases.It is very handy to have a transactional database to implement a service. But it makes no sense to expose the transaction to the client.

Stateless is like IPO: Input-Processing-Output.



### No Shell Scripting

The shell is nice for interactive usage. But shell scripts are
unreliable: Most scripts fail if filenames contain whitespaces.
Shell-Gurus know how to work around this. But quoting can get really
complicated. I use the shell for interactive stuff daily. But I stopped
writing shell scripts.

Reasons:

-   If a error happens in a shell script, the interpreter steps silently
    to the next line. Yes I know you can use "set -e". But you don't get
    a stacktrace. Without stacktrace you waste a lot of time to analyze
    why this error happened.
-   AFAIK you can't do object oriented programming in a shell. I like
    inheritance.
-   AFAIK you can't raise exceptions in shell scripts.
-   Shell-Scripts tend to call a lot of subprocesses. Every call to
    grep, head, tail, cut creates a new process. This tends to get slow.
    I have seen shell scripts which start thousand processes per second.
    After re-writing them in Python they were 100 times faster und 100
    times more readable.
-   I do this "find ... | xargs" daily, but only while using the shell
    interactively. But what happends if a filename contains a newline
    character? Yes, I know "find ... -print0 | xargs -r0", but now "find
    .. | grep | xargs" does not work any more .... It is dirty and will
    never get clean.
-   Look at all the pitfalls: [Bash
    Pitfalls](https://mywiki.wooledge.org/BashPitfalls) My conclusion: I
    prefer to walk on solid ground, I don't write shell scripts any
    more.

Even Crontab lines are dangerous. Look at this:

> @weekly . \~/.bashrc && find \$TMPDIR -mindepth 1 -maxdepth 1 -mtime
> +1 -print0 | xargs -r0 rm -rf

Do you spot the big risk?

### Portable Shell Scripts

I think writing portable shell scripts and avoiding bashism (shell
scripts which use features which are only available in the bash) is a
useless goal. It is wasting time. It feels productive, but it is not.

Avoid \#!/bin/sh. The interpreter could be bash, dash or something else.
See [Comparison of command
shells](https://en.wikipedia.org/wiki/Comparison_of_command_shells).
Please be explicit. Use \#!/bin/your-favorite-shell.

If I look at this page
([DashAsBinSh](https://wiki.ubuntu.com/DashAsBinSh)), which explains how
to port shell scripts to /bin/dash I would like to laugh, but I can't
because I think it is sad that young and talented people waste their
precious time which this nonsense. Since systemd gets used, the shell
gets started less often (compared to the old system-V or BSD init). This
architectural change brought improvement. And I think that using dash
instead of bash brings no measurable benefit today.

If you are not able to create a dependency to bash, then solve this
issue. Use rpm/dpkg or configuration management to handle "my script
foo.sh needs bash".

I know that there are some edge cases where the bash is not available,
but in most cases the time to get things done is far more important.
Execution performance is not that important. First: get it done
including automated tests.

### Server without a shell is possible

In the past, it was unbelievable: A unix/linux server which does not
execute a shell while doing its daily work. The dream is true today.
These steps do not need a shell: operating system boots. Systemd starts.
Systemd spawn daemons. For example a web server. The web server spawns
worker processes. A http request comes in and the worker process handles
one web request after the other. In the past the boot process and the
start/stop scripts were shell scripts. I am very happy that systemd
exists.

But time has changed. Today applications run in containers. Containers
don't need systemd. In [Kubernetes](https://en.wikipedia.org/wiki/Kubernetes) containers
get started and stopped, not services. There is no need for a daemon starting and
stopping services, since this gets done on a higher level.

### Avoid calling command line tools

I try to avoid calling a command line tool, if a library is available.

Example: You want to know how long a process is running (with Python).
Yes, you could call `ps -p YOUR\_PID -o lstart=` with the subprocess
library. This works.

But why not use a library like
[psutil](https://pypi.python.org/pypi/psutil)?

Why do you want to avoid a third party library?

Is there a feeling like "too much work, too complicated"? Installing a
library is easy, do it.

Check the license of the library. If it is BSD, MIT or Apache like, then
use the library.

Calling a subprocess is slow, especially if it gets done often you will notice
the difference soon. 

### Avoid toilet paper programming (wrapping)

What is "toilet paper programming"? This is a pattern which was often
used in the past: There is something wrong inside - something is
smelling. Let's write a wrapper. Still something wrong? Let's write a
second wrapper.....

All these wrappers do not solve the underlaying issue.

In the past there were less alternatives. And since you hand no choices,
you were forced to use a particular tool. If this did not work the way
you wanted it, you need to write a wrapper.

Today you have much more alternatives. If tool x does not work work the
way you want it to, you can use tool y.

I am happy that the anti-pattern "toilet paper programming" gets used
less often today.

Example: WxPython (GUI toolkit) wraps WxWindows wraps gtk wraps xlib.

There are still some places where toilet paper wrappers need to get coded again and again.

For example JSON does not support datetime, timedelta and binary data. See [Let's fix JS](https://github.com/guettli/lets-fix-js). Speak to the upstream, to whoever is responsible for this, even if you think they are way too big, and you way too small.

### If unsure use MIT or Apache-2 License

The MIT and Apache Licenses are simple and short.

The GPL license is much too long. I tried to read it twice, but I felt
asleep. I don't like things which I don't understand.

Next argument: The GPL license is viral.

### Loop in DB, not in your code

Do the filtering in the database. In most cases it is faster then the
loops in your programming language. And if the DB is not fast enough,
then I guess there is just the matching index missing up to now.

### Do permission checking via SQL

Imagine you have three models (users, groups and permissions) as tables
in a relational database system.

Most systems do the permission checking via source code. Example: if
user.is\_admin then return True

Sooner or later you need the reverse: Show all users which have a given
permission.

Now you write SQL (or use your ORM) to create a queryset which returns
all users which satisfy the needed conditions.

Now you have two implementations. The first `if user.is_admin then
return True` and one which uses set operations (SQL). This is redudant and looking
for trouble. Sooner or later your permission checks get more complex and then 
one implementation will get out of sync.

### Real men don't use ORM

[ORM (Object-relational mapping)](https://en.wikipedia.org/wiki/Object-relational_mapping) makes daily
work much easier. Above heading is a stupid joke. Clever people use tools to make work simpler, more fun and more
convenient. ORMs are great. 

Some (usualy elderly) developers fear that a ORM is slower than hand-crafted and optimized SQL. Maybe, maybe not.

See [premature optimization is the root of all evil](#premature-optimization-is-the-root-of-all-evil)

### SQL is an API

If you have an database driven application and a third party tool wants
to send data to the application, then sometimes the easiest solution is
to give the third party access to the database.

You can create a special database user which has only access to one table.
That's easy.

Nitpickers will disagree: If the database schema changes, then the
communication between both systems will break. Of course that's true.
But in most cases this will be the same if you use a "real" API. If
there is a change to the data structure, then the API needs to be
changed, too.

I don't say that SQL is always the best solution. Of course http based
APIs are better in general. But in some use cases doing more is not
needed.

### C is slow

... looking at the time you need to get things implemented. Yes, the
execution is fast, but the time to get the problem done takes "ages". I
avoid C programming, if possible. If Python gets to slow, I can optimize
the hotspots. But do this later. Don't start with the second step. First
get it done and write tests. Then clean up the code (simplify it). Then
.... What is the next step? Optimize? On most cases the customer has new
needs and it is likely that he wants new features not faster execution.

Higher level languages have a better "zero to [MVP](https://en.wikipedia.org/wiki/Minimum_viable_product)" speed.

### Version Control: git

For version control of software I use git. I think all other tool (svn,
mercurial, cvs, darcs, bazaar) can be considered "dead". See
[StackOverflow TagTrend](http://sotagtrends.com/?tags=git+svn+mercurial+cvs+darcs+bazaar)



### Avoid long living branches

Avoid long living branches in your git repos. The more time that passes,
the less likely is that your work will ever get merged. For me two weeks
are ok, but five weeks are too long.

Ten lines of improvement which get pushed to master today have much more value
than 1000 of lines which are in branch which will never get pushed to master.

### Not one branch per customer

Some people use git branches to store the individual settings for
customers or installations. Don't do this. 

Creating one git repo for every customer or installation is strange, too. This might work if you have 
less than 50 customers. But this won't work for 3000 customers.

### Don't put generated code into version control

Please read [Source code vs generated code](#source-code-vs-generated-code). Generated code or binary
data should not be in a git repository. It is possible, but strange.

### The best commits remove code

For me, the best commits adds some lines to the docs, add some lines to
tests and removes more lines than it adds to the production code.

### Time is too short to run all tests before commit+push

If the guideline of your team is: "Run all tests before commit+push",
then there is something wrong. Time is too short to watch tests running!
Run only the tests of the code you touched `py.test -k my_keyword`.

It's the job of automated CI (Continuous Integration) to run all tests.
That's not your job.

### CI

Use continuous integration. Only tested code is allowed to get deployed.
This needs to be automated. Humans make more errors than automated
processes.

I documented how to set up github commit, travis CI, bumpversion, Upload
to pypi: <https://github.com/guettli/github-travis-bumpversion-pypi>

All I need to do is to commit. All other steps are automated :-)

### Tests should work offline

Imagine a developer sits in a train and has an unreliable network connection.

Nevertheless I want that all tests can get executed.

For simple unit-tests which don't need a server this is easy.

But if you test needs a http-server, a database (PostgreSQL, MySQL),
a key-value DB (redis), ... What can you do?

Automation is the solution. You can use a tool like Ansible to set up
the needed environment.


### CI Config

CI tools (gitlab, travis, jenkins) usualy have a web gui. Keep the
things you configure with the GUI simple. Yes, modern ci tools can do a
lot. With every new version they get even more turing complete (this was
a joke, I hope you understood it). Please do speration of concerns. The
CI tool is the GUI to start a job. Then the jobs runs, and then you can
see the result of the job in your browser. If you do configure condition
handling "if ... then ... else ..." inside the web-gui, then I think you
are on the wrong track.

The ci tool calls a command line. To make it easy for debugging and
development this job should be callable via the command line, too. With
other word: the web GUI gets used to collect the arguments. Then a
command line script gets called. Then the web GUI displays the result
for you. I think it is wise to avoid a complex CI config. If you want to
switch to a different ci tool (example from jenkins to gitlab), then
this is easy if your logic is in scripts and not in ci tool
configuration.

### Avoid Threads and Async

Threads and Async are fascinating. BUT: It's hard to debug. You will
need much longer than you initially estimated. Avoid it, if you want to
get things done. It's different in your spare time: Do what you want and
what is fascinating for you.

There is one tool and one concept that is rock solid, well known, easy
to debug and available everywhere and it is great for parallel
execution. The tool is called "operating system" and the concept is
called "process". Why re-invent it? You think starting a new process is
"expensive" ("it is too slow")? Just, do not start a new process for
every small method you want to call in parallel. Use a [Task
Queue](https://www.fullstackpython.com/task-queues.html). Let this tool
handle the complicated async stuff and keep your own code simple like
running in one process with one thread. It is all about IPO:
Input-Processing-Output.

There is a good reason to use async: The [C10k
Problem](https://en.wikipedia.org/wiki/C10k_problem). BUT: I guess you
don't have this problem. If you don't have this problem, then don't use
technology which was invented to solve this issue :-)

Related part of the [Google Codereview Guidelines "Functionality"](https://google.github.io/eng-practices/review/reviewer/looking-for.html#functionality)

### Don't waste time doing it "generic and reusable" if you don't need to

If you are doing some kind of software project for the first time, then
focus on getting it done. Don't waste time to do it perfect, reusable,
fast or portable. You don't know the needs of the future today. One main
goal: Try to make your code easy to understand without comments and make
the customer happy. First get the basics working, then tests and CI,
then listen to the new needs, wishes and dreams of your customers.

Example: If you are developing web or server applications, don't waste
time for making your code working on Linux and MS-Windows. Focus on one
platform.

See [Minimum viable
product](https://en.wikipedia.org/wiki/Minimum_viable_product)

Related Book: [The Lean Startup](http://theleanstartup.com/book)

Several month after writing above text I found this 

[Google Codereview Guidelines "Complexity"](https://google.github.io/eng-practices/review/reviewer/looking-for.html#complexity)
> A particular type of complexity is over-engineering, where developers have made the code more generic than it needs to be, or added functionality that isn’t presently needed by the system. Reviewers should be especially vigilant about over-engineering. Encourage developers to solve the problem they know needs to be solved now, not the problem that the developer speculates might need to be solved in the future. The future problem should be solved once it arrives and you can see its actual shape and requirements in the physical universe.

### Use a modern IDE

Time for vi and emacs has passed. Use a modern IDE on modern hardware
(SSD disk). For example PyCharm. I switched from Emacs to PyCharm in
2016. I used Emacs from 1997 until 2015 (18 years).

### Easy to read code: Use guard clauses (early return)

Guard clauses (early return) help to avoid indentation. It makes code
easier to read and understand. See
<http://programmers.stackexchange.com/a/101043/129077>

Example:

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

Look at the actual code which does something. I used five lines with
.... points for it. I think more indendation, makes the code more
complex. The "return" simplifies the code. For me the second version is
much easier to read.

For Python there exists a "complexity checker": [Design
checker](http://pylint.pycqa.org/en/latest/technical_reference/extensions.html#design-checker).

### Source code vs generated code

I guess every young programmer wants to write a tool which automatically
creates source code. Stop! Please think about it again. What do you
gain? Don't confuse data and code. Imagine you have a source code
generator which takes DATA as input and creates SOURCE as output. What
is the difference between the input (DATA) and the output (SOURCE)? What
do you gain? Even if you have some kind of artificial intelligence, you
can't create new information if your only input is DATA. It is just a
different syntax. Why not write a program which reads DATA and does the
thing you want to do?

For the current context I see only two different things: **source code**
for humans and **generated code** for the machine.

Just because a file contains code of a programming language, this does
not means that this file is source code.

If the TypeScript compiler creates JavaScript, then the output is
generated code since the created JavaScript source is intended for the
interpreter only. Not for the human. If you create JavaScript with a
keyboard and a text editor it is source code. Don't mix source code and
generated code in one file.

With other words: source code gets created by humans with the help of an
editor or IDE.

### Don't believe the "automatically create foo" hype

If you are new to software development you are fascinated by the magic.
You can create things! In this section I call the magic output "foo".

Yes, you can automatically create foo with a script. Whatever "foo" is
in your context: It has no value. It is worth nothing. It is dust in the 
wind like a web page. This output is only temporarily valuable. 

Look at the basic IPO pattern: Input - Processing - Output (in this case
"foo").

Do not store "foo", the output of your script, in a database. Do not
store "foo" in version control.

It has no value since you can always create "foo" again. You just need
the input and your script.

You can store "foo" in a cache to improve performance. But do not store
it permanently. Don't make backup of it.

A term which is often a hint to this anti-pattern is "generator". Yes,
you can generate a lot of data. But this bloated, generated data is just hot air with
little value.

DevOps who prefer "Op" to "Dev" tend to create configuration with a script.
You can do this, but then create the config again daily. Do not edit
the generated config by hand.


### Regex are great - But it's like eating rubbish

Yes, I like regular expression. But slow down: What do I do, if I use a
regex? I think it is "parsing". I remember to have read this some time
ago: "Time is too short to rewrite parsers". Don't parse data! We live
in the 21 century. Consume high level data structures like json, yaml or
protcol buffers. If possible, refuse to accept CSV or custom text format
as input data.

From time to time you need to do text processing. Unfortunately there
are several regex flavors. My guide-line: Use PCRE. They are available
in Python, Postfix and many other tools. Don't waste time with other
regex flavors, if PCRE are available.

Current Linux distributions ship with a grep versions which has the -P
option to enable PCRE. AFAIK this is the only way to grep for special
characters like the binary null: [How to grep for special
character](https://superuser.com/a/612336/95878)

### CSV - Comma-separated values

CSV is not a data format. It is an illness. See the introduction at:
<https://docs.python.org/3/library/csv.html>

If your customer sends you tabular data in Excel, read the excel
directly. Do not convert it to CSV just because you think this is
easier.

If a customer wants you to send him CSV, ask if he can consume JSON.

There are great libraries for reading and writing Excel. For example:
[openpyxl](https://openpyxl.readthedocs.io/en/stable/)

### Give booleans a "positive" name

I once gave a DB column the name "failed". It was a boolean indicating
if the transmission of data to the next system was successful. The
output as table in the GUI looked confusing for humans. The column
heading was "failed". What should be visible in the cell for failed
rows? Boolean usually get translated to "Yes/No" or "True/False". But if
the human brain reads "Yes" or "True" it initially things "all right".
But in this case "Yes" meant "Yes, it failed". The next time I will call
the column "was\_successful", then "Yes" means "Yes, it was successful".
Some GUI toolkits render "True" as a green (meaning "everything is ok")
hook and "False" as a red cross (meaning "it failed").

### Love your docs

I have seen it several times on github. Just have a look
at the README files on github. They starts with "Installing", then
"Configuring" ... What is missing? An Introduction! Just some sentences
what this great project is all about. Programmers prefer the details to the big picture,
the overview. 

Dear programmers, learn to relax and look at the thing you create like a new
comer. Imagine a new comer who know how to add two integers with his favorite programming
language. What is missing to make him understand why the project/lib/tool is needed?

First you need to convince him that this project is worth a try, then if he knows
the "why?", then explain how to install it.

If you have this mind set "I do the important (programming)
stuff. Someone else can care for the docs", then your open source
project won't be successful.

If you write docs, then do it for new comers. Start with the
introduction, define the important terms, then provide the simple and straightforward use
cases. Put details and special cases at the end.

If your library gets used and you add a bug, you will get feedback soon.

Tests fail or even worse customers will complain.

But if you write broken docs, no one will complain.

Even if someone reads your mistake, it is unlikely that you get
feedback. Unfortunately only few people take this serious and tell you
that there is a mistake in your docs.

How to solve this?

You need to act.

Let someone else read your docs.

The quality of feedback you get depends on the type of person you ask to
read your docs.

If it is a programmer, it is likely that he does not read your docs
carefully. Most software developers do not care for orthography and it
is hard for them to read the docs like a new comer. They already know
what's writen there, and they will say "it is ok".

My solution: resubmission: Read the text again 30 days later.

### Canonical docs

Look at the question concerning OpenSSH (aka `ssh`) options at the Q+A site serverfault.
There is a lot of guessing. Something is wrong. Nobody knows where the
canonical upstream docs are. Easy linking to specific configuration is not
possible. What happens? Redudant docs. Many blog posts try to explain
stuff.... Don't write blog posts, instead improve the upstreams docs. Talk with
the core developers. Open an issue in the issue tracker if you think there is
something missing in the docs.

Open an issue if the docs start with the hairy details and don't start
with an introduction/overview. Developers don't realize this, since they
need to deal with the hairy details daily. Don't be shy: Help them to
see the world through the eyes of a new comer.

I am unsure if I should love or hate "wiki.archlinux.org". On the one
hand I found there valuable information about systemd and other linux
related secrets. On the other hand it is redundant and since a lot of
users take their knowledge from this resource, the canonical upstream
docs get less love. First determine where the canonical upstream docs
are. Then communicate with the maintainers. Avoid redundant docs.

With other words: Blog posts are nice, but they are like dust in
the wind. They explain a snapshot. Three months later they are outdated.
It makes more sense to add one missing sentance to the upstream docs,
then to create a blog post explaining something which is not explained
in the docs. At least in the open source world. Since it is more likely
that you are able to influence the upstream docs.

### Do not send long instructions to customers via mail

If you send long instructions to customers via mail, then these docs in
the mail are hidden magic. Only the customer who receives this mail
knows the hidden magic.

Publish your docs in your app. Send your customer a link to the online
docs.

Despite all myth: There are users who read the docs.

And that's great, if the user has more knowledge. Because this means you
have less work. Less mails, less interrupts, less phone calls :-)

This even applies to public discussion forums. Don't write too much. Create great docs and answer questions by
providing links to the docs. And be polite and include the question if this answers the question of the user.

[Permalinks](https://en.wikipedia.org/wiki/Permalink) are great, since they provide a single source of truth.


###  Don't write tech-docs in non-english language

General rule: don't waste time.

It is feasible to write high level blog posts about tech topics in your favorite language.

Sometimes it is easier to communicate the holistic view in your mother-tongue.

But it is not feasible to write detailed tech stuff in a non-english language.

Example:

https://wiki.ubuntuusers.de/Installation_auf_externen_Speichermedien/

I came across this page because I want to install Linux on an external hard disc.

Unfortunately there seems to be no good English guide how to do this.

The most solid guide was above link. Unfortunately above guide is outdated.

Grrrrrr. Now I need to choose:

* V1: Should I update the outdated german guide? It is a wiki editable by everybody.

* V2: I use a english guide, but they look not solid. 

Grrr. I don't like thinking.

The people who created the German guide thought they help the world. They felt good
while doing what they did. I think they wasted time. Automatic translations are quite
good today. At least if you translate English to your favorite language.
I won't update the outdated German guide in the wiki. This would help only very few people.
Most people which want to install Linux on an external hard drive can either
read English text or they know who to translate Englisch text to their favorite
language. I would update an Englisch wiki page, since this would help a lot of people.

Don't get me wrong: Docs for applications you write should be in the language of your customers. Above text
is about tech related docs.

Again: Don't write tech-docs in non-english language

### Care for newcomers

In the year 1997 I was very thankful that there was a hint "If unsure
choose ..." when I needed to compile a linux kernel. In these days you
need to answer dozens question before you could compile the invention of
Linus Torvalds.

I had no clue what most questions where about. But this small advice "If
unsure choose ..." helped me get it done.

If you are managing a project: Care for new comers. Provide them with
guide lines. But don't reinvent docs. Provide links to the relevant
upstream docs, if you just use a piece of software.

### Keep custom IDE configuration small

Imangine you lost your PC and you lost your development environment:

-   IDE configuration
-   Test data
-   Test database

All that's left is your source code from version control, CI servers and
deployment workflow.

How much would you lose? How much time would you waste to set up your
personal development environment again?

Keep this time small. This is related to "care for new comers". If you
need several hours to setup your development environment, then new team
members would need even much more time.

### Setting up a new development environment should be easy

This happened to me several times: I wanted to improve some open source
software. Up to now I only used the software, now I want to write a
patch. If setting up a new development environment and running the tests
is too complicated or not documented, then I will resign and won't
provide a patch. These steps need to be simple for people starting from
scratch:

-   check out source from version control
-   check that all tests are working (before modifying something)
-   write patch and write test for patch
-   check that all tests are working (after modifying something)

### Passing around methods make things hard to debug

Even in C you can pass around method-pointers. It's very common in
JavaScript and sometimes it gets done in Python, too. It is hard to
debug. IDE's can't resolve the code: "Find usages" don't work. I try to
avoid it. I prefer OOP (Inheritance) and avoid passing around methods or
treating them like variables.

I don't like [Closures (wikipedia)](https://en.wikipedia.org/wiki/Closure_(computer_programming)).
Reason: Anonymous functions have no name. If you use a [Statistical Profiler](https://en.wikipedia.org/wiki/Profiling_%28computer_programming%29#Statistical_profilers), you don't see
immediately in which function the interpreter spent its time.

For simple use cases like a custom compare operator for sort methods closures are
ok.

I like it simple: Input-Processing-Output.

With "Input" being 100% data. Not a method.

### Software Design Patterns are overrated

If you need several pages in a book to explain a software design
pattern, then it is too complicated. I think Software Design Patterns
are overrated.

Why are so many books about software design patterns and nearly no books
about database design patterns?

### OOP is overrated

About OOP (Object-oriented programming)

**Stateless** has won. OOP is stateful:

1. Create an instance of a class
2. Call a method of this instance
3. Destruct the instance

Three steps vs one step.

OOP is great for implementing an [ORM (Object-relational mapping)](https://en.wikipedia.org/wiki/Object-relational_mapping). But implemeting this should be done by people who have more experience than I have :-)

### Test Driven Development

red, green, refactor. More verbose: make the test fail, make the test
pass, refactor (simplify) code.

### From bug to fix

First make your bug reproducible. If it is reproducible, then it is easy
to fix it.

Make it reproducible in a test.

Imagine there is a bug in your method do\_foo(). You see the mistake
easily and you fix it. Done?

I think you are not done yet. I try to follow this guideline:

Before fixing the bug, search test\_do\_foo(). There is no test for this
method up to now? Then write it.

Now you have test\_do\_foo().

You have two choices now: extend test\_do\_foo() or write
test\_do\_foo\_\_your\_special\_case(). I use the double underscore
here.

Make the test fail (red)

Fix the code. Test is green now.

Slow down. Take a sip of tea. Look at your changes ("git diff" in your
preferend IDE). Is there a way to simplify your patch? If yes, simplify
it.

Run the "surrounding tests". If do\_foo() is inside the module "bar".
Then run all tests for module "bar" (I use py.test -k bar). But if this
would take more then three minutes, then leave the testing to the CI
which happens after you commit+push (you have a CI, haven't you?)

### Tests and production code go hand in hand.

You implemented the great method foo() and you implement a corresponding
method called test\_foo(). It does not matter if you write foo() first,
and then test\_foo() or the other way round. But it makes sense to store
both methods with one commit to one git repo.

Several months later you discover a bug in your code. Or worse: your
customer discovers it.

If you fix foo() you need to extend test\_foo() or write a new method
test\_foo\_with\_special\_input(). Again both changes (production code
and testing code) walk into the git repo like a pair of young lovers
holding hands :-)

Related [Guideline of Google: Codereview "Tests"](
https://google.github.io/eng-practices/review/reviewer/looking-for.html#tests)

### aaa-tests (smoke tests)

You want to check your source code before commit? Great initiative, you are on
the right track. This source code checking is usualy called [Linting](https://en.wikipedia.org/wiki/Linting).

Now you write a [git pre-commit hook](https://git-scm.com/docs/githooks#_pre_commit) as shell script which does some magic checking/linting and you are
happy.

I think this has a draw-back. You created a second/redundant place where tests happen: In CI and in pre-commit hook.
You invest your valuable time into the pre-commit hook, and finally it might not get called.
If you work in a team it is likely that somebody does not use the pre-commit hook you created.

That's why I suggest this: aaa-test. These test pass quick and check basic stuff. It is up to you
how much want to do in these tests. Some basic linting, some basic unit-test, ... Then calls these
tests during CI and can call them in your (optional) pre-commit hook. 

Some call this "smoke tests".

In most development environments you can execute tests by giving a part of the name. Example: `pytest -k aaa` executes all test containing "aaa". 
This has the benefit that you can add a new test anywhere you like. You don't need to register this somewhere.


### Creating test data is much more important than you initialy think

Creating test data is very important. It can help you for several
things:

1: It can help you to create a re-usable application. If you have only
one customer, it does not matter. But the real benefit of software is its re-usabilty.
Your code wants to get executed by several customers. As soon as you have two or more 
customers you need a neutral test environment which is no specific to one of your customers.
It is a lot of work to create a neutral test environment, if you have not done it from
day one. But the work only needs to be done once and helps in the long run.

2: It can help you to create presentation/demo systems.

3: It can help you in automated tests.

Your tests should not run on real data from customers.

If you create test data this should be automated. This way you are able
to fill a new database with useful data. You should be able to create a
demo system with one command (or one click).

Write the creation of test data once and use it for both: presentions
and automated tests.

Do not use random data for testing. It just makes no sense: tests should
be reproducible.

I don't see why a special library for creating test data is needed. If
you use an ORM in your production code, then use the ORM to create your
test data.

In Python/Django I use cached-properties and
MyModel.objects.update\_or\_create(...) to create the test data.

### This is untestable code

If you are new to software testing, then you might think ... "some parts
of my code are *untestable*".

I don't think so. I guess your software uses the [IPO pattern](https://en.wikipedia.org/wiki/IPO_model): Input, Processing, Output. The
question is: How to feed the input for testing to my code? Mocking,
virtualization and automation are your friends.

The "untestable" code needs to be cared of. Code is always testable,
there is no untestable code. Maybe your knowledge of testing is limited
up to now. Finding untestable code and making it testable is the
beginning of an interesting adventure.

### Is config code or data?

This is a difficult question. At least at the beginning. For me most
configuration is data, not code. That's why the config is in a
**database**, not in a text or source code file in a version control
system.

This has one major draw-back. All developers love their version control
system. Most love git. It is such a secure place. Nothing can get lost
or accidently modified. And if a change was wrong, you can always revert
to an old version. It is like heaven. Isn't it?

No it is not. The customer can't change it. The customer needs to call
you and you need to do stupid repeatable useless work.

For me configuration should be in the database. This way you can provide
a GUI for the customer to change the config.

The configuration and recipies for the configuration management is
stored in git. But this is a different topic. If I speak about
configuration management, then I speak mostly about configuring linux
servers and networks (aka [Infrastructure as code](https://en.wikipedia.org/wiki/Infrastructure_as_code)). In my case this is nothing which my customer
touches.

### ForeignKey from code to DB

This code uses the ORM of django

``` {.sourceCode .python}
if ....:
    issue.responsible_group=Group.objects.get(name='Leaders')
```

`Group` is a class and referes to a table with the same name. Each group has a name. There
is one group (one row) with the name "Leaders".

Above code is dirty because 'Leaders' is like a ForeignKey from code to
a database row.

How to avoid this?

Create global config table in your database. This table has exactly one
row. That's the global config. There you can create column called
"Leaders" and there you store the ForeignKey to the matching group.

### Testcode is conditionless

Testcode should not contain conditions (the keyword if). If you have
loops (for, while) in your tests, then this looks strange, too.

Tests should be straight forward:

> 1.  Build environment: Data structures, ...
> 2.  Run the code which operates on the data structures
> 3.  Ensure that the output is like you want it to.

``` {.sourceCode .python}
class MyTest(unittest.TestCase):
    def test_foo(self):
        foo=Foo()
        self.assertEqual(42, foo.find_answer())
```

### Don't search the needle in a haystack. Inject dynamite and let it explode

Imagine you have a huge code base which was written by a nerd which is
gone since several months. Somewhere in the code a database a row gets
updated. This update should not happen, and you can't find the relevant
source code line during the first minutes. You can reproduce this
failure in a test environment. What can you do? You can start a debugger
and jump through the lines which get executed. Yes, this works. But this
can take long, it is like "Searching the needle in a haystack". Here is
a different way: Add a constraint or trigger to your database which
fires on the unwanted modification. Execute the code and BANG - you get
the relevant code line with a nice stacktrace. This way you get the
solution provided on a silver plattern with minimal effort :-)

With other words: Don't waste time with searching.

Sometimes you can't use a database constraint to find the relevant
stacktrace, but often there are other ways.....

If you can't use a database constraint, maybe this helps: Raise
Exception on unwanted syscall
<http://stackoverflow.com/a/42669844/633961>

If you want to find the line where unwanted output in stdout gets
emitted: <http://stackoverflow.com/a/43210881/633961>

If you have a library which logs a warning, but the warning does not
help, since it is missing important information. And you have no clue
where this warning comes from. You can use this solution:
<http://stackoverflow.com/a/43232091/633961>

### Avoid magic or uncommon things

-   hard links in linux file systems.
-   file system ACLs (Access control lists). Try to use as little as
    possible chmod/chown.
-   git submodules (Please use configuration management, deployment
    tools, ...)
-   [seek()](https://en.cppreference.com/w/c/io/fseek). Stateless is
    better. If you use seek() the file position is a state. Sooner or
    later the position (state) will be wrong.
-   Scripts which get executed via OpenSSH
    [ForceCommand](http://man.openbsd.org/OpenBSD-current/man5/sshd_config.5#ForceCommand)
    or "command" in .ssh/authorized\_keys. SSH is not an API use http.

### Avoid writing a native GUI

Imagine you have developed web applications up to now. You have never
developed a native gui before. Now a new potential customer has a use
case and you think: This time a native GUI would be a good solution.

Caution: slow down. Developing a native gui is much more work and needs
much more time than you think.

The edit, compile, run cycle is much longer. This will slow you down.

If you develop a native GUI, you might need several mouse clicks until
you reach the part where you improving the current code. And like all
humans, you are not perfect, and you have a typo. The application
crashes, and you need to do the edit, compile, run, five clicks cylce
again...

Compare this to a web application: You do not need to do five clicks to
reach the part where you improve the current code. You just hit ctrl-r
and reload the page. The stateless http protocol makes this possible. I
love it.

Next argument: The native GUI community is tiny compared to web
development. If you have a question, you have only a few people to talk
to.

I am at the Chemnitzer Linux Days yearly, and meat a lot of new comers
there. Some people new to software development think: "I just want to
develop a simple app for me. No need to run a web server. I want a real
application running on my pc."

My advice: use Python and Django. The things you learn have more value.
The knowledge you gain can be used to build cool stuff. If you have a
question, there is always someone who has an advice.

See the [TagTrend gtk, qt,
django](http://sotagtrends.com/?tags=%5Bgtk,qt,django%5D)

### Learn one programming language, not ten.

Most young developers think you need to learn many programming languages
to be a good developer.

Yes, sometimes it helps to know the programming language C.

My opinion: Learn Python, SQL and some JavaScript.

Then learn other topics: PostgreSQL, Configuration management,
continuous integration, organizing, team work, learn to play a music
instrument, long distance running, family

### Learn "git bisect"

"git bisect" is a great tool in conjunction with unittests. It is easy
to find the commit, which introduced an error. Unfortunately it is not a
one-liner up to now. You can use it like this:

``` {.sourceCode .shell}
user@host> git bisect start HEAD HEAD~10 


user@host> git bisect run py.test -k test_something
 ...
c8bed9b56861ea626833637e11a216555d7e7414 is the first bad commit
Author: ...
```

But if your pull-requests get tested before they get merged, then you
hardly need "git bisect".

### Avoid Conditional Breakpoints

Imagine, you are able to reproduce a bug in a test. But you could not
fix it up to now. If you want to create a conditional breakpoint to find
the root of the problem, then you could be on the wrong track. Rewrite
the code first, to make it more fine-grained debuggable and testable.

Modify the source and test where a normal (non-conditional) breakpoint is enough.

It is very likely that this means you need to move the body of a loop
into a new method.

``` {.sourceCode .}
# Old
def my_method(...):
    for foo in get_foos():
        do_x(foo)
        do_y(foo)
        ...
```

``` {.sourceCode .}
# new
def my_method(...):
    for foo in get_foos():
        my_method__foo(foo)

def my_method__foo(foo):
    do_x(foo)
    do_y(foo)
    ...
```

Now you can call `my_method__foo()` in a test, and you don't need a
conditional breakpoint any more. This helps you now (during debugging), but raises
the overall value of the source code in the long run, too. Instead of few big monster methods
you have more small and easy to understand methods which follow the simple input-processing-output model.

### Make a clear distinction between Authentication and Permission Checks

It is important to understand the difference.

**Authentication** happens first: Is the user really Bob, or is there
just someone who pretends to be Bob?

**Permission Checks** Is Bob allowed to do action "foo"? Here we already
trust that the user is Bob and not someone else. I use the term
"Permission Checks" on purpuse since the synonym "Authorization" sounds
too similar to "Authentication".

Related question:
<https://softwareengineering.stackexchange.com/questions/362350/synonym-for-authorization/363690#363690>

General guidelines: Avoid [Homonyms](https://en.wikipedia.org/wiki/Homonym)

### Idempotence is great

Idempotence is great, since it ensures, that it does not do harm if the
method is called twice.

Errors (for example power outage) can happen in every millisecond.
That's why you need to decide what you want:

-   if the power outage happened, some jobs do not get executed.
    Cronjobs work this way.
-   if the power outage happened, some jobs do get executed twice to
    ensure they get done.

Further reading:
<http://docs.celeryproject.org/en/latest/userguide/tasks.html> (I don't
use celery, but I like this part of the docs)

<https://en.wikipedia.org/wiki/Idempotence>

### File Locking is deprecated

In the past [File\_Locking](https://en.wikipedia.org/wiki/File_locking)
was a very interesting and adventurous topic. Sometimes it worked,
sometimes not, and you got interesting edge cases to solve again and
again. It was fun. Only hard core experts know the difference between
fcntl, flock and lockf.

.... But on the other hand: It's too complicated, too many edge cases,
too much wasting time.

There will be chaos if there is no central dispatcher.

I like tools like <http://python-rq.org/> It is simple and robust.

BTW, the topic is called
[Synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)).

Further reading about "task queues":
<https://www.fullstackpython.com/task-queues.html>

### No nested directory trees

If you store files, then avoid nested directory trees. It is complicated
and if you want to use a storage server like
[S3](https://en.wikipedia.org/wiki/Amazon_S3) later, you are in trouble.

Most storage servers support containers and
[blobs](https://en.wikipedia.org/wiki/Binary_large_object) inside a
container. Containers in containers are not supported, and that's good,
since it makes the environment simpler.

### Code doesn't call mkdir

Code runs in an environment. This environment was created with
configuration management. This means: source code usualy does not call
mkdir. With other words: Creating directories is the part of the
configuration management. Setting up the environment and executing code
in this environment are two distinct parts. If your software runs, the
environment does already exist. Code creating directories if they do not
exist yet, should be cut into two parts. One part is creating the
environment (gets executed only once) and the second part is the daily
executing (which is 100% sure that the environment is like it is. With
other words: the code can trust the environmen that the directory
exists). These two distinct parts should be seperated.

How to create directories if I should not do it with my software? With
automated configuration management (Ansible, Chef, ...) or during
installation (RPM/DPKG).

Exception: You create a temporary directory which is only needed for
some seconds. But since switching from subprocess/shell calling to using
libraries (see "Avoid calling command line tools") temporary files get
used much less.

### Debugging Performance

I use two ways to debug slow performance:

> -   Logging and profiling, if you have a particular reproducable use
>     case
> -   Django Debug Toolbar to see which SQL statements took long in a
>     http request.
> -   Statistics collected on production environments. For Python:
>     <https://github.com/uber/pyflame> or
>     <https://github.com/benfred/py-spy>

### You provide the GUI for configuring the system. Then the customer (not you) uses this GUI

I developed a workflow system for a customer. The customer gave me an
excel sheet with steps, transitions and groups.

The coding was the difficult part.

Then I configured the system according to the excel sheet.

The code was bug free, but I made a mistake when I entered the values
(from excel to the new web based workflow GUI).

The customer was upset, because the configuration contained mistakes.

I learned. Now I ask if it would be ok if I provide the GUI and the
customer enters the configuration. In most cases the customer likes to
do this.

There is a big difference. The customer feels productive if he does
something like this. I hate it. I care for the database design and the
code, but entering data with copy+paste from the Excel sheet ... No I
don't like this. Results will be better if you like what you do :-)

For detail lovers: No, it was not feasible to write a script which
imported the excel sheet to the database. The excel sheet was not well
structured.

*give a man a fish and you feed him for a day; teach a man to fish and
you feed him for a lifetime*

### Better error messages

If you have worked with Windows95, then you must have seen them: Empty
error messages with just a red icon and a button labeled "OK". You had
no clue what was wrong. On the one hand it was great fun, on the other
hand it was very sad, since you wasted your precious time.

Do it better.

Imagine user "foo" wants to access data (lets call it "pam") which you
only can see, if you are in the group "baywatch". Unfortunately user
"foo" is not in the group. You could show him the simple message
"permission denied". And no further information.

I don't like messages like this. They create extra work. The user will
call the support and ask the question "Why am i not allowed to see the
data?". The support needs to check the details.... and soon a half hour
of two people is gone.

Provide better error messages: In this particular case be explicit and
let the code produce a message like: "to access the data you need to be
in one of the following groups: baywatch, admin, ...".

Software security expert might disagree. I disagree their disagreement.
Hiding the facts is just "Security through obscurity".

### Avoid clever guessing

These days I needed to debug a well known Python library. It works fine,
but you don't want to look under hood.

One method accepted a object with three different meanings types as
first argument:

-   case1: a string containing html markup
-   case2: a string containing a file path. This file contained the html
    to work on.
-   case3: a file descriptor with a read() method.

This looks convinient at the first sight. But in the long run it makes
things complicated. This kind of guessing can always lead to false
results. In my case I always used case1 (it contained a small html
snippet). But, once the string was a accidently the name of an existing
directory! This crashed, because the library thought this is was a
file....

Conclusion: STOP GUESSING.

In Python you can use classmethods for alternative constructors.

``` {.sourceCode .}
# case 1
obj = MyClass.from_string('.....')

# case2
obj = MyClass.from_file_name('/tmp/...')

# case3
with io.open('...') as fd:
    obj = MyClass.from_file_object(fd)
```

### Don't stop with "permission denied"

In most non trivial projects there are several reasons why the
permission was denied.

If you (the software developer) only return "permission denied", then
the user/admin don't know the **reason**.

If you add a reason, then it is more likely that the user/admin can help
themselves.

This means they don't call you, our a team mate, to solve this.

Less interrupts for your and happy customers, it's easy.

Or more general: Add enough information to error messages, to make it
easier to understand the current situation.

For example you can add hyperlinks to docs/wiki/issue-tracker in you
errors messages.

### OOP: Composition over inheritance

If unsure, then choose "has a" and not "is a".

<https://en.wikipedia.org/wiki/Composition_over_inheritance>

### Cache for ever or don't cache at all

> [Two Hard Things](https://martinfowler.com/bliki/TwoHardThings.html): There are only two hard things in Computer Science: cache invalidation and naming things. -- Phil Karlton


Avoid "maybe". If your http code returns a response you have two choices
concering caching:

-   the web client should cache this response for ever.
-   the web client should not cache this response at all.

If you follow this guide you will get great performance since
revalidation and ETag magic is not needed.

I possible, avoid fiddling with ETag and If-Modified-Since http headers.

But you have to care for one thing: If you cache for ever, whenever you
update your data, you need to give your resource a new URL. That's easy:

For example: Instead of serving the file `/css/base.css` you serve `/css/base.27e20196a850.css`. The string "27e20..." is the md5 sum of the content of the file. Configure your webserver to serve this file with the appopriate "cache forever" headers, and you client will not ask for this file again. 


If you use django, you can use the [ManifestStaticFilesStorage](https://docs.djangoproject.com/en/3.0/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage)


[Best Practices for Speeding Up Your Web Site (Yahoo)](https://developer.yahoo.com/performance/rules.html)

Good introduction to caching: [Caching (Mozilla Foundation)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

### Avoid coding for one customer

Try to avoid to write software just for one customer. If you write code
for one customer, you miss the great benefit of software: You can write
it once and make several customers happy. Of course every business
starts small. But try to create a re-usable product soon.

### Misc

-   [Release early, release
    often](https://en.wikipedia.org/wiki/Release_early,_release_often)
-   [Rough consensus and running
    code.](https://en.wikipedia.org/wiki/Rough_consensus)

### Background tasks should preserving Buffer Cache State

You should know what this article talks about. But of course you don't
need to recall every detail.

<https://insights.oetiker.ch/linux/fadvise/>

<https://github.com/Feh/nocache>

Use case: you use rsync to backup a linux machine. The rsync process
should not slow down the production environment.

By default linux thinks "A process just read file 'foo'. Let's keep the
content in the buffer cache". But rsync runs in background and it does
not touch the same file twice.

It makes not sense to store the files which get read by rsync in the
buffer cache. The buffer cache should be available for the production
environment.

### Avoid POSIX locale

Avoid the [locale](https://en.wikipedia.org/wiki/Locale_(computer_software)). It causes your code do behave different in different environments. Your code might working during development and CI. But it might fail in production if there is a different locale active.

It is broken by design: First you call `setlocal()` and after that methods do different things. That's stateful and confusing.

It does not follow the simple input-processing-output model.

I wasted too many hours with it. For example: [SAP PyRFC Bug #142](https://github.com/SAP/PyRFC/issues/142)

A library should always return the same output given the same input. The result should not
be different for different locales.

And the GUI? The user wants to use her/his favorite language. But native GUIs fade, and web GUIs come. And modern
web GUIs don't use locale any more. Here you see "i18next vs locale" on [Stackoverflow tag trend](http://sotagtrends.com/?tags=[i18next,locale])

### Datetime class vs "seconds since 1970"

There are two ways to work with dates:

* old: Seconds since 1970 (the [Unix Epoch](https://en.wikipedia.org/wiki/Unix_time))
* new: Datatypes like [datetime](https://docs.python.org/3/library/datetime.html#datetime.datetime)

If unsure use the new datatypes. Don't fiddle with seconds since 1970 any more.

Exception: Since file systems store the mtime (modification time) in "seconds since 1970" and you only want the age of the file in seconds, then it is simpler to stick to the old way. Related: [getmtime() vs datetime.now()](https://stackoverflow.com/questions/58587891/getmtime-vs-datetime-now)


### Statistical Profiler

Debugging and profiling is easy in a development environment. But how to debug a running production system?
A [Statistical Profiler](https://en.wikipedia.org/wiki/Profiling_%28computer_programming%29#Statistical_profilers) (or Sampling Profiler) is very cool. Every N millisecond the stacktraces of the processes get dumped. This does not slow down your production environment at all. These dumps can reveal interesting fact. In which source code lines does the running application spend the most time?

There are commercial tools and some open source tools.

For Python there is [py-spy](https://github.com/benfred/py-spy) to dump the stacktraces. The dumps can get analyzed by [speedscope](https://github.com/jlfwong/speedscope).

### Use "master" for development

You confuse new comers, if your development branch has a different name. If you call the development branch "master", then all introduction material at github does apply. And if you code is at github, all people can see that your project is still alive, since the master branch gets displayes per default.

Anecdote: The [tinelic](https://github.com/sergeyksv/tinelic/) project did all the coding in the "development" branch. The master branch was not updated since three years. I thought this project was dead. The maintainer was upset because he recently pushed changes into this branch. See [issue #9](https://github.com/sergeyksv/tinelic/issues/9#issuecomment-558557925)

------------------------------------------------------------------------

## 4. Remote APIs

### Use http, avoid ftp/sftp/scp/rsync/smb/mail

Use http for data transfer. Avoid the old ways
(ftp/sftp/scp/rsync/smb/mail).

If you want to transfer files via http from shell/cron you can use:
[tbzuploader](https://github.com/guettli/tbzuploader).

The next step is to avoid clever
[inotify](https://en.wikipedia.org/wiki/Inotify)-daemons. You don't need
this any more if you receive your data via http.

Why is http better? Because http can validate the data. If it is not
valid, the data can be rejected. That's something you can't do with
ftp/sftp/scp/rsync/smb/mail.

### Avoid Polling

Polling means checking for new data again and again. Avoid it, if
possible. Try to find a way to "listen" for changes. In most databases
you can execute a trigger if new data arrives.

### Provide specific import directories, not one generic

If you still receive files via ftp/scp since you have not switched to
http-APIs yet, then be sure to provide specific input directories.

In the past I recevied files in a directory called "import". Several
third party systems sent data to this directory. It looks easy in the
first place. But sooner or later there will be chaos since you need to
now where the data came from. Was it from third party system FOO or was
the data from third party system BAR? You can't distinguish any more if
you profide only one import directory.

Now we provide import-FOO, import-BAR, import-qwerty ...

### Don't set up a SMTP daemon

If you can avoid it, then refuse to set up a SMTP daemon. If the
application you write should import mails, then do it by using POP3 or
IMAP and poll for new mail N seconds. Setting up a SMTP daemon is easy,
but being responsible for it is effort. Dealing with attacks, keeping an
eye on security announces... Live is easier without being responsible
for a SMTP server.

A SMTP daemon needs to run 24 hours a day. You get trouble if it is
down. Or even worse: it is misconfigured and rejects all mails. These
mails get lost and won't come back.

If the [getmail](http://pyropus.ca/software/getmail/) job is down or is misconfigured it just won't fetch
mails. But it is unlikely that mails get lost.

I know this conflicts with the general guideline "avoid polling".

------------------------------------------------------------------------

## 5. Op

Operation. The last two characters of DevOp.

### Configuration Management

Use a configuration management tool like Ansible.

Use CI here, too. Otherwise only few people dare to make changes. And
this means the speed of incremental evolution to a more efficent way
will decreases.

I do not use RPM/DPKG to configure a system.

Do you know why modern configuration management tools like Ansible use
the term
"[file.absent](https://docs.ansible.com/ansible/latest/modules/file_module.html)"
and not "file.remove"?

[Google search for "Declarative vs
Imperative"](https://www.google.com/search?q=Declarative+vs+Imperative)

#### The magic reload feature of Config Management is not needed any more

Config management tools have a magic reload feature. Imagine you update
the configuration of a webserver. The config management tools can detect
if a restart of a server is needed or not. For example: If the
configuration of the web server was changed, then the webserver gets
reloaded. If the configuration of the web server was not changed, then
there is no need to restart it. Great feature?

Ansible Docs: [Handlers: Running Operations On
Change](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#handlers-running-operations-on-change)

I liked this feature in the past.

Time has changed.

See above for "From CRUD to CRD". Kubernetes is comming. You create
containers, you run containers, you delete them. You don't update them
any more.

The magic reload feature is not needed any more.

Of course this change from statefull servers to stateless containers
does not happen from one day to the next. One thing is sure: Statefull
servers and the need to reload running services after an update will
decrease.

### Config Management: Change file vs put file

Often there are two ways to do configuration management:

-   change a part of a file: "replace", "append", "patch"
-   put a whole file under configuration management.

You have far less trouble if you use "put a whole file". Example: Do not
fiddle with the file /etc/sudoers. Put a whole file into
/etc/sudoers.d/.

### Config Management: No need for custom RPMs/DPKGs

In the past it was common to create a custom RPM or Debian package to
install a file on a server.

For example a SSL cert.

If you have a configuration management tool, then this extra container
(RPM/DPKG) does not make much sense.

### Cron Jobs

A server exists to serve. If the server does not receive requests, why
should the server do something? This results into my rule of thumb:
Avoid cron jobs.

Sometimes you need to have a cron job for house keeping stuff.

Keep cron jobs simple.

In general there are two ways to configure the arguments of a cron job:

-   the command line arguments which are part of the crontab line
-   additional source of configuration: config files or config from a
    database

Avoid mixing these two ways of configuring a cron job. I prefer to
configure the cron job via the later of both ways. This keeps the cron
job simple. My guide line: Do not configure the cron job via optional
command line arguments. Only use required arguments.

### SSH to production-server

I still do interactive logins to production remote-server (mostly via
ssh). But I want to reduce it.

Sooner or later you will make a typo. See this article from GitLab for a
exciting report what happened during a denial of service:
<https://about.gitlab.com/2017/02/01/gitlab-dot-com-database-incident/>
We are humans, and humans make mistakes. Automation helps to reduce the
risk of data loss.

If you are doing "ssh production-server ... vi /etc/..." or "... apt
install": Configuration management is much better. For example ansible.

If you are doing "ssh production-server .... less /var/log/...": No
log-management yet? Get your logs to a central place.

If you are doing "ssh production-server ... rm ...": Please ask yourself
what you are doing here. How can you automate this, to make this
unneccessary in the future.

### Keep your directories clean

There are two kind of files in the context of backup: Files which should
be in the backup and temporary files which should not be in the backup.
Keep you directories clean. In a directory there should be either only
files which should be in the backup xor only files which should not be
in the backup. This will make live easier for you. The configuration of
your backup is easier and cleaning temporary files is easier and looking
at the directory makes more joy since it is clean.

### Avoid logging to files

I still do this, but I want to reduce it. Logs are endless streams.
Files are a buch of bytes with fixed length. Both concepts don't fit
together. Sooner or later your logs get rotated. Now you are in trouble
if you want to run a log checker for every line in your logfile. I mean
the mathematically version of "every line". This gets really complicated
if you want to check every line. Rotating logfiles needs to be done
sooner or later. But how to rotate the file, if a process still write to
it? This is one problem, which was solved several hundred times and each
time different ...

In other words: Avoid logging to files and avoid logrotate. Logging is
an endless stream.

Of course somewhere on the hard disk data gets stored in files. But it is highly
recommended to use a tool where don't fiddle with files daily.

### Use Systemd

It is available, don't reinvent. Don't do double-fork magic any more.
Use a systemd service with Type=simple. See [Systemd makes many daemons
obsolete](https://stackoverflow.com/a/30189540/633961)

### Don't use Systemd "instantiated units"

Systemd allows you to create template and create several services from
this template. See: <http://0pointer.de/blog/projects/instances.html>

First I thought this is great. But some months later I realized: It is
better to have one source for templates: Your configuration management.
If you want several almost equal services, then use templates in your
configuration management.

This makes it simpler.

### etckeeper is handy

The tool etckeeper stores changes in the /etc directory in a git
repository. This does not make much sense for containers. But for
servers which live several weeks it makes sense. You don't need to push
the changes to a different location. It is very handy. Example:

    cd /etc/apt
    git log .

    ---> you see all git commits which changed files in the /etc/apt directory.

But etckeeper is no backup tool. It is just a handy tool to see what has
changed and when this change happened.

We wrap it and dump additional information into /etc/etckeeper/extra/
before "git commit". We add: /var/spool/cron, output of hwinfo, lsblk,
fdisk, pvdisplay, vgdisplay, lvdisplay, dpkg/rpm package list, postgres
config.

Does it make sense to add the output of df into /etc/etckeeper/extra/ ?

I think it makes no sense, since this changes daily. If no change was
made to the configuration, then there should be no commit in /etc/.git.

### If you do coding to implement backup ...

If you do coding/programming to implement your backup of data, then you
are on the wrong track.

It is very likely that you will do it wrong, and this will be a big
risk.

Why? Because you will notice your fault if you try to recover your data.

**Use** a backup tool, even if you love to do programming. **Configure**
it, but don't write it yourself.

### Avoid re-inventing replication

That's what the customer wants from you to implement:

You should transfer data from database A to database B. Every time there
is an update in database A, data should get copied to database B.

Slow down: What you are doing is replication. Replication creates
redundancy and redundancy needs to be avoided.

Why do you want redundancy in your data storage? The only reasons I can
think of are speed/performance and faul-toleranz (like DNS/LDAP).

If replication is really needed, then take the replication tools the
databases offer. Do not implement replication yourself. This is not
trival and experts with more knowledge than you and me have solved this
issue before.

### Master-Master Replication

The real magic is Master-Master Replication. Here are some examples where it gets used:

todo: add examples
## 6. Networking

### No routing on servers

Imagine there are 20 servers in your network. Imagine there are two
network routes. One route goes to a second internal network and the
other route goes to the internet. All 20 servers should be able to
access both networks. There are two ways to solve this:

-   V1: Each of the 20 servers has the two routes configured.
-   V2: There is one default gateway for the 20 servers. Every server
    has one route. (The common term is "default gateway")

Please choose V2. It is simpler, it is easier to understand, it is less
error prone, it is more sane.

### traceroute won't help you

If you have trouble with a tcp connection, then use tcptraceroute. It
can help to find the firewall which blocks your IP packages trying to
get from host A to host B. Again \*tcp\*traceroute. It is the tool for
tcp connection tests (http, https, ssh, smtp, pop3, imap, ...). Reason:
normal traceroute uses UDP, not TCP.

## 7. Monitoring

### Nagios Plugin API (0=ok, 1=warn ...)

Writing Nagios like checks is very simple. The exit status has this
meaning:

-   0: ok
-   1: warn
-   2: error
-   3: unknown

Is this KISS (keep it simple and stupid)? Yes, I think it is **simple**.
You can write a nagios plugin with any language you like. Often less
then ten lines of source code are enough to implement a nagios check.

But on the other hand it is not **stupid**. The checks does two things:
It collects some numbers (for example "How much disk space is left") and
it does evaluate and judge ("only N MByte left, I think this is a
warning"). That's not stupid this is some kind of intelligence.

After writing and working with nagios checks for several years I think
the evaluation of the data should not be done inside the check. Some
data-collector should collect data. Then a different tool should
evaluate the data and judge if this ok, warn or error.

### Checks vs Logs

Checks are mostly for operators and logs are mostly for developers.

Since there are always some temporary network failures, checks help more
than logs do.

Example:

1.  yesterday night at 3:40 there was a temporary network failure and
    this results in log messages.
2.  At 3:45 the network failure was gone.
3.  You look at the log message at 9:15. You don't know: Is this message
    still valid?

Checks get executed again and again.

If a check fails at 3:41 it will be ok some minutes later.

Then you know immidiately that there was **temporary** failure.

Logs are important for developers for debugging.

But in this case, the developer can't do anything usefull. Temporary
network failures happen again and again. That's live. Looking at the log
which was created by a temporary network failure wastes the time of the
developer.

Logs should contain the stacktrace and the local variables of each frame
in the stacktrace (a tool like sentry could be used), if real errors
occur.

------------------------------------------------------------------------

## 8. Communication with others

### Avoid to get a nerd

If you do "talk" with software to databases and APIs daily, your ability
to communicate with humans might decrease.

You might start to think like a computer (at least a bit).

The human mind works completly different, not just bits and bytes. It
has [Emotions](https://en.wikipedia.org/wiki/Emotion)

Avoid to get a Nerd https://en.wikipedia.org/wiki/Nerd

Here some hints:

-   Nerds like complaining. This book can help: "Rethinking Positive
    Thinking: Inside the New Science of Motivation" by Gabriele
    Oettingen. The method is called WOOP.
-   Nerds like to think at their problems first. [Nonviolent
    Communication](https://en.wikipedia.org/wiki/Nonviolent_Communication#Four_components)
    can help.
-   Meet with "normal" people. With "normal" I mean people who do not do
    IT stuff.
-   Raise a family.
-   Do sport
-   Relax

### Avoid stress

Stress trigger your body’s “fight or flight” response. It pushes your
blood into the muscles. That's great if you need to jump onto the side
walk because a fast red race car would hit you. But in your daily life
this "fight or flight" response is hardly needed. You need the energy in
your brain :-)

Avoid stress, relax daily.

On the other hand stress is fun: I like tennis and long distance
running.

Care for both: brain and body.

### Discussion, but no progress? V1, V2, V3, ...

This and the following parts are about "Requirement Engineering".

If a discussion brings not progress, then grab a pen. Start with V1. The
letter V stands for "Solution Variant" or "One strategy of several to
get to a goal". Find a term or short description of the first possible
strategy. Write it down. Then: which other ways could be used? V2, V3,
...

Rember, there is always the last variant: Leave things like they are
today and think about this again N days later.

If you have found several solution variants, then look at them in
detail. Most of the time it is useful to define the need sequence of
steps. You can use the letter "S" for this: S1, S2, S3 ...

A simple example:

In the morning, you wake up.

-   V1: Go to work now
-   V2: Do some more sleeping
-   V3: Try to remember what you dreamed, write it down
-   V4: Do some sports
-   V5: Play piano
-   V6: Recall your personal goals, what is the next step?
-   ...

If you look at V1 in detail you get to a list of steps:

-   S1: get up
-   S2: make bed
-   S3: wash yourself
-   S4: put on clothings
-   S5: eat
-   S6: take bike and ride to work

I think the first letter (V, S) helps if you are brainstorming.

### Avoid Office Documents or UML-tools

Use a way to edit content (use cases, specs, ...) over the internet. Use
an issue tracking system or wiki.

Don't waste time with UML tools. UML is like
[esperanto](https://en.wikipedia.org/wiki/Esperanto). It is (in theory)
a great solution which solves a lot of problems. But somehow it does not
work.

Write down the high level use case, the cardinality and the steps.
Sequence diagrams can be simplified to enumerations: first step, second
step, third step ...

[Sketch](https://en.wikipedia.org/wiki/Sketch_(drawing)) screenshots you
want to build with your team with a pen. I avoid any digital device for
this, since up to now paper or a whiteboard are far more real. If you
need the result in digital format, just take a picture with your cell
phone at the end.

### Communication with Customers: Binary decision "do list" or "do later list"

Define "done" with your customers. Humans like to be creative and if
thing X gets changed, then they have fancy ideas how to change thing Y.
Be friendly and listen: Write these fancy ideas down on the "do later
list".

If the customer have new ideas, let them decide: Should this be on the
"do list" or the "do later list".

If you don't have a definition of done/ready, then you should not start
to write source code. First define the goal, then choose a strategy to
get to the goal.

Focus on a simple working solution first. Add optional stuff to the "do
later" list.

### Tell customers what they should test

I have seen it several times: Software gets developed. The customer was
told to test and ... nothing happens. That's not satisfying since
software developers want to hear that their work does help. If you (the
developer) provide a checklist of things to test, then the likelihood to
get feedback is bigger.

It is wise to create this checklist for testing as early as possible. It
tells the developer the desired result.

### Dare to say "Please wait, I want to take a note"

Most people can listen and write at once. I can't. And I guess a lot of
developers have this problem. I can only do one thing at a time. If you
are telephoning with a customer and he has a lot of things to tell you,
don't fool yourself. You will only remember 4 of 5 issues. Dare to say
"please wait, I want to take a note". This way you can care for all
issues, which results in happy customers.

### Avoid Gossip

Gossip creates an atmosphere which promotes negativity (bad karma).
Avoid to make jokes about other team mates or customers. Yes, there are
people who do strange stuff and who have strange attitudes. Making jokes
about them makes everything worse. Please be aware that this guideline
has a major drawback. Sometimes all people around you are laughing about
a customer or a team mate which is not here right now ... and you are
the only one who is not laughing. It is up to you how to react. Be
patient.

Same for irony and sarcasm. You and your friends might think it is funny. New team members and
other people won't understand you. It is not funny, it is confusing and childish.


### Sometimes you just have to speak out a wish

Anecdote: In the year 2019 I loved to use the Stackoverflow Tag Trend. I wanted to
leave my current context (Python, Django, PostgreSQL Development) and learn new stuff.
In the Javascript ecosystem there where so many tools available and it was difficult
to see which tool was great two years ago, but won't be used today if you can start from 
scratch. The Stackoverflow Tag Trend helped me to see what is hot and what was hot 
some years ago. Working 16 years for the same company I was blinded by routine and missed
a lot of changes outside my small IT context. 

A lot of things which I learned during studing information technology in Dresden from 1996 to
2001 was outdated. I wrote down these things on the [Deadends of IT](https://github.com/guettli/deadends-of-it) page.
The  Stackoverflow Tag Trend worked well, but I had ideas to improve it:

* [Tag aliases can be selected](https://github.com/robianmcd/tag-trends/issues/32)
* I was missing [Link from tag to describition of tag](https://github.com/robianmcd/tag-trends/issues/34)
* [URL could be simplified (no square brackets)](https://github.com/robianmcd/tag-trends/issues/33)
* [It was down for some days](https://github.com/robianmcd/tag-trends/issues/31)

After some weeks all my whishes where implemented by the author.

Win-win: He could improve the usability and for me is more convenient now. I love it. Sometimes you just have to speak out a wish. I am not a native speaker. I think the term "make a wish" does not match. You need to tell the wish to the right person.








------------------------------------------------------------------------

## 9. Epilog

### It is always possible to make things more complicated

It is always possible to make things more complicated. The interesting
adventure is to make things simpler and easier.

### It helps to talk

Most software developers do not talk much. Otherwise they would not have
choosen this job. If you think about something too long, then you get
blind for the obvious and easy solution. It helps to talk.

There is something called [Rubber duck
debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging). This
might help, but talking to humans helps much more. If you find no
solution in 30 minutes. Take a break. Do something different, talk to a
team mate or friend, take a small walk outside.

### Be curious

There is always something you don't have understood up to now. Ask
questions, even if you think you know the answer. For one question,
there are always several answers. If you know one answer, then it is
likely that someone has a better answer.

I like:

-   <https://stackoverflow.com/>
-   <https://softwarerecs.stackexchange.com/>
-   <https://serverfault.com/>
-   And some mailing lists.

Often I just write the question, and don't write about the solution I
have on my mind. If you write about our solution, then the discussion is
narrowed to a simple pro/contra of your idea. Ask the question like a
newbee.

### Creativity Management

A lot of ideas come to my mind, if I am far away from a laptop or pc.
For example if I cylce from home to office or back.

I started with this way of creativity management some years go: I write
a mail to myself.

If I cycle home on a friday evening, I want to keep my mind relaxed and
focused on my family. All work related thoughts should be far away. I
don't want to "carry" around work-related thoughts on the weekend. On
the road from office to home I might have an idea what to do (how to
hunt a strange bug, how to implement a cool feature which needs only a
very little effort and time to implement, ...). I stop (that is great
advantage of riding a bike - I can stop almost always immidiatley, and
take my mobile phone). Then I write a mail to my business adress and now
I am sure: This idea won't get lost. And I am free to have a nice
weekend with my family.

The same happends when I drive from home to office: I have an idea
related to my personal live? I stop and write a mail to my personal
account.

That's how most of this guide-line was created: Most items came to my
mind during cycling, walking, listening to music or laying in bathtub.
Short mail to myself, and some days later I take the mail which contains
just a handfull of words and I formulate it.

### Cut bigger problems into smaller ones

A lot of new comers have problems with this. Here is one example to
illustrate the guideline "Cut bigger problems into smaller ones".

Imagine you are responsible for several servers and you should create
graphs of their disk/cpu usage.

Cut the bigger problem into smaller ones:

-   How to collect the data on one host
-   How to transport the data from the host to a central place?
-   How to store the data in a central database?
-   How to generate the graphs?

BTW, why not use the PostgreSQL feature "Logical Replication"?

### Go with the flow, not with the hype

Flow: With "flow" I mean "mainstream". And mainstream is according to
oxford dictionary: "The ideas, attitudes, or activities that are shared
by most people and regarded as normal or conventional."

Hype: According to wikipedia: "Hype (derived from hyperbole) is
promotion, especially promotion consisting of exaggerated claims."

But how to distinguish between a flow and a hype?

My answer: Stats or more verbose "statistics".

How to get stats?

I like StackOverflow Tag-Trend. For example, you can compare "python"
and "java". Maybe you have been coding Java since several years. You
heard of python once or twice. But is it "flow/mainstream" or is it
"hype"? Since you only know you context and not every developer and
every project in the world, you can't know the answer. Be upright to
yourself: You are like a small ant. You walked severals paths in the
past, but you don't have the helicopter view.

Check this graph: <http://sotagtrends.com/?tags>=\[java,python\] you
will see: Python is not just a hype it is the flow.

Do not trust one source. Take a look at google trends:
<https://trends.google.de/trends/explore?date=today%205-y&q=%2Fm%2F05z1_,%2Fm%2F07sbkfb>

Go with the flow, not with the hype. Check the stats, not just our daily
context.

### Read the Release Notes

Read the release notes and news of the tools you use daily. Looking there twice a year is enough.

I like these release notes:

- [PostgreSQL News](https://www.postgresql.org/about/newsarchive/)
- [Django News](https://www.djangoproject.com/weblog/)
- [What's new in Python](https://docs.python.org/3/whatsnew/index.html)
- [CNCF Announcements](https://www.cncf.io/newsroom/announcements/)

### Blogs I follow

- [Google Cloud Blog](https://cloud.google.com/blog/)
- [Google Testing Blog](https://testing.googleblog.com/)

### Three Mail Accounts

I have three mail accounts:

-   personal mails (family, friends, ...)
-   work related mails
-   mailing lists

### Clean up your desk

Don't forget to clean your desk. I don't write this here because I do it
often and with joy. No, excat the opposite. I write it down since I want
to push myself.

Don't look at all these things on your desk at once. Start on the left
side take the first thing. Where is the best place for this thing single
thing? Unsure? Why not throw it in the trash can? If you are unsure put
it at least in box behind a closed cabinet door. Some month later you
might be able to throw it in the garbage.

Then wipe the dust.

If you have never time do this, then there is something wrong. Slow
down.

### Highlander, "There can be only one"

"Highlander" is a 1986 British-American adventure action fantasy film
with tagline "There can be only one". Thinking like this narrows your
mind. There can be several thousand. Look how successfull ants and bees
work. If someone is better or faster, then smile. Give applaud and say
"wow".

[Don't be evil.](https://en.wikipedia.org/wiki/Don%27t_be_evil) Don't
waste time and mental energie. Applauding if the competitor is better,
was new to me in 2017. I was at Rothenbaum and attended the German Open
(Tennis). The coach of one player was applauding every time the opponent
made a good shot. I was astonished. Why was the coach applauding the
enemy? But this works. If you get angry, you waste energy and you start
to think like a wild and stupid animal. Even if you have made a mistake
or lost some how, no reason not to walk upright.

### Don't waste your time with cheap hardeware

Some people love the [Raspberry
Pi](https://en.wikipedia.org/wiki/Raspberry_Pi). I don't like it. It
does not have enough computing power for my use cases. Yes, the device
is cheap, but I prefer to spend some more money to have more
performance. I don't like waiting.

### Write a diary

I think it helps to write a diary. Sitting down and writing about the
last days help you to reflect the things you did. I helps you to focus
on your goals. Do you have goals? I found out that late (age of 40). A
diary is fun to read several months later. I try to do it at least once
a week. I have three types of diaries.

One on facebook readable for everyone. It contains things from my daily
life, written in german. <https://www.facebook.com/thomas.guttler.52>

There is one on google-plus which contains IT topics (open source,
python, linux, PostgreSQL), written in english and readable by everyone.
<https://plus.google.com/112821159206665920618>

And there is a private which I maintain with Anki. Anki is a flash card
app. The front side is the question and the back side is the answer. I
use the first side for the date and one to three words, and the back
side contains the text. This way I can ask myself what was on my mind
these days. But all this should be fun, not a burden.

### The Bus factor

From Wikipedia: The bus factor is a measurement of the risk resulting
from information and capabilities not being shared among team members

[Bus factor](https://en.wikipedia.org/wiki/Bus_factor)

Avoid to create secret knowledge which is only available to you. Share
knowledge.

Avoid overspecialization of yourself. It will have drawbacks. Imagine
there are some things which only you know. Sooner or later you want to
go on holiday and you want a relaxed holiday. You don't want to be
called on your mobile phone by your boss or a team mate. You want two
weeks off without a single interrupt which is related to your work.

I guess all people love it, if they are important. Everybody loves it,
if someone needs them. But you will get a burnout if no one else can do
the things you do.

Avoid overspecialization of a team mate, too. If a team mates has secret
knowledge and there is no one else who has a clue: Talk. Try to reveal
the things which only one person knows. Tell him about your concerns
(Bus factor). Maybe talk to his boss.

Imagine there is an action which needs to be done roughly twice a year.
For example setting up a new server. Up to now Bob did this everytime.
Talk to your team mates. Explain that every action should be known to at
least two people. In practice this means: The next time Bob won't do it.
It needs to be done by someone else.

If you read above sentences and think "that's not my job, that's the job
of the team leader", then I think it is time stop acting like a dumb
sleeping sheep. Get resonsible. React relaxed if nobody is listening or
understanding your concerns. "The Best Path to Long-Term Change Is Slow,
Simple and Boring."

### Related things I wrote

-   [Deadends of Information
    Technology](https://github.com/guettli/deadends-of-it)
-   [Why I like Django and why I like
    SAP](https://github.com/guettli/why-i-like-django-and-sap)
-   [Leaving the
    autopilot](https://github.com/guettli/leaving-the-autopilot)

### Thank you

-   Robert C. Martin for the book "Clean Coder"
-   Malcolm Tredinnick. Only few people listened like he did. With
    "listen" I mean "trying to understand the conversation partner".
-   Linus Torvalds for the quote "Bad programmers worry about the code.
    Good programmers worry about data structures and their
    relationships.".
-   Bill Gates for the quote "I choose a lazy person to do a hard job.
    Because a lazy person will find an easy way to do it."
-   All people who contribute to open source software (Linux, Python,
    PostgreSQL, ...)
-   All people who ask question and/or answers them at places like
    StackOverflow.
-   People I met during study at HTW-Dresden
-   My teammates at [tbz-pariv](http://www.tbz-pariv.de/).
-   <https://chemnitzer.linux-tage.de/> All people involved in this
    great yearly event.
-   Ionel Cristian Mărieș for the link to bash pitfalls.
-   Audiance at my presenstion at [Python User Group Leipzig 2019](https://www.meetup.com/de-DE/Leipzig-Python-User-Group/events/rbwmtpyzmbnb/)
-   Marco Bakera for hints (mailing-list python-de 2019)
