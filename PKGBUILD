pkgname=winformsui-monoport
pkgver=5
pkgrel=2
pkgdesc="The docking library for .Net Windows Forms development which mimics Visual Studio .Net."
url="https://bitbucket.org/hindlemail"
arch=(any)
license=("MIT")
depends=(mono)
makedepends=(mono mercurial)
options=(!strip)
_hgroot=$url
_hgrepo=winformsui-monoport

build() {
  cd $srcdir
  msg "Connecting to hg server..."
  if [[ -d "$_hgrepo/.hg" ]]; then
    msg "pull"
    ( cd $_hgrepo && hg pull -u )
  else
    msg "clone"
    hg clone "${_hgroot}/${_hgrepo}"
  fi
  cd "${_hgrepo}"

  msg "Mercurial checkout done or server timeout"
  msg "Starting build..."

  rm -rf "${srcdir}/${_hgrepo}-build"
  cp -r "${srcdir}/${_hgrepo}" "${srcdir}/${_hgrepo}-build"
  cd "${srcdir}/${_hgrepo}-build"
  
  xbuild WinFormsUI.Docking.sln
  cd "WinFormsUI/bin/Debug"
  monodis WeifenLuo.WinFormsUI.Docking.dll --output=WeifenLuo.WinFormsUI.Docking.il
  sn -k 1024 WinFormsUI.snk
  ilasm /dll /key:WinFormsUI.snk WeifenLuo.WinFormsUI.Docking.il
}

package() {
  cd "${srcdir}/winformsui-monoport-build/WinFormsUI/bin/Debug"
  install -Dm644 WeifenLuo.WinFormsUI.Docking.dll "$pkgdir/usr/lib/winformsui/WeifenLuo.WinFormsUI.Docking.dll"
  install -Dm644 WeifenLuo.WinFormsUI.Docking.dll.mdb "$pkgdir/usr/lib/winformsui/WeifenLuo.WinFormsUI.Docking.dll.mdb"
  gacutil -i WeifenLuo.WinFormsUI.Docking.dll -root "$pkgdir/usr/lib"
  install -Dm644 "${srcdir}/${_hgrepo}-build/license.txt" "$pkgdir/usr/share/licenses/$pkgname/license.txt"
}
