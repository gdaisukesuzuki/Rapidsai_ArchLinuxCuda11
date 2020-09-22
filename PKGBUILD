# Maintainer: Sven-Hendrik Haase <svenstaro@gmail.com>
# Contributor: bartus <arch-user-repoᘓbartus.33mail.com>
# Contributor: pingplug <pingplug@foxmail.com>
# Contributor: cornholio <vigo.the.unholy.carpathian@gmail.com>

pkgname=(rapids-rmm)
_pkgname=rmm
pkgver=0.16.0a.r383.gfd3dc48
pkgrel=1
pkgdesc="RAPIDS Memory Manager"
arch=('x86_64')
url="https://rapids.ai/"
license=('custom')
depends=('gcc9' 'cython' 'cuda' 'cmake' 'python-numba' 'fmt')
# depends=('gcc9' 'cython' 'cuda' 'cmake' 'python-numba' 'spdlog')
# source=("${_pkgname}::git+https://github.com/rapidsai/rmm.git")
source=("${_pkgname}::git+https://github.com/rapidsai/rmm.git#branch=branch-0.16"
     "setup.py.patch"
  )

# sha256sums=('SKIP')
sha256sums=('SKIP' 'SKIP')

pkgver() {
    cd rmm

    local _version
    local _revision
    local _shorthash

    _version="$(git tag | sort -Vr | head -n1 | sed 's/^v//')"
    _revision="$(git rev-list v"${_version}"..HEAD --count)"
    _shorthash="$(git rev-parse --short HEAD)"

    printf '%s.r%s.g%s' "$_version" "$_revision" "$_shorthash"
}


build() {
  cd "${srcdir}/rmm"
  git submodule update --init --remote --recursive

  export CC=/usr/bin/gcc-9
  export CXX=/usr/bin/g++-9
  export CUDAHOSTCXX=/usr/bin/g++-9


  mkdir -p build
  cd build
  #  -DCMAKE_CXX_FLAGS="-march=skylake -O3 -DSPDLOG_FMT_EXTERNAL" \
  cmake .. \
    -DCMAKE_CXX_FLAGS="-march=skylake -O3" \
    -DCMAKE_CUDA_HOST_COMPILER=/usr/bin/g++-9 \
    -DCMAKE_C_COMPILER=/usr/bin/gcc-9 \
    -DCMAKE_CXX_COMPILER=/usr/bin/g++-9 \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DBUILD_SHARED_LIBS=ON \
    -DBUILD_TESTS=OFF \
    -DBUILD_BENCHMARKS=OFF \
    -DPER_THREAD_DEFAULT_STREAM=ON \
    -DCMAKE_CXX11_ABI=ON \
    -DGPU_TARGET="sm_75"
  make spdlog
  make -j6 all

  cd "${srcdir}"/rmm/include
  ln -sf ../build/_deps/cnmam-src/include/cnmem.h .
  ln -sf ../build/_deps/spdlog-src/include/spdlog .

  cd "${srcdir}"
  patch -p0 < setup.py.patch

  cd "${srcdir}"/rmm/python
  export LD_LIBRARY_PATH=/usr/lib:/opt/cuda/lib64:/opt/cuda/extras/CUPTI/lib64:/opt/magma/lib
  export CUDA_PATH=/opt/cuda
  export CUDA_HOME=/opt/cuda

  export PATH=/opt/cuda:$PATH
  export CFLAGS='-march=skylake -mtune=native -O3 -pipe -DSPDLOG_FMT_EXTERNAL -fstack-protector -I/opt/cuda/include'
  export NVCC='/opt/cuda/bin/nvcc'
  export CUB_PATH=/usr
  export LDFLAGS=-L/opt/cuda/lib64

  python setup.py build_ext --inplace --library-dir="${srcdir}/rmm/build"
  # python setup.py build  --library-dir="${srcdir}/rmm/build"
}

package_rapids-rmm() {
  cd "${srcdir}"/rmm/build
  make DESTDIR="${pkgdir}" install
  cd "${srcdir}"/rmm/build/_deps/spdlog-build
  make DESTDIR="${pkgdir}" install
  cd "${srcdir}"/rmm/build/_deps/spdlog-src/include
  cp -pr spdlog ${pkgdir}/usr/include
  cd "${srcdir}"/rmm/python
  export LD_LIBRARY_PATH=/opt/cuda/lib64:${srcdir}/rmm/build:$LD_LIBRARY_PATH
  python setup.py install --prefix=/usr --root="${pkgdir}"  --optimize=1  --skip-build
  cp rmm/*.py ${pkgdir}/usr/lib/python3.8/site-packages/rmm/
  cp rmm/_lib/*.pxd ${pkgdir}/usr/lib/python3.8/site-packages/rmm/_lib/
  cp rmm/_cuda/*.pxd ${pkgdir}/usr/lib/python3.8/site-packages/rmm/_cuda/
#  cp rmm/tests/*.pxd ${pkgdir}/usr/lib/python3.8/site-packages/rmm/tests/
  cp rmm/_cuda/*.py ${pkgdir}/usr/lib/python3.8/site-packages/rmm/_cuda/
  cp rmm/_lib/*.py ${pkgdir}/usr/lib/python3.8/site-packages/rmm/_lib/
  cp rmm/tests/*.py ${pkgdir}/usr/lib/python3.8/site-packages/rmm/tests/

  install -Dm644 ${srcdir}/rmm/LICENSE ${pkgdir}/usr/share/licenses/rapids-rmm/LICENSE
}

# vim:set ts=2 sw=2 et:
