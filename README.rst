Programming Guidelines
======================

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

A relational database is a rock solid fundment. Use a tool to get schema migrations done (for example django).

Version Control
---------------

Use git.


Time is too short to run all tests before commit+push
-----------------------------------------------------
If the guideline of your team is: "Run all tests before commit+push", then there
is something wrong. Time is too short to watch tests running!

CI
--
Use continous integration. Only tested code is allowed to get deployed. This needs to be automated. Humans make more errors than automated processes.

Avoid Threads and Async
-----------------------
Threads and Async are fascinating. BUT: It's hard to debug. You will need much longer than you initial estimated. Avoid it, if you want to get done. It's different in your spare time: Do what you want and what is fascinating for you.

