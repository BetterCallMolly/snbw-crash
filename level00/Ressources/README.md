## level00

1. Check files in current working directory (home dir)
    - Nothing to see here

2. Checking what files I (`level00`) own (excluding `/proc/*`)
    - Nothing to see here

3. Checking what files is owned by the user `flag00`
    - `/usr/sbin/john`
    - `/rofs/usr/sbin/john`

    1. Using `cat` on any of these files returns `cdiiddwpgswtgt`
    2. After a quick educated guess, it seemed like `rot` cipher
    3. After tinkering with [CyberChef](https://gchq.github.io/CyberChef/) I found out that it was `rot11`
    4. Trying to login to user `flag00` with the password `nottoohardhere`