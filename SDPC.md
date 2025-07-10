#

## SDPC support

```sh
docker run -ti --rm -v ${PWD}:/work -w /work ghcr.io/openslide/winbuild-builder

# in container
mkdir override && cd override
git clone https://github.com/Harold2017/openslide_more_vendors openslide

# default thread model is `win32`
# need `pthread` to build `avcodec` and `avutil`
# according to https://wiki.gentoo.org/wiki/Mingw#POSIX_threads_for_Windows
crossdev --target cross-x86_64-w64-mingw32
crossdev --genv 'EXTRA_ECONF="--enable-threads=posix"' --init-target --target cross-x86_64-w64-mingw32
echo "cross-x86_64-w64-mingw32/mingw64-runtime libraries idl tools" > /etc/portage/package.use/cross-x86_64-w64-mingw32
emerge --oneshot cross-x86_64-w64-mingw32/mingw64-runtime
emerge --oneshot cross-x86_64-w64-mingw32/gcc

# build
./bintool bdist
```

please note the built binary depends on `libwinpthread-1.dll`, you need to copy it to the same directory as the binary.
