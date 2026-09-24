https://medium.com/@zeroDaykt/mastering-oscp-in-2025-26-the-updated-exam-my-fails-wins-how-you-can-do-it-c44534bfcf54 
https://github.com/antonytuff/Red-Team-Notes/blob/master/InfoSec-Learning-Materials/OSCP-Survival-Guide.md
https://github.com/d4t4s3c/OffensiveReverseShellCheatSheet
# 0. Notes
## Recording
```shell
# Start
script -t timing.log session.log 

# Stop
exit

# putar lagi
scriptreplay timing.log session.log
```
## Split Screen
```shell
tmux -s pirate.htb

# detach Ctrl+b d
# resume 
tmux attach -t pirate.htb
```
# 1. Linux
1. [ ] Nmap 
	1. [ ] save SCV -p-
	2. [ ] save SUV 
2. [ ] Cek Web Service yang ada
	1. [ ] React2shell

# 2. Windows Server Checklist

1. [ ] Nmap 
	1. [ ] save SCV -p-
	2. [ ] save SUV 
2. [ ] Scan Credential
3. [ ] Scan otomatis SMB
4. [ ] Scan otomatis LDAP
	1. [ ] Save Users
	2. [ ] Save Groups
5. [ ] Cek Kerberos
6. [ ] Scan Bloodhound
7. [ ] Scan Bloodyad
8. [ ] Scan Certipy
	1. [ ] Cek Ghost Template
9. Cek Web Service yang ada