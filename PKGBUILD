pkgname=zfs-esp-sync
pkgver=0.1
pkgrel=1
arch=('any')

package() {

  install -D -m 644 -t \
      "${pkgdir}/usr/share/libalpm/hooks" \
      "${srcdir}/95-zfs-esp-sync.hook"

  install -D -m 755 -t \
      "${pkgdir}/usr/share/libalpm/scripts" \
      "${srcdir}/zfs-esp-sync"
}
