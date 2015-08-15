# $Id$
# Maintainer: Jubei-Mitsuyoshi <jubei.house.of.five.masters@gmail.com>
# Contributor: Link Dupont <link@subpop.net>
#
pkgname=dbus-core-lsd
pkgver=1.6.4
pkgrel=1
pkgdesc="Freedesktop.org message bus system"
url="http://www.freedesktop.org/Software/dbus"
arch=(i686 x86_64)
license=('GPL' 'custom')
depends=('expat' 'coreutils' 'filesystem' 'shadow' ) # shadow for install scriptlet FS#29341
provides=("dbus-core=${pkgver}")
conflicts=('dbus-core')
makedepends=('libx11')
options=(!libtool)
install=dbus.install
source=(http://dbus.freedesktop.org/releases/dbus/dbus-$pkgver.tar.gz{,.asc}
        dbus)
md5sums=('5ec43dc4554cba638917317b2b4f7640'
         '3d4482ee39b49da334441c76f83bf1cb'
         '81EB58BBE2B2212210E592CCD1EB5C3C')

build() {
  cd dbus-$pkgver
  ./configure --prefix=/usr \
      --sysconfdir=/etc \
	  --localstatedir=/var \
      --libexecdir=/usr/lib/dbus-1.0 \
	  --with-dbus-user=81 \
      --with-system-pid-file=/run/dbus/pid \
      --with-system-socket=/run/dbus/system_bus_socket \
      --with-console-auth-dir=/run/console/ \
      --enable-inotify \
	  --disable-dnotify \
      --disable-verbose-mode \
	  --disable-static \
      --disable-tests \
	  --disable-asserts \
	  --with-session-socket-dir=/tmp 
#      --with-systemdsystemunitdir=/usr/lib/systemd/system \
#      --enable-systemd
	  
  make
}

package(){
  cd dbus-$pkgver
  make DESTDIR="$pkgdir" install

  rm -f "$pkgdir/usr/bin/dbus-launch"
  rm -f "$pkgdir/usr/share/man/man1/dbus-launch.1"
  rm -rf "$pkgdir/var/run"

  install -m755 -d "$pkgdir/etc/rc.d"
  install -m755 ../dbus "$pkgdir/etc/rc.d/"

  #Fix configuration file
  sed -i -e 's|<user>81</user>|<user>dbus</user>|' "$pkgdir/etc/dbus-1/system.conf"

  install -dm755 "$pkgdir/usr/share/licenses/dbus-core"
  install -m644 COPYING "$pkgdir/usr/share/licenses/dbus-core/"
}
