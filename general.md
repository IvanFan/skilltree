# Multiplexing

If the kernel find that the condition read operation for a particular process is matched, then the kernel will notify that process.

It's used for the following situations:

1. if the client need to handle multiple input interfaces \(e.g. screen input and network input\)
2. If the tcp server need to listen to ports and need to handle connecting ports
3. if the server need to handle tcp and udp 
4. if the server need to provide different services and different protocols

 

# select,poll & epoll

They are all sync I/O \(the R/W is blocked\)

select has 3 issues:

1. limitation of connection number
2. slow to find comparsion
3. 


