# Maintainer: VoobScout <voobscout+arch@gmail.com>
# Axdia international GmbH wintab 9 plus 3G/Tablet, BIOS 5.6.5 03/10/2015

pkgname=rtl8723bs-wifi-bt-git
_wifidriver=r8723bs_wifi
_btdriver=r8723bs_bt
pkgver=0.5
pkgrel=1
pkgdesc="Realtek R8723BS WIFI & BT"
url="http://www.realtek.com.tw/"
provides=('rtl8723bs-wifi-bt')
arch=('any')
license=('GPL')
install=rtl8723bs-wifi-bt-git.install
#builddepends=('linux-zen-baytab-headers')
builddepends=('linux-headers')
#depends=('linux-zen-baytab')

source=("${_wifidriver}::git+https://github.com/hadess/rtl8723bs"
        "${_btdriver}::git+https://github.com/lwfinger/rtl8723bs_bt.git"
        '99-rtl8723bs-bt.rules'
        '99-disable-sap.conf'
        'rtl8723bs-bt.service'
        'rtl8723bs-bt-power-on.service')
sha256sums=('SKIP'
            'SKIP'
            'dc6de087ee0e34cc71651f3d7202d08784f5f70adba812697f02d8f3376590ad'
            '760becdd20e4790ff93c34d59e5fcc473dc47ccea1eb8aa17e1070bac69ca506'
            '1eb279fc0923d5efb3d3046cb528faf46f1b927d9912c754a830938e0b89ddbc'
            '8b72ff56ee187aa08401d0a381696207f40d9d305a2e4a5abfd18f37af2ee401')

_build_wifi() {
	cd "${srcdir}/${_wifidriver}/"
	make $MAKEFLAGS all
	make strip
}

_build_bt() {
  cd "${srcdir}/${_btdriver}/"
  make $MAKEFLAGS
}

build() {
  _build_wifi
  _build_bt
}

_pkg_wifi() {
	cd "${srcdir}/${_wifidriver}/"
	_kver=$(uname -r)
	mkdir -p "${pkgdir}/usr/lib/modules/${_kver}/kernel/drivers/net/wireless"
	mkdir -p "${pkgdir}/usr/lib/firmware/rtlwifi/"
	install -p -m 644 r8723bs.ko "${pkgdir}/usr/lib/modules/${_kver}/kernel/drivers/net/wireless/"
	install -p -m 644 rtl8723bs_nic.bin "${pkgdir}/usr/lib/firmware/rtlwifi/"
	install -p -m 644 rtl8723bs_wowlan.bin "${pkgdir}/usr/lib/firmware/rtlwifi/"
}

_pkg_bt() {
  cd "${srcdir}/${_btdriver}/"
  mkdir -p "${pkgdir}/usr/lib/firmware/rtl_bt"
  install -p -m 644 rtlbt_config "${pkgdir}/usr/lib/firmware/rtl_bt/"
  install -p -m 644 rtlbt_fw "${pkgdir}/usr/lib/firmware/rtl_bt/"
  install -p -m 644 rtlbt_fw_new "${pkgdir}/usr/lib/firmware/rtl_bt/"

  mkdir -p "${pkgdir}/usr/bin"
  install -p -m 755 rtk_hciattach "${pkgdir}/usr/bin/"
}

_pkg_config() {
  cd "${srcdir}"
  mkdir -p "${pkgdir}/etc/systemd/system/bluetooth.service.d/"
  install -p -m 644 99-disable-sap.conf "${pkgdir}/etc/systemd/system/bluetooth.service.d/"
  mkdir -p "${pkgdir}/usr/lib/udev/rules.d/"
  install -p -m 644 99-rtl8723bs-bt.rules "${pkgdir}/usr/lib/udev/rules.d/"
  mkdir -p "${pkgdir}/usr/lib/systemd/system/"
  install -p -m 644 rtl8723bs-bt.service "${pkgdir}/usr/lib/systemd/system/"
  install -p -m 644 rtl8723bs-bt-power-on.service "${pkgdir}/usr/lib/systemd/system/"
}

package() {
  _pkg_wifi
  _pkg_bt
  _pkg_config
}
