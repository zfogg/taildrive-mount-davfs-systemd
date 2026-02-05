# Maintainer: zfogg <zfogg@gmail.com>
pkgname=taildrive-mount-davfs-systemd
pkgver=1.0.0
pkgrel=1
pkgdesc="Automatically mount WebDAV via davfs2 when Tailscale is running"
arch=('any')
url="https://github.com/zfogg/taildrive-mount-davfs-systemd"
license=('MIT')
depends=('tailscale' 'davfs2' 'systemd')
source=("${pkgname}::git+file:///home/zfogg/src/github.com/zfogg/${pkgname}")
sha256sums=('SKIP')

package() {
    cd "${srcdir}/${pkgname}"

    # Install scripts
    install -Dm755 src/do-mount-webdav "${pkgdir}/usr/bin/do-mount-webdav"
    install -Dm755 src/ensure-webdav-mounted "${pkgdir}/usr/bin/ensure-webdav-mounted"

    # Install systemd units
    install -Dm644 systemd/ensure-webdav-mounted.service "${pkgdir}/usr/lib/systemd/system/ensure-webdav-mounted.service"
    install -Dm644 systemd/ensure-webdav-mounted.timer "${pkgdir}/usr/lib/systemd/system/ensure-webdav-mounted.timer"
    install -Dm644 systemd/tailscaled-unmount-webdav.conf "${pkgdir}/etc/systemd/system/tailscaled.service.d/unmount-webdav.conf"

    # Install documentation
    install -Dm644 README.md "${pkgdir}/usr/share/doc/${pkgname}/README.md"
}

post_install() {
    echo "==> To enable automatic WebDAV mounting:"
    echo "    sudo systemctl enable ensure-webdav-mounted.timer"
    echo "    sudo systemctl start ensure-webdav-mounted.timer"
    echo ""
    echo "==> Edit the following files to customize:"
    echo "    /usr/local/bin/do-mount-webdav - WebDAV URL and mount point"
    echo "    /etc/systemd/system/ensure-webdav-mounted.timer - Check interval"
}

post_upgrade() {
    echo "==> Restart the timer to apply updates:"
    echo "    sudo systemctl restart ensure-webdav-mounted.timer"
}
