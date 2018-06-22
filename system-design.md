Design a distributed ID generator and try to sort roughly by time

Tips:

The id should be unique. e.g. Database clustered index

The id should be short to save the memory. Usually 64bit integer is good enough

the record should be sortable by time

Solution: UUID

Mongodb Object id is a uuid 12 bytes = 12 \* 8 bits:

* 4 bytes for unix timestamp
* 3 bytes for machine id
* 2 bytes for process id
* 3 bytes for counter

Cons: too long bad for memory not efficient for index



