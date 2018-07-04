design what's app

design fb/twitter

### Feed

Scenario:

Three feed list

Timeline

Read: QPS = 300k /s

Write: QPS = 5k/s

Notify 1 m follower in 1 s

concurrent users = 150m

Daily news = 400m

**Write \(push\)**

get Fanout and social graph

write to each timeline

Optimise:

only notify the online user/active user

**Write \(pull\)**

write to it's own timeline

## Optimize:

Save the timeline into the memory

Only save the active users

Only save the recent feed

To reduce the size of the feed, we can only save the metadata

### How to handle the cache miss?

read some feed from disk to prepare the data

### Interface Design:

Get user list with time range or recent k news

To avoid shift issue:

set max limit and min limit with 3 parts



### How to save the space on the disk:

GFS desc with timestamp



