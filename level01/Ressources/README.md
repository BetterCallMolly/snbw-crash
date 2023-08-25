## level01

1. Check files in current working directory (home dir)
    - Nothing to see here

2. Checking what files I (`level01`) own (excluding `/proc/*`)
    - Nothing to see here

3. Checking what files is owned by the user `flag01`
    - Nothing to see here

4. Testing `getflag` ?
    - Of course it doesn't work

5. Checking which file doesn't have normal permissions
    1. Checking read permissions in `/etc` using the command : `find /etc -maxdepth 1 -type f -perm -u+r | less`
    2. `/etc/passwd` is readeable, it's a Linux file that contains infos about users, and it can contain the hashed password
    3. The line `flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash` does indeed contains the hashed password, which is a problem
    4. Cracking the password with `hashcat 42hDRfypTqqnw -O -w 4 -d 1 -m 1500 -a 3 -1 '?l?u' '?1?1?1?1?1?1?1'`

    ```
    Session..........: hashcat
    Status...........: Running
    Hash.Mode........: 1500 (descrypt, DES (Unix), Traditional DES)
    Hash.Target......: 42hDRfypTqqnw
    Time.Started.....: Sun Aug 20 15:00:21 2023 (1 sec)
    Time.Estimated...: Sun Aug 20 15:03:11 2023 (2 mins, 49 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Mask.......: ?1?1?1?1?1?1?1 [7]
    Guess.Charset....: -1 ?l?u, -2 Undefined, -3 Undefined, -4 Undefined 
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:  6038.9 MH/s (176.17ms) @ Accel:32 Loops:1024 Thr:256 Vec:1
    Recovered........: 0/1 (0.00%) Digests (total), 0/1 (0.00%) Digests (new)
    Progress.........: 2147483648/1028071702528 (0.21%)
    Rejected.........: 0/2147483648 (0.00%)
    Restore.Point....: 0/7311616 (0.00%)
    Restore.Sub.#1...: Salt:0 Amplifier:2048-3072 Iteration:0-1024
    Candidate.Engine.: Device Generator
    Candidates.#1....: oHAeran -> bheFQBL
    Hardware.Mon.#1..: Temp: 71c Fan: 31% Util:100% Core:2730MHz Mem:10501MHz Bus:16
    
    42hDRfypTqqnw:abcdefg
    ```
