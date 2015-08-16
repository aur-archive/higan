# Maintainer: Abelardo Ricart

pkgname=higan
pkgver=092
pkgrel=6
pkgdesc="A Nintendo multi-system emulator formerly known as bsnes"
arch=('i686' 'x86_64')
url=http://byuu.org/higan/
license=(GPL)
replaces=('bsnes')
conflicts=('higan-git')
depends=('openal' 'libgl')
optdepends=('alsa' 'pulseaudio' 'sdl')
source=("http://higan.googlecode.com/files/${pkgname}_v${pkgver}-source.tar.xz" 'higan_gcc.patch' 'ananke_gcc.patch')
install=higan.install
md5sums=('ef6a6a9bc6861d26da01c2e03e6dfe9e' '049b514c1c3ca847b7efae50dcd0e3b6' '8431814f9ff3d8e5b2f438816efa1ef5')

build() {
   cd "${srcdir}/${pkgname}_v${pkgver}-source/${pkgname}/nall"
   patch -p0 -i ${srcdir}/higan_gcc.patch
   cd "${srcdir}/${pkgname}_v${pkgver}-source/${pkgname}"
   make clean && make profile=performance
   cd "${srcdir}/${pkgname}_v${pkgver}-source/ananke/nall"
   patch -p0 -i ${srcdir}/ananke_gcc.patch
   cd "${srcdir}/${pkgname}_v${pkgver}-source/ananke"
   make clean && make
}

package()
{
  # Install higan
  cd "${srcdir}/${pkgname}_v${pkgver}-source/${pkgname}"
  install -D -m 755 out/higan ${pkgdir}/usr/bin/higan
  install -D -m 644 data/higan.png ${pkgdir}/usr/share/pixmaps/higan.png
  install -D -m 644 data/higan.desktop ${pkgdir}/usr/share/applications/higan.desktop
  
  # Install ananke support library
  cd "${srcdir}/${pkgname}_v${pkgver}-source/ananke"
  install -D -m 755 libananke.so ${pkgdir}/usr/lib/libananke.so.1
  ln -s /usr/lib/libananke.so.1 ${pkgdir}/usr/lib/libananke.so
  
  # Create config folders in user's $HOME directory
  if [ ! -d ~/.config/ananke ]; then mkdir ~/.config/ananke; fi
  if [ ! -d ~/.config/higan ]; then mkdir ~/.config/higan; fi
  
  # Copy over config files.
  cd "${srcdir}/${pkgname}_v${pkgver}-source/${pkgname}"
  mkdir -p ${pkgdir}/usr/share/higan
  cp -R ../shaders ${pkgdir}/usr/share/higan/Video\ Shaders
  cp -R profile/* ${pkgdir}/usr/share/higan
  cp data/cheats.bml ${pkgdir}/usr/share/higan
}

