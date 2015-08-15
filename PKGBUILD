# Maintainer: Francisco Martinez <zomernifalt at gmail com>

pkgname=blender-buildbot
_pkbbasename=blender

[[ -z "$CARCH" ]] && CARCH='x86_64'
msg "Aquiring most recent build's version information..."
  _remotefilename=$(wget -O- -q "https://builder.blender.org/download" | grep linux | grep $CARCH)
  
  _pkgmainver=$(echo $_remotefilename \
	  | sed -n -r "/glibc[[:digit:]]{2,3}-$CARCH/p" \
	  | egrep -o -m 1 '[[:digit:]]\.[[:digit:]]{2}[[:alnum:]]?' \
	  | head -n 1)
  msg2 "Version: $_pkgmainver"
  _pkgrev=$(echo $_remotefilename \
	  | sed -n -r "/glibc[[:digit:]]{2,3}-$CARCH/p" \
	  | egrep -o '\-[[:alnum:]]{5,}\-' \
	  | sed -e 's/-//g' \
	  | head -n 1)
  msg2 "Revision: $_pkgrev"
_filename="$_pkbbasename-$_pkgmainver-$_pkgrev-linux-glibc211-$CARCH.tar.bz2"
pkgver=2.72
pkgrel=1
pkgdesc="Development version of Blender, automatically built by a bot every couple of days."
arch=('i686' 'x86_64')
url="http://blender.org/"
depends=('libpng' 'libtiff' 'openexr' 'python' 'desktop-file-utils' 'shared-mime-info' 'hicolor-icon-theme'
	 'xdg-utils' 'glew' 'freetype2' 'openal' 'ffmpeg' 'fftw' 'boost-libs' 'openssl098' 'xz'
	 'opencollada' 'llvm' 'openimageio' 'libsndfile' 'jack' 'opencolorio' 'openshadinglanguage')
license=('GPL')
makedepends=('wget')
provides=('blender')
conflicts=('blender')
options=('!strip')
source=("https://builder.blender.org/download/$_filename"\
	blender.desktop)
noextract=()
md5sums=('SKIP'
	 'd9e38bd6cf3c9d59c5b7f83cf2d631e1') 
install='blender.install'

pkgver() {
	echo "$_pkgmainver.$_pkgrev"
}

package() {
	cd $pkgdir
	mkdir -p opt/  usr/{share/{applications,icons/hicolor,licenses/$_pkbbasename,blender},bin}
	mv $srcdir/${_filename/.tar.bz2/} opt/$_pkbbasename
	cp -L $srcdir/blender.desktop usr/share/applications/
	for d in opt/$_pkbbasename/icons/*; do	
		if [ -d $d ]; then
			find $d -exec chmod ugo+r {} \;
			mv $d usr/share/icons/hicolor/
		fi
	done
	rm -r opt/$_pkbbasename/icons
	
	for f in opt/$_pkbbasename/*.txt; do 
		cat $f >> LICENSE
		rm $f
	done
	mv LICENSE usr/share/licenses/$_pkbbasename/
	
	find opt/$_pkbbasename -exec chmod 754 {} \; -exec chmod +x {} \;
	ln -s /opt/$_pkbbasename/$_pkbbasename usr/bin/blender
	ln -s /opt/$_pkbbasename/$_pkbbasename-softwaregl usr/bin/$_pkbbasename-softwaregl
	ln -s /opt/$_pkbbasename/$_pkbbasename-thumbnailer.py usr/bin/$_pkbbasename-thumbnailer.py
	mv opt/blender/$_pkgmainver usr/share/blender/$_pkgmainver
	ln -s /usr/share/blender/$_pkgmainver opt/$_pkbbasename/$_pkgmainver
}
