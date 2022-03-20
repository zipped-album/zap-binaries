# Pre-compiled binaries for Zipped Album Player (ZAP)

## Build instructions

### FFmpeg 4

#### Debian
```
sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libtool \
  pkg-config \
  texinfo \
  wget \
  zlib1g-dev \
  yasm \
  nasm \
  patchelf

mkdir -p ~/ffmpeg_sources ~/bin

cd ~/ffmpeg_sources && \
wget -O ffmpeg-4.4.1.tar.bz2 http://ffmpeg.org/releases/ffmpeg-4.4.1.tar.bz2 && \
tar xjvf ffmpeg-4.4.1.tar.bz2 && \
cd ffmpeg-4.4.1 && \
PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --extra-cflags="-I$HOME/ffmpeg_build/include" \
  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$HOME/bin" \
  --disable-programs \
  --disable-doc \
  --disable-static \
  --enable-shared \
  --disable-avdevice \
  --disable-postproc && \
PATH="$HOME/bin:$PATH" make && \
make install && \
hash -r

cd ~/ffmpeg_build/lib

for file in *.so.*.*.*;
    do patchelf --set-rpath \$ORIGIN "$file";
done;

mkdir archive
cp *.so.*.*.* archive
cp ~/ffmpeg_sources/ffmpeg-4.4.1/LICENSE.md archive
cp ~/ffmpeg_sources/ffmpeg-4.4.1/COPYING* archive
cd archive
platform=$(uname -m)
zip ffmpeg4-linux_$platform.zip *
```
