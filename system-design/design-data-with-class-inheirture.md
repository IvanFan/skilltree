Design the data with class and inheirture

user register

**daily active usr = 1,000,000**

**daily active user in 3 month = 1,000,000 \* 10**

**register percentage = 1% \(will changed\)**

**daily new registered user = 10,000,000 \* 1% \(not very big\)**

**login percentage = 15%**

**average login time = 1.2**

**daily login time = 10,000,000 \* 15% \* 1.2 = 1,800,000 \(a little bit large\)**

**average login frequency = 1,800,000 / 24 hour = 21/s **

**normal login frequency = 21 \* 2 = 42/s**

**peak login frequency = 42 \* 3 = 126/s**

Data - user \(v1\)

```
class User:
    int user_id // primary key
    string name
    string password
```

Data user table\(v1\)

```
class UserTable:
      def __init__(self):
             self.users = []
      CURD
```

Can you support advanced account management functions such as verification and ban?

State lifecycle graph\(v1\)

start -&gt; register-&gt; Active-&gt;end

start -&gt; register-&gt; Unverified-&gt;Active-&gt;end

Data - user \(v2\)

```
class User:
    int user_id // primary key
    string name // fix the size 10
    int state
    string password // fix the size of password
```

Data user table\(v2\)

```
class UserTable:
      def __init__(self):
             self.users = []
      CURD
```

Session lifecycle graph\(v1\)

login -&gt; PC-online -&gt; timeout

iphone-online ipad-online

different devices different sessions

Session with sessionID userID timeOut device Code \(also we can use it for token\)

Session should be removed from user table

## ![](/assets/Screen Shot 2018-07-02 at 10.32.55 pm.png)

## Challenge of lookup

## index \(hashtable\) -&gt; clustered index unclustered index

B+Tree

Space: O\(n\)

Time of operation:O\(1\) time of range query O\(logbN+k\)

disk friendly with continuous reading  在硬盘上是顺序的存

transaction log

write log

read log

lock

Mysql has a checker when changing the data

## DataBase Duplication:

linear 传行多级备份 A=&gt;A1=&gt;A2=&gt;...

daily checkpoints with daily log easy to recover

Disk data consistency can use check sum to make sure the consistency

read/write apart =&gt; no consistency because of the time delay

check sum should stored separately. Some of it should be put into memory or into log

