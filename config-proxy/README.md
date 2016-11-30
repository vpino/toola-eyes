# GEEKOS - HISTORY 

## DAY :: 28 - 11 - 2016

### CREATE USER IN WORKED

### CONFIG PROXY IN DESKTOP


#### Files to modify:


* /etc/environment: File contain the vars that specific the enviroment basic to process of system.

* /etc/apt/apt.conf: File to config the manager of packages APT.

* /etc/bash.bashrc: File of configuration that execute bash. Use to include vars environment etc.

### /etc/environment


Added to file the rules of proxy:

```bash
nano /etc/environment
```

Copy your rules

```
http_proxy=192.168.x.x:8080
 
https_proxy=192.168.x.x.:8080
 
ftp_proxy=192.168.x.x:8080
```

### Now/etc/apt/apt.conf


```bash
nano /etc/apt/apt.conf
```

And copy of next:

```
Acquire::http::Proxy "http://192.168.x.x:8080/";
 
Acquire::https::Proxy "http://192.168.x.x:8080/";
 
Acquire::ftp::Proxy "http://192.168.x.x:8080/";
``` 

### And finnish /etc/bash.bashrc

Execute:

```
nano /etc/bash.bashrc
```

And the finnish of the file add the next lines:

```
#Rules of proxy

export http_proxy=http://192.168.x.x:8080/
 
export https_proxy=http://192.168.x.x:8080/
 
export ftp_proxy=http://192.168.x.x:8080/
```


### Config your Browser 

In Firefox:


* 1 - Click in ```settings/avanzado/red``` and click in configurations.

* 2 - Check ```configuration manual of proxy```.

* 3 - In the fiel ```PROXY HTTP``` copy your ip proxy and ```PORT``` the port.

* 4 - Check ```Use this server proxy to all protocols```.

### Sources

[https://www.ochobitshacenunbyte.com/2013/02/07/configurar-sistemas-basados-en-debian-para-salir-por-proxy/]