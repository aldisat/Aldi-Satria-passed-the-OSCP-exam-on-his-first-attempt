https://medium.com/@zeroDaykt/mastering-oscp-in-2025-26-the-updated-exam-my-fails-wins-how-you-can-do-it-c44534bfcf54 
https://github.com/antonytuff/Red-Team-Notes/blob/master/InfoSec-Learning-Materials/OSCP-Survival-Guide.md
https://github.com/d4t4s3c/OffensiveReverseShellCheatSheet
# 1. Notes
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

# 2. Privilege Escalation
https://github.com/Sp4c3Tr4v3l3r/OSCP/blob/main/Windows%20Privilege%20Escalation.md