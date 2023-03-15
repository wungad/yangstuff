# libyang

Statically-linked [yanglint](https://github.com/CESNET/libyang) binary.

### Build

The following will build and copy the resulting binary to `$HOME/bin` directory (created with mode=0700 if missing):

```
$
$ DOCKER_BUILDKIT=1 docker build -o $HOME/bin https://raw.githubusercontent.com/wungad/yangstuff/main/docker/yanglint/Dockerfile
[+] Building 0.1s (8/8) FINISHED
 => CACHED [internal] load remote build context                                                                                   0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                  0.0s
 => [stage-0 1/4] FROM docker.io/library/alpine                                                                                   0.0s
 => CACHED [stage-0 2/4] RUN     apk -U add alpine-sdk cmake pcre2-dev                                                            0.0s
 => CACHED [stage-0 3/4] RUN     git clone --depth=1 --branch=master https://github.com/CESNET/libyang.git                        0.0s
 => CACHED [stage-0 4/4] RUN     cmake -S libyang -B build         -DCMAKE_BUILD_TYPE=MinSizeRel         -DENABLE_STATIC=ON &&    0.0s
 => CACHED [stage-1 1/1] COPY --from=0 /build/yanglint yanglint                                                                   0.0s
 => exporting to client                                                                                                           0.0s
 => => copying files 1.57MB                                                                                                       0.0s
$
```

### Run

Verify and run

```
$
$ file -b $HOME/bin/yanglint
ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, stripped
$ ldd $HOME/bin/yanglint
        not a dynamic executable
$ $HOME/bin/yanglint -v
yanglint 2.1.30
$ echo 'module abc { namespace abc:example; prefix abc; leaf chars { type string; }}' > abc.yang && $HOME/bin/yanglint -ftree abc.yang
module: abc
  +--rw chars?   string
$
```
