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



what is GFS, big table, map reduce

find the top10 frequent keywords /mean number with map reduce

thread-safe consumer and producer

implement google instant search or twitter typeahead

