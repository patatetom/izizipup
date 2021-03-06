# izizipup

oneshot easy zip uploader

_transferring a file can quickly become a puzzle... **izizipup** tends to solve this difficulty in a simple way._
_of course, the sender must be able to contact **izizipup** (firewall et cetera...)._



## installation

**izizipup** is a little [Python](https://www.python.org/) script based on the [Flask](https://palletsprojects.com/p/flask/) and [Apscheduler](https://apscheduler.readthedocs.io/) modules.


### system side

```
# pip3 install flask apscheduler
# curl -L https://github.com/patatetom/izizipup/raw/main/izizipup > /usr/local/bin/izizipup
# less /usr/local/bin/izizipup
# chmod +x /usr/local/bin/izizipup
# izizipup
warning: using root account !
usage: izizipup [-p PORT] (-b IPv4 | -u URL) filename
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
usage: izizipup [-p PORT] (-b IPv4 | -u URL) filename
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
usage: izizipup [-p PORT] (-b IPv4 | -u URL) filename
izizipup: error: the following arguments are required: filename
```

> `sed` replaces the system python interpreter by the one of the virtual environment.



## usage

```
$ izizipup --help
usage: izizipup [-h] [-p PORT] (-b IPv4 | -u URL) filename

Start izizipup : oneshot easy zip uploader

positional arguments:
  filename              where to store uploaded file

optional arguments:
  -h, --help            show this help message and exit
  -p PORT, --port PORT  PORT to listen (default 5000)
  -b IPv4, --bind IPv4  IPv4 address to bind (default to 127.0.0.1 with URL)
  -u URL, --url URL     URL if proxied
```

**izizipup** expects at least two parameters:
- the name of the file in which the transfer will be saved (eg. `filename`)
  - either the IPv4 address on which to listen when it is used in frontend (eg. `-b IPv4` or `--bind IPv4`)
  - either the URL which must be used when it is used in backend (eg. `-u URL` or `--url URL`).

> `IPv4` address and `URL` are mutually exclusive.


### timeout

**izizipup** starts two time counters that will interrupt the service if the expected request has not occurred within the allotted time.

the first time counter waits **3 minutes** for an HTML GET request.

```
…
waiting 3min for GET...
…
```

the second time counter, triggered at the arrival of the expected HTML GET request, waits **15 minutes** for an HTML POST request.

```
…
waiting 3min for GET...
…
127.0.0.1 - - [17/Dec/2020 15:44:13] "GET /0e022145-5dcc-4008-94d4-4210e031121f HTTP/1.1" 200 -
GET ok, waiting now 15min for POST...
```


### upload form

when the expected HTML GET request occurs within the allotted time, a small and basic upload form is sent back to the sender (eg. the client).

[![Select](https://img.shields.io/badge/Select%20a%20file-No%20file%20selected-gray?style=social)](README.md#upload-form)
[![Send](https://img.shields.io/badge/Send-%20-gray)](README.md#upload-form)


### izizipup @ frontend

when **izizipup** works this way (eg. `IPv4` address), it is directly exposed.

```
$ ip addr show dev wlan0
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 93:55:8D:56:95:91 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.1/24 brd 192.168.1.255 scope global dynamic noprefixroute wlan0
       valid_lft 61115sec preferred_lft 50315sec

$ izizipup -b 192.168.1.1 /tmp/long-awaited-file
http://192.168.1.1:5000/0206ada9-8313-499b-8025-ada3fb4c85f2
waiting 3min for GET...
 * Running on http://192.168.1.1:5000/ (Press CTRL+C to quit)
```

the highlighted URL `http://192.168.1.1:5000/0206ada9-8313-499b-8025-ada3fb4c85f2` can be shared with the sender who will have 3 minutes to retrieve the upload form and then 15 minutes to upload the file to be transferred.

the port can of course be a privileged port.

```
$ sudo izizipup -b 192.168.1.1 -p 80 /tmp/long-awaited-file
[sudo] password for user: 
warning: using root account !
http://192.168.1.1/5f305b56-af0f-4835-ab5e-98de60e6d5d4
waiting 3min for GET...
 * Running on http://192.168.1.1:5000/ (Press CTRL+C to quit)
```

> `warning: using root account !` is issued in this case.


### izizipup @ backend

when **izizipup** works this way (eg. `URL`), it is usually proxied (behind Apache or Nginx for example).

```
$ izizipup -u http://www.domain.tld/location/ /tmp/long-awaited-file
http://www.domain.tld/location/d761df3e-1193-44e4-b9cb-3997d4e17d72
waiting 3min for GET...
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

like in frontend, the highlighted URL `http://www.domain.tld/location/d761df3e-1193-44e4-b9cb-3997d4e17d72` can be shared with the sender who will have 3 minutes to retrieve the upload form and then 15 minutes to upload the file to be transferred.

https can be used here if the frontend server supports it.

```
izizipup -u https://www.domain.tld/izizipup/ /tmp/long-awaited-file
https://www.domain.tld/izizipup/6a0d2a4c-d036-454d-acee-beabd3bf92bd
waiting 3min for GET...
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

#### Apache

adapt and add this directive to your Apache configuration:

```
ProxyPass "/izizipup/" "http://127.0.0.1:5000/"
```

#### Nginx

adapt and add the following location block to your Nginx configuration:

```
location /izizipup/ {
 client_max_body_size 256M;
 proxy_pass http://127.0.0.1:5000/;
}
```
