license=('BSD')
arch=('x86_64')
pkgname=mosquitto
pkgver=2.0.11
pkgrel=1
pkgdesc="An Open Source MQTT Broker"
depends=('openssl' 'c-ares' 'util-linux' 'cjson')
makedepends=('docbook-xsl' 'c-ares' 'libwebsockets')
url="https://mosquitto.org"
source=(https://mosquitto.org/files/source/mosquitto-$pkgver.tar.gz
        "$pkgname.service"
        "sysusers_mosquitto.conf")
backup=("etc/$pkgname/$pkgname.conf")
sha512sums=('d0c7c52cb76c4711e54f841217529326d682c4decfc7a1bc96d872904e68df444ca3918fab7ba041b62f7b5420c89c631227b69a8eec51fd2e2dd480d8244710'
            '7dd86bb454e6df45e609fc3cb53d3cae8cc1c36d459b1e23be9ab10c9770c7a406fbd34e47703b0db3056f4bc8550994666b8a398d4506f786bf274e4618b7e9'
            '21848b890c2db258138795ec21a009e022b6a8369217eb31939f976ad434229dd9f61d33e8109ade7bc001e8668e9d42b59c1ab079753860417961e102356f0e')

prepare() {
  cd "$pkgname-$pkgver"
  # FIX upstream by making SBINDIR=foo or use CMAKE
  sed -i 's|/sbin|/bin|g' src/Makefile
}

build() {
  cd "$pkgname-$pkgver"
  make WITH_WEBSOCKETS=yes
}

package() {
  depends+=('libwebsockets')
  cd "$pkgname-$pkgver"

  make prefix=/usr DESTDIR="$pkgdir/" install

  # Shipped in git.
  install -Dm644 "$srcdir/$pkgname.service" "$pkgdir/usr/lib/systemd/system/$pkgname.service"
  install -Dm644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  mv "$pkgdir/etc/$pkgname/$pkgname.conf.example" "$pkgdir/etc/$pkgname/$pkgname.conf"

  install -D -m644 "${srcdir}"/sysusers_mosquitto.conf "${pkgdir}"/usr/lib/sysusers.d/mosquitto.conf
}
