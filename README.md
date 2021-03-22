# Programming Guidelines

My opinionated programming guidelines. 

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

I was born in 1976. I started coding with basic and assembler when I was
13. Later turbo pascal. From 1996-2001 I studied computer science at
HTW-Dresden (Germany). I learned Shell, Perl, Prolog, C, C++, Java, PHP, and finally Python.

Sometimes I see young and talented programmers wasting time. There are
two ways to learn: Make mistakes yourself, or read from the mistakes
which were done by other people.

This list summarises a lot of mistakes I did in the past. I wrote it, to
help you, to avoid these mistakes.

It's my personal opinion and feeling. No facts, no single truth.

### I need your feedback

If you have a general question, please start a [new discussion](https://github.com/guettli/programming-guidelines/discussions/new).

If you think something is wrong or missing, feel free to open an issue or pull request.

### Relaxed focus on your monitor

Do not look at the keyboard while you type. Have a relaxed focus on your
monitor.

I type with ten fingers. It's like flying if you learned it. Your eyes
can stay on the rubbish you type, and you don't need to move your eyes
down (to keyboard) and up (to monitor) several hundred times per day.
This saves a lot of energy. This is a simple tool to help you to learn touch typing:
[tipp10](https://www.tipp10.com/en/)

Avoid switching between mouse and keyboard too much.

I like Lenovo keyboards with track point. If you want the track point to
have more grip you can use sandpaper. Here is an image to illustrate
what I use [sandpaper-sticked-on-track-point.jpg](https://raw.githubusercontent.com/guettli/programming-guidelines/master/sandpaper-sticked-on-track-point.jpg)

Once I was fascinated by the copy+paste history of Emacs and PyCharm.
But then I thought to myself: "I want more. I am hungry. I want a
copy+paste history not only in one application, but I also want it for the whole
desktop". The solution is very simple, but somehow only a few people use
it. The solution is called a clipboard manager. I use Diodon (Linux) and
CopyQ (for Windows). I use ctrl+alt+v to open the list of last
copy+paste texts.

### Avoid searching with your eyes

Avoid searching with your eyes. Search with the tools of your IDE. You
should be able to use it "blind". You should be able to move the cursor
to the matching position in your code without looking at your keyboard,
without grabbing your mouse/touchpad/TrackPoint and without looking
up/down on your screen.

Compare two files with a diff tool, otherwise, you might get this ugly skeptical frown.

How often per day do you search for the mouse cursor on your screen?
Support your eyes by increasing the cursor size. If you use Ubuntu,
you can do it via [Universal Access / Cursor Size](https://askubuntu.com/questions/1266951/increase-mouse-cursor-size-on-ubuntu-20-04/1266961#1266961)

### Increase font size

During daily work, you often jump from one information snippet to the next
information snippet.

When was the last time you read a text with more than 20 sentences?

I think from time to time you should do so. Slow down, focus on one
text, and read slowly. It helps to increase the font-size. `ctrl-+` is
your friend.

### KISS

Keep it simple and stupid. The most boring and most obvious solution is
often the best. Although it sometimes takes months until you know which
solution it is.

From the book "Site Reliability Engineering" (O'Reilly Media 2016)
<https://landing.google.com/sre/book/chapters/simplicity.html>

Quote:

:   The Virtue of Boring

    Unlike just about everything else in life, "boring" is a
    positive attribute when it comes to software! We don’t want our programs to be spontaneous and interesting; we want them to stick to the script and predictably accomplish their business goals.

Example: [Pure Functions](https://en.wikipedia.org/wiki/Pure_function) are great. They are stateless, their output can be cached forever, they are easy to test.

### Increase the obviousness

But it is not only about code. It is about the experience of all stakeholders: Users, salespeople, support hotline, developers,...

It is hard work to keep it simple. 

One thing I love to do: "Increase the obviousness". 

One tool to get there: Use a central wiki (without spaces), and
define terms. Related text from me: [Documentation in Intranets: My point of view](https://github.com/guettli/intranets)


### Avoid redundancy

See heading.

### Premature optimization is the root of all evil.

The famous quote "premature optimization is the root of all evil." is true.
You can read more about this here [When to optimize](https://en.wikipedia.org/wiki/Program_optimization#When_to_optimize).

### MVP

You should know what an [MVP (minimum valuable product)](https://en.wikipedia.org/wiki/Minimum_viable_product) is. Building an MVP means to bring something useable to your customer, and then listen to their feedback. Care for their needs, not for your vision of a super performant application.

Avoid i18n in MVP. German is my mother tongue. If I develop a MVP for German users, than I won't to i18n. This can be done later, if needed.

------------------------------------------------------------------------

## 2. Data structures

### Introduction

"Bad programmers worry about the code. Good programmers worry about data
structures and their relationships." -- Linus Torvalds (creator and
developer of the Linux kernel and the version control system git)

### Relational Database

I know SQL is..... It is either obvious or incomprehensible. And, yes, it is
boring.

A relational database is a rock-solid data storage. Use it.

When I studied computer science, I disliked SQL. I thought it was an
outdated solution. I tried to store data in files in XML format, used
in memory Berkley-DB, I used an object-oriented database written in Python (ZODB),
I used NoSQL .... And finally, I realized that boring SQL is the best solution
for most cases.

I use PostgreSQL.

I don't like NoSQL, except for caching (simple key-value DB).

The [PostgreSQL Documentation](https://www.postgresql.org/docs/current/index.html) contains
an introduction to SQL and is easy to read.


### Cardinality

It does not matter how you work with your data (struct in C, classes in
OOP, tables in SQL, ...). Cardinality is very important. Using 0..\* is
often easier to implement than 0..1. The first can be handled by a
simple loop. The second is often a nullable column/attribute. You need
conditions (IFs) to handle nullable columns/attributes.

<https://en.wikipedia.org/wiki/Cardinality_(data_modeling)>

If this is new to you, I will give you two examples:

-   1:N --&gt; One invoice has several invoice positions. For example,
    you buy three books in one order, the invoice will have three invoice positions. This is a 1:N relationship. The invoice position is contained in exactly one invoice.
-   N:M --&gt; If you look at tags, for example at the Question+Answer
    site StackOverflow: One question can be related to several
    tags/topics and of course a topic can be set on several questions.
    For example, you have a strange UnicodeError in Python then you can set the tags "python" and "unicode" on your question. This is an N:M
    relationship. One well know example of N:M is user and groups.

### Conditionless Data Structures

If you have no conditions in your data structures, then the coding for
the input/output of your data will be much easier.

### Avoid nullable Foreign Keys

Imagine you have a table "meeting" and a table "place". The table
"meeting" has a ForeignKey to table "place". In the beginning, it might
be not clear where the meeting will be. Most developers will make
the ForeignKey optional (nullable). WAIT: This will create a condition
in your data structure. There is a way easier solution: Create a place
called "unknown". Use this [senitel value](https://en.wikipedia.org/wiki/Sentinel_value) as default. This data
structure (without a nullable ForeignKey) makes implementing the GUI
much easier.

In other words: If there is no NULL in your data, then there will be
less NullPointerException in your source code while processing the data
:-)

Fewer conditions, fewer bugs.

### Avoid nullable boolean columns

\[True, False, Unknown\] is not a nullable Boolean Column.

If you want to store data in a SQL database that has three states
(True, False, Unknown), then you might think a nullable boolean column
(here "my\_column") is the right choice. But I think it is not. Do you
think the SQL statement "select \* from my\_table where my\_column = %s"
works? No, it won't work since "select \* from my\_table where
my\_column = NULL" will never return a single line. If you don't
believe me, read: [Effect of NULL in WHERE clauses
(Wikipedia)](https://en.wikipedia.org/wiki/Null_(SQL)#Effect_of_Unknown_in_WHERE_clauses).
If you like typing, you can work-around this in your application, but I
prefer straightforward solutions with only a few conditions.

If you want to store True, False, Unknown: Use text, integer, or a new
table and a foreign key.

### Avoid nullable characters columns

If you allow NULL in a character column, then you have two ways to
express "empty":

-   NULL
-   empty string

Avoid it if possible. In most cases, you just need one variant of
"empty". Simplest solution: avoid that a column holding character data is allowed to be null.

If you think the character column should be allowed to be NULL,
then consider a constraint: If the character string in the column is not
NULL, then the string must not be empty. This way ensure that there are
is only one variant of "empty".

### SQL: I prefer subqueries to joins

In most cases, I use an ORM to access data and don't write SQL by hand.

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
belong to a category that has expired.


### Use all features PostgreSQL does offer

If you want to store structured data, then PostgreSQL is a safe default
choice. It fits in most cases. Use all features PostgreSQL does offer.
Don't constrain yourself to use only the portable SQL features. It's ok
if your code does work only with PostgreSQL and no other database if
this will solve your current needs. If there is a need to support
other databases in the future, then handle this problem in the future,
not today. PostgreSQL is great, and you waste time if you don't use its
features.

Imagine there is a Meta-Programming-Language META (AFAIK this does
not exist) and it is an official standard created by the ISO (like SQL).
You can compile this Meta-Programming-Language to Java, Python, C, and
other languages. But this Meta-Programming-Language would only support
70% of all features of the underlying programming languages. Would it
make sense to say "My code must be portable, you must use META, you must
not use implementation-specific stuff!"?. No, I think it would make no
sense.

My conclusion: Use all features PostgreSQL has. Don't make your life more
complicated than necessary and don't restrict yourself to use only
portable SQL.

Great features PG has, which you might not know yet:

* [Insert/Update/Delete Trigger](https://www.postgresql.org/docs/current/sql-createtrigger.html)
* "SELECT FOR UPDATE .... SKIP LOCKED" gives you the perfect foundation for a task-queue. For example [Procrastinate](https://github.com/peopledoc/procrastinate)
* [PGAdmin](https://www.pgadmin.org/) nice GUI to configure your databases.
* [Fulltext Search](https://www.postgresql.org/docs/current/textsearch.html)

There is just one hint: Avoid storing binary data in PostgreSQL. An S3 
service like [minio](https://min.io/) is a better choice.

### Where to not use PostgreSQL?

-   For embedded systems SQLite may fit better
    \* Prefer SQLite if there will only be one process accessing the database at a time. As soon as there are multiple users/connections,
    you need to consider going elsewhere
-   TB-scale full-text search systems.
-   Scientific number crunching:
    [hdf5](https://en.wikipedia.org/wiki/Hierarchical_Data_Format)
-   Caching: Redis fits better
-   Go with the flow: If you are wearing the admin hat (instead of the
    dev hat), and you should install (instead of developing) a product,
    then try the default DB (sometimes MySQL) first.

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
then it is **D**urable.

Imagine you have one outer-transaction and two inner transactions.

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

### My customer wants to extend the data schema...

Imagine you created some kind of issue-tracking system. Up until now, you provide attributes like "subject", "description", "datetime created",
"datetime last-modified", "tags", "related issues", "priority", ...

Now the customer wants to add some new attributes to issues. It would be quite easy for you to update
the database schema and update the code.

Maybe you are lucky and you have 100 customers. Then you would like to prefer to spend your time
improving the core product. You don't want to spent too much time on the features which
only one customer wants.

Or the customer wants to update the schema on its own. 

What can you do now?

One solution is EAV: The [Entity–attribute–value model](https://en.wikipedia.org/wiki/Entity%E2%80%93attribute%E2%80%93value_model)

### Why I don't want to work with MongoDB

> MongoDB is a cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas. ([Wikipedia](https://en.wikipedia.org/wiki/MongoDB))

One document in a collection can differ in its structure. For example, most all documents in a collection have an integer value on the attribute "foo", but for unknown reasons, one document has a float instead of an integer. Grrr.

What does the solution look like?

```
return try {
    this.getLong(key)
  } catch (e: ClassCastException) {
    if (this[key] is Double) this.getDouble(key).toLong() else null
  }
```

No! I want a clear schema where all values in a column are of the same type.

Of course, my wish has a draw-back: If you want to upgrade a table in a production relational database, you might have downtime, because the database needs some
minutes to convert all rows to the new schema. But at least in my context, this was never a big problem up until now.

Related: [StackOverflow "class java.lang.Double cannot be cast to class java.lang.Long"](https://stackoverflow.com/questions/65141475/mongodb-class-java-lang-double-cannot-be-cast-to-class-java-lang-long)


------------------------------------------------------------------------

## X. UI

### Mockups help

Start with painting. A [Mockup](https://en.wikipedia.org/wiki/Mockup#Software_engineering) helps.

If you improve an existing application, then take a screenshot and then paint it with expressive colors. I like #ff00ff.

If you doing something from scratch, then create some slides paint it roughly, add numbers to buttons and add a little
text on what should happen if someone pushes the button. Again, use expressive colors, so that it easy to see what is ideation and
what is existing GUI.

You don't need expensive tools like Figma or InVision for this. Especially if I create something new, I like to do it on paper with a pencil and crayons.

Of course, the above hints make no sense if you write a device driver that has no graphical user interface.

### Faceted search

You should know this term: [Faceted search](https://en.wikipedia.org/wiki/Faceted_search)

### FTUE

[First-time user experience](https://en.wikipedia.org/wiki/First-time_user_experience) is very important. Does a user who has never used the application before understanding it immediately? 

### Don't make me think

[Don't make me think](https://en.wikipedia.org/wiki/Don%27t_Make_Me_Think) is the title of a book. I don't think it is necessary to read it. Just remember this title and try to create user interfaces that are easy to understand.

### SEO

I think the best docs about search engine optimization are from the company which creates the currently most popular internet search engine:

[developers.google.com/search](https://developers.google.com/search)

------------------------------------------------------------------------

## 3. Dev

### Input-Processing-Output

There are thousands of programming languages and thousands of ways to exchange data. But finally, it is one concept:

[Input-Processing-Output](https://en.wikipedia.org/wiki/IPO_model)

If you tell your navigation system of your car "Please show me the route to Casablance Pub, Leipzig" or if you write your first program which adds two integers and prints the result.

### Less code, fewer bugs

-   Not existing code is the best: Less code, fewer bugs
-   Code maintained by a reliable upstream (like Python, PostgreSQL,
    Django, Linux, Node.js, Typescript, ...) is more reliable than my code.

### fewer resources, fewer bugs

There are several ways to give data to a method.

Let's have a look at this simple method call: `my_method(some_string)`

You might think there is only of variable which gets accessed by the method?

Let's find more ways this method could get input:

* Environment variables: Maybe setting LANG=de_DE influences the output?
* Filesystem: Maybe the existence or content of a file in the local file system influences the method.
* `my_method()` could access a database, storage, or a cache to read additional data
* Maybe there is a global variable that contains a value that was set by a previous call to `my_method()`
* Maybe the date influences the method. Maybe the method creates a different output at a full moon.
* ...

AFAIK there is no clear name that distinguishes between explicit and implicit input.

You can't avoid implicit input, and it is 100% ok if it is obvious. If your method
should return the data of the user with the id 12345, then your code needs to access
the database.

If the same code works in one environment, but not in a different environment, and you don't know why
then this tool might help: [dumpenv](https://github.com/guettli/dumpenv) it writes the environment to
a list of files, which you can compare with your favorite diff tool (e.g. Meld).



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
-   There should be one-- and preferably only one --obvious way to do it.
-   Although that way may not be obvious at first unless you're Dutch.
-   Now is better than never.
-   Although never is often better than *right* now.
-   If the implementation is hard to explain, it's a bad idea.
-   If the implementation is easy to explain, it may be a good idea.
-   Namespaces are one honking great idea -- let's do more of those!

In the year 2001, I knew these programming languages: Basic, Pascal,
Assembler, C, C++, Prolog, Lisp, Visual Basic, Java, JavaScript, Tcl/Tk,
Perl.

I was unhappy with all of them and looked for a new language. I narrowed
down the languages, I was interested in and there were two choices left.
One was ruby, the other was Python. I choose Python. It looked simpler,
like executable pseudo-code. Since 2001 I use it nearly every work-day.
I like it, and till now, no other language attracts me.

I am not married to Python. I am willing to change. But the next
language needs to be better. Up until now, I see no alternative.

JavaScript has a big benefit, that it can be executed in the browser.
But I don't like it. Why I don't like it? I don't know. Sometimes
feelings are more important than facts.

### CRUD --&gt; CRD

In most cases, the software does create, read, update, delete data. See
[CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)

The "update" part is the most difficult one.

Sometimes CRD helps: Do not implement the update operation. Use
delete+create. But be sure to use transactions to avoid data loss, if your
data storage supports this:
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
update from the vanilla container to your custom container. But this is like
"create". No updates will follow once the container was created. This
makes it easier and more predictable.

The same is true for operating on data-structures in memory. In most cases, you should not alter the data structure which is iterating. Create a new data structure while iterating the input data. In other words: no in-place editing.

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
Client: Here it is xyuasdusd8... I hope you like it.
Server: Fine, I accept this. Now I trust you. Now I know you are Bob
Client: Please show me the first message
Server: here it is:
Server:...
Client: looks like spam. Please delete this message
Server: Now I know that you want to delete this message. 
Server: But won't delete it now. Please send me EXPUNGE to execute the delete.
Client: grrrr, this is complicated. I already told you that I want the message to be deleted.
Client: EXPUNGE
...
```

Of course roughly the same needs to be done with HTTP. But HTTP you can cut the task into several smaller HTTP requests. This gives the service the chance of delegating request-1 to server-a and request-2 to server-b. In the cloud, environment containers get created and destroyed in seconds. It is easier without a long-living connection.

In the above case (IMAP protocol) the EXPUNGE is like a COMMIT in relational databases. It is very handy to have a transactional database to implement a service. But it makes no sense to expose the transaction to the client.

Stateless is like IPO: Input-Processing-Output.



### No Shell Scripting

The shell is nice for interactive usage. But shell scripts are
unreliable: Most scripts fail if filenames contain whitespaces.
Shell-Gurus know how to work around this. But quoting can get complicated. I use the shell for interactive stuff daily. But I stopped
writing shell scripts.

Reasons:

- If an error happens in a shell script, the interpreter steps silently to the next line. Yes, I know you can use "set -e". But you don't get a stack trace. Without a stack trace, you waste a lot of time analyzing why this error happened.
- AFAIK you can't do object-oriented programming in a shell. Often OOP is overkill, but sometimes it is really great.
- AFAIK you can't raise exceptions in shell scripts.
- It makes sense to use (or run) an application monitoring platform. For example "Shell" is not a [supported plattform of Sentry](https://docs.sentry.io/platforms/). If you configure it for your prefered environment once, then you get great error reporting in once place. Even if your small backup-script is only a three lines long shell script: It is unreliable, use a real language!
- Shell-Scripts tend to call a lot of subprocesses. Every call to grep, head, tail, cut creates a new process. This tends to get slow.
    I have seen shell scripts that start thousands of processes per second.
    After re-writing them in Python they were 100 times faster and 100
    times more readable.
- I do this "find ... | xargs" daily, but only while using the shell interactively. But what happens if a filename contains a newline character? Yes, I know "find ... -print0 | xargs -r0", but now "find
    .. | grep | xargs" does not work anymore... It is dirty and will never get clean.
- Look at all the pitfalls: [Bash
    Pitfalls](https://mywiki.wooledge.org/BashPitfalls) My conclusion: I
    prefer to walk on solid ground, I don't write shell scripts anymore.
- Even Crontab lines are dangerous. Look at this cron-job which should clean the directory of the temporary files:

> @weekly . ~/.bashrc && find $TMPDIR -mindepth 1 -maxdepth 1 -mtime +1 -print0 | xargs -r0 rm -rf

Do you spot the big risk?

Shell scripts are fine if they are conditionless. This means no "if", no "else", no "for".
For example in a Dockerfile you can use "RUN ...." commands to create a custom image. But I would not call things like this a shell script. 
It is just a sequence of commands to execute.

### Portable Shell Scripts

I think writing portable shell scripts and avoiding bashism (shell
scripts that use features that are only available in the bash) is a
useless goal. It is wasting time. It feels productive, but it is not.

Avoid \#!/bin/sh. The interpreter could be bash, dash, busybox, or something else.
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
instead of bash brings no measurable benefit today. If you want it
minimal, then use Alpine Linux with Busybox.

If you are not able to create a dependency to bash, then solve this
issue. Use rpm/dpkg or configuration management to handle "my script
foo.sh needs bash".

I know that there are some edge cases where the bash is not available
(for example, a container image based on Alpine Linux),
but in most cases, the time to get things done is far more important.
Execution performance is not that important. First: get it done
including automated tests.

### Server without a shell is possible

In the past, it was unbelievable: A Unix/Linux server that does not
execute a shell while doing its daily work. The dream is true today.
These steps do not need a shell: operating system boots. Systemd starts.
Systemd spawn daemons. For example a web server. The web server spawns
worker processes. An HTTP request comes in and the worker process handles
one web request after the other. In the past, the boot process and the
start/stop scripts were shell scripts. I am very happy that systemd
exists.

But time has changed. Today applications run in containers. Containers
don't need systemd. In [Kubernetes](https://en.wikipedia.org/wiki/Kubernetes) containers
get started and stopped, not services. There is no need for a daemon starting and
stopping services since this gets done on a higher level.

### Avoid calling command-line tools

I try to avoid calling a command-line tool if a library is available.

Example: You want to know how long a process is running (with Python).
Yes, you could call `ps -p YOUR_PID -o lstart=` with the subprocess
library. This works.

But why not use a library like
[psutil](https://pypi.python.org/pypi/psutil)?

Why do you want to avoid a third-party library?

Is there a feeling like "too much work, too complicated"? Installing a
library is easy, do it.

Check the license of the library. If it is BSD, MIT, LGPL, or Apache like, then
use the library.

Calling a subprocess is slow, especially if it gets done often you will notice
the difference soon.

That's one reason I dislike shell scripting. Calling `grep`, `cut`, `sed` again and
again wastes a lot of CPU time. You can see this with the command line tool `top`.
If the `sy` value is high, then your server is busy starting new processes. A library is
way more efficient, since you don't start new processes again and again.

### Shell Scripts are ok, if ...

Shell Scipts are ok, if they are conditionless: No "if", no "else", no "for".

For example creating containers with one or several `RUN ...` commands is ok.

I use this heading, to ensure that the script stops if something is wrong:

```
#!/bin/bash
set -euxo pipefail
...
```

### Avoid toilet paper programming (wrapping)

What is "toilet paper programming"? This is a pattern which was often
used in the past: There is something wrong inside - something is
smelling. Let's write a wrapper. Still something wrong? Let's write a
second wrapper.....

All these wrappers do not solve the underlying issue.

In the past, there were fewer alternatives. And since you had no choices,
you were forced to use a particular tool. If this did not work the way
you wanted it, you need to write a wrapper.

Today you have many more alternatives. If tool x does not work
the way you want it to, you can use tool y.

I am happy that the anti-pattern "toilet paper programming" gets used
less often today.

Example: WxPython (GUI toolkit) wraps WxWindows wraps gtk wraps xlib.

There are still some places where toilet paper wrappers need to get coded again and again.

For example, JSON does not support datetime, timedelta, and binary data. See [Let's fix JS](https://github.com/guettli/lets-fix-js). Speak to the upstream, to whoever is responsible for this, even if you think they are way too big, and you are way too small.


### If unsure use MIT License

The [MIT License](https://en.wikipedia.org/wiki/MIT_License) is simple and short. Most projects
at Github use it.

Some licenses are much too long. I tried to read the GPL twice, but I fell
asleep. I don't like things that I don't understand.

Next argument: The GPL and AGPL licenses are [viral](https://en.wikipedia.org/wiki/Viral_license). If you want
to create a commercial product, you can't use this.

For me "freedom" means no constraints. That's why I prefer
the MIT License, since GPL and AGPL have the constraint
that you must open your source, too.

See [Code licensed under AGPL MUST NOT be used at Google](https://opensource.google/docs/using/agpl-policy/)



### Loop in DB, not in your code

Do the filtering in the database. In most cases, it is faster than the
loops in your programming language. And if the DB is not fast enough,
then I guess there is just the matching index missing up until now.

### Do permission checking via SQL

Imagine you have three models (users, groups, and permissions) as tables
in a relational database system.

Most systems do the permission checking via source code. Example: `if
user.is_admin then return True`. 

Sooner or later you need the list of items: Show all items which the
current user may see.

Now you write SQL (or use your ORM) to create a queryset that returns
all items which satisfy the needed conditions.

Now you have two implementations. The first `if user.is_admin then
return True` and one which uses set operations (SQL). This is redundant and looking
for trouble. Sooner or later your permission checks get more complex and then one implementation will get out of sync.

That's why I think: do permission checking via SQL

### Real men use ORM

[ORM (Object-relational mapping)](https://en.wikipedia.org/wiki/Object-relational_mapping) makes daily
work much easier. The above heading is a stupid joke. Clever people use tools to make work simpler, more fun, and more
convenient. ORMs are great. 

Some (usually elderly) developers fear that an ORM is slower than hand-crafted and optimized SQL. Maybe
there are corner cases where this prejudice is true. But that's not a reason to avoid ORMs. Just use them,
and if you hit a corner case, then use raw SQL.

See [premature optimization is the root of all evil](#premature-optimization-is-the-root-of-all-evil)

Make your life easy, use ORM.

Example: [Django ORM "Filtering on a Subquery() or Exists() expressions"](https://docs.djangoproject.com/en/dev/ref/models/expressions/#filtering-on-a-subquery-or-exists-expressions). 

```
# Select all rows of the model Post, which have a comment which was created a day ago:

one_day_ago = timezone.now() - timedelta(days=1)
recent_comments = Comment.objects.filter(
     post=OuterRef('pk'),
     created_at__gte=one_day_ago,
)

Post.objects.filter(Exists(recent_comments))
```
For me above code is super easy to read.

### SQL is an API

If you have a database-driven application and a third party tool wants
to send data to the application, then sometimes the easiest solution is
to give the third party access to the database.

You can create a special database user that has only access to one table.
That's easy.

Nitpickers will disagree: If the database schema changes, then the
communication between both systems will break. Of course, that's true.
But in most cases, this will be the same if you use a "real" API. If
there is a change to the data structure, then the API needs to be
changed, too.

I don't say that SQL is always the best solution. Of course, HTTP based
APIs are better in general. But in some use cases doing more is not
needed.

### C is slow

... looking at the time you need to get things implemented. Yes, the
execution is fast, but the time to get the problem done takes "ages". I
avoid C programming, if possible. If Python gets too slow, I can optimize
the hotspots. But do this later. Don't start with the second step. First,
get it done and write tests. Then clean up the code (simplify it). Then
.... What is the next step? Optimize? In most cases, the customer has new
needs and he likely wants new features not faster execution.

Higher-level languages have a better "zero to [MVP](https://en.wikipedia.org/wiki/Minimum_viable_product)" speed.

### Version Control: git

For version control of software, I use git. I think all other tools (svn,
mercurial, CVS, darcs, bazaar) can be considered "dead". See
[StackOverflow TagTrend](http://sotagtrends.com/?tags=git+svn+mercurial+cvs+darcs+bazaar)



### Avoid long-living branches

Avoid long-living branches in your git repos. The more time that passes,
the less likely is that your work will ever get merged. For me one week
is ok, but three weeks are too long.

Ten lines of improvement that get pushed to master today have much more value
than 1000 lines which are in a branch which will never get pushed to master.

Trunk based development goes further. Sounds good:

> ... each developer divides their work into small batches and merges that 
> work into the trunk at least once (and potentially several times) a day.

See [Google DevOp Guide "Trunk based development"](https://cloud.google.com/solutions/devops/devops-tech-trunk-based-development)


### Don't put generated code into version control

Please read [Source code vs generated code](#source-code-vs-generated-code). Generated code or binary
data should not be in a git repository. It is possible but strange.

### The best commits remove code

For me, the best commits add some lines to the docs, add some lines to
tests and removes more lines than it adds to the production code.

### Time is too short to run all tests before commit+push

If the guideline of your team is: "Run all tests before commit+push",
then there is something wrong. Time is too short to watch tests running!
Run only the tests of the code you touched `py.test -k my_keyword`.

It's the job of automated CI (Continuous Integration) to run all tests.
That's not your job.

### Time is too short to care for "E302 expected 2 blank lines, found 1"

Style Guide Enforcements (like [flake8 for Python](https://flake8.pycqa.org/en/latest/)) don't help much.

Time is too short to manually make the style guide checker happy by 
editing the source code.

>  E302 expected 2 blank lines, found 1

I don't want to waste my time with "errors" like above. This is no error.
The code is great and makes the customer happy.

Reading the message, understanding it, opening the file, editing it, re-runing
the checker .... No, this is not productive.

The solution is (like almost always) **automation**

Style guide enforcement does not help.

Automated source code styling helps.

Unfortuantely this is not solved yet.

For the Python there is [black](https://github.com/psf/black), but it is not ready yet.

### CI

Use continuous integration. Only tested code is allowed to get deployed.
This needs to be automated. Humans make more errors than automated
processes.

Github Actions are great.

Increasing the version number can be done with [BumpVer](https://pypi.org/project/bumpver/) which
can use [Calendar Versioning](https://calver.org/) (for example YYYY.MM.X)


All I need to do is to commit. All other steps are automated :-)

### Tests should work offline

Imagine a developer sits on a train and has an unreliable network connection.

Nevertheless, I want that all tests can get executed.

For simple unit-tests that don't need a server, this is easy.

But if your test needs an HTTP-server, a database (PostgreSQL, MySQL),
a key-value DB (Redis), ... What can you do?

Automation is the solution. You can use a tool like Ansible to set up
the needed environment.


### CI Config

CI tools (GitLab, Travis, Jenkins) usually have a web GUI. Keep the
things you configure with the GUI simple. Yes, modern ci tools can do a
lot. With every new version, they get even more [turing complete](https://en.wikipedia.org/wiki/Turing_completeness) (this was
a joke, I hope you understood it). Please do separation of concerns. The
CI tool is the GUI to start a job. Then the jobs run, and then you can
see the result of the job in your browser. If you do configure condition
handling "if ... then ... else ..." inside the web-GUI, then I think you
are on the wrong track.

The ci tool calls a command line. To make it easy for debugging and
development this job should be callable via the command line, too. In
other words: the web GUI gets used to collect the arguments. Then a
command-line script gets called. Then the web GUI displays the result
for you. I think it is wise to avoid a complex CI config. If you want to
switch to a different ci tool (example from Jenkins to GitLab), then
this is easy if your logic is in scripts and not in ci tool
configuration.

### Avoid Threads, Async and Promises

Threads and Async are fascinating. BUT: It's hard to debug. You will
need much longer than you initially estimated. Avoid it, if you want to
get things done. It's different in your spare time: Do what you want and
what is fascinating for you.

There is one tool and one concept that is rock solid, well known, easy
to debug, and available everywhere and it is great for parallel
execution. The tool is called "operating system" and the concept is
called "process". Why re-invent it? Do you think starting a new process is
"expensive" ("it is too slow")? Just, do not start a new process for
every small method you want to call in parallel. Use a [Task
Queue](https://www.fullstackpython.com/task-queues.html). Let this tool
handle the complicated async stuff and keep your code simple like
running in one process with one thread. It is all about IPO:
Input-Processing-Output.

There is a good reason to use async: The [C10k
Problem](https://en.wikipedia.org/wiki/C10k_problem). BUT: I guess you
don't have this problem. If you don't have this problem, then don't use
technology which was invented to solve this issue :-)

The related part of the [Google Codereview Guidelines "Functionality"](https://google.github.io/eng-practices/review/reviewer/looking-for.html#functionality)

There is a huge difference between implementing a task-queue and using
a task-queue. If you implement a task-queue, then threads/async/promises/multiprocessing are
the building blocks. But taks-queues exist. There is no need to re-invent them.

I like to use task-queues, and write my code in a very predictable single-thread,
single-process synchronous way.


### Functions should return values, Not Promises.

Especially in JavaScript, functions often return [Promises](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Promise).

The `Promise` represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

I don't like this. I want a method to execute synchronously and then return the result.

If I want a method to be executed asynchronously, then I (the caller) can use a Promise. But I don't want the function to decide "async or sync?".

I want to decide this, and I want the default to be "synchronous execution".

Pseudo Code (synchronous):
```
response = fetch('https://example.com')
my_json = response.json()
```

JavaScript (asynchronously)
```
const my_json = async () => {
    const response = await fetch('https://example.com');
    return response.json();
}
```

The second code snippet is way more complicated. 

I think this can be compared to hyperlinks on web pages. The default is to follow the
hyperlink (synchronous). If the user wants to open the hyperlink in a new tab (asynchronous), 
then this decision should
be done by the user, not by the one who created this hyperlink.

I have seen JavaScript code where almost every line contained `await`. That's childish.

### Don't waste time doing it "generic and reusable" if you don't need to

If you are doing some kind of software project for the first time, then
focus on getting it done. Don't waste time to do it perfectly, reusable,
fast, or portable. You don't know the needs of the future today. One main
goal: Try to make your code easy to understand without comments and make
the customer happy. First, get the basics working, then tests and CI,
then listen to the new needs, wishes, and dreams of your customers.

Example: If you are developing web or server applications, don't waste
time making your code working on Linux and MS-Windows. Focus on one
platform.

See [Minimum viable
product](https://en.wikipedia.org/wiki/Minimum_viable_product)

Related Book: [The Lean Startup](http://theleanstartup.com/book)

Several months after writing the above text I found this 

[Google Codereview Guidelines "Complexity"](https://google.github.io/eng-practices/review/reviewer/looking-for.html#complexity)
> A particular type of complexity is over-engineering, where developers have made the code more generic than it needs to be, or added functionality that isn’t presently needed by the system. Reviewers should be especially vigilant about over-engineering. Encourage developers to solve the problem they know needs to be solved now, not the problem that the developer speculates might need to be solved in the future. The future problem should be solved once it arrives and you can see its actual shape and requirements in the physical universe.

Related: [YAGNI (You aren't gonna need it)](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)

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
.... points for it. I think more indentation, makes the code more
complex. The "return" simplifies the code. For me, the second version is
much easier to read.

For Python there exists a "complexity checker": [Design
checker](http://pylint.pycqa.org/en/latest/technical_reference/extensions.html#design-checker).

### Source code vs generated code

I guess every young programmer wants to write a tool that automatically
creates source code. Stop! Please think about it again. What do you
gain? Don't confuse data and code. Imagine you have a source code
generator that takes DATA as input and creates SOURCE as output. What
is the difference between the input (DATA) and the output (SOURCE)? What
do you gain? Even if you have some kind of artificial intelligence, you
can't create new information if your only input is DATA. It is just a
different syntax. Why not write a program which reads DATA and does the
thing you want to do?

For the current context, I see only two different things: **source code**
for humans and **generated code** for the machine.

Just because a file contains code of a programming language, this does
not means that this file is source code.

If the TypeScript compiler creates JavaScript, then the output is
generated code since the created JavaScript source is intended for the
interpreter only. Not for humans. If you create JavaScript with a
keyboard and a text editor it is source code. Don't mix source code and
generated code in one file.

In other words: source code gets created by humans with the help of an
editor or IDE.

### Don't believe the "automatically create foo" hype

If you are new to software development you are fascinated by the magic.
You can create things! In this section, I call the magic output "foo".

Yes, you can automatically create foo with a script. Whatever "foo" is
in your context: It has no value. It is worth nothing. It is dust in the wind like a web page that displays the current time. 
This output is only temporarily valuable. 

Look at the basic IPO pattern: Input - Processing - Output (in this case
"foo").

Do not store "foo", the output of your script, in a database. Do not
store "foo" in version control.

It has no value since you can always create "foo" again. You just need
the input and your script.

You can store "foo" in a cache to improve performance. But do not store
it permanently. Don't make a backup of it.

A term that is often a hint to this anti-pattern is "generator". Yes,
you can generate a lot of data. But this bloated, generated data is just hot air with
little value.

DevOps who prefer "Op" to "Dev" tend to create a configuration with a script.
You can do this but then create the config again daily. Do not edit
the generated config by hand.

Related: [Single source of truth](https://en.wikipedia.org/wiki/Single_source_of_truth)

### Regex are great - But it's like eating rubbish

Yes, I like a regular expression. But slow down: What do I do, if I use a
regex? I think it is "parsing". I remember to have read this some time
ago: "Time is too short to rewrite parsers". Don't parse data! We live
in the 21 century. Consume high-level data structures like JSON, YAML, or
protocol buffers. If possible, refuse to accept CSV or custom text format
as input data.

From time to time you need to do text processing. Unfortunately, there
are several regex flavors. My guide-line: Use PCRE. They are available
in Python, Postfix, in `grep -P` and many other tools. Don't waste time with other
regex flavors, if PCRE is available.

Current Linux distributions ship with a grep version which has the -P
option to enable PCRE. AFAIK this is the only way to grep for special
characters like the binary null: [How to grep for special
character](https://superuser.com/a/612336/95878)

### Use a password manager

I use keepass. And sync it via Nextcloud.

Don't forget to add the content of your ~/.ssh/id_rsa file to it. 

### CSV - Comma-separated values

CSV is not a data format. It is an illness. See the introduction at:
<https://docs.python.org/3/library/csv.html>

If your customer sends you tabular data in Excel, read the excel
directly. Do not convert it to CSV just because you think this is
easier.

If a customer wants you to send him CSV, ask if he can consume JSON.

There are great libraries for reading and writing Excel. For example:
[openpyxl](https://openpyxl.readthedocs.io/en/stable/)

Other alternatives to CSV:

* [zarr](https://zarr.readthedocs.io/en/stable/) (For data science (very long arrays))
* [jsonlines](http://jsonlines.org/) (for example for logfiles)

### Give booleans a "positive" name

I once gave a DB column the name "failed". It was a boolean indicating
if the transmission of data to the next system was successful. The
output as a table in the GUI looked confusing for humans. The column
heading was "failed". What should be visible in the cell for failed
rows? Boolean usually get translated to "Yes/No" or "True/False". But if
the human brain reads "Yes" or "True" it initially thinks "all right".
But in this case "Yes" meant "Yes, it failed". The next time I will call
the column "was\_successful", then "Yes" means "Yes, it was successful".
Some GUI toolkits render "True" as a green (meaning "everything is ok")
hook and "False" as a red cross (meaning "it failed").

### Love your docs

I have seen it several times on Github. Just have a look
at the README files on GitHub. They start with "Installing", "Configuring", then "Special Cases"... 

What is missing? An introduction! Just some sentences
about what this great project is all about. Programmers prefer the details to the big picture,
the overview. 

But "Project simple-foo simplifies foo" is not enough. What is "foo"?

Dear programmers, learn to relax and look at the thing you create like a newcomer. Imagine a newcomer who knows how to add two integers with his favorite programming
language. What is missing to make him understand why the project/lib/tool is needed?

First, you need to convince him that this project is worth a try, then if he knows
the "why?", then explain how to install it.

If you have this mindset "I do the important (programming)
stuff. Someone else can care for the docs", then your open source
project won't be successful.

If you write docs, then do it for newcomers. Start with the
introduction, define the important terms, then provide simple and straightforward use
cases. Put details and special cases at the end.

If your library gets used and you add a bug, you will get feedback soon.

Tests fail or even worse customers will complain.

But if you write broken docs, no one will complain.

Even if someone reads your mistake, it is unlikely that you get
feedback. Unfortunately, only a few people take this seriously and tell you
that there is a mistake in your docs.

How to solve this?

You need to act.

Let someone else read your docs.

The quality of feedback you get depends on the type of person you ask to
read your docs.

If it is a programmer, likely, he does not read your docs
carefully. Most software developers do not care for orthography and it
is hard for them to read the docs like a newcomer. They already know
what's written there, and they will say "it is ok".

My solution: resubmission: Read the text again 30 days later.

### Canonical docs

Look at the question concerning OpenSSH options at the Q+A site [serverfault.com](https://serverfault.com/).
There is a lot of guessing. Something is wrong. Nobody knows where the
canonical upstream docs are. Easy linking to a specific configuration is not
possible. What happens? Redundant docs. Many blog posts try to explain
stuff... Don't write blog posts, instead, you should improve the upstreams docs. Talk with
the core developers. Open an issue in the issue tracker if you think something is missing in the docs.

Open an issue if the docs start with the hairy details and don't start
with an introduction/overview. Developers don't realize this, since they
need to deal with the hairy details daily. Don't be shy: Help them to
see the world through the eyes of a newcomer.

I am unsure if I should love or hate "wiki.archlinux.org". On the one
hand, I found there valuable information about systemd and other Linux
related secrets. On the other hand, it is redundant and since a lot of
users take their knowledge from this resource, the canonical upstream
docs get less love. First, determine where the canonical upstream docs
are. Then communicate with the maintainers. Avoid redundant docs.

In other words: Blog posts are nice, but they are like dust in
the wind. They explain a snapshot. Three months later they are outdated.
It makes more sense to add one missing sentence to the upstream docs,
then to create a blog post explaining something which is not explained
in the docs. At least in the open-source world. Since it is more likely
that you can influence the upstream docs.

Related: [Single Source of Truth (Wikipedia)](https://en.wikipedia.org/wiki/Single_source_of_truth)

Related: [Canonical URL](https://en.wikipedia.org/wiki/Canonicalization#URL)

Related: ["Don't repeat yourself" vs "We enjoy typing"](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself#DRY_vs_WET_solutions)

### One central glossary: One page per term

> There are only two hard things in Computer Science: cache invalidation and naming things.
> -- Phil Karlton

[Martin Fowler](https://martinfowler.com/bliki/TwoHardThings.html)

My best practice to solve the "naming things" challenge

* Define your terms, your terminology. For small projects, a glossary is enough, but for bigger projects, every term should have its page. It should be easy to create a hyperlink to this term. That's why I prefer the "one term, one-page" approach. Creating hyperlinks into a page (https://..../...#foo) are possible but less fun.
* The defined terms should not differ too much from the spoken words (or the words used in your chat/mail messages). If there is a difference, then alter the written definition. 
* Someone should be responsible for the docs. "Everybody is responsible for it" does not work.
* Encourage and motivate people, again and again, to speak up if the docs are outdated.

More about this topic from me: [Intranets](https://github.com/guettli/intranets)

### Do not send long instructions to customers via mail

If you send long instructions to customers via mail, then these docs in
the mail are hidden magic. Only the customer who receives this mail
knows the hidden magic.

Publish your docs in your app. Send your customer a link to the online
docs.

Despite all myths: Some users read the docs!

And that's great if the user has more knowledge. Because this means you
have less work. Fewer emails, fewer interrupts, fewer phone calls :-)

This even applies to public discussion forums. Don't write too much. Create great docs and answer questions by
providing links to the docs. And be polite and include the question if this answers the question of the user.

[Permalinks](https://en.wikipedia.org/wiki/Permalink) are great, since they provide a single source of truth.


###  Don't write tech-docs in a non-English language

General rule: don't waste time.

It is feasible to write high-level blog posts about tech topics in your favorite language.

Sometimes it is easier to communicate the holistic view in your mother-tongue.

But it is not feasible to write detailed tech stuff in a non-English language.

Example:

https://wiki.ubuntuusers.de/Installation_auf_externen_Speichermedien/

I came across this page because I want to install Linux on an external hard disc.

Unfortunately, there seemed to be no good English guide on how to do this.

The most solid guide I found during the first minutes was the above link. Unfortunately, the above guide was outdated.

Grrrrrr. Now I needed to choose:

* V1: Should I update the outdated german guide? It is a wiki editable by everybody.

* V2: I use an English guide, but they look not solid. 

Grrr. I don't like thinking.

The people who created the German guide thought they help the world. They felt good
while doing what they did. I think they wasted time. Automatic translations are quite
good today. At least if you translate English to your favorite language.
I won't update the outdated German guide in the wiki. This would help only very few people.
Most people which want to install Linux on an external hard drive can either
read English text or they know who to translate English text to their favorite
language. I would update an Englisch wiki page since this would help a lot of people.

Don't get me wrong: Docs for applications you write should be in the language of your customers. Above text
is about tech-related docs.

My conclusion: Don't write tech-docs in a non-English language

### Care for newcomers

In the year 1997, I was very thankful that there was a hint "If unsure
choose ..." when I needed to compile a Linux kernel. These days you
need to answer dozens of question before you could compile the invention of
Linus Torvalds.

I had no clue what most questions were about. But this small advice "If
unsure choose ..." helped me get it done.

If you are managing a project: Care for newcomers. Provide them with
guidelines. But don't reinvent docs. Provide links to the relevant
upstream docs, if you just use a piece of software.

### Good example for "care for newcomers"

> Writing plugins
> 
> It is easy to implement local conftest plugins .... Please refer to [Installing and Using plugins](https://docs.pytest.org/en/stable/plugins.html#using-plugins) if you only want to use but not write plugins.

That's great. That's newcomer focused documentation.

### Keep custom IDE configuration small

Imagine you lost your PC and you lost your development environment:

-   IDE configuration
-   Test data
-   Test database

All that's left is your source code from version control, CI servers and
deployment workflow.

How much would you lose? How much time would you waste to set up your
personal development environment again?

Keep this time small. This is related to "care for newcomers". If you
need several hours to set up your development environment, then a new team member would need even much more time.

Although I use PyCharm and VSCode, the introduction of [Gitpod](https://www.gitpod.io/) gets it to the point:

> Gitpod does to Dev Environments what Docker did to Servers. Today we are emotionally attached (for better or worse) to our dev environments, give them names & massage them over time. They are pets - similar to servers before docker took advantage of namespaces and cgroups in Linux and turned these nice puppies into cattle. 
> With Gitpod it is the same - we treat dev environments as automated resources you can spin up when you need them and close down (and forget about) when you are done with your task. Dev environments become fully automated and ephemeral. Only then you are always ready-to-code - immediately creative, immediately productive with the click of a button, and without any friction.



### Setting up a new development environment should be easy

This happened to me several times: I wanted to improve some open source
software. Up until now I only used the software, now I want to write a
patch. If setting up a new development environment and running the tests
is too complicated or not documented, then I will resign and won't
provide a patch. These steps need to be simple for people starting from
scratch:

-   check out the source from version control
-   check that all tests are working (before modifying something)
-   write a patch and write a test for your patch
-   check that all tests are working (after modifying something)

### Passing around methods make things hard to debug

Even in C, you can pass around method-pointers. It's very common in
JavaScript and sometimes it gets done in Python, too. It is hard to
debug. IDE's can't resolve the code: "Find usages" don't work. I try to
avoid it. I prefer OOP (Inheritance) and avoid passing around methods or
treating methods like variables.

But maybe this is just my strong backend related roots. I have never
coded in a big modern JavaScript-based environment.

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

OOP is great for implementing an [ORM (Object-relational mapping)](https://en.wikipedia.org/wiki/Object-relational_mapping). But implementing this should be done by people who have more experience than I have :-)

Here is code that uses the well-known jUnit style:
```
# OOP way
import unittest


class TestSMTP(unittest.TestCase):
    def smtp_connection(self):
        import smtplib
        return smtplib.SMTP("smtp.gmail.com", 587, timeout=5)
    
    def test_helo(self):
        response_code, msg = self.smtp_connection().ehlo()
        self.assertEqual(response_code, 250)
```

The non-object-oriented way:
```
# pytest way
import pytest


@pytest.fixture
def smtp_connection():
    import smtplib
    return smtplib.SMTP("smtp.gmail.com", 587, timeout=5)


def test_ehlo(smtp_connection):
    response_code, msg = smtp_connection.ehlo()
    assert response_code == 250
```

My rule of thumb: Less indentation, means less complexity, means better code.

Two things are simplified: The second version does not need a class or inheritance. Nice, since less code means fewer bugs.

In the second example the method `smpt_connection()` is not an instancemethod of a class, it just an unbound method. If a test
asks for a parameter with this name, then pytest gives the test the result of this method.

And look at the assertion: `self.assertEqual(response_code, 250)` vs `assert response_code == 250`. Namespaces
introduced by dots are great (`assertEqual` is in the namespace of `self`). But if one level is enough, then
this is even better.

Of course, this is opinionated, and it is 100% ok if you prefer the OOP-way and not the shorter solution.

### "Dependency injection" is just "Configuration"

For me, the term [Dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) and the corresponding Wikipedia article are way too complicated.

For me, it is just "Configuration". But some people don't like it simple, they prefer .... (I removed this since it was provocative. Feel free to add your favorite terms here)

From Wikipedia "Dependency injection"

> In the following Java example, the Client class contains a Service member variable that is initialized by the Client 
> constructor. The client controls which implementation of service is used and controls its construction.
> In this situation, the client is said to have a hard-coded dependency on ExampleService.

Now have a look at these docs [Database Settings](https://docs.djangoproject.com/en/3.0/ref/settings/#databases)

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': 'mydatabase',
    }
}
```

That's all: Instead of hard-coded dependencies, you provide a way to configure your software.

### Test-Driven Development

red, green, refactor. More verbose: make the test fail, make the test
pass, refactor (simplify) code.

### Extract Method to get full coverage

Imagine you have a method like this:
```
def my_method(a, b, c):
    # ten 
    # lines
    # of 
    # code
    
    if a > b:
        # ....
        
    # again
    # ten
    # lines 
    # of 
    # code
```

One thing is 100% sure: You can get full coverage with one test. You would
need to call the method twice: Once with `a > b` and once with opposite.

But you don't want to call this method twice, since useless executing
of the code above and below the "if" statementent. You want to avoid
that you test suite gets too big and too slow.

Maybe you could extract the condition into an new method:

```
def my_method(a, b, c):
    # ten 
    # lines
    # of 
    # code
    
    d = handle_case_foo(a, b)
        
    # again
    # ten
    # lines 
    # of 
    # code

def handle_case_foo(a, b):
    if a > b:
        return ...
    return ...
```

This way you can test `my_method()` with one test, and you can write
a small test for `handle_case_foo()`.

### From bug to fix

First, make your bug reproducible. If it is reproducible, then it is easy
to fix it.

Make it reproducible in a test.

Imagine there is a bug in your method do\_foo(). You see the mistake
easily and you fix it. Done?

I think you are not done yet. I try to follow this guideline:

Before fixing the bug, search test\_do\_foo(). There is no test for this
method up until now? Then write it.

Now you have test\_do\_foo().

You have two choices now: extend test\_do\_foo() or write
test\_do\_foo\_\_your\_special\_case(). I use the double underscore
here.

Make the test fail (red)

Fix the code. The test is green now.

Slow down. Take a sip of tea. Look at your changes ("git diff" in your
preferred IDE). Is there a way to simplify your patch? If yes, simplify
it.

Run the "surrounding tests". If do\_foo() is inside the module "bar".
Then run all tests for module "bar" (I use py.test -k bar). But if this
would take more than three minutes, then leave the testing to the CI
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

### pre-commit hook

For basic syntax checking (aka linting) before commit I use [pre-commit](https://pre-commit.com/)

### aaa-tests (smoke tests)

If you have a huge test-suite, which takes more than ten minutes to execute, then I recommend
to flag some tests. I call these tests "aaa" tests. These tests should be fast and check the basic
stuff. 

This way you can check if most parts are all right before pushing code and triggering CI.

Some call these "smoke tests".

Why "aaa"?

Most test runners allow you to execute all tests which match a certain pattern. I name the tests "test_aaa_...",
and then I can easily run all these tests. Example: `pytest -k aaa`.

Running all aaa-tests should take less then a minute

But I don't call it automatically before each commit. 

### Creating test data is much more important than you initially think

Creating test data is very important. It can help you with several
things:

1: It can help you to create a re-usable application. If you have only
one customer, it does not matter. But the real benefit of software is its re-usability.
Your code wants to get executed by several customers. As soon as you have two or more 
customers you need a neutral test environment that is no specific to one of your customers.
It is a lot of work to create a neutral test environment if you have not done it from
day one. But the work only needs to be done once and helps in the long run.

2: It can help you to create presentation/demo systems.

3: It can help you in automated tests.

Your tests should not run on real data from customers.

If you create test data this should be automated. This way you can fill a new database with useful data. You should be able to create a
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
virtualization, and automation are your friends.

The "untestable" code needs to be cared for. Code is always testable,
there is no untestable code. Maybe your knowledge of testing is limited
up until now. Finding untestable code and making it testable is the
beginning of an interesting adventure.

### Flaky Tests

Tests are never flaky. If the same code ran fine yesterday, and it the same code
fails today, then the test itself is stable. 

The environment is flaky. Some small bit in the environment is different today.

Maybe the servers are under more load today, which results in slower responses, which
results in timeouts.

Maybe it fails because there is a new test that executes before the flaky test and which
modifies the database.

Maybe a shared resource contains different data today.

...

The bigger your environment, the more likely you have flaky tests.

This is the way to avoid flaky tests:

* Keep your test, simple. Try to write stateless methods that receive only a few input.
* keep the environment, simple. If you can avoid Selenium, then avoid it. This will save you time.
* Avoid shared resources. Tests should have their own database, their own cache, ...
* ...

### Hermetic Testing

This blog from [Google Testing Blog "Hermetic Servers"](https://testing.googleblog.com/2012/10/hermetic-servers.html) explains it in-depth: 
End-to-End tests are faster and less flaky if they run on localhost and don't need other resources.

This usualy means:

* The database is running on localhost
* storage server (S3) is running on localhost or in-memory
* Cache Server (Redis) is running on localhost or in-memory.

For storage and cache it is easy to find an in-memory solution (for Django [dj-inmemorystorage](https://github.com/waveaccounting/dj-inmemorystorage),
but for the database it is more difficult. My opinion: Use PostgreSQL during development.
Don't use SQLite, since it does not support all features of PostgreSQL.

### Unit-Tests may use the ORM.

Imagine you use a framework that provides you a nice ORM to create, read, update, and delete your data.

Now you write some backend-methods on top of this ORM.

And on top of your methods, you might provide an HTTP API.

Imagine you have a class `Ticket` which has a method called `resolve()`. This method uses the ORM.

You want to write a unit-test for this method.

A purist argues: I only want to unit-test the method, I must not use the ORM since blablabla.

I understand what the purist wants. But I want to get things done. I want to make
customers happy, not unit-test purists.

For me, it is 100% ok if unit-tests use the ORM.

In other words: Only mock away things that take too long or things that need resources
which are not available (e.g. an SMTP server).

### Is config code or data?

The heading "Is config code or data?" could be phrased as "config: DB or git?", too.

Where should configuration be stored?

This is a difficult question. At least at the beginning. For me, most
configuration is data, not code. That's why the config is in a
**database**, not in a text or source code file in a version control
system.

This has one major drawback. All developers love their version control
system. Most developers love git. It is such a secure place. Nothing can get lost
or accidentally modified. And if a change was wrong, you can always revert
to an old version. It is like heaven. Isn't it?

No, it is not. The customer can't change it. The customer needs to call
you and you need to do stupid repeatable useless work.

For me, the configuration should be in the database. This way you can provide
a GUI for the customer to change the config.

The configuration and recipes for the configuration management are
stored in git. But this is a different topic. If I speak about
configuration management, then I speak mostly about configuring Linux
servers and networks (aka [Infrastructure as code](https://en.wikipedia.org/wiki/Infrastructure_as_code)). In my case, this is nothing which my customer
touches.

### ForeignKey from code to DB

This code uses the ORM of Django

``` {.sourceCode .python}
if ....:
    issue.responsible_group=Group.objects.get(name='Leaders')
```

`Group` is a class and refers to a table with the same name. Each group has a name. There
is one group (one row) with the name "Leaders".

The above code is dirty because 'Leaders' is like a ForeignKey from code to
a database row.

How to avoid this?

Create a global config table in your database. This table has exactly one
row. That's the global config. There you can create a column called
"Leaders" and there you store the ForeignKey to the matching group.

### Testcode is conditionless

Test code should not contain conditions (the keyword `if`). If you have
loops (`for`, `while`) in your tests, then this looks strange, too.

Tests should be straight forward:

> 1.  Build environment: Data structures, ...
> 2.  Run the code which operates on the data structures
> 3.  Ensure that the output is like you want it to.

### Don't search the needle in a haystack. Inject dynamite and let it explode

Imagine you have a huge codebase that was written by a nerd who is
gone for several months. Somewhere in the code, a row in the database gets
updated. This update should not happen, and you can't find the relevant
source code line during the first minutes. You can reproduce this
failure in a test environment. What can you do? You can start a debugger
and jump through the lines which get executed. Yes, this works. But this
can take longer, it is like "Searching the needle in a haystack". Here is
a different way: Add a constraint or trigger to your database which
fires on the unwanted modification. Execute the code and BANG - you get
the relevant code line with a nice stack trace. This way you get the
solution provided on a silver platter with minimal effort :-)

In other words: Don't waste time searching.

Sometimes you can't use a database constraint to find the relevant
stack trace, but often there are other ways.....

If you can't use a database constraint, maybe this helps: Raise
Exception on unwanted syscall
<http://stackoverflow.com/a/42669844/633961>

If you want to find the line where unwanted output in stdout gets
emitted: <http://stackoverflow.com/a/43210881/633961>

If you have a library that logs a warning, but the warning does not
help, since it is missing important information. And you have no clue
where this warning comes from. You can use this solution:
<http://stackoverflow.com/a/43232091/633961>


### Avoid magic or uncommon things

- hard links in Linux file systems.
- file system ACLs (Access control lists). Try to use as little as possible chmod/chown.
- git submodules (Please use dependency management, configuration management, deployment, tools, ...)
- [seek()](https://en.cppreference.com/w/c/io/fseek). Stateless is better. If you use seek() the file position is a state. Sooner or later the position (state) will be wrong.
- Scripts which get executed via OpenSSH [ForceCommand](http://man.openbsd.org/OpenBSD-current/man5/sshd_config.5#ForceCommand) or "command" in .ssh/authorized_keys. SSH is not an API use http.
- I think even [symbolic links](https://en.wikipedia.org/wiki/Symbolic_link) are strange and outdated. Just some minutes ago I got confused because `grep -r foo .` did not show a result, but `grep foo ./my-dir/abc.txt` showed a result. Root-cause: `my-dir` was a symlink.

### Avoid writing a native GUI

Imagine you have developed web applications up until now. You have never
developed a native GUI before. Now a new potential customer has a use
case and you think: This time a native GUI would be a good solution.

Caution: slow down. Developing a native GUI is much more work and needs
much more time than you think.

The edit, compile, run cycle is much longer. This will slow you down.

If you develop a native GUI, you might need several mouse clicks until
you reach the part where you improving the current code. And like all
humans, you are not perfect, and you have a typo. The application
crashes, and you need to do the edit, compile, run, five clicks cycle
again...

Compare this to a web application: You do not need to do five clicks to
reach the part where you improve the current code. You just hit ctrl-r
and reload the page. The stateless HTTP protocol makes this possible. I
love it.

Next argument: The native GUI community is tiny compared to web
development. If you have a question, you have only a few people to talk
to.

I am at the Chemnitzer Linux Days yearly and meet a lot of newcomers
there. Some people new to software development think: "I just want to
develop a simple app for me. No need to run a web server. I want a real
application running on my pc."

My advice: use Python and Django. The things you learn have more value.
The knowledge you gain can be used to build cool stuff. If you have a
question, there is always someone who has an useful advice.

See the [TagTrend gtk, qt,
django](http://sotagtrends.com/?tags=%5Bgtk,qt,django%5D)


### Avoid writing native Apps

Developing a mobile-friendly web application is much easier than writing a 
native app. If you can avoid it, then avoid writing a native app.

The development and release process is much slower.

Of course, the age of [Progressive Web Apps](https://web.dev/progressive-web-apps/) has just begun.
A lot of things are not possible in a web app up until now. Just be warned, that this road is
slow and in the long run deprecated, since the environments for PWAs are getting better every year.

### My prefered Web Stack

Python, Django, Gunicorn, Nginx, PostgreSQL, [htmx](https://htmx.org/), Bootstrap5.

This way I can write responsive mobile friendly applications.

I think React/Vue are in general overrated and not needed for my use cases.

### Learn one programming language, not ten.

Most young developers think they need to learn many programming languages
to be a good developer.

My opinion: Learn Python, SQL, and some JavaScript.

Then learn other topics: PostgreSQL, Configuration management,
continuous-integration, organizing, teamwork, learn to play a musical
instrument, long-distance running, family

### Learn "git bisect"

"git bisect" is a great tool in conjunction with unit tests. It is easy
to find the commit, which introduced an error. Unfortunately, it is not a
one-liner for now. You can use it like this:

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

Imagine, you can reproduce a bug in a test. But you could not
fix it at the moment. If you want to create a conditional breakpoint to find
the root of the problem, then you could be on the wrong track. Rewrite
the code first, to make it more fine-grained debuggable and testable.

Modify the source and test where a normal (non-conditional) breakpoint is enough.

This likely means you need to move the body of a loop
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
conditional breakpoint anymore. This helps you now (during debugging), but raises
the overall value of the source code in the long run, too. Instead of a few big monster methods,
you have more small and easy to understand methods that follow the simple input-processing-output model.

### Make a clear distinction between Authentication and Permission Checks

It is important to understand the difference.

**Authentication** happens first: Is the user really Bob, or is there
just someone who pretends to be Bob?

**Permission Checks** Is Bob allowed to do action "foo"? Here we already
trust that the user is Bob and not someone else. I use the term
"Permission Checks" on purpose since the synonym "Authorization" sounds
too similar to "Authentication".

Related question:
<https://softwareengineering.stackexchange.com/questions/362350/synonym-for-authorization/363690#363690>

Even the http-spec confuses both similar sounding words: 

> There's a problem with 401 Unauthorized, the HTTP status code for authentication errors. And that’s just it: it’s for authentication, not authorization. Receiving a 401 response is the server telling you, “you aren’t authenticated–either not authenticated at all or authenticated incorrectly–but please reauthenticate and try again.

Source: [403 Forbidden vs 401 Unauthorized HTTP responses](https://stackoverflow.com/questions/3297048/403-forbidden-vs-401-unauthorized-http-responses)

General guidelines: Avoid [Homonyms](https://en.wikipedia.org/wiki/Homonym)

### Idempotence is great

Idempotence is great, since it ensures that it does no harm if
the method is called twice.

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

In the past [File Locking](https://en.wikipedia.org/wiki/File_locking)
was a very interesting and adventurous topic. Sometimes it worked,
sometimes not, and you got interesting edge cases to solve again and
again. It was fun, especially on NFS (Network File System). Only hardcore experts know the difference between
fcntl, flock, and lockf.

.... But on the other hand: It's too complicated, too many edge cases,
too much wasting time.

There will be chaos if there is no central dispatcher.

I like tools like <http://python-rq.org/>. It is simple and robust.
But the next time I create something like this, I will try [django-pg-queue](https://github.com/SweetProcess/django-pg-queue)

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
container. Containers in containers are not supported, and that's good
since it makes the environment simpler.

### Code doesn't call mkdir

Code runs in an environment. This environment was created with
configuration management. This means: source code usually does not call
mkdir. In other words: Creating directories is part of configuration management. Setting up the environment and executing code
in this environment are two distinct parts. If your software runs, the
environment does already exist. Code creating directories if they do not
exist yet should be cut into two parts. One part is creating the
environment (gets executed only once) and the second part is the daily
executing (which is 100% sure that the environment is like it is. In
other words: the code can trust the environment that the directory
exists). These two distinct parts should be separated.

How to create directories if I should not do it with my software? With
automated configuration management (Ansible, Chef, ...) or during
installation (RPM/DPKG).

Exception: You create a temporary directory that is only needed for
some seconds. But since switching from subprocess/shell calling to using
libraries (see "Avoid calling command line tools") temporary files get
used much less.

### Debugging Performance

I use two ways to debug slow performance:

> -   Logging and profiling, if you have a particular reproducible use
>     case
> -   Django Debug Toolbar to see which SQL statements took long in a
>     HTTP request.
> -   Statistics collected on production environments. For Python:
>     <https://github.com/uber/pyflame> or
>     <https://github.com/benfred/py-spy>

### You provide the GUI for configuring the system. Then the customer (not you) uses this GUI

I developed a workflow system for a customer. The customer gave me an
excel sheet with steps, transitions, and groups.

The coding was the difficult part.

Then I configured the system according to the excel sheet.

The code was bug-free, but I made a mistake when I entered the values
(from excel to the new web-based workflow GUI).

The customer was upset because the configuration contained mistakes.

I learned. Now I ask if it would be ok if I provide the GUI and the
customer enters the configuration. In most cases, the customer likes to
do this.

There is a big difference. The customer feels productive if he does
something like this. I hate it. I care for the database design and the
code, but entering data with copy+paste from the Excel sheet ... No I
don't like this. Results will be better if you like what you do :-)

For detail lovers: No, it was not feasible to write a script that
imported the excel sheet to the database. The excel sheet was not well
structured.

*give a man a fish and you feed him for a day; teach a man to fish and
you feed him for a lifetime*



### Better error messages

If you have worked with Windows95, then you must have seen them: Empty
error messages with just a red icon and a button labeled "OK". You had
no clue what was wrong. On the one hand, it was great fun, on the other
hand, it was very sad since you wasted your precious time.

Do it better.

Imagine user "foo" wants to access data (let's call it "pam") which you
only can see if you are in the group "Baywatch". Unfortunately, user
"foo" is not in the group. You could show him the simple message
"permission denied". And no further information.

I don't like messages like this. They create extra work. The user will
call the support and ask the question "Why am I not allowed to see the
data?". The support needs to check the details.... and soon a half-hour
of two people is gone.

Provide better error messages: In this particular case be explicit and
let the code produce a message like: "to access the data you need to be
in one of the following groups: Baywatch, Admin, ...".

Software security experts might disagree. I disagree with their disagreement.
Hiding the facts is just "Security through obscurity".

### "false" means failure... The root cause is gone.

In the early days, when the C programming language was predominant, it was common for the return value of a method call to specify
whether the call was successful or not.

I run a [Nextcloud](https://nextcloud.com/) server, but
the synchronization fails for some files. In the logs, I see that GenericFileException()
gets thrown. Let's have a look at these lines. 

```
if ($this->view->file_put_contents($this->path, $data) === false) {
    throw new GenericFileException('file_put_contents failed');
}
```

I try to find the implementation of `file_put_contents()` and see that it
is implemented 19 times! There several different backends (ObjectStore, Local, DAV, ...)

The error message is completely meaningless: "file_put_contents failed".

The root cause is unclear. I would like to know more. I want to know why it failed.

Debugging this would be much easier if `file_put_contents()` would throw
an exception on failure instead of returning `false`.

The [man page of errno](https://man7.org/linux/man-pages/man3/errno.3.html) lists all the common errors which can happen.
It would help me if I would know if it is EDQUOT (Disk quota exceeded), ENAMETOOLONG (Filename too long), ENOSPC (No space left on device) ...

How would the above application code look like, if `file_put_contents()` would raise an exception instead of returning `false`?

It would be much simpler:

```
$this->view->file_put_contents($this->path, $data);
```

Next issue with returning "false" on error: I guess there are several calls to `file_put_contents()` which don't check the return
value and silently don't realize that something failed.

Guideline: Use exceptions to signal that something went wrong.


### Avoid clever guessing

These days I needed to debug a well-known Python library. It works fine,
but you don't want to look under the hood.

One method accepted an object with three different meanings types as
the first argument:

-   case1: a string containing HTML markup
-   case2: a string containing a file path. This file contained the HTML
    to work on.
-   case3: a file descriptor with a read() method.

This looks convenient at the first sight. But in the long run, it makes
things complicated. This kind of guessing can always lead to false
results. In my case, I always used case1 (it contained a small HTML
snippet). But, once the string was accidentally the name of an existing
directory! This crashed, because the library thought this is was a
file...

Conclusion: STOP GUESSING.

In Python, you can use classmethods for alternative constructors.

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

In most non-trivial projects there are several reasons why the
permission was denied.

If you (the software developer) only return "permission denied", then
the user/admin doesn't know the **reason**.

If you add a reason, then it is more likely that the user/admin can help
themselves.

This means they don't call you, our teammate, to solve this.

Fewer interruptions for your and happy customers, it's easy.

Or more general: Add enough information to error messages, to make it
easier to understand the current situation.

For example, you can add hyperlinks to docs/wiki/issue-tracker in you
errors messages.

### OOP: Composition over inheritance

If unsure, then choose "has a" and not "is a".

<https://en.wikipedia.org/wiki/Composition_over_inheritance>

### Cache forever or don't cache at all

Caching is like a false friend or a drug. It makes you happy today, but
in the long run, it brings you a headache.

Caching is easy, but cache invalidation is hard.

That's why the pattern "cache for every" is handy: You don't need
to invalidate anything.

> [Two Hard Things](https://martinfowler.com/bliki/TwoHardThings.html): There are only two hard things in Computer Science: cache invalidation and naming things. -- Phil Karlton


Avoid "maybe". If your HTTP code returns a response you have two choices
concerning caching:

-   the web client should cache this response forever.
-   the web client should not cache this response at all.

If you follow this guide you will get great performance since
revalidation and ETag magic is not needed.

I possible, avoid fiddling with ETag and If-Modified-Since HTTP headers.

But you have to care for one thing: If you cache forever, whenever you
update your data, you need to give your resource a new URL. That's easy:

For example: Instead of serving the file `/css/base.css` you serve `/css/base.27e20196a850.css`. The string "27e20..." is the md5 sum of the content of the file. Configure your webserver to serve this file with the appropriate "cache forever" headers, and your client will not ask for this file again. 


If you use Django, you can use the [ManifestStaticFilesStorage](https://docs.djangoproject.com/en/3.0/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage)


[Best Practices for Speeding Up Your Web Site (Yahoo)](https://developer.yahoo.com/performance/rules.html)

A good introduction to caching: [Caching (Mozilla Foundation)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

### Robust Cache-Invalidation

A database index is like caching: Redundant data gets created to achieve faster lookups.

If possible, use this robust caching and cache-invalidation provided by databases instead of creating your implementation.

### CDN for private data?

You might think unguessable URLs are enough to protect data. Only people with the URL can access it.

No, likely, your customers don't like this. The URL could be shared easily. 

You might argue that unguessable URLs are fine since an evil user could
download and upload the content, but this is a different case.

My rule of thumb: Put only public data on a CDN.

Nginx and Apache have this feature called [X-Sendfile](https://www.nginx.com/resources/wiki/start/topics/examples/xsendfile/). This
handy, since you can do authentication and permission checking in your application and tell the webserver to serve the blob data.

### Avoid coding for one customer

Try to avoid writing software just for one customer. If you write code
for one customer, you miss the great benefit of software: You can write
it once and make several customers happy. Of course, every business
starts small. But try to create a re-usable product soon.

### Misc

-   [Release early, release
    often](https://en.wikipedia.org/wiki/Release_early,_release_often)
-   [Rough consensus and running
    code.](https://en.wikipedia.org/wiki/Rough_consensus)

### Background tasks should preserve Buffer Cache State

You should know what this article talks about. But of course, you don't
need to recall every detail.

<https://insights.oetiker.ch/linux/fadvise/>

<https://github.com/Feh/nocache>

Use case: you use rsync to backup a Linux machine. The rsync process
should not slow down the production environment.

By default Linux thinks "A process just read file 'foo'. Let's keep the
content in the buffer cache". But rsync runs in the background and it does
not touch the same file twice.

It makes no sense to store the files which get read by rsync in the
buffer cache. The buffer cache should be available for the production
environment.


### Avoid POSIX locale

Avoid the [locale](https://en.wikipedia.org/wiki/Locale_(computer_software)). It causes your code to behave differently in different environments. Your code might be working during development and CI. But it might fail in production if there is a different locale active.

It is broken by design: First, you call `setlocal()` and after that methods do different things. That's stateful and confusing.

It does not follow the simple input-processing-output model.

I wasted too many hours with it. For example: [SAP PyRFC Bug #142](https://github.com/SAP/PyRFC/issues/142)

A library should always return the same output given the same input. The result should not
be different for different locales.

And the GUI? The user wants to use her/his favorite language. But native GUIs fade, and web GUIs come. And modern
web GUIs don't use locale anymore. Here you see "i18next vs locale" on [Stackoverflow tag trend](http://sotagtrends.com/?tags=[i18next,locale])

### Datetime class vs "seconds since 1970"

There are two ways to work with dates:

* old: Seconds since 1970 (the [Unix Epoch](https://en.wikipedia.org/wiki/Unix_time))
* new: Datatypes like [datetime](https://docs.python.org/3/library/datetime.html#datetime.datetime)

If unsure use the new datatypes. Don't fiddle with seconds since 1970 anymore.

Exception: Since file systems store the mtime (modification time) in "seconds since 1970" and you only want the age of the file in seconds, then it is simpler to stick to the old way. Related: [getmtime() vs datetime.now()](https://stackoverflow.com/questions/58587891/getmtime-vs-datetime-now)


### Statistical Profiler

Debugging and profiling are easy in a development environment. But how to debug a running production system?
A [Statistical Profiler](https://en.wikipedia.org/wiki/Profiling_%28computer_programming%29#Statistical_profilers) (or Sampling Profiler) is very cool. Every N millisecond the stack traces of the processes get dumped. This does not slow down your production environment at all. These dumps can reveal interesting facts. In which source code lines does the running application spend the most time?

There are commercial tools and some open-source tools.

For Python, there is [py-spy](https://github.com/benfred/py-spy) to dump the stack traces. The dumps can get analyzed by [speedscope](https://github.com/jlfwong/speedscope).

### Use "master" for development

You confuse newcomers if your development branch has a different name. If you call the development branch "master", then all introduction material at Github does apply. And if your code is at Github, all people can see that your project is still alive, since the master branch gets displayed per default.

Anecdote: The [tinelic](https://github.com/sergeyksv/tinelic/) project did all the coding in the "development" branch. The master branch was not updated for three years. I thought this project was dead. The maintainer was upset because he recently pushed changes into this branch. See [issue #9](https://github.com/sergeyksv/tinelic/issues/9#issuecomment-558557925)

------------------------------------------------------------------------

## 4. CI/CD

Continuous Integration, Continuous Deployment

### Canary release


> Canary release is a technique to reduce the risk of introducing a new software version in production by 
> slowly rolling out the change to a small subset of users before rolling it out to the entire 
> infrastructure and making it available to everybody.

Source: [martinfowler.com](https://martinfowler.com/bliki/CanaryRelease.html)

Imagine you have two versions: 

* Version A (the stable mainstream version)
* Version B (the version containing a new feature)


There are three ways you can implement the switch between the versions:

* Via routing: You have one system and several servers. Some servers use version A, some version B. A router decides which server should handle the request
* Via system: You have several systems (for example one system for each customer). You can update the system of one customer to version B, while the other customers use version A.
* Via code: The source code contains both implementations. According to some conditions either code version A or code version B gets executed. You usually use a feature-flag for this. This feature-flag can be set per user (or per customer).

For example, you updating an internal tool, but you don't want to interrupt the work of all developers.

Since it is just for internal tooling, you can implement canary releasing in an early adaptors list like this:

```
if user in ['thomas', 'peter', 'hugo', ....]:
    #### new way
else:
    #### old way
```

This way you ensure to don't annoy colleagues with your great new, but not yet mature features.

Example 2: You can use a mixture of "via system" and "via code". I guess you have some system-wide configuration
in your database. Then you can deploy the same source code to all systems and use code like this:

```
if system_config.feature_flag_foo:
    ### new way
else:
    ### old way
```

I like the "via code" way because this simplifies your CI/CD. You only have one current version which gets deployed to all places. Of course, this gets a bit more difficult if your change requires a database schema update.


### fewer conditions, less code, fewer bugs

I don't know if this is fake or real, but I guess it is true. In [this article from a former Oracle developer](https://news.ycombinator.com/item?id=18442941)
you can read the reason why developing on the huge codebase is hard. It is not the size, it is the number of flags.

Flags are nice, but they introduce complexity. Every flag is a condition. It changes the environment and the simple Input-Processing-Output method
does not work anymore. You have the explicit input and the indirect input from the environment.

If you can avoid conditions, then do it. Conditionless is the goal.

### Live at head

[Semantic Versioning (SemVer)](https://semver.org/) is well-known because it promises stability. But why do applications like Google Chrome or Firefox just push the first number? At the moment I use Chromium 87.0.4280.88, and soon I will use Version 88.x.y.z.

Chrome is not a software library, but where does LTS (long-term support) make sense?

There is only one future. Which version of Gmail do you use? 

Bridges, streets, and houses need to be stable. The stability of the overall infrastructure is important. And therefore we see a steady
reconstruction of bridges, streets, houses, and supply channels.

And software? Some associate with stability "peace of mind", I associate with stability a graveyard.

If you still think Semantic Versioning is useful, then please read [Philosophy of Abseil](https://abseil.io/about/philosophy)

Related: [Does SemVer work?](https://books.google.de/books?id=V3TTDwAAQBAJ&pg=PA445&dq=does%20semver%20work) [Software Engineering at Google](https://www.oreilly.com/library/view/software-engineering-at/9781492082781/)

So if not SemVer, what else? 

I like [Calendar Versioning](https://calver.org/) with [BumpVer](https://pypi.org/project/bumpver/).

### List of API Types

#### IPC
[IPC (Inter-Process Communication](https://en.wikipedia.org/wiki/Inter-process_communication)

For example you can configure an Nginx webserver to talk to your Python code via an unix-domain socket. Or you can access your PostgreSQL database running on localhost via an unix-domain socket.

#### Command line

Command line tools are like an API. The command receives structured input and creates output.

#### TCP/UDP

For example Wireguard uses UDP to create a VPN.

### http based 

RESTful

grpc

#### Library

A library can provide methods which you can use. For example the Python Standard Library provides many usefull methods which make your live easier.

### GraphQL / SQL

I have not found a matching generic term for this.

#### Conclusion?

Most people think "API" mean "http". But that's not true. 

For me, as a developer a library provides more value than an http API.

For example: Reducing the size of an image. If I have library which reduces
the size of an image, I have a handy tool. An http API which provides me
this services is not that easy. I need bandwith, what happens if the service is down,
I need to pay the bill for the service, ....



------------------------------------------------------------------------

## 4. Remote APIs

### Use http, avoid ftp/sftp/scp/rsync/smb/mail

Use http for data transfer. Avoid the old ways
(ftp/sftp/scp/rsync/smb/mail).

If you want to transfer files via HTTP from shell/cron you can use:
[tbzuploader](https://github.com/guettli/tbzuploader).

The next step is to avoid clever
[inotify](https://en.wikipedia.org/wiki/Inotify)-daemons. You don't need
this anymore if you receive your data via HTTP.

Why is HTTP better? Because HTTP can validate the data. If it is not
valid, the data can be rejected. That's something you can't do with
FTP/sFTP/SCP/rsync/smb/mail.

### Avoid Polling

Polling means checking for new data again and again. Avoid it, if
possible. Try to find a way to "listen" for changes. In most databases,
you can execute a trigger if new data arrives.

### Provide specific import directories, not one generic

If you still receive files via FTP/SCP since you have not switched to
HTTP-APIs yet, then be sure to provide specific input directories.

In the past, I received files in a directory called "import". Several
third-party systems sent data to this directory. It looks easy in the
first place. But sooner or later there will be chaos since you need to
know where the data came from. Was it from third party system FOO or was
the data from the third party system BAR? You can't distinguish anymore if
you provide only one import directory.

Now we provide import-FOO, import-BAR, import-qwerty ...

### Don't set up an SMTP daemon

If you can avoid it, then refuse to set up an SMTP daemon. If the
application you write should import mails, then do it by using POP3 or
IMAP and poll for new mail N seconds. Setting up an SMTP daemon is easy,
but being responsible for it is effort. Dealing with attacks, keeping an
eye on security announces... Live is easier without being responsible
for an SMTP server.

An SMTP daemon needs to run 24 hours a day. You get into trouble if it is
down. Or even worse: it is misconfigured and rejects all mails. These
emails get lost and won't come back.

If the [getmail](http://pyropus.ca/software/getmail/) job is down or is misconfigured it just won't fetch
mails. But it is unlikely that mails get lost.

I know this conflicts with the general guideline "avoid polling".


------------------------------------------------------------------------

## 5. Op

Operation. The last two characters of DevOp.

### Configuration Management

In the past configuration management tool like Ansible, SaltStack, Puppet or Chef
where used. Roughly since 2020 they are less important.

Configuration management is great for updating servers. But there are fewer
servers these days which get updated. Code runs in containers. 

To configure a container, you don't need fancy configuration management.

A simple conditionless (no "if", no "else", no "for") shell script (with `set -e`) 
is enough to create a container image.

#### The magic reload feature of Config Management is not needed anymore

Config management tools have magic reload feature. Imagine you update
the configuration of a webserver. The config management tools can detect
if a restart of a server is needed or not. For example: If the
configuration of the webserver was changed, then the webserver gets
reloaded. If the configuration of the webserver was not changed, then
there is no need to restart it. Great feature?

Ansible Docs: [Handlers: Running Operations On
Change](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#handlers-running-operations-on-change)

I liked this feature in the past.

Time has changed.

See above for "From CRUD to CRD". Kubernetes is coming. You create
containers, you run containers, you delete them. You don't update them
anymore.

The magic reload feature is not needed anymore.

Of course, this change from stateful servers to stateless containers
does not happen from one day to the next. One thing is sure: Stateful
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

In the past, it was common to create a custom RPM or Debian package to
install a file on a server.

For example an SSL cert.

If you have a configuration management tool, then this extra container
(RPM/DPKG) does not make much sense.

### Cron Jobs

A server exists to serve. If the server does not receive requests, why
should the server do something? This results in my rule of thumb:
Avoid cron jobs.

Sometimes you need to have a cron job for housekeeping.

Keep cron jobs simple.

In general, there are two ways to configure the arguments of a cron job:

-   the command line arguments which are part of the crontab line
-   additional source of configuration: config files or config from a
    database

Avoid mixing these two ways of configuring a cron job. I prefer to
configure the cron job via the latter of both ways. This keeps the cron
job simple. My guideline: Do not configure the cron job via optional
command-line arguments. Only use the required arguments.

### SSH to production-server

I still do interactive logins to production remote-server (mostly via
ssh). But I want to reduce it.

Sooner or later you will make a typo. See this article from GitLab for an
exciting report on what happened during a denial of service:
<https://about.gitlab.com/2017/02/01/gitlab-dot-com-database-incident/>
We are humans, and humans make mistakes. Automation helps to reduce the
risk of data loss.

If you are doing "ssh production-server ... vi /etc/..." or "... apt
install": Configuration management is much better. For example ansible.

If you are doing "ssh production-server .... less /var/log/...": No
log-management yet? Get your logs to a central place.

If you are doing "ssh production-server ... rm ...": Please ask yourself
what you are doing here. How can you automate this, to make this
unnecessary in the future?

### VPS vs Kubernetes vs PaaS

There are several ways to execute your code and make your application available to the public.

VPS: Virtual Private Server. These are cheap, you can get one for 3 Euro per month.
Benefit: You can install and configure it right the way you want it to be.
Drawback: You need knowledge about Linux.

Kubernetes: This is the current hype. It's great for huge data centers... but ...
do you have a huge data center?

PaaS: Platform as a Service. For the example provided by Heroku, Google, Amazon. They try
to make your life easier.
Pro: easy to use.
Con: more expensive.

My hint: if unsure use PaaS. If you want to learn the basics of running a server, then use a VPS.


### Keep your directories clean

There are two kinds of files in the context of backup: Files that should
be in the backup and temporary files which should not be in the backup.
Keep your directories clean. In a directory, there should be either only
files which should be in the backup xor only files which should not be
in the backup. This will make life easier for you. The configuration of
your backup is easier and cleaning temporary files is easier and looking
at the directory makes more joy since it is clean.

And overall, storing data in directories and files is outdated. If you
start from scratch, then put structured data in a database and binary data in an S3
compatible server.

### Avoid logging to files

I still do this, but I want to reduce it. Logs are endless streams.
Files are a bunch of bytes with a fixed length. Both concepts don't fit
together. Sooner or later your logs get rotated. Now you are in trouble
if you want to run a log checker for every line in your log file. I mean
the mathematical version of "every line". This gets complicated
if you want to check every line. Rotating log files needs to be done
sooner or later. But how to rotate the file, if a process still writes to
it? This is one problem, which was solved several hundred times and each
time different ...

In other words: Avoid logging to files and avoid logrotate. Logging is
an endless stream.

Of course, somewhere on the hard disk data gets stored in files. But it is highly
recommended using a tool where don't fiddle with files daily.

See [12factor App: #11 Treat logs as event streams](https://12factor.net/logs)

### Logging: Symptom vs root-cause

Filtering or ignoring errors is easy. People love it. But wait, why not fix the root-cause?

One example from hosting a Django application on a VPS:

You will see these messages soon after you set up your VPS:

> DisallowedHost at / Invalid HTTP_HOST header: '198.211.x.y'. You may need to add '198.211.x.y' to ALLOWED_HOSTS.

I guess you don't want your site to be accessed via IP, but some script-kiddies scan all public accessible
IPs all the time and check if http-host-header-validation is active.

The easy solution: Extend the ALLOWED_HOST setting and add your IP. But this does solve the issue just for some hours.
Soon some script-kiddies will use fakted HTTP_HOST header and you get useless warnings again.

What the next step? You use google and find a way to ignore the annoying error messages.

You could solve this by filtering the logs. You could ignore all `django.security.DisallowedHost` logs.

Now to the heading of this chapter: "Symptom vs root-cause"

Filtering logs just hides the symptom.

Fixing the root-cause means to configure the webserver in front of Django (for example Nginx)
to handle the broken request. These broken requests should not be forwarded to Django,
and then you don't need to add IP addresses to ALLOWED_HOSTS or ignore logs.

### Use Systemd

It is available, don't reinvent it. Don't do double-fork magic anymore.
Use a systemd service with Type=simple. See [Systemd makes many daemons
obsolete](https://stackoverflow.com/a/30189540/633961)

### Don't use Systemd "instantiated units"

Systemd allows you to create a template and create several services from
this template. See: <http://0pointer.de/blog/projects/instances.html>

First, I thought this was great. But some months later I realized: It is
better to have one source for templates: Your configuration management.
If you want several almost equal services, then use templates in your
configuration management.

This makes it simpler.

### etckeeper is handy

If you are using Kubernetes, then this makes no sense. But if you
are running services in a Linux server, then you want to know
what has changed?

The tool etckeeper stores changes in the /etc directory in a git
repository. This does not make much sense for containers. But for
servers that live several weeks, it makes sense. You don't need to push
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

Does it make sense to add the output of df into /etc/etckeeper/extra/?

I think it makes no sense since this changes daily. If no change was
made to the configuration, then there should be no commit in /etc/.git.

### If you do coding to implement backup ...

If you do coding/programming to implement your backup of data, then you
are on the wrong track.

You will likely do it wrong, and this will be a big risk.

Why? Because you will notice your fault if you try to recover your data.

**Use** a backup tool, even if you love to do programming. **Configure**
it, but don't write it yourself.

### Avoid re-inventing replication

That's what the customer wants you to implement:

You should transfer data from database A to database B. Every time there
is an update in database A, data should get copied to database B.

Slow down: What you are doing is replication. Replication creates
redundancy and redundancy need to be avoided.

Why do you want redundancy in your data storage? The only reasons I can
think of are speed/performance and fault-tolerant (like DNS/LDAP).

If replication is needed, then take the replication tools the
databases offers. Do not implement replication yourself. This is not
trivial and experts with more knowledge than you and me have solved this
issue before.

### Master-Master Replication

The real magic is Master-Master Replication. Here are some examples where it gets used:

todo: add examples

### Data transfer rates

It makes sense to have some rough numbers for rough estimates.

USB-2: 1.9 GByte per minute (rsync from PC to external hard disk). 3.6 GByte per minute (theory)

Office: Download: 200 Mbit/s, Upload: 20 Mbit/s

Home: Download 60 Mbit/s, Upload 12 Mbit/s, Latency 10 ms

Related: [List of interface bit rates](https://en.wikipedia.org/wiki/List_of_interface_bit_rates)

## 6. Networking

### No routing on servers

Imagine there are 20 servers in your network. Imagine there are two
network routes. One route goes to a second internal network and the
other route goes to the internet. All 20 servers should be able to
access both networks. There are two ways to solve this:

-   V1: Each of the 20 servers have the two routes configured.
-   V2: There is one default gateway for the 20 servers. Every server has one route. (The common term is "default gateway")

Please choose V2. It is simpler, it is easier to understand, it is less
error-prone, it is saner.

### traceroute won't help you

If you have trouble with a TCP connection, then use tcptraceroute. It
can help to find the firewall which blocks your IP packages trying to
get from host A to host B. Again \*tcp\*traceroute. It is the tool for
TCP connection tests (HTTP, HTTPS, ssh, SMTP, pop3, IMAP, ...). Reason:
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
You can write a Nagios plugin with any language you like. Often less
then ten lines of source code are enough to implement a Nagios check.

But on the other hand, it is not **stupid**. The checks do two things:
It collects some numbers (for example "How much disk space is left") and
it does evaluate and judge ("only N MByte left, I think this is a
warning"). That's not stupid this is some kind of intelligence.

After writing and working with Nagios checks for several years I think
the evaluation of the data should not be done inside the check. Some
data-collector should collect data. Then a different tool should
evaluate the data and judge if this ok, warn, or error.

### Checks vs Logs

Checks are mostly for operators and logs are mostly for developers.

Since there are always some temporary network failures, checks help more
than logs do.

Example:

1.  yesterday night at 3:40 there was a temporary network failure and this results in log messages.
2.  At 3:45 the network failure was gone.
3.  You look at the log message at 9:15. You don't know: Is this message still valid?

Checks get executed again and again.

If a check fails at 3:41 it will be ok some minutes later.

Then you know immediately that there was **temporary** failure.

Logs are important for developers for debugging.

But in this case, the developer can't do anything useful. Temporary
network failures happen again and again. That's live. Looking at the log
which was created by a temporary network failure wastes the time of the
developer.

Logs should contain the stack trace and the local variables of each frame
in the stack trace (a tool like sentry could be used), if real errors
occur.

### Logs vs Rows

Imagine you have a application-to-application synchronization process. Data from
system A needs to be pushed to system B. It is a custom synchronization and
you won't support this syncing, you just want to develop it. After developing this plugin
for system A you want it hand over to the customer.

Of course there are several error-conditions which can occur. The network between
both systems can be broken. One system can be down for some minutes, or there
is data which does not validate ....

If you implement traditional logging you have a time-stamp and some additional 
data like error messages or snippets of data which provide additional information.

You could dump this log together with thousand completely unrelated logs into NoSQL
based logging solution. The Logs coming from different sources have nothing in
common, except that they accidently have all a timestamp and a message.

Does this provide the best possible user experience? I don't think so.

There is big gap between system A and the important logging information.

Imagine ther is a data-set called "foo". This data-set could not get synced.
Of course system A has a page for "foo". 

The best user-experience would be a link from the page "foo" of system A to the
current state of the sync.

How to implement this? It is easy: Stop logging, start creating rows in a table.

If you have a table and rows in system A, then it is easy to provide a useful interface
for the operator of system A to see what's going. The big gap is gone. Information
is visible where you need it.

Of course this does not apply to every kind of logging. It hardly makes sense to 
write every http request/response into a database table. 


## ???. Web-Development

### HTML and CSS over JS

See [Bootstrap 5 "HTML and CSS over JS"](https://v5.getbootstrap.com/docs/5.0/extend/approach/#html-and-css-over-js)

### Use VanillaJS

VanillaJS is a very cool framework. By the way, it is a joke. VanillaJS means "Use Javascript without a framework".

Some think modern web applications need to use React or Vue. I don't think so. It is perfectly fine to send [html-over-the-wire](https://github.com/guettli/html-over-the-wire) and add some JS if needed.

### Validate Forms on the Server

You need to validate your form on the server anyway. So why implement it on the client-side? I think that in most cases it is perfectly fine
to validate forms only on the server and not on the client.


### A simple homepage? SSG!

If you create a simple homepage (without the need for a database), then an SSG (Static Site Generator) might be enough. A CMS is not needed.

See [Liste of Static Site Generators](https://github.com/guettli/static-site-generators)





------------------------------------------------------------------------

## 8. Communication with others

### Avoid getting a nerd

If you do "talk" with software to databases and APIs daily, your ability
to communicate with humans might decrease.

You might start to think like a computer (at least a bit).

The human mind works completely differently, not just bits and bytes. It
has [Emotions](https://en.wikipedia.org/wiki/Emotion)

Avoid getting a Nerd https://en.wikipedia.org/wiki/Nerd

Here some hints:

-   Nerds like complaining. This book can help: "Rethinking Positive
    Thinking: Inside the New Science of Motivation" by Gabriele
    Oettingen. The method is called WOOP.
-   Nerds like to think about their problems first. [Nonviolent
    Communication](https://en.wikipedia.org/wiki/Nonviolent_Communication#Four_components)
    can help.
-   Meet with "normal" people. With "normal" I mean people who do not do
    IT stuff.
-   Raise a family.
-   Do sport. Physical health is important.
-   Relax. Creativity raises if there is no input (no noise, no visible motion, ...)

### Avoid stress

Stress triggers your body’s “fight or flight” response. It pushes your
blood into the muscles. That's great if you need to jump onto the sidewalk because a fast red race car would hit you. But in your daily life,
this "fight or flight" response is hardly needed. You need the energy in
your brain :-)

Avoid stress, relax daily.

On the other hand, stress is fun: I like tennis and long-distance
running.

Care for both: brain and body.

### Discussion, but no progress? V1, V2, V3, ...

This and the following parts are about "Requirement Engineering".

If a discussion does not bring progress, then grab a pen. Start with V1. The
letter V stands for "Solution Variant" or "One strategy of several to
get to a goal". Find a term or short description of the first possible
strategy. Write it down. Then: which other ways could be used? V2, V3,
...

Rember, there is always the last variant: Leave things like they are
today and think about this again N days later.

If you have found several solution variants, then look at them in
detail. Most of the time it is useful to define the needed sequence of
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
-   S2: make the bed
-   S3: wash yourself
-   S4: get dressed
-   S5: eat
-   S6: take the bike and ride to work

I think the first letter (V, S) helps if you are brainstorming.

This can help if you or your team is stuck in [Analysis paralysis](https://en.wikipedia.org/wiki/Analysis_paralysis) (aka "overthinking" if
you are on your own, or endless discussions if there is a team).

Getting out of thinking/talking into writing and "naming solutions" helps to get an actionable plan.

In a professional environment, these notes about the options and the decision can get used as an entry in the "Decision Log".

### Avoid Office Documents or UML-tools

Choose a way to edit content (use cases, specs, ...) over the internet. Use
an issue tracking system or wiki.

Don't waste time with UML tools. UML is like
[esperanto](https://en.wikipedia.org/wiki/Esperanto). It is (in theory)
a great solution that solves a lot of problems. But somehow it does not
work.

Write down the high-level use case, the cardinality, and the steps.
Sequence diagrams can be simplified to enumerations: first step, second
step, third step ...

[Sketch](https://en.wikipedia.org/wiki/Sketch_(drawing)) screenshots you
want to build with your team with a pen. I avoid any digital device for
this since paper or a whiteboard is far more real. If you
need the result in digital format, just take a picture with your cell
phone at the end.

### Public ChangeLog

The public ChangeLog will never be perfect.

It is in the nature of things. Some customers want to know every small change.
Some customers only want to know the important changes. Some say that
they want to know every change, but then never look at it.

If you are working in a modern SaaS environment where you can
release often, then you should provide a ChangeLog, but don't take it too seriously for GUI changes.

Example: I see small changes in the GUI of GSuite weekly. I guess they don't have
a public ChangeLog, and I think it is not needed, too. 

A GUI should be self-explanatory.
That's important. Maybe the GUI changed, maybe not. It is like in the game chess: 
You look at the board and you face the question of what to do next. It does not matter
which chess piece was moved before. You see the board and now it is your turn.
The past does not matter in this context.

Things are different if you provide a public API. This must not change without clear communication.
Changes to public APIs should be announced several months in advance.


Things are different if you are working in an environment that has more constraints
(Medical devices, Banking, Insurance, ...), then you don't need my advice, since
you get told what to do.

### Communication with Customers: Binary decision "next" or "later" lists.

Define "done" with your customers. Humans like to be creative and if
thing X gets changed, then they have fancy ideas about how to change thing Y.
Be friendly and listen: Write these fancy ideas down on the "do later
list".

If the customer has new ideas, let them decide: Should this be on the
"do list" or the "do later list".

If you don't have a definition of done/ready, then you should not start
to write source code. First, define the goal, then choose a strategy to
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
tells the customer the desired result.

### Dare to say "Please wait, I want to take a note"

Most people can listen and write at once. I can't. And I guess a lot of
developers have this problem. I can only do one thing at a time. If you
are telephoning with a customer and he has a lot of things to tell you,
don't fool yourself. You will only remember 4 of 5 issues. Dare to say
"please wait, I want to take a note". This way, you can take care of all
issues, which results in happy customers.


### Avoid Gossip

Gossip creates an atmosphere that promotes negativity (bad karma).
Avoid making jokes about other teammates or customers. Yes, some people do strange stuff and have strange attitudes. Making jokes
about them makes everything worse. Please be aware that this guideline
has a major drawback. Sometimes, the people around you are laughing about
a customer or a teammate in their absenc ... and you are
the only one who is not laughing. It is up to you how to react. Be patient.

Same for irony and sarcasm. You and your friends might think it is funny. New team members and
other people won't understand you. It is not funny, it is confusing and childish.

### Avoid bike-shedding

Avoid [bike-shedding](https://exceptionnotfound.net/bikeshedding-the-daily-software-anti-pattern/). Please read this link. It is fun.
After you read this, you will see it happen again and again.

### S/N

[S/N](https://en.wikipedia.org/wiki/Signal-to-noise_ratio) means "Signal-to-noise ratio". It is a measure used in science and engineering that compares the level of the desired signal to the level of background noise. But it can be used for reflecting on his input, too.

How many useless news do I receive daily? Is there a way to improve S/N?

What is in my circle of influence, what not?

### Sometimes you just have to speak out a wish

Anecdote: In the year 2019 I loved to use the Stackoverflow Tag Trend. I wanted to
leave my current context (Python, Django, PostgreSQL Development) and learn new stuff.
In the Javascript ecosystem, there were so many tools available and it was difficult
to see which tool was great two years ago, but won't be used today if you can start from scratch. The Stackoverflow Tag Trend helped me to see what is hot and what was hot some years ago. Working 16 years for the same company I was blinded by routine and missed
a lot of changes outside my small IT context. 

A lot of things which I learned during studying information technology in Dresden from 1996 to
2001 was outdated. I wrote down these things on the [Deadends of IT](https://github.com/guettli/deadends-of-it) page.
The  Stackoverflow Tag Trend worked well, but I had ideas to improve it:

* [Tag aliases can be selected](https://github.com/robianmcd/tag-trends/issues/32)
* I was missing [Link from tag to the description of tag](https://github.com/robianmcd/tag-trends/issues/34)
* [URL could be simplified (no square brackets)](https://github.com/robianmcd/tag-trends/issues/33)
* [It was down for some days](https://github.com/robianmcd/tag-trends/issues/31)

After some weeks all my wishes were implemented by the author.

Win-win: He could improve the usability and for me is more convenient now. I love it. Sometimes you just have to speak out a wish. 
You just need to tell your wish to the right person in a friendly way.



### Self-Service instead of Communication

Setting up a simple workflow (maybe inside a chat tool) is easy. For example, the salesperson wants
a new demo system for a potential customer. He writes a message into a special chat channel and
someone with more technical background creates the demo system. Fine, isn't it?

Yes, this chat-based workflow is doable and it is better than direct messages to individuals.

Nevertheless **self-service** is better. Wouldn't it be great if setting up a new demo system
is simple and can be done by a salesperson who has no deep technical knowledge?

Communication is important, but daily tasks should get automated so that anyone who wants something
can help himself.

### Architectural Decision Records

Architectural Decision Records (ADR) help to explain the "why?" if new team members don't understand the current state.

Template:

> In the context of <use case/user story u>, facing <concern c> we decided for <option o> and neglected <other options>, to achieve <system qualities/desired consequences>, accepting <downside d/undesired consequences>, because <additional rationale>.

Source: [adr.github.io](https://adr.github.io/)




------------------------------------------------------------------------

## 9. Epilog

### It is always possible to make things more complicated

It is always possible to make things more complicated. The interesting
adventure is to make things simpler, easier, and more obvious.

### It helps to talk

Most software developers do not talk much. Otherwise, they would not have
chosen this job. If you think about something too long, then you get
blind to the obvious and easy solution. It helps to talk.

There is something called [Rubber duck
debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging). This might help, but talking to humans helps much more. If you find no
solution in 30 minutes. Take a break. Do something different, talk to a
teammate or friend, take a small walk outside.

### Being kind

Imagine you ask a question in a forum and your question gets down-voted and this comment:

> Hardware recommendations are off-topic at this site. 

That's not kind, not friendly.

But this is:

> You can may be try [Hardware Recommendations](https://hardwarerecs.stackexchange.com/)

Guideline: If you say "no", then at least provide a hint where he/she could find help.

### Be curious

There is always something you have not understood. Ask
questions, even if you think you know the answer. For one question,
there are always several answers. If you know one answer, then it is
likely that someone has a better answer.

I like:

-   <https://stackoverflow.com/>
-   <https://softwarerecs.stackexchange.com/>
-   <https://serverfault.com/>
-   And some mailing lists.

Often I just write the question and don't write about the solution I
have on my mind. If you write about our solution, then the discussion is
narrowed to a simple pro/contra of your idea. Ask the question like a
newbie.

### Sponsored Content in IT

Why does someone write an article on medium.com? Why does someone create content
which is free?

There can be a thousand reasons.

Some writers have good intentions. Some just do it to earn money by advertising something.

There are a lot of articles about AWS, Kubernetes, Azure, Docker, ElasticSearch, Graphana, Jamstack, Scrum, and other topics. Do you
think they all got written by volunteers who eat and breathe IT topics?

Some authors share their knowledge since they like to share their knowledge.

Sponsored Content is everywhere: Web, Facebook, Instagram, Youtube, Medium ...

Maybe it is even on Stackoverflow.

I would like to pay for high-quality news and articles, but so far I have not found a channel that
 offers a rich and broad-spectrum.



### Creativity Management

A lot of ideas come to my mind if I am far away from a laptop or pc.
For example, if I cycle from home to the office or back.

I started with this way of creativity management some years go: I write
a mail to myself.

If I cycle home on a Friday evening, I want to keep my mind relaxed and
focused on my family. All work-related thoughts should be far away. I
don't want to "carry" around work-related thoughts on the weekend. On
the road from office to home I might have an idea what to do (how to
hunt a strange bug, how to implement a cool feature which needs only a
very little effort and time to implement, ...). I stop (that is the great
advantage of riding a bike - I can stop almost always immediately, and
take my mobile phone). Then I write a mail to my business address and now
I am sure: This idea won't get lost. And I am free to have a nice
weekend with my family.

The same happens when I drive from home to the office: I have an idea
related to my personal life? I stop and write a mail to my account.

That's how most of this guide-line was created: Most items came to my
mind during cycling, walking, listening to music, or laying in the bathtub.
A short mail to myself, and some days later I opened the mail which contains
just a handful of words that I have articulated.

### Cut bigger problems into smaller ones

A lot of newcomers have problems with this. Here is one example to
illustrate the guideline "Cut bigger problems into smaller ones".

Imagine you are responsible for several servers and you should create
graphs of their disk/CPU usage.

Cut the bigger problem into smaller ones:

-   How to collect the data on one host
-   How to transport the data from the host to a central place?
-   How to store the data in a central database?
-   How to generate the graphs?

BTW, why not use the PostgreSQL feature "Logical Replication"?

### Go with the flow, not with the hype

Flow: With "flow" I mean "mainstream". And the mainstream is according to
oxford dictionary: "The ideas, attitudes, or activities that are shared
by most people and regarded as normal or conventional."

Hype: According to Wikipedia: "Hype (derived from hyperbole) is
promotion, especially promotion consisting of exaggerated claims."

But how to distinguish between a flow and hype?

My answer: Stats or more verbose "statistics".

How to get stats?

I like StackOverflow Tag-Trend. For example, you can compare "python"
and "java". Maybe you have been coding Java for several years. You
heard of python once or twice. But is it "flow/mainstream" or is it
"hype"? Since you only know your context and not every developer and
every project in the world, you can't know the answer. Be upright to
yourself: You are like a small ant. You walked several paths in the
past, but you don't have the helicopter view.

Check this graph: <http://sotagtrends.com/?tags>=\[java,python\] you
will see: Python is not just hyped it is the flow.

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

### Newsletters

* [Google Webmaster Central Blog](https://webmasters.googleblog.com/)

### Do you want your job to be exciting?

If your job is exciting, then it will be exhausting.

This is again the "boring" topic, which was one of the first topics of this text.

Urgency gives you a feeling of importance. But that's a [bias](https://en.wikipedia.org/wiki/Bias). Your emotions
are playing tricks on you. Important things are not urgent.

If you receive messages constantly (Mail, Chat, WhatsApp, Facebook, ...)
then you are not able to focus. My guide: switch off notifications and check
for messages only twice a day.

Focus on your boring todo-list which will bring you to your fancy goal.
Meet in small groups regularly. Before closing a meeting create
a small list of "who does what until when".

### Three Mail Accounts

I have three mail accounts:

-   personal mails (family, friends, ...)
-   work-related mails
-   mailing lists

Maybe you love your job. But your job is not your family. It is a kind of mental hygiene to
keep this separated.

### Clean up your desk

Don't forget to clean your desk. I didn't write this here because I do it
often and with joy. No, exactly the opposite. I wrote it down since I want
to push myself.

Don't look at all these things on your desk at once. Start on the left
side, take the first thing. Where is the best place for this single
thing? Unsure? Why not throw it in the trash can? If you are unsure put
it at least in a box behind a closed cabinet door. Some month later you
might be able to throw it in the garbage.

Then wipe the dust.

If you never have time to do this, then there is something wrong. Slow
down.

### Thrive for consistency

Imagine your software product is like a CMS. A user can edit pages. The product keeps 
versions of this page, so that the editor can revert to an older version.

Imagine the button to access history is labeled "History".

Terminology is more important than you think.

If the internal docs have the heading "Page versioning", then you know
that these docs explain the button labeled "History".

But that's not consistent. 

Imagine this story: A young and new developer wants to learn more about this feature. The
developers search in the internal docs with the keyword "history". This results
in wasting some minutes of the developer's precious time. Sure, the developer will find
the docs sooner or later, but it is not clean. But this is just the first part of the story.
The second part is that the new developer will get demotivated in having clean docs.

I don't want to call this "Page History" vs "Page Versioning" a problem. It is a spirit.

Usually, there are "thousand" places where a term gets used. In the above example, there will
be files, classes, methods, data structures containing this name. Changing the label of
the button is easy. Changing files, classes, methods, data structures is hard.

Sometimes it is too much effort to reflect a new wording in the GUI in all places. With "all"
I mean the places in the GUI and all the non-user visible places which are only visible for the
department developing the software.

Consistency on the GUI is a must. Consistency in other places makes sense.

Sometimes it makes sense. It depends.

The spirit of the future lies in your hand. You are always able to influence the upcoming
months.

### Highlander, "There can be only one"

"Highlander" is a 1986 British-American adventure action fantasy film
with the tagline "There can be only one". Thinking like this narrows your
mind. There can be several thousand. Look at how successful ants and bees
work. If someone is better or faster, then smile. Give applaud and say
"wow".

[Don't be evil.](https://en.wikipedia.org/wiki/Don%27t_be_evil) Don't
waste time and mental energy. Applauding if the competitor is better,
was new to me in 2017. I was at Rothenbaum and attended the German Open
(Tennis). The coach of one player was applauding every time the opponent
made a good shot. I was astonished. Why was the coach applauding the
enemy? But this works. If you get angry, you waste energy and you start
to think like a wild and stupid animal. Even if you have made a mistake
or lost somehow, no reason not to walk upright.

There are some rumors about "real programmers" and what they do. I think "real programmers"
use vi and are terrible slow because of the tunnel vision created by too much testosterone.
Smart developers have friends to talk to.

### Don't waste your time with cheap hardware

Some people love the [Raspberry
Pi](https://en.wikipedia.org/wiki/Raspberry_Pi). I don't like it. It
does not have enough computing power for my use cases. Yes, the device
is cheap, but I prefer to spend some more money to have more
performance. I don't like waiting.

### Write a diary

I think it helps to write a diary. Sitting down and writing about the
last days help you to reflect on the things you did. It helps you to focus
on your goals. Do you have goals? I found out that late (age of 40). A
diary is fun to read several months later. I try to do it at least once
a week. I have three types of diaries.

One on Facebook readable for everyone. It contains things from my daily
life, written in german. <https://www.facebook.com/thomas.guttler.52>

There is one on google-plus which contains IT topics (open source,
python, Linux, PostgreSQL), written in English and readable by everyone.
<https://plus.google.com/112821159206665920618>

And there is a private which I maintain with Anki. Anki is a flashcard
app. The front side is the question and the backside is the answer. I
use the first side for the date and one to three words, and the backside contains the text. This way I can ask myself what was on my mind
these days. But all this should be fun, not a burden.

### The Bus factor

From Wikipedia: The bus factor is a measurement of the risk resulting
from information and capabilities not being shared among team members

[Bus factor](https://en.wikipedia.org/wiki/Bus_factor)

Avoid creating secret knowledge that is only available to you. Share
knowledge.

Avoid overspecialization of yourself. It will have drawbacks. Imagine
there are some things which only you know. Sooner or later you want to
go on holiday and you want a relaxed holiday. You don't want to be
called on your mobile phone by your boss or a teammate. You want two
weeks off without a single interruption that is related to your work.

I guess all people love it if they are important. Everybody loves it
if someone needs them. But you will get burnout if no one else can do
the things you do.

Avoid overspecialization of a teammate, too. If a teammate has secret
knowledge and there is no one else who has a clue: Talk. Try to reveal
the things which only one person knows. Tell him about your concerns
(Bus factor). Maybe talk to his boss.

Imagine there is an action that needs to be done roughly twice a year.
For example, setting up a new server. Up until now, Bob did this every time.
Talk to your teammates. Explain that every action should be known to at
least two people. In practice, this means the next time Bob won't do it.
It needs to be done by someone else.

If you read the above sentences and think "that's not my job, that's the job
of the team leader", then I think it is time to stop acting like a dumb
sleeping sheep. Get responsible. React relaxed if nobody is listening or
understanding your concerns. "The Best Path to Long-Term Change Is Slow,
Simple and Boring."

### Related things I wrote

See [Thomas doing working out loud](https://github.com/guettli/wol)

### Related

- [Hacker-Laws](https://github.com/dwmkerr/hacker-laws)


### I need your feedback

If you have a general question, please start a [new discussion](https://github.com/guettli/programming-guidelines/discussions/new).

If you think something is wrong or missing, feel free to open an issue or pull request.

### Thank you

-   Robert C. Martin for the book "Clean Coder"
-   Malcolm Tredinnick. Only a few people listened as he did. With
    "listen" I mean "trying to understand the conversation partner".
-   Linus Torvalds for the quote "Bad programmers worry about the code.
    Good programmers worry about data structures and their relationships.".
-   Bill Gates for the quote "I choose a lazy person to do a hard job.
    Because a lazy person will find an easy way to do it."
-   All people who contribute to open-source software (Linux, Python,
    PostgreSQL, ...)
-   All people who ask questions and/or answers them at places like
    StackOverflow.
-   People I met during study at HTW-Dresden
-   My teammates at [tbz-pariv](http://www.tbz-pariv.de/).
-   <https://chemnitzer.linux-tage.de/> All people involved in this
    great yearly event.
-   Ionel Cristian Mărieș for the link to bash pitfalls.
-   Audience at my presentation at [Python User Group Leipzig 2019](https://www.meetup.com/de-DE/Leipzig-Python-User-Group/events/rbwmtpyzmbnb/)
-   Marco Bakera for hints (mailing-list python-de 2019)
