# Maintainer: Grey Christoforo <grey@christoforo.net>

_number_of_bits=8
pkgname=microchip-mplabxc${_number_of_bits}-bin
pkgver=1.34
pkgrel=6
pkgdesc="Microchip's MPLAB XC${_number_of_bits} C compiler toolchain for their PIC10/12/16/18 microcontroller families and their PIC14000 device"
arch=(i686 x86_64)
url=http://www.microchip.com/xc${_number_of_bits}
license=(custom)
depends_i688=(gcc-libs)
depends_x86_64=(lib32-gcc-libs)
makedepends=(sdx)
makedepends_x86_64=(lib32-tclkit)
makedepends_i686=(tclkit)

options=(!strip docs libtool emptydirs !zipman staticlibs !upx)
source=("installerBlobFromMicrochip::http://ww1.microchip.com/downloads/en/DeviceDoc/xc${_number_of_bits}-v$pkgver-full-install-linux-installer.run"
        "bitrock-unpacker.tcl")

md5sums=('443c0b7045bf414aaccb64b91036aac8'
         'b4483a2f4caf75c9af35a1d303d6b709')
install=$pkgname.install

instdir="/opt/microchip/xc${_number_of_bits}/v${pkgver}"

build() {
  msg2 "Unpacking files from installer"
  ./bitrock-unpacker.tcl ./installerBlobFromMicrochip ./unpacked.vfs
}

package() {
  mkdir -p "${pkgdir}${instdir}"
  mv unpacked.vfs/compiler/programfiles*/* "${pkgdir}${instdir}"
  mv unpacked.vfs/licensecomponent "${pkgdir}${instdir}"
  mv "${pkgdir}${instdir}"/*License.txt "${pkgdir}${instdir}/docs" 2>/dev/null || true

  mkdir -p "$pkgdir/etc/profile.d"
  echo "export PATH=\"\$PATH\":'${instdir}/bin'" > "${pkgdir}/etc/profile.d/${pkgname}.sh"
  echo "export XC${_number_of_bits}_TOOLCHAIN_ROOT='${instdir}'" >> "$pkgdir/etc/profile.d/${pkgname}.sh"

  mkdir -p "$pkgdir/usr/share/licenses/$pkgname"
  ln -s "${instdir}/docs/$(basename "${pkgdir}${instdir}/docs"/*[Ll]icense.txt)" "${pkgdir}/usr/share/licenses/$pkgname/LICENSE"
}
