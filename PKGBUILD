# $Id: PKGBUILD 83885 2010-06-23 16:02:18Z andrea $
# Maintainer:
# Contributor: Eric Johnson <eric@coding-zone.com>

pkgname=words
pkgver=2.0
pkgrel=2
pkgdesc="A collection of International 'words' files for /usr/share/dict"
arch=('any')
url="http://www.archlinux.org"
license=('GPL' 'GPL2' 'custom')
depends=('util-linux-ng')
install=${pkgname}.install
source=(ftp://ftp.archlinux.org/other/words/$pkgname-$pkgver.tar.bz2)
md5sums=('cc73f9dd1fb5fb358badcfc23f7963bf')

package() {
  cd $srcdir/$pkgname-$pkgver
  install -d $pkgdir/usr/share/dict
  install -m644 dict/* $pkgdir/usr/share/dict

  ln -s catala $pkgdir/usr/share/dict/catalan
  ln -s british-english $pkgdir/usr/share/dict/british
  ln -s american-english $pkgdir/usr/share/dict/usa
  ln -s ogerman $pkgdir/usr/share/dict/german

  # create empty place holder - this will be replaced with a
  # symlink pointing to the locale's lang during install
  #
  touch $pkgdir/usr/share/dict/words

 # Licenses:
  cd doc
  for i in * ; do
    install -D -m644 $i/copyright ${pkgdir}/usr/share/licenses/${pkgname}/$i.copyright
  done
}
