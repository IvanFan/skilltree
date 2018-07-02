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

Data user table\(v1\)

```
class UserTable:
      def __init__(self):
             self.users = []
      CURD

```





Please design a user system

Please design a payment system

