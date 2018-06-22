Design a distributed ID generator and try to sort roughly by time

Tips:

The id should be unique. e.g. Database clustered index

The id should be short to save the memory. Usually 64bit integer is good enough

the record should be sortable by time



Solution: UUID

Mongodb Object id is a uuid:

* 4 bits for unix timestamp
* 3 bits for machine id
* 2 bits for process id
* 3 bits for counter



