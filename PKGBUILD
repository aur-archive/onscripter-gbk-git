pkgname=onscripter-gbk-git
pkgver=8.951935b
pkgrel=1
pkgdesc="A game engine compatible to NScripter with GBK patch, which is used to create and perform visual novel games"
arch=('i686' 'x86_64')
provides=(onscripter-gbk)
conflicts=(onscripter-gbk)
url="https://github.com/natdon/ONScripter-CN"
license=('GPL')
depends=('bzip2' 'sdl_image' 'sdl_mixer' 'sdl_ttf' 'lua51')

_gitroot="git://github.com/natdon/ONScripter-CN.git"
_gitname="onscripter-gbk"
_gitbranch="951935b"

pkgver() {
  cd "$_gitname"
  echo $(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}

build() {
  msg "Connecting to GIT server..."
  if [[ -d $_gitname ]]; then
    (cd $_gitname && git pull )
  else
    git clone $_gitroot $_gitname
  fi
  cd "$_gitname"
  git checkout "$_gitbranch" .
  msg "GIT checkout done or server timeout"
  msg "Starting make..."
  cd jni
  # cd onscripter*
  cd app_onscripter-32bpp
  target=$(ls | grep '^onscripter-' | grep -v 'tar.gz' | sort | tail -n1)
  cd "$target"

  echo "LIBS = `sdl-config --libs` `smpeg-config --libs` -lSDL_ttf -lSDL_image -lSDL_mixer -lbz2 -ljpeg -lm -logg -lvorbis -lvorbisfile
LIBS += -llua5.1 -lfontconfig
CFLAGS += -c -DLINUX -DUSE_OGG_VORBIS -DUSE_LUA -DUSE_FONTCONFIG
CFLAGS += `sdl-config --cflags` `smpeg-config --cflags` -I/usr/include/lua5.1
OBJSUFFIX = .o
#EXT_OBJS = LUAHandler.o
CC = g++
LD = g++ -o
TARGET = onscripter
include Makefile.onscripter" > Makefile

  make
  mv onscripter "$srcdir/$_gitname/"
}

package() {
  cd "$_gitname"
  install -Dm755 onscripter $pkgdir/usr/bin/onscripter-gbk
}
