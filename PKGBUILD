# Maintainer: Tobias T. <OldShatterhand at gmx-topmail dot de>

pkgname=sword-old
pkgver=1.6.2
pkgrel=2
pkgdesc="Libraries for Bible programs"
arch=('i686' 'x86_64')
url="http://www.crosswire.org/sword/index.jsp"
license=('GPL')
depends=('curl' 'clucene-old' 'swig')
makedepends=('cmake')
conflicts=('sword')
provides=('sword=1.6.2')
backup=('etc/sword.conf')
source=("http://www.crosswire.org/ftpmirror/pub/sword/source/v1.6/sword-${pkgver}.tar.gz" curl.patch)
md5sums=('a7dc4456e20e915fec46d774b690e305'
         'e84a226ce3697af33b9fdd9a22884a2a')

build() {
  cd "${srcdir}"/sword-$pkgver
  patch -p1 < $srcdir/curl.patch
  cd $srcdir
  [[ -d build ]] || mkdir build
  cd build
  cmake ../sword-${pkgver} \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=Release
  make 
}

package() {
  cd "${srcdir}"/build
  make DESTDIR="${pkgdir}" install

  # Ugly workarounds to fix a weird cmakelists.txt
  install -d "${pkgdir}"/usr/lib/sword
  #mv "${pkgdir}"/usr/lib/1.6.2_icu_4.8 "${pkgdir}"/usr/lib/sword/

  cd "${srcdir}"/sword-${pkgver}/locales.d/
  for file in *.conf; do
    install -Dm644 $file "${pkgdir}"/usr/share/sword/locales.d/$file
  done

  cd ../include
  install -d "${pkgdir}"/usr/include/sword
  install -Dm644 canon_{catholic{,2},synodalp}.h "${pkgdir}"/usr/include/sword

  cd ../samples
  install -Dm644 mods.d/globals.conf "${pkgdir}"/usr/share/sword/mods.d/globals.conf
  install -Dm644 recommended/sword.conf "${pkgdir}"/etc/sword.conf
}
