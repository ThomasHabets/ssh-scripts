# ssh-scripts

Collection of scripts to use with SSH.

## `sshexpect.py`

Upload/download files inline with an interactive SSH session.

```
./ssexpect.py ssh shell.example.com
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
