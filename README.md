# CSAPP_Labs
Notes for Labs for Computer Systems: A Programmer's Perspective

# Content

- [Content](#Content)
- [Website](#Website)
- [Development Environment](#Development Environment)
- [Lab Progress](#Lab Progress)
- [Reference](#Reference)


# Website
[Lab Website](http://csapp.cs.cmu.edu/3e/labs.html)


# Development Environment

Linux : Using Docker to build a lightweight Linux environment

1. Download and install Docker
2. Build ubuntu image using Docker
```
docker pull ubuntu:18.04
```
3. mount local folder
```
docker container run -it -v /Users/USERNAME/DOCUMENTS/csapp:/
csapp --name=csapplab_env ubuntu:18.04 /bin/bash
```
4. run the container
```
docker exec -it csapplab_env /bin/bash
```
5. configure environment and install packages
```
apt-get update
```
```
apt-get install sudo
```
```
sudo apt-get install build-essential
```
```
sudo apt-get install gcc-multilib
```
```
sudo apt-get install gdb
```


# Lab Progress
- [x] [Data Lab](https://github.com/Lizhao-Liu/CSAPP_Labs/tree/main/Data%20Lab)
- [x] [Bomb Lab](https://github.com/Lizhao-Liu/CSAPP_Labs/tree/main/Bomb%20Lab)
- [ ] [Attack Lab]
- [ ] [Buffer Lab]
- [ ] [Architecture Lab]
- [ ] [Cache Lab]
- [ ] [Performance Lab]
- [ ] [Shell Lab]
- [ ] [Malloc Lab]
- [ ] [Proxy Lab]

# References
[厚度CSAPP](https://wdxtub.com/csapp/thick-csapp-lab-1/2016/04/16)
