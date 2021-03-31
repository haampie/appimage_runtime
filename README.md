# AppImage runtime without all the appfolder business

Building the runtime (shared libfuse)

```
$ spack external find --not-buildable libfuse pkg-config
$ spack -e . install -v
$ export C_INCLUDE_PATH=.spack-env/view/include
$ export LIBRARY_PATH=.spack-env/view/lib
$ make
```

Creating a dir + executable + squashfs

```
$ mkdir -p example
$ echo $'#!/usr/bin/bash'$'\n'$'echo "hello world"' > example/AppRun
$ chmod +x example/AppRun
$ mksquashfs example example.squashfs -comp zstd -quiet
```

Creating an executable


```
$ cat runtime example.squashfs > app
$ chmod +x app
$ ./app
hello world
```
