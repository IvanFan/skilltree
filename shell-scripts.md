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



