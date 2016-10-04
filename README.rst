Programming Guidelines
======================

My programming guidelines. Please provide feedback, tell me what's wrong and what's missing: https://github.com/guettli/programming-guidelines/issues

No Shell Scripting
------------------

The shell is nice for interactive usage. I use it daily. But shell scripts are unreliable. Don't do it. Use Python.

C is slow
---------

... looking at the time you need to get things implemented. Avoid it.

SSH to remote-server
--------------------

I still do this, but I want to reduce it. Configuration management is much better. For example salt-stack

Logging to files
----------------
I still do this, but I want to reduce it. Logs are endless streams. Files are a buch of bytes with fixed length.
Both concepts don't fit together. Logrotating is a happit from the last century.

Use Systemd
------------

It is avaible, don't reinvent. Don't do double-fork magic any more. Use service Type=simpel

Avoid Office Documents
----------------------

Use a way to edit content (use cases, specs, ...) over the internet: Use wikis.


Focus on datastructures
-----------------------

A relational database is a rock solid fundment. Use a tool to get schema migrations done (for example django). Use PostgreSQL.

Version Control
---------------

Use git.


Time is too short to run all tests before commit+push
-----------------------------------------------------
If the guideline of your team is: "Run all tests before commit+push", then there
is something wrong. Time is too short to watch tests running! Run only the tests of the code you touched (py.test -k my_keyword).

CI
--
Use continous integration. Only tested code is allowed to get deployed. This needs to be automated. Humans make more errors than automated processes.

Avoid Threads and Async
-----------------------
Threads and Async are fascinating. BUT: It's hard to debug. You will need much longer than you initial estimated. Avoid it, if you want to get done. It's different in your spare time: Do what you want and what is fascinating for you.

Don't waste time doing it "generic and reusable" if you don't need to
----------------------------------------------------------------------
If you are doing some kind of software project for the first time, then focus on getting it done. Don't was time to do it perfect, reusable, perfomant or portable. 
You don't know the needs of the future today.

Use all features PostgreSQL does offer
--------------------------------------

Use all features PostgreSQL does offer. Don't constrain yourself to use only the portable features. It's ok if your code does work only with PostgreSQL and no other database. If there is the need to support other databases in the future, then handle this problem in the future, not today. PostgreSQL is great, and you waste time if you don't use its features.


Use a modern IDE
----------------

Time for vi and emacs has passed. Use a modern IDE on modern hardware (SSD disk). For example PyCharm. I switched from Emacs to PyCharm in 2016. I used Emacs from 1997 until 2015 (18 years).

Cardinality
-----------

It does not matter how you work with your data (struct in C, classes in OOP, tables in SQL, ...). Cardinality is very important. Using 0..* is often easier to implement than 0..1. The first can be handled by a simple loop. The second is often a nullable column/attribute. You need conditions (IFs) to handle nullable columns/attributes.

https://en.wikipedia.org/wiki/Cardinality_(data_modeling)

Communication with Customers: Tell customers what they should test
------------------------------------------------------------------
I have seen it several times: Software gets developed. The customer was told to test and ... nothing happens. That's not satisfying since software developers want to hear that their work does help. If you (the developer) provide a check-list of things to test, then the likelihood to get feedback soon is bigger.

