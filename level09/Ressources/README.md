## level09

1. Check files in current working directory (home dir)
    - `-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09*`
    - `----r--r-- 1 flag09  level09   26 Mar  5  2016 token`

This time, we can read the `token` file, but it contains non-printable characters, which is a good hint that we have some things to do with the `level09` binary.

Running it gives us a pretty weird string :


```bash
$ ./level09
You need to provied only one arg.
$ ./level09 AAAAAAAAAAAAA
ABCDEFGHIJKLM
$ ./level09 BBBBBBBBBBBBB
BCDEFGHIJKLMN
```

The code of the binary seems to be something like :

```c
int main(int argc, char **argv)
{
    if (argc == 1) {
        puts("You need to provied only one arg."); // nice typo
        return (1);
    }
    for (int i = 0; i < strlen(argv[1]); i++) {
        printf("%c", argv[1][i] + i);
    }
}
```

So I guess the `token` file contains the encoded flag, and we have to find the string that will decode it.

```perl
my $argv = $ARGV[0];

for (0..length($argv)-1) {
	my $val = substr($argv,$_,1);
	print chr(ord($val)-$_);
}
print "\n";
```

And we can pass the content of the `token` file to this script.