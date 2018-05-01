```
ls -la | grep "XXXX"
```

To get system details:

```
uname -a
```

get all background tasks

```
jobs
```

To execute commands at background, add & at the very end

```
$ sudo rsync Templates/* /var/www/html/files/ &
```

To bring the background job to foreground

```
fg jobid
```

To kill process

```
kill -9 PID
killall keyword
```

scp

```
scp your_username@remotehost.edu:foobar.txt /some/local/directory
$ scp foobar.txt your_username@remotehost.edu:/some/remote/directory
```

grep

You can force grep to ignore word case i.e match boo, Boo, BOO and all other combination with the -i option:

```
$ scp foobar.txt your_username@remotehost.edu:/some/remote/directory
```

You can search recursively i.e. read all files under each directory for a string “192.168.1.5”

```
$ grep -r "192.168.1.5" /etc/
```

search  different words

```
egrep -w 'word1|word2' /path/to/file
```

Pass the

-n

option to precede each line of output with the number of the line in the text file from which it was obtained:

```
$ grep -n 'root' /etc/passwd

```



