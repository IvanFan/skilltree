## How to support lookup and range query on a file?

Table = A list of sorted key value![](/assets/Screen Shot 2018-07-03 at 11.26.57 pm.png)

## how to save a large table

one table to hold the metadata and other table to save the details

![](/assets/Screen Shot 2018-07-03 at 11.28.25 pm.png)

a table =  a list of tablets

a tablet = a list of sorted key value

### How to save an extra-large table?

### ![](/assets/Screen Shot 2018-07-03 at 11.30.11 pm.png)

### How to write into a table

![](/assets/Screen Shot 2018-07-03 at 11.32.15 pm.png)key point: write into memtable\(within memory\)

### ![](/assets/Screen Shot 2018-07-03 at 11.35.10 pm.png)How to avoid system failure

### ![](/assets/Screen Shot 2018-07-03 at 11.37.21 pm.png)how to read data fast

SStable inside is sorted![](/assets/Screen Shot 2018-07-03 at 11.40.00 pm.png)A SSTable = a list of sorted key value= a list of 64kb blocks + index

indices are pre-loaded into memory

read fast by: user indices to identify the place in disk

### How to read data faster?

### ![](/assets/Screen Shot 2018-07-03 at 11.44.10 pm.png)

check the existence of the element with bloomfilters

### How to build bloom filter?

bit map

## ![](/assets/Screen Shot 2018-07-03 at 11.46.29 pm.png)![](/assets/Screen Shot 2018-07-03 at 11.47.00 pm.png)![](/assets/Screen Shot 2018-07-03 at 11.47.16 pm.png)How to save a table into GFS

## ![](/assets/Screen Shot 2018-07-03 at 11.48.54 pm.png)

### What is the logical view of table

![](/assets/Screen Shot 2018-07-03 at 11.52.27 fuypm.png)save different versions of pages at the same position

### How to transform logic view into physical storage?

### ![](/assets/Screen Shot 2018-07-03 at 11.54.50 pm.png)What's the architecture of Big table?

![](/assets/Screen Shot 2018-07-03 at 11.58.39 pm.png)

