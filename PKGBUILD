# Maintainer: Ngô Minh Vĩ <ngmvix2010@gmail.com>
pkgname=zalo-linux-git
_pkgname=zalo-linux
pkgver=1.0.0.r0.g1234567
pkgrel=1
pkgdesc="Zalo desktop app for Linux built from macOS DMG bundle"
arch=('x86_64')
url="https://github.com/ngmvix2010/zalo-linux"
license=('MIT' 'custom:proprietary')

makedepends=(
  'git' 'nodejs' 'npm' 'p7zip' 'fakeroot'
  'base-devel' 'cmake' 'meson' 'ninja' 'pkgconf'
  'libtool' 'autoconf' 'automake' 'gettext' 'nasm'
  'patchelf' 'clang' 'wget' 'unzip' 'python' 'python-pillow'
  'openssl' 'sqlcipher' 'xz'
)

depends=(
  'openssl'
  'sqlcipher'
  'xz'
  'gtk3'
  'nss'
  'alsa-lib'
)

provides=("${_pkgname}")
conflicts=("${_pkgname}")
source=("git+${url}.git")
sha256sums=('SKIP')

pkgver() {
  cd "${srcdir}/${_pkgname}"
  git describe --long --tags --always | sed 's/-/.r/;s/-/./g'
}

build() {
  cd "${srcdir}/${_pkgname}"

  export npm_config_cache="${srcdir}/npm-cache"

  npm ci || npm install
  npm run setup
  npm run build
}

package() {
  cd "${srcdir}/${_pkgname}"

  local deb_file=$(ls dist/Zalo-*.deb | head -n 1)
  
  if [ ! -f "$deb_file" ]; then
    echo "Lỗi: Không tìm thấy file .deb trong thư mục dist/"
    exit 1
  fi

  bsdtar -xf "$deb_file" -C "${pkgdir}" "data.tar.*"
  bsdtar -xf "${pkgdir}/data.tar."* -C "${pkgdir}"
  rm -f "${pkgdir}/data.tar."*
}
