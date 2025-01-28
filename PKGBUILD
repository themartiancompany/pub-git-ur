# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
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

# Maintainer:  Truocolo <truocolo@aol.com>
# Maintainer:  Pellegrino Prevete <pellegrinoprevete@gmail.com>

_git="false"
_local="false"
_proj=hip
_py="python"
_pkgname=pub
pkgname="${_pkgname}-git"
pkgver="0.0.0.0.0.0.0.0.0.0.0.0.0.1".r14.g"64e74bc1ff4eb569acef05453ac3c5c41dc9311c"
pkgrel=1
_pkgdesc=(
  "ur publisher"
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  any
)
_gl="gitlab.com"
_gh="github.com"
_host="https://${_gh}"
_ns='themartiancompany'
_local="${HOME}/${_pkgname}"
url="${_host}/${_ns}/${_pkgname}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkgname}"
license=(
  AGPL3
)
depends=(
  'git'
  'evm-gpg'
  'findutils'
  'gpg-key-info'
  'lur'
  'libcrash-bash'
  'rsync'
  'ur'
)
makedepends=(
  'make'
  "${_py}-docutils"
)
checkdepends=(
  'shellcheck'
)
optdepends=(
  'lur: lur origin support'
  'fur: fur publishing support'
)
_os="$( \
  uname \
    -o)"
[[ "${_os}" == "Android" ]] && \
  optdepends+=(
    'inteppacman: manage android native applications packages'
  )
provides=(
  "${_pkgname}=${pkgver}"
)
conflicts=(
  "${_pkgname}"
)
groups=(
 "${_proj}"
 "${_proj}-git"
)
_url="${url}"
[[ "${_local}" == true ]] && \
  _url="${_local}"
source=()
_branch="master"
[[ "${_git}" == true ]] && \
  makedepends+=(
    git
  ) && \
  source+=(
    "${_pkgname}-${_branch}::git+${_url}#branch=${_branch}"
  )
[[ "${_git}" == false ]] && \
  makedepends+=(
    curl
    jq
  ) && \
  source+=(
    "${_pkgname}-${_branch}.tar.gz::${_url}/archive/refs/heads/${_branch}.tar.gz"
  )
sha256sums=(
  SKIP
)

_nth() {
  local \
    _str="${1}" \
    _n="${2}"
  echo \
    "${_str}" | \
    awk \
      -F '+' \
      '{print $'"${_n}"'}'
}

_jq_pkgver() {
  local \
    _version \
    _rev \
    _commit
  _version="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].name')"
  _version_commit="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].commit.sha')"
  _rev="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        'map(.sha == '${_version_commit}' ) | index(true)')"
  _commit="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        '.[0].sha')"
  printf \
    "%s.r%s.g%s" \
    "${_version}" \
    "${_rev}" \
    "${_commit}"
}

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$( \
    _nth \
      "${_pkgver}" \
      "1")"
  _rev="$( \
    _nth \
      "${_pkgver}" \
      "2")"
  _commit="$( \
    _nth \
      "${_pkgver}" \
      "3")"
  _out=${_ver}
  [[ "${_rev}" != "" ]] && \
    _out+=".r${_rev}"
  [[ "${_commit}" != "" ]] && \
    _out+=".${_commit}"
  echo \
    "${_out}"
}

_git_pkgver() {
  local \
    _pkgver
  _pkgver="$( \
    git \
      describe \
      --tags \
      --long | \
      sed \
        's/-/+/g')"
  _parse_ver \
    "${_pkgver}"
}

pkgver() {
  cd \
    "${_pkgname}-${_branch}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

package() {
  cd \
    "${_pkgname}-${_branch}"
  make \
    DESTDIR="${pkgdir}" \
    PREFIX="/usr" \
    install
  install \
    -Dm644 \
    "COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}

# vim: ft=sh syn=sh et
