# Recording
```shell
# Start
script -t timing.log session.log 

# Stop
exit

# putar lagi
scriptreplay timing.log session.log
```
# Split Screen
```shell
tmux -s pirate.htb

# detach Ctrl+b d
# resume 
tmux attach -t pirate.htb
```

