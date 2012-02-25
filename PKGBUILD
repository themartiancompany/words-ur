# Maintainer: Lukas Fleischer <archlinux at cryptocrack dot de>
# Contributor: Eric Johnson <eric@coding-zone.com>

pkgname=words
pkgver=2.0
pkgrel=4
pkgdesc="A collection of International 'words' files for /usr/share/dict."
arch=('any')
url='http://www.archlinux.org'
license=('GPL' 'GPL2' 'custom')
depends=('util-linux-ng')
install=${pkgname}.install
source=("ftp://ftp.archlinux.org/other/community/${pkgname}/${pkgname}-${pkgver}.tar.bz2")
md5sums=('cc73f9dd1fb5fb358badcfc23f7963bf')

package() {
  cd "$srcdir/$pkgname-$pkgver"

  install -d "${pkgdir}/usr/share/dict"
  install -m0644 dict/* "${pkgdir}/usr/share/dict"

  ln -s catala "${pkgdir}/usr/share/dict/catalan"
  ln -s british-english "${pkgdir}/usr/share/dict/british"
  ln -s american-english "${pkgdir}/usr/share/dict/usa"
  ln -s ogerman "${pkgdir}/usr/share/dict/german"

  # Licenses:
  cd doc/
  for i in *; do
    install -Dm0644 "$i/copyright" "${pkgdir}/usr/share/licenses/${pkgname}/$i.copyright"
  done
}
