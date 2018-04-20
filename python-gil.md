## Global Interpreter Lock

GIL is a solution to make sure thread safety. Once core can only execute one thread at the same time.

For I/O task, python multi-thread will work. But for CPU heavy task,python multi-thread doesn't have advantage.



