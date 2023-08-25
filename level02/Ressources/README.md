## level02

1. Check files in current working directory (home dir)
    - `----r--r-- 1 flag02 level02 8302 Aug 30  2015 level02.pcap`

So here, we have a Wireshark packet capture file, we will download it using `sftp`

Running `strings level02.pcap` we see a "Login incorrect" message, so we'll use that to search with Wireshark.

It corresponds to the frame #91, so we just have to check frames sent before with the filter `ip.src == 59.233.235.218`

And I couldn't find anything, so after looking around, I found we could Follow a TCP Stream, which, I did. This reveals a `Password: ft_wandr...NDRel.L0L` string.

In the context of Wireshark, dots represents.. dots, but also non-printable characters, clicking on each dots shows us multiple packets with a payload of 1 byte, `0x7F` which is `DEL`.

So the password should be : `ft_waNDReL0L` !