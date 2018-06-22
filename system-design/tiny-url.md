how to generate a tiny url

**Length:**

it's good enough to use a 64bits integer

Also we can use a 7 chars string which can be treated as 62 hex \(A-Za-z0-9\)

So we can convert the 64 bits integer into a 62hex string

**Mapping:**

one long url to many tiny url

**reason:**

different users, time for different url

collect relevant data

**Calculate:**

Use the distributed id generator to generate a 64bits integer and convert the 64bits integer to a 62hex

**Redirect**

use 302 which is temporary redirect

301 is always redirect but it will miss the relevant information



**Limit**

limit the request number of single ip

use redis as a cache. save the long address-&gt; tiny id



