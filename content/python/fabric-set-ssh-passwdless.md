+++
date = "2017-02-15T08:36:54+05:30"
draft = false
title = "使用fabric工具配置ssh免密码登录"
+++

### must setup the localhost keygen

generate keys for localhost
```bash
ssh-keygen -t rsa
```

### setup passwdless in remote servers
   
the script set_pass.py is like below:
```python
from fabric.api import *
from fabric.contrib.files import *


user = 'user'
env.password = 'password'

env.hosts=[]
for ip in range(232, 240):
    env.hosts.append('%s@192.146.156.%d' % (user, ip))

def setpass():
        local("rm -rf passwd_set/authorized_keys")
        home = os.getenv("HOME")
        with open("%s/.ssh/id_rsa.pub" % home, "r") as fr:
                key = fr.read()
        get("~/.ssh/*", "passwd_set/.")
        local("touch passwd_set/authorized_keys")
        exists1 = False
        with open("passwd_set/authorized_keys", "r") as fr:
                if key in fr.read():
                        exists1 = True
        if not exists1:
                with open("passwd_set/authorized_keys", "a") as fw:
                        fw.write(key)
                put("passwd_set/authorized_keys", "~/.ssh/")

```

in the bash shell, could run the fabric to set servers passwdless in batch
```bash
fab -f set_pass.py setpass
```
