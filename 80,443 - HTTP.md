# Directory
```shell
# wordlist
dirsearch -u http://underpass.htb/ -r -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-directories.txt

# All extention
dirsearch.py -u https://qantor-add.southeastasia.cloudapp.azure.com -e asp,aspx,jsp,php,csv,doc,docx,xls,xlsx,ppt,pptx,pdf,bak,conf,config,old,sql,jar,rar,zip,tar,tar.gz,apk,ipa,cgi,do,htm,html,js,json,rb,xml,yml,svn,git
```

# Subdomain
```shell
ffuf -c -u http://monitorsfour.htb/ -H "Host: FUZZ.fries.htb" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt 
```