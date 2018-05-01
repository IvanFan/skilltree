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

Sed:

export line

```
cat test.txt| sed -n "2p"
2, Taylor Swift, Title 723, Price $7.90
```

replace

```
cat test.txt | sed 's/Title/TEST/'
1, Justin Timberlake, TEST 545, Price $6.30
2, Taylor Swift, TEST 723, Price $7.90
3, Mick Jagger, TEST 610, Price $7.90
4, Lady Gaga, TEST 118, Price $6.30
5, Johnny Cash, TEST 482, Price $6.50
6, Elvis Presley, TEST 335, Price $6.30
7, John Lennon, TEST 271, Price $7.90
```

Sed is also frequently used to filter lines in a file or stream. For example, if you only want to see the lines containing "John," you use:

```
cat test.txt | sed -n '/John/p'
5, Johnny Cash, Title 482, Price $6.50

```

Replace every occurrence of Nick with John in report.txt

```
sed 's/Nick/John/g' report.txt
sed 's/Nick|nick/John/g' report.txt

cat test.txt | sed 's/^/   /'
   1, Justin Timberlake, Title 545, Price $6.30
   2, Taylor Swift, Title 723, Price $7.90
   3, Mick Jagger, Title 610, Price $7.90
   4, Lady Gaga, Title 118, Price $6.30
   5, Johnny Cash, Title 482, Price $6.50
   6, Elvis Presley, Title 335, Price $6.30
   7, John Lennon, Title 271, Price $7.90
```

Show only lines 12-18 of file.txt

```
sed 12,18d file.txt

```

Write all commands in script.sed and execute them

```
sed -f script.sed file.txt

```

Replace ham with cheese in file.txt except in the 5th line

```
sed '5!s/ham/cheese/' file.txt

```



