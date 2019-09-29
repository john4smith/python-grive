# Make a Ubuntu 18.04 Package
### Replace GRIVE_ID and GRIVE_SECRET!
#### Run this in your Terminal:
```
wget -O /tmp/python-grive.zip https://github.com/john4smith/python-grive/archive/master.zip
unzip /tmp/python-grive.zip -d /tmp/; cd /tmp/python-grive-*/

name="grive"
pkgdir="/tmp/build-$$"
mkdir -p "${pkgdir}"/DEBIAN
install -dm755 "${pkgdir}"/usr/lib/sysctl.d
install -dm755 "${pkgdir}"/usr/share/applications
install -dm755 "${pkgdir}"/usr/share/glib-2.0/schemas
install -dm755 "${pkgdir}"/usr/share/${name}/icons
install -m644 60-${name}-pyinotify.conf "${pkgdir}"/usr/lib/sysctl.d
install -m644 ${name}.desktop "${pkgdir}"/usr/share/applications
install -m644 icons/*.png "${pkgdir}"/usr/share/${name}/icons
install -m644 apps.${name}.gschema.xml "${pkgdir}"/usr/share/glib-2.0/schemas
install -m755 ${name} "${pkgdir}"/usr/share/${name}
install -m644 LICENSE "${pkgdir}"/usr/share/${name}
install -m644 debian/control "${pkgdir}"/DEBIAN
install -m644 debian/copyright "${pkgdir}"/DEBIAN
pkgsize=$(du -s "${pkgdir}"/usr | cut -f1)
pkgver=$(grep ^Version: "${pkgdir}"/DEBIAN/control | cut -d" " -f2)
pkgfile=$(grep ^Package: "${pkgdir}"/DEBIAN/control | cut -d" " -f2)
sed -i "s/^Installed-Size:.*/Installed-Size: ${pkgsize}/" "${pkgdir}"/DEBIAN/control
bash -c "cd \"${pkgdir}\"; find usr/ -type f -exec md5sum '{}' \; > DEBIAN/md5sums"
dpkg-deb --build "${pkgdir}" ../${pkgfile}-${pkgver}_all.deb
```
