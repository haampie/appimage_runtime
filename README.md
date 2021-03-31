# AppImage runtime without all the appfolder business

Building the runtime (shared libfuse)

```
$ spack external find --not-buildable libfuse pkg-config
$ spack -e . install -v
$ export C_INCLUDE_PATH=.spack-env/view/include
$ export LIBRARY_PATH=.spack-env/view/lib
$ make
```

Size overhead from the runtime is small:

```
$ libtree runtime
runtime
└── libfuse3.so.3 [ld.so.conf]

$ du -sh runtime
128K	runtime
```

Now create an AppRun executable in a folder and squashfs it:

```
$ mkdir -p example
$ echo $'#!/usr/bin/bash'$'\n'$'echo "hello world"' > example/AppRun
$ chmod +x example/AppRun
$ mksquashfs example example.squashfs -comp zstd -quiet
```

And merge runtime and the squashfs file into an executable:


```
$ cat runtime example.squashfs > app
$ chmod +x app
$ ./app
hello world
```
