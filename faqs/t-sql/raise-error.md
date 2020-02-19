---
layout: faq
title: Debug output
sub_title: Getting non-buffered feedback/ progress from batches and stored procedures
faq_type: t-sql
---

## Debug output

SQL Server has a [PRINT](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/print-transact-sql) command that
can be used to send a message back to the client app (e.g. SSMS) but it's not ideal.  Firstly it's buffered, the message
will only be sent back to the client once the current output buffer is full.  Also it doesn't easily support printing
arguments into the message.

An alternative is to use the
[RAISERROR](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/raiserror-transact-sql) function with the
"WITH NOWAIT" option.  This will send the message back to the client immediately and it also supports printf style
argument printing.  This works just like the
[FORMATMESSAGE](https://docs.microsoft.com/en-us/sql/t-sql/functions/formatmessage-transact-sql) function.

``` sql
RAISERROR('This is the message', 0, 1) WITH NOWAIT
```

``` sql
RAISERROR('Checkpoint, @@ERROR=%d', 0, 1, @@ERROR) WITH NOWAIT
```

``` sql
RAISERROR('Checkpoint, stage=%s', 0, 1, @stage) WITH NOWAIT
```

And by the way, why is it *RaisError* and not *RaiseError*?  I don't know if this is true but I saw this quote once ...

"Ever asked yourself why there is only one “e” in RaisError?  The command comes from the old Sybase days where two
consecutive characters were cut back to one to save space."