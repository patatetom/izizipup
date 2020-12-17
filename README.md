# izizipup

oneshot easy zip uploader

_transferring a file can quickly become a puzzle... **izizipup** tends to solve this difficulty in a simple way._
_of course, the sender must be able to contact **izizipup** (firewall et cetera...)._



## installation
### system side

```
# pip install flask apscheduler
# curl https://github.com/patatetom/izizipup/raw/main/izizipup > /usr/local/bin/izizipup
# less /usr/local/bin/izizipup
# chmod +x /usr/local/bin/izizipup
# izizipup
warning: using root account !
usage: izizipup [-p PORT] (-b BIND | -u URL) filename
izizipup: error: the following arguments are required: filename
```


### user side

```
$ pip install --user flask apscheduler
$ curl https://github.com/patatetom/izizipup/raw/main/izizipup > ~/.local/bin/izizipup
$ less ~/.local/bin/izizipup
$ chmod +x ~/.local/bin/izizipup
$ izizipup
usage: izizipup [-p PORT] (-b BIND | -u URL) filename
izizipup: error: the following arguments are required: filename
```
