# Maintainer: Ngô Minh Vĩ <ngo.minhvi.04082010@gmail.com>
pkgname=zalo-linux-git
_pkgname=zalo-linux
pkgver=1.0.0.r0.g1234567
pkgrel=1
pkgdesc="Zalo desktop app for Linux built from macOS DMG bundle"
arch=('x86_64')
url="https://github.com/ngmvix2010/zalo-linux"
license=('MIT' 'custom:proprietary')

# Tên các dependency đã được chuẩn hóa theo đúng tên repo Arch (core/extra)
makedepends=(
  'git' 'nodejs' 'npm' 'p7zip' 'fakeroot'
  'base-devel' 'cmake' 'meson' 'ninja' 'pkgconf'
  'libtool' 'autoconf' 'automake' 'gettext' 'nasm'
  'patchelf' 'clang' 'wget' 'unzip' 'python'
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

  # Đặt cache npm trong srcdir để tránh lỗi quyền
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

  # Dùng bsdtar xả nén dữ liệu từ file deb ra pkgdir
  bsdtar -xf "$deb_file" -C "${pkgdir}" "data.tar.*"
  bsdtar -xf "${pkgdir}/data.tar."* -C "${pkgdir}"
  rm -f "${pkgdir}/data.tar."*
}
