pkgname=zfs-esp-sync
pkgver=0.4.6
pkgrel=1
arch=('any')
depends=(inotify-tools)
install=${pkgname}.install

package() {

  install -D -m 644 -t \
      "${pkgdir}/usr/lib/systemd/system" \
      "${srcdir}/zfs-esp-sync@.service"

  install -D -m 755 -t \
      "${pkgdir}/usr/bin" \
      "${srcdir}/zfs-esp-sync" \
      "${srcdir}/zfs-esp-sync-watch"
}
