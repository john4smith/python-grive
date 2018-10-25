# Make a Ubuntu 18.04 Package
### Replace GRIVE_ID and GRIVE_SECRET!
#### Run this in your Terminal:
```
CLIENT_ID="GRIVE_ID"
CLIENT_SECRET="GRIVE_SECRET"

pkgname="python3-grive"
_pkgname="grive"
pkgdir="/tmp/build-$$"
mkdir -p "${pkgdir}"/DEBIAN
wget -O /tmp/python-grive.zip https://github.com/john4smith/python-grive/archive/master.zip
unzip /tmp/python-grive.zip -d /tmp/; cd /tmp/python-grive-*/
echo "{\"client_id\": \"$CLIENT_ID\", \"client_secret\": \"$CLIENT_SECRET\"}" > google-client.json
install -dm755 "${pkgdir}"/usr/lib/sysctl.d
install -dm755 "${pkgdir}"/usr/share/applications
install -dm755 "${pkgdir}"/usr/share/glib-2.0/schemas
install -dm755 "${pkgdir}"/usr/share/${_pkgname}/icons/loader
install -m644 google-client.json "${pkgdir}"/usr/share/${_pkgname}
install -m644 60-${_pkgname}-pyinotify.conf "${pkgdir}"/usr/lib/sysctl.d
install -m644 ${_pkgname}.desktop "${pkgdir}"/usr/share/applications
install -m644 icons/*.png "${pkgdir}"/usr/share/${_pkgname}/icons
install -m644 icons/loader/*.png "${pkgdir}"/usr/share/${_pkgname}/icons/loader
install -m644 apps.${_pkgname}.gschema.xml "${pkgdir}"/usr/share/glib-2.0/schemas
install -m755 ${_pkgname} "${pkgdir}"/usr/share/${_pkgname}
install -m644 LICENSE "${pkgdir}"/usr/share/${_pkgname}
install -m644 debian/control "${pkgdir}"/DEBIAN
pkgsize=$(du -s "${pkgdir}"/usr | cut -f1)
pkgver=$(grep ^Version: "${pkgdir}"/DEBIAN/control | cut -d" " -f2)
sed -i "s/^Installed-Size:.*/Installed-Size: ${pkgsize}/" "${pkgdir}"/DEBIAN/control
bash -c "cd \"${pkgdir}\"; find usr/ -type f -exec md5sum '{}' \; > DEBIAN/md5sums"
dpkg-deb --build "${pkgdir}" ../${pkgname}-${pkgver}_all.deb
```
