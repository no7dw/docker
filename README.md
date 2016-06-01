# docker

# manage data in container
edit in 

 - Dockerfile: ADD srcFoler destFolder
 - edit in Dockerfile COPY srcFoler destFolder
 - use -v  when run 

example command:

    docker run -v /Users/dengwei/project/srcFolder:/var/git/ -it nodejs010 /bin/bash 

**Attention**:
    If you are using Docker Machine on Mac or Windows, your Engine daemon has only limited access to your OS X .
So, you can mount files or directories on OS X using.

    docker run -v /Users/<path>:/<container path> ...

    so as windows, On Windows, mount directories using:

    docker run -v /c/Users/<path>:/<container path> ...

