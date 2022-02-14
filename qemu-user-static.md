# qemu-user-static

## Instruction

See http://logan.tw/posts/2018/02/18/build-qemu-user-static-from-source-code/
and https://github.com/multiarch/qemu-user-static for more details.

### 1) Download Source

See https://www.qemu.org/download/

### 2) Download Dependencies

```shell
sudo apt-get apt-get build-dep qemu
```

### 3) Build

```shell
./configure \
  --prefix=/usr/local \
  --static \
  --disable-system \
  --enable-linux-user

make
sudo make install

(cd /usr/local/bin; \
for bin in qemu-*; do \
  sudo mv $bin ${bin}-static; \
done)
```

### 4) Install binfmts

To use with docker, ```--persistent yes``` is required.

```shell
sudo scripts/qemu-binfmt-conf.sh \
  --qemu-path /usr/local/bin \
  --qemu-suffix "-static" \
  --persistent yes \
  --debian

for q in /usr/share/binfmts/qemu-*; do \
  sudo update-binfmts \
    --importdir /usr/share/binfmts \
    --import `basename $q`; \
done
```

### 5) Test

```shell
docker run --rm \
  --platform linux/arm64/v8 \
  arm64v8/alpine:latest \
  uname -m
```
