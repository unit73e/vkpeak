pkgname=vkpeak
pkgver=20240505
pkgrel=2
pkgdesc="A tool which profiles Vulkan devices to find their peak capacities"
arch=('x86_64')
url="https://github.com/nihui/vkpeak"
license=('MIT')
source=("${pkgname}::git+https://github.com/nihui/vkpeak.git#tag=${pkgver}"
        "ncnn::git+https://github.com/Tencent/ncnn.git"
        "cmake4-fix.patch")
depends=('vulkan-icd-loader')
makedepends=('cmake' 'glslang' 'ninja' 'protobuf' 'vulkan-headers')
sha256sums=('804733ce178f8a3c5a5e1139fd48708286ea40095a0c835e7796d63fbb85fa92'
            'SKIP'
            '0788204a831bb2c87dc6ab30e84377a91938671c602e986959740b96c1f733b2')

prepare() {
    cd "${srcdir}/${pkgname}"
    git submodule init
    git config submodule."ncnn".url "${srcdir}/ncnn"
    git -c protocol.file.allow=always submodule update --init --recursive
    patch -Np1 -i ../cmake4-fix.patch
}
build(){
    cd "${srcdir}/${pkgname}"

    rm -rf build
    mkdir build

    cd build
    cmake ..
    cmake --build . -j $(nproc)
}

package() {
    cd "${srcdir}/${pkgname}"

    install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/vkpeak/LICENSE
    install -Dm755 build/vkpeak "${pkgdir}/usr/bin/vkpeak"
}
