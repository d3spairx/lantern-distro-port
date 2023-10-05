# Maintainer: Lantern Team <team@getlantern.org>
pkgname=lantern
pkgver=7.4.0
pkgrel=1
# TODO Description - recommended to be 80 characters or less
pkgdesc="VPN client app for desktop"
arch=('x86_64' 'i686')
url="https://lantern.io"
license=('Apache-2.0')
depends=('libpcap' 'libappindicator-gtk3')
conflicts=("${pkgname%-*}")
source_x86_64=("${pkgname}-${pkgver}-x86_64.deb::https://s3.amazonaws.com/lantern/lantern-installer-${pkgver}-64-bit.deb")
source_i686=("${pkgname}-${pkgver}-i686.deb::https://s3.amazonaws.com/lantern/lantern-installer-${pkgver}-32-bit.deb")
b2sums_x86_64=('791408082452d5663aca5399995e7ceab35494624aaa8537d54f0652d79752e4aa5119c82fa57780b78b596466294432dbd12aa3c7620e4da4c2efef14f299b5')
b2sums_i686=('2ea5b717cce0ec9474be81148578513f4917ae2ee1c0437849cb4b84e820a5533b2366934d3b5e4df5957da1f5ae90e49a69a42f14111904ef907466bdd0a453')
sha256sums_x86_64=('7996c1707b9b8550203fb1037960312fbab266b6e439684f3f1d6aaee27e5e4d')
sha256sums_i686=('0488927484396a7912a4b0d64b8209f3e01b87e47c3665a8c55508014650e4b8')


package() {
	# Extracting the data.tar.xz
    bsdtar -xzf data.tar.gz -C "${pkgdir}/"

	# Editing lantern.desktop
	sed -i 's/Name=lantern/Name=Lantern/g' "${pkgdir}/usr/share/applications/lantern.desktop"

	# Symlinking libpcap.so.0.8 to libpcap.so
	ln -s /usr/lib/libpcap.so "${pkgdir}/usr/lib/libpcap.so.0.8"
}