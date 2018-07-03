## write a crawler

```
import requests

r = requests.get('https://api.github.com/events')
```

## Explain the process of crawler

## ![](/assets/Screen Shot 2018-07-03 at 2.32.09 pm.png)

## the process of connection \(TCP\)

TCP 3 handshake

Where is socket? between application, session, presentation layer and transport layer network layer, data link layer

For the crawler:

list crawler:  get latest data list

data crawler

a better way is to design a scheduler and crawler will be passed into different strategies

scheduler can also have priority status retry timeout task created time

thread safe with lock

![](/assets/Screen Shot 2018-07-03 at 3.02.50 pm.png)

what is GFS, big table, map reduce

find the top10 frequent keywords /mean number with map reduce

thread-safe consumer and producer

implement google instant search or twitter typeahead

## conditional variable

## Design a search engine on news

Cases:

search keyword

search news list

add update delete news

MapReduce, BigTable, GFS

## What's the layer of search engine?

![](/assets/Screen Shot 2018-07-03 at 5.18.52 pm.png)

**Applications =&gt; Algorithm \(MapReduce or others\) =&gt;  data model \(big table\) =&gt; file \(GFS\)**

Let's design from bottom to up

## GFS

![](/assets/Screen Shot 2018-07-03 at 10.37.42 pm.png)

1 block = 64kb

![](/assets/Screen Shot 2018-07-03 at 10.39.00 pm.png)

how to save a large file save as chunk

Pros: reduce the size of the metadata, reduce traffic

Cons: waster disk space for small files

How to save an extra-large file? \(cannot save within one disk\)

![](/assets/Screen Shot 2018-07-03 at 10.41.46 pm.png)

key: one master + many chunk servers

Cons: any change of the disk offset of a chunk on the chunk server have to notify the master

![](/assets/Screen Shot 2018-07-03 at 10.43.53 pm.png)

master doesn't save the offset only record the server

the chunk server will save the offset

chunk itself is continuous

Pros: reduce the size of metadata in master, reduce the traffic between master and chunk server

## How to identify whether a chunk on the disk is broken?

For each block we will save a checksum in the metadata

Key Point:

1 chunk = a list of biocks

1 block = 64kb

each block has a checksum

1 checksum = 32 bit

The size of checksum of 1 T file = 1T/64kb\*32bit = 64mb

It will compare its checksum when it read the block

## 

## How to avoid data loss when a chunk server is down/fail?

Key point:

Replicate the chunks

how many replications? 3

how to select chunk servers of a chunk?

* server with below-average disk space utilization
* limited number of 'recent' creation \(read too often\)
* across racks/area 2+1

checksum is saved on the chunk server within the memory for quick read ops

### How to recover when a chunk is broken

![](/assets/Screen Shot 2018-07-03 at 10.56.10 pm.png)

Key point: ask master for help

master will record the broken and recover

We check the checksum when we read the data

we can also have a background process to check the old file correctness

## How to find whether the chunk server is down

master will check the heart beat of the chunk server

## How to recover the data if chunk server is down

first the master will check the lost data on the failed chunk server

Then the master will start a repair procedure to make up the data to 3 copy

Key point : repair priority is based on the number of replications

### How to avoid hot spot?

![](/assets/Screen Shot 2018-07-03 at 11.05.12 pm.png)

the master will check the chunk stats and the server stats

Key point:

* replicate a chunk into more replications when it's busy
* fill the replication into the chunkserver with more pace and more bandwidth

## How to read from a file?

![](/assets/Screen Shot 2018-07-03 at 11.10.37 pm.png)

