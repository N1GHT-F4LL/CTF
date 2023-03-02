# Tool For CTF:

## [pwnable](https://ctf101.org/binary-exploitation/overview/)

[PWNGDB](https://github.com/pwndbg/pwndbg)
```bash
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```

[PEDA](https://github.com/longld/peda)
```bash
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
echo "DONE! debug your program with gdb and enjoy"
```

[GEF](https://github.com/hugsy/gef)
```bash
# via the install script
## using curl
bash -c "$(curl -fsSL https://gef.blah.cat/sh)"

## using wget
bash -c "$(wget https://gef.blah.cat/sh -O -)"

# or manually
wget -O ~/.gdbinit-gef.py -q https://gef.blah.cat/py
echo source ~/.gdbinit-gef.py >> ~/.gdbinit

# or alternatively from inside gdb directly
gdb -q
(gdb) pi import urllib.request as u, tempfile as t; g=t.NamedTemporaryFile(suffix='-gef.py'); open(g.name, 'wb+').write(u.urlopen('https://tinyurl.com/gef-main').read()); gdb.execute('source %s' % g.name)
```

