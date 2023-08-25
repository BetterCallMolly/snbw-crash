## level13

1. Check files in current working directory (home dir)
    - `-rwsr-sr-x 1 flag13 level13 7303 Aug 30  2015 level13`

Binary execution :

```bash
$ ./level13
UID 2013 started us but we we expect 4242
```

Symbols :

```
    U exit
    U getuid
    U printf
    U strdup
```

Well, the code might be as simple as that :

```c
int main(void) 
{
    uid_t uid = getuid();
    if (uid != 4242) {
        exit(1);
    }
    ...
}
```

Using `objdump` to disassemble the binary :

```asm
call   8048380 <getuid@plt> ; call getuid
cmp    $0x1092,%eax         ; compare the return value of getuid to 4242 (1092h)
je     80485cb <main+0x3f>  ; if equal, jump to 80485cb
```

There is multiple ways of solving this challenge, but one is safer.

Putting a break point on the `cmp` instruction, rather than replacing the `je` with a `jne` to jump if our uid is not 4242, we will change the `eax` register to 4242.

Why ? Because it seems like a function called `ft_des` is called after, so the correct uid (4242) might be used for some decryption or whatever.

```bash
gdb ./level13
b *0x804859a
r
p $eax = 0x1092
```

### Other trick

copy binary on VM and create user with UID 4242:
```
$ sudo useradd -u 4242 epicflag13
$ sudo su -l epicflag13
$ ./level13
your token is 2A31L79asukciNyi8uppkEuSx
```