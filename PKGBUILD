# Maintainer: Your Name <your.email@example.com>
pkgname=zalo-linux-git
_pkgname=zalo-linux
pkgver=1.0.0.r0.g1234567
pkgrel=1
pkgdesc="Zalo desktop app for Linux built from macOS DMG bundle"
arch=('x86_64')
url="https://github.com/ngmvix2010/zalo-linux"
license=('MIT' 'custom:proprietary')

# Các thư viện & công cụ cần thiết để build native modules và đóng gói
makedepends=(
  'git' 'nodejs' 'npm' 'p7zip' 'dpkg' 'fakeroot'
  'build-essential' 'cmake' 'meson' 'ninja' 'pkgconf'
  'libtool' 'autoconf' 'automake' 'gettext' 'nasm'
  'patchelf' 'clang' 'wget' 'unzip' 'python'
  'openssl' 'sqlcipher' 'xz'
)

# Các thư viện cần thiết lúc ứng dụng chạy
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

  # Đảm bảo cache npm nằm trong thư mục build để tránh dính quyền root
  export npm_config_cache="${srcdir}/npm-cache"

  # 1. Cài đặt các phụ thuộc npm
  npm ci || npm install

  # 2. Tải DMG, giải nén, patch và build native modules
  npm run setup

  # 3. Tạo file gói .deb vào thư mục dist/
  npm run build
}

package() {
  cd "${srcdir}/${_pkgname}"

  # Giải nén file .deb đã build từ npm run build vào thẳng pkgdir của Arch
  local deb_file=$(ls dist/Zalo-*.deb | head -n 1)
  
  if [ ! -f "$deb_file" ]; then
    echo "Lỗi: Không tìm thấy file .deb trong thư mục dist/"
    exit 1
  fi

  # Giải nén data.tar.xz (hoặc data.tar.zst) từ deb vào $pkgdir
  bsdtar -xf "$deb_file" -C "${pkgdir}" data.tar.*
}
