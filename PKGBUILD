pkgname=zfs-esp-sync
pkgver=0.3.1
pkgrel=1
arch=('any')
install=${pkgname}.install

package() {

  install -D -m 644 -t \
      "${pkgdir}/usr/lib/systemd/system" \
      "${srcdir}/zfs-esp-sync@.path" \
      "${srcdir}/zfs-esp-sync@.service"

  install -D -m 755 -t \
      "${pkgdir}/usr/bin" \
      "${srcdir}/zfs-esp-sync"
}
