# Maintainer: john smith <hidden at mail dot com>
pkgname="python-grive"
_pkgname="grive"
pkgver=0.1
pkgrel=1
pkgdesc="Grive (Python) for Google-Drive"
arch=('any')
url="https://github.com/john4smith/${pkgname}"
license=('GPL3')
conflicts=('grive' 'grive-git')
depends=('python-google-api-python-client' 'python-oauth2client' 'python-pyinotify' 'python-gobject' 'libappindicator-gtk3' 'dconf' 'xdg-utils' 'libnotify')
optdepends=('gnome-shell-extension-appindicator-git: gnome indicator support'
            'gnome-shell-extension-topicons-plus: gnome indicator support')
source=("${pkgname}-${pkgver}.zip::${url}/archive/master.zip"
        "google-client.json")
sha256sums=('SKIP'
            'SKIP')

package() {
 install -dm755 "${pkgdir}"/usr/lib/sysctl.d
 install -dm755 "${pkgdir}"/usr/share/applications
 install -dm755 "${pkgdir}"/usr/share/glib-2.0/schemas
 install -dm755 "${pkgdir}"/usr/share/${_pkgname}/icons
 cd "${srcdir}"/
 install -m644 google-client.json "${pkgdir}"/usr/share/${_pkgname}
 cd "${srcdir}"/*${_pkgname}*/
 install -m644 60-${_pkgname}-pyinotify.conf "${pkgdir}"/usr/lib/sysctl.d
 install -m644 ${_pkgname}.desktop "${pkgdir}"/usr/share/applications
 install -m644 icons/*.png "${pkgdir}"/usr/share/${_pkgname}/icons
 install -m644 apps.${_pkgname}.gschema.xml "${pkgdir}"/usr/share/glib-2.0/schemas
 install -m755 ${_pkgname} "${pkgdir}"/usr/share/${_pkgname}
 install -m644 LICENSE "${pkgdir}"/usr/share/${_pkgname}
}
