# ssh-scripts

Collection of scripts to use with SSH.

This is not an official Google product.

## `ssmodem`

Upload/download files inline with an interactive SSH session.

```
./ssmodem shell.example.com
Password: <…>
user@host$ ls
user@host$ rz
[… garbage …]
^]
> [u]pload/[d]ownload/[t]ype: u
> Local file name: foo.*
[… garbage …]
user@host$ ls
foo.crt
foo.key
user@host$
```

## `oassh`

Reconnect typing password and `screen -x` if you get disconnected.

```
./oassh shell.example.com
Overly attached password: <…>
Immediate command: <…>
[… logged in…]
```
