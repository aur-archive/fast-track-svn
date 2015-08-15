# Contributor: Jens Pranaitis <jens@chaox.net>
pkgname=fast-track-svn
pkgver=150
pkgrel=1
pkgdesc="Automated penetration suite"
arch=("i686")
url="https://www.securestate.com/Pages/Fast-Track.aspx"
license=('BSD')
makedepends=("subversion")
depends=("pymssql" "metasploit3" "python-pexpect" "python-clientform" "beautiful-soup" "psyco" "firefox"\
         "pymills")
conflicts=("fast-track")
replaces=("fast-track")
provides=("fast-track")
_svntrunk=http://svn.thepentest.com/fasttrack
_svnmod=$pkgname
build() {
  cd "$srcdir"
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi
 
  msg "SVN checkout done or server timeout"
  msg "Starting make..."
 
  rm -rf "$srcdir/$_svnmod-build"
  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"
  python setup.py install
  mkdir -p "${pkgdir}"{/opt/fast-track/,/usr/share/licenses/fast-track,/usr/bin}
  cp -r bin fast-track.py ftgui readme "${pkgdir}"/opt/fast-track/
  install -m 644 readme/LICENSE "${pkgdir}"/usr/share/licenses/fast-track/
  cat > "${pkgdir}"/usr/bin/ftgui << EOF
#!/bin/bash
cd /opt/fast-track
python ./ftgui \$@
cd \$OLDPWD
EOF
  chmod 755 "${pkgdir}"/usr/bin/ftgui
}