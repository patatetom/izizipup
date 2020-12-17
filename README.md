# izizipup

oneshot easy zip uploader

_transferring a file can quickly become a puzzle... **izizipup** tends to solve this difficulty in a simple way._
_of course, the sender must be able to contact **izizipup** (firewall et cetera...)._



## installation

### system side

```
# pip3 install flask apscheduler
# curl -L https://github.com/patatetom/izizipup/raw/main/izizipup > /usr/local/bin/izizipup
# less /usr/local/bin/izizipup
# chmod +x /usr/local/bin/izizipup
# izizipup
warning: using root account !
usage: izizipup [-p PORT] (-b BIND | -u URL) filename
izizipup: error: the following arguments are required: filename
```

> `/usr/local/bin` must be part of the `PATH` variable, which is usually the case.<br/>`curl -L` can be replaced by `wget -O-`.


### user side

```
$ pip3 install --user flask apscheduler
$ curl -L https://github.com/patatetom/izizipup/raw/main/izizipup > ~/.local/bin/izizipup
$ less ~/.local/bin/izizipup
$ chmod +x ~/.local/bin/izizipup
$ izizipup
usage: izizipup [-p PORT] (-b BIND | -u URL) filename
izizipup: error: the following arguments are required: filename
```

> `~/.local/bin` must be part of the `PATH` variable.


### virtual env

```
$ python3 -m venv izizipup
$ source izizipup/bin/activate
$ pip install flask apscheduler
$ curl -L https://github.com/patatetom/izizipup/raw/main/izizipup > izizipup/bin/izizipup
$ sed -i -e "1i#\!$( readlink -e izizipup/ )/bin/python" -e 1d izizipup/bin/izizipup
$ less izizipup/bin/izizipup
$ chmod +x izizipup/bin/izizipup
$ izizipup
usage: izizipup [-p PORT] (-b BIND | -u URL) filename
izizipup: error: the following arguments are required: filename
```

> `sed` replaces the system python interpreter by the one of the virtual environment.
