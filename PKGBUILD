# Maintainer: Michele Beccalossi <beccalossi.michele@gmail.com>
# Contributor: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: josephgbr <rafael.f.f1@gmail.com>
# Contributor: Themaister <maister@archlinux.us>
# Contributor: vEX <vex@niechift.com>

pkgname=pcsx2-git
pkgver=1.5.0+3314+gdbfbc333f
pkgrel=1
pkgdesc="A Sony PlayStation 2 emulator"
arch=('x86_64')
url="https://pcsx2.net/"
license=('GPL2' 'GPL3' 'LGPL2.1' 'LGPL3')
depends=('lib32-libaio' 'lib32-libpcap' 'lib32-libxml2' 'lib32-portaudio'
         'lib32-sdl2' 'lib32-soundtouch' 'lib32-wxgtk2')
makedepends=('cmake' 'git' 'clang')
provides=('pcsx2')
conflicts=('pcsx2')
source=("git+https://github.com/PCSX2/pcsx2.git"
        "git+https://github.com/PCSX2/xz.git")
sha256sums=('SKIP' 'SKIP')

pkgver() {
  cd pcsx2

  git describe --long --tags | sed 's/^v//; s/-dev//; s/-/+/g'
}

prepare() {
  cd pcsx2

  git submodule init
  git config submodule.3rdparty/xz/xz.url ../xz
  git submodule update 3rdparty/xz/xz

  if [ -d build ]; then
    rm -rf build
  fi
  mkdir build
}

build() {
  cd pcsx2/build

  export CC=clang
  export CXX=clang++

  cmake .. \
    -DCMAKE_BUILD_TYPE='Release' \
    -DCMAKE_BUILD_PO='FALSE' \
    -DCMAKE_TOOLCHAIN_FILE='cmake/linux-compiler-i386-multilib.cmake' \
    -DCMAKE_INSTALL_PREFIX='/usr' \
    -DCMAKE_LIBRARY_PATH='/usr/lib32' \
    -DPLUGIN_DIR='/usr/lib32/pcsx2' \
    -DGAMEINDEX_DIR='/usr/share/pcsx2' \
    -DPACKAGE_MODE='TRUE' \
    -DXDG_STD='TRUE'

  make -j$(nproc)
}

package() {
  cd pcsx2/build

  make DESTDIR="${pkgdir}" install
}

# vim: ts=2 sw=2 et:
