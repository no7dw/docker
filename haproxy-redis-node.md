# docker haproxy-redis-node

### start redis master

    dengwei@dengweis-MacBook-Pro:~$ docker run -it --name redis-master redis /bin/bash
    root@8786448e82e0:/data# cat /etc/hosts
    127.0.0.1	localhost
    ::1	localhost ip6-localhost ip6-loopback
    fe00::0	ip6-localnet
    ff00::0	ip6-mcastprefix
    ff02::1	ip6-allnodes
    ff02::2	ip6-allrouters
    172.17.0.2	8786448e82e0
    root@8786448e82e0:/data# 

### start redis-slaver1

    dengwei@dengweis-MacBook-Pro:~$ docker run -it --name redis-slave1 --link redis-master:master redis  /bin/bash
    root@4a4cdc4f99a3:/data# cat /etc/hosts
    127.0.0.1	localhost
    ::1	localhost ip6-localhost ip6-loopback
    fe00::0	ip6-localnet
    ff00::0	ip6-mcastprefix
    ff02::1	ip6-allnodes
    ff02::2	ip6-allrouters
    172.17.0.2	master 8786448e82e0 redis-master
    172.17.0.3	4a4cdc4f99a3
    root@4a4cdc4f99a3:/data# 
    
    
    ### start redis-slaver2
    dengwei@dengweis-MacBook-Pro:~$ docker run -it --name redis-slave2 --link redis-master:master redis  /bin/bash
    root@37addc93221b:/data# which telnet
    root@37addc93221b:/data# cat /etc/hosts
    127.0.0.1	localhost
    ::1	localhost ip6-localhost ip6-loopback
    fe00::0	ip6-localnet
    ff00::0	ip6-mcastprefix
    ff02::1	ip6-allnodes
    ff02::2	ip6-allrouters
    172.17.0.2	master 8786448e82e0 redis-master
    172.17.0.4	37addc93221b
    root@37addc93221b:/data# 


### result

    dengwei@dengweis-MacBook-Pro:~$ docker ps -a
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
    37addc93221b        redis               "/entrypoint.sh /bin/"   6 minutes ago       Up 6 minutes        6379/tcp            redis-slave2
    4a4cdc4f99a3        redis               "/entrypoint.sh /bin/"   7 minutes ago       Up 7 minutes        6379/tcp            redis-slave1
    8786448e82e0        redis               "/entrypoint.sh /bin/"   8 minutes ago       Up 8 minutes        6379/tcp            redis-master
    d

### start nodejs APP1

    docker run  --name APP1 --link redis-master:db  -v ~/project/github/sails-demo/:/var/git/ -it nodejs010 /bin/bash
    root@dbfec0898db7:/# cd /var/git/
    root@dbfec0898db7:/var/git# node app.js

### start nodejs APP2

    dengwei@dengweis-MacBook-Pro:~/project/github$ docker run  --name APP2 --link redis-master:db  -v ~/project/github/sails-demo/:/var/git/ -it nodejs010 /bin/bash
    root@1b4e853ce636:/# cd /var/git/
    root@1b4e853ce636:/var/git# node app.js

### result 

    dengwei@dengweis-MacBook-Pro:~$ docker ps -a
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
    1b4e853ce636        nodejs010           "/bin/bash"              23 minutes ago      Up 23 minutes       1337/tcp                 APP2
    dbfec0898db7        nodejs010           "/bin/bash"              23 minutes ago      Up 23 minutes       1337/tcp                 APP1
    37addc93221b        redis               "/entrypoint.sh /bin/"   44 minutes ago      Up 44 minutes       6379/tcp                 redis-slave2
    4a4cdc4f99a3        redis               "/entrypoint.sh /bin/"   45 minutes ago      Up 45 minutes       6379/tcp                 redis-slave1
    8786448e82e0        redis               "/entrypoint.sh /bin/"   46 minutes ago      Up 45 minutes       6379/tcp                 redis-master


### start haproxy

    dengwei@dengweis-MacBook-Pro:~/project/github/no7dw.github.io$ docker run --name HAProxy --link APP1:APP1 --link APP2:APP2 -p 6301:6301 -v ~/project/haproxy/:/tmp -it  haproxy  /bin/bash
    root@ac518eb446ff:/# cp /tmp/haproxy.cfg /usr/local/sbin/
    root@ac518eb446ff:/# cd /usr/local/sbin/
    root@ac518eb446ff:/usr/local/sbin# ls
    haproxy  haproxy-systemd-wrapper  haproxy.cfg
    root@ac518eb446ff:/usr/local/sbin# haproxy -f haproxy.cfg 
    root@ac518eb446ff:/usr/local/sbin# ps -ef| grep haproxy
    root         8     1  0 15:52 ?        00:00:00 haproxy -f haproxy.cfg
    root        10     1  0 15:52 ?        00:00:00 grep haproxy
    root@ac518eb446ff:/usr/local/sbin# 

nodejs app index page
![visit app index page][1]
haproxy stats monitor page
![visit haproxy stats page][2]


  [1]: http://7xk67t.com1.z0.glb.clouddn.com/nodedocker.png
  [2]: http://7xk67t.com1.z0.glb.clouddn.com/haproxydocker.png