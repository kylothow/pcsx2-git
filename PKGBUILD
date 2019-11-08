# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: josephgbr <rafael.f.f1@gmail.com>
# Contributor: vEX <vex@niechift.com>

pkgbase=pcsx2-git
pkgname=pcsx2-git
pkgver=10821.e2d899231
pkgrel=1
pkgdesc='A Sony PlayStation 2 emulator'
arch=(x86_64)
url=https://www.pcsx2.net
license=('GPL2' 'GPL3' 'LGPL2.1' 'LGPL3')
depends=('lib32-libaio'
         'lib32-portaudio'
         'lib32-sdl2' 'lib32-soundtouch' 'lib32-wxgtk2'
         'lib32-libpcap' 'lib32-libxml2')
makedepends=(
  cmake
  git
  clang
)
source=(
  git+https://github.com/PCSX2/pcsx2.git
  git+https://github.com/PCSX2/xz.git
)
sha256sums=('SKIP' 'SKIP')

pkgver() {
  cd pcsx2

  echo $(git rev-list --count master).$(git rev-parse --short master)
}

prepare() {
  cd pcsx2
  git submodule init
  git config submodule.3rdparty/xz/xz.url ../xz
  git submodule update 3rdparty/xz/xz

  if [[ -d build ]]; then
    rm -rf build
  fi
  mkdir build
}

build() {
  cd pcsx2/build
  cmake .. \
    -DCMAKE_BUILD_TYPE='Release' \
    -DCMAKE_TOOLCHAIN_FILE='cmake/linux-compiler-i386-multilib.cmake' \
    -DCMAKE_INSTALL_PREFIX='/usr' \
    -DCMAKE_LIBRARY_PATH='/usr/lib32' \
    -DPLUGIN_DIR='/usr/lib32/pcsx2' \
    -DGAMEINDEX_DIR='/usr/share/pcsx2' \
    -DPACKAGE_MODE='TRUE' \
    -DXDG_STD='TRUE'

  make
}

package() {
  cd pcsx2/build
  make DESTDIR="${pkgdir}" install
}

# vim: ts=2 sw=2 et:
