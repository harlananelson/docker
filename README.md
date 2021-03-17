# Use of Docker

Purpose
: Explain the use of Docker

Reference
: [rootless docker](https://docs.docker.com/engine/security/rootless/)


## Required Installation

### File Permissions

When running `docker`, it needs the ability to run code and access files
your userid or groupid have access to even though `docker` is an isolated 
container.  This access is granted using `newuidmap` and `newgidmap`. 
This accomplishes something similar to `SETUID` and `SETGID`.


The package `uidmap` should be installed and provide the commands
`newuidmap` and `newgidmap`

Install the package `uidmap`, which should provide `newuidmap` and `newgidmap`.

```
apt-get install uidmap
```

The following command checks the number of subordiinate UID/GIDs 
available for the user:  needs to be at least 65,536

```
id -u
```

```
whoami
```

```
grep ^$(whoami) /etc/subuid
```

```
grep ^$(whoami) /etc/subgid
```

Check to see of dockerd is alread installed.

```
ls /usr/bin/dockerd-rootless-setuptool.sh
```

If it is not found, you can install

```
 sudo apt-get install -y docker-ce-rootless-extras
```


This is a use case of starting a rootless rstudio docker using 
Ubuntu 20.11.

Start Docker

```
systemctl --user start docker
```



You can also enable docker

```
systemctl --user enable docker
```

Specify the socket

```
export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock
```

Start RStudio

The code as given by [rocker-project](https://www.rocker-project.org/)
```
docker run -e PASSWORD=yourpassword --rm -p 8787:8787 rocker/rstudio
```

The code I usually run.

```
docker run -v "$(pwd)":healtheintents:rocker/rstudio -e PASSWORD=aPogee --rm -p 8787:8787 rocker/rstudio
```
```
docker run -v "$(pwd)":/home/rstudio/healtheintent -e PASSWORD=aPogee --rm -p 8787:8787 rocker/rstudio
```
Point your browser to [rstudio instance](localhost:8787)




