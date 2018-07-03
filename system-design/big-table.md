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

