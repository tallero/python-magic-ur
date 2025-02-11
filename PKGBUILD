# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: David Runge <dvzrv@archlinux.org>
# Contributor: Lukas Fleischer <lfleischer@archlinux.org>

# tests not in pypi sdist tarball
_origin="github"
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=magic
pkgname="${_py}-${_pkg}"
pkgver=0.4.27
pkgrel=5
epoch=1
pkgdesc="A python wrapper for libmagic"
arch=(
  'any'
)
_http="https://github.com"
_ns="ahupp"
url="${_http}/${_ns}/${pkgname}"
depends=(
  'file'
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest"
)
license=(
  'MIT'
)
if [[ "${_origin}" == "pypi" ]]; then
  _pypa="https://files.pythonhosted.org/packages/source"
  _src="${_pypa}/${pkgname::1}/$pkgname/$pkgname-$pkgver.tar.gz"
elif [[ "${_origin}" == "github" ]]; then
  _src="${pkgname}-${pkgver}.tar.gz::${url}/archive/refs/tags/${pkgver}.tar.gz"
fi
source=(
  "${_src}"
  "545a2a561522efc2869066792062694b59b1b39c.patch"
)
sha512sums=(
  'a476730a5caa9a2a784187f57743d5cec4b1829a6a76d4d1fb4e0112caf5487888961df293bc38074ef1a5d313b0fc4aed4cc99b980f5336e8a907c44a33e84e'
  'ce7cd8d719c01124a97802e298a9396ece92c4545e336b39bf67bbf821b7bac2adb097294cf705052676992085b0682a2367bf368f0bb70d33b24b5123c9bfb1'
)
b2sums=(
  '4ba22d0f8bd5e70eb37e3b46eba1b885d49682bf45d703ad7966bcc67614427ebe597e3100575f863b7e54421c6de6fc875af24a9d5b49742fe07b361b65f198'
  '9365f6046688526fb4b0f6380748dfe40e6a1fad54cadda508513559e19dceccc81343f8599063fff7cf7f85f77d126a5d37298d285d99a1feffdce89b052917'
)

prepare() {
  cd \
    "${pkgname}-${pkgver}"
  patch \
    -p1 \
    -i \
    "../545a2a561522efc2869066792062694b59b1b39c.patch"
}

build() {
  cd \
    "${pkgname}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  local \
    _pytest_options=()
  _pytest_options=(
    --deselect test/python_magic_test.py::MagicTest::test_extension
  )
  cd \
    "${pkgname}-${pkgver}"
  LC_ALL="en_US.UTF-8" \
  pytest \
    -vv \
    "${_pytest_options[@]}"
}

package() {
  cd \
    "${pkgname}-${pkgver}"
  "${_py}" \
    -m \
      installer \
      --destdir="${pkgdir}" \
      "dist/"*".whl"
  install \
    -vDm644 \
    "LICENSE" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}
