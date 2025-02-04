# Maintainer: Kiet "Shihotori" Huynh <11086238+superkidvn@users.noreply.github.com>
pkgname=whitesur-kde-theme
_gitname=WhiteSur-kde
pkgver=2024.11.18
pkgrel=1
pkgdesc="MacOS Big Sur theme for KDE Plasma"
arch=('x86_64')
url="https://github.com/vinceliuice/${_gitname}"
license=('GPL-3.0')
optdepends=(kvantum)
source=("${pkgname}-${pkgver//./-}.tar.gz::${url}/archive/refs/tags/${pkgver//./-}.tar.gz")
sha256sums=('8ab21920e8df0647431b7f23ace7f9a7a51b320263f4364ac286bae1a359e91a')

package() {
  # Variables
  _aurorae_dir="${pkgdir}/usr/share/aurorae/themes"
  _schemes_dir="${pkgdir}/usr/share/color-schemes"
  _plasma_dir="${pkgdir}/usr/share/plasma/desktoptheme"
  _lookfeel_dir="${pkgdir}/usr/share/plasma/look-and-feel"
  _kvantum_dir="${pkgdir}/usr/share/Kvantum"
  _wallpaper_dir="${pkgdir}/usr/share/wallpapers"
  _src_extract="${srcdir}/${_gitname}-${pkgver//./-}"
  _pcolors=("" "-alt" "-dark")
  _lights=("${_gitname%-kde}" "${_gitname//kde/sharp}" "${_gitname//kde/opaque}")
  _darks=("${_gitname//kde/dark}" "${_gitname//kde/dark-sharp}" "${_gitname//kde/dark-opaque}")
  _scales=("" "_x1.25" "_x1.5" "_x1.75" "_x2.0")
  _aurorae_light_names=()
  _aurorae_dark_names=()

  for _scale in "${_scales[@]}"; do
    for _light in "${_lights[@]}"; do
      _aurorae_light_names+=("${_light}${_scale}")
    done
    for _dark in "${_darks[@]}"; do
      _aurorae_dark_names+=("${_dark}${_scale}")
    done
  done

  # Create directories
  install -d \
    "${_aurorae_dir}" \
    "${_schemes_dir}" \
    "${_plasma_dir}" \
    "${_lookfeel_dir}" \
    "${_kvantum_dir}" \
    "${_wallpaper_dir}"
 
 # Install Kvantum theme and wallpaper
  cp -r "${_src_extract}/Kvantum/"* "${_kvantum_dir}"
  cp -r "${_src_extract}/wallpaper/"* "${_wallpaper_dir}"

  # Install plasma theme and color scheme
  cp -r "${_src_extract}/color-schemes/"* "${_schemes_dir}"
  cp -r "${_src_extract}/plasma/desktoptheme/${_gitname%-kde}"* "${_plasma_dir}"
  cp -r "${_src_extract}/plasma/look-and-feel/"* "${_lookfeel_dir}"

  # Install aurorae
  cp -r "${_src_extract}/aurorae/"{main,main-opaque,main-sharp}/* "${_aurorae_dir}"

  for _aurorae_light_name in "${_aurorae_light_names[@]}"; do
    cp -r "${_src_extract}/aurorae/common/assets/"* "${_aurorae_dir}/${_aurorae_light_name}"
    cp -r "${_src_extract}/aurorae/"{metadata.desktop,metadata.json} "${_aurorae_dir}/${_aurorae_light_name}"
    cp -r "${_aurorae_dir}/${_aurorae_light_name%_x*}rc" "${_aurorae_dir}/${_aurorae_light_name}"
    sed -i "s/${_gitname%-kde}/${_aurorae_light_name}/g" \
      "${_aurorae_dir}/${_aurorae_light_name}/metadata.desktop" \
      "${_aurorae_dir}/${_aurorae_light_name}/metadata.json"
  done
  for _aurorae_dark_name in "${_aurorae_dark_names[@]}"; do
    cp -r "${_src_extract}/aurorae/common/assets-dark/"* "${_aurorae_dir}/${_aurorae_dark_name}"
    cp -r "${_src_extract}/aurorae/"{metadata.desktop,metadata.json} "${_aurorae_dir}/${_aurorae_dark_name}"
    cp -r "${_aurorae_dir}/${_aurorae_dark_name%_x*}rc" "${_aurorae_dir}/${_aurorae_dark_name}"
    sed -i "s/${_gitname%-kde}/${_aurorae_dark_name}/g" \
      "${_aurorae_dir}/${_aurorae_dark_name}/metadata.desktop" \
      "${_aurorae_dir}/${_aurorae_dark_name}/metadata.json"
  done

  # Clean up redundant aurorae rc files
  rm -f "${_aurorae_dir}/${_gitname%-kde}"*rc

  # Install license
  install -Dm0644 -t "${pkgdir}/usr/share/licenses/${pkgname}" "${_src_extract}/LICENSE" 
}
