# Maintainer: Joshua Warren
pkgname=lastbook
pkgver=1.1.9
pkgrel=1
pkgdesc='Offline survival library: guides, manuals, videos, maps and a local AI chat'
arch=(x86_64 aarch64)
url='https://lastbook.org'
license=(MIT)
# The window is the system's GTK 3 and WebKitGTK 4.1 through python-gobject. The Python wheels that Arch does not package
# (libzim, pypdfium2, beautifulsoup4, soupsieve) are pinned below and installed into /opt/lastbook/lib, as upstream's .deb does.
depends=(python python-gobject python-numpy python-typing_extensions gtk3 webkit2gtk-4.1 libsoup3
         gstreamer gst-plugins-base gst-plugins-good gst-libav vulkan-icd-loader gcc-libs)
makedepends=(cmake ninja gcc python-pillow shaderc vulkan-headers spirv-headers)
optdepends=('vulkan-radeon: local AI on AMD graphics' 'nvidia-utils: local AI on NVIDIA graphics'
            'vulkan-intel: local AI on Intel graphics' 'vulkan-asahi: local AI on Apple Silicon')
options=(!strip)
_lb=4d6c2e75d481d22341f31d02586fee79c86066c2
_llama=dde879bc4f954b809892fd0d660515f42536394a
source=("lastbook-$_lb.tar.gz::https://github.com/Immortal-Protocols/lastbook/archive/$_lb.tar.gz"
        "llama.cpp-$_llama.tar.gz::https://github.com/ggml-org/llama.cpp/archive/$_llama.tar.gz"
        'manifest-v1.json::https://downloads.lastbook.org/manifest-v1.json'   # changes upstream, so it has no checksum
        'beautifulsoup4-4.15.0-py3-none-any.whl::https://files.pythonhosted.org/packages/88/c6/92fcd42f1ba33e1184263f25bfabf3d27c383410470f169e4b8163bf9c17/beautifulsoup4-4.15.0-py3-none-any.whl'
        'soupsieve-4.1.1-py3-none-any.whl::https://files.pythonhosted.org/packages/cd/65/7fae69a095696195334a934eab23aeb12e2715e6c3f08025d2f91942750e/soupsieve-4.1.1-py3-none-any.whl')
source_x86_64=('libzim-3.13.1-cp314-cp314-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl::https://files.pythonhosted.org/packages/31/f3/16820fa6041ecbf6c6b54d1b3dcc23e3489f71447870f36a3d2779092434/libzim-3.13.1-cp314-cp314-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl'
               'pypdfium2-5.14.0-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl::https://files.pythonhosted.org/packages/4f/a3/c9cc797fc8bdfb8f37b9b0f8b9d02a5fc196b2015f408d53624cab5b0519/pypdfium2-5.14.0-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl')
source_aarch64=('libzim-3.13.1-cp314-cp314-manylinux_2_27_aarch64.manylinux_2_28_aarch64.whl::https://files.pythonhosted.org/packages/b6/98/08ce16f4e3d6ec20a8c775272943efda63cd077ef9ebd0e64b83df544f9c/libzim-3.13.1-cp314-cp314-manylinux_2_27_aarch64.manylinux_2_28_aarch64.whl'
                'pypdfium2-5.14.0-py3-none-manylinux_2_17_aarch64.manylinux2014_aarch64.whl::https://files.pythonhosted.org/packages/92/b4/0c31aa51887cd6cd032191dfe010a6d01ed43cf03204cfbd2184ebe4b715/pypdfium2-5.14.0-py3-none-manylinux_2_17_aarch64.manylinux2014_aarch64.whl')
sha256sums=('f9a74784fa7315f54dd33c9e43a2ceae8f467645f59aacee9811a26da41ce77d'
            '83f099b6bbb531ca72d22cdd76d86641438469a214fd49a38f026c6a868598ed'
            'SKIP'
            'd6f88de62e1d4e38ecb1077eb9724cd0eff29d2a08ca16a401e9b9e93f117cf9'
            '8ff3422dab0b720fb667ee79864d80531a0e57994a13eb701697719ffcb751f6')
sha256sums_x86_64=('bc29b54e91690de33706d765605d3ca9a63e066ce70175f51bf1169ba63bef70'
                   '11f281613fa22313d9c7ab89947665e84eccf8ebe40e1198a84a88352305648d')
sha256sums_aarch64=('77864105f08cceaf8ad3c9d7d4a9cc682a33b4962ac0ee1c31976e5330b24dc8'
                    'f1b696e6901e16f114a2ec6332e5e3f8f5033a901614ead28499ab18ca6024f5')

build() {
  local lb="$srcdir/lastbook-$_lb" ll="$srcdir/llama.cpp-$_llama"
  local map="-ffile-prefix-map=$ll=llama.cpp -ffile-prefix-map=$lb=lastbook -ffile-prefix-map=$srcdir/build=build"

  # lastbook-llm: llama.cpp with Vulkan, no sockets, same flags as packaging/llm/build_llm_linux.sh. CPU variants load at run time.
  cmake -S "$ll" -B "$srcdir/build" -G Ninja -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_SHARED_LIBS=ON -DGGML_BACKEND_DL=ON -DGGML_CPU_ALL_VARIANTS=ON -DGGML_NATIVE=OFF -DGGML_OPENMP=ON \
    -DGGML_VULKAN=ON -DGGML_RPC=OFF -DGGML_CCACHE=OFF \
    -DLLAMA_BUILD_COMMON=OFF -DLLAMA_BUILD_TOOLS=OFF -DLLAMA_BUILD_EXAMPLES=OFF -DLLAMA_BUILD_TESTS=OFF -DLLAMA_BUILD_SERVER=OFF \
    -DLLAMA_CURL=OFF -DLLAMA_OPENSSL=OFF \
    -DCMAKE_SKIP_BUILD_RPATH=OFF -DCMAKE_BUILD_WITH_INSTALL_RPATH=ON -DCMAKE_INSTALL_RPATH='$ORIGIN' \
    -DCMAKE_SHARED_LINKER_FLAGS="-Wl,--enable-new-dtags" -DCMAKE_MODULE_LINKER_FLAGS="-Wl,--enable-new-dtags" \
    -DCMAKE_C_FLAGS="$map" -DCMAKE_CXX_FLAGS="$map"
  local cpus
  cpus="$(cmake --build "$srcdir/build" --target help | grep -oE '^ggml-cpu-[a-z0-9_.]+:' | tr -d : | grep -v -- '-feats$' | sort -u | tr '\n' ' ')"
  [ -n "$cpus" ] || cpus=ggml-cpu
  cmake --build "$srcdir/build" --target llama ggml-vulkan $cpus
  g++ -std=c++17 -O3 -DNDEBUG $map \
    -I"$ll/include" -I"$ll/ggml/include" -I"$ll/common" -I"$ll/vendor" -I"$ll/src" \
    "$lb/packaging/llm/lastbook_llm.cpp" "$ll/common/fit.cpp" "$ll/common/log.cpp" "$ll/common/json.cpp" "$lb/packaging/llm/fit_unused_win.cpp" \
    -L"$srcdir/build/bin" -lllama -lggml -lggml-base -pthread \
    -Wl,-rpath,'$ORIGIN' -Wl,--enable-new-dtags -Wl,--as-needed -o "$srcdir/build/bin/lastbook-llm"

  # The GStreamer source for lastbook://app/videos/
  gcc -shared -fPIC -O2 -Wall -s -ffile-prefix-map="$lb"=lastbook "$lb/packaging/linux/gst/lastbooksrc.c" \
    -o "$srcdir/build/libgstlastbooksrc.so" $(pkg-config --cflags --libs gstreamer-1.0 gstreamer-base-1.0)

  # web/ and content/ as they ship
  cd "$lb"
  python -I -B packaging/ship_app_files.py "$srcdir/ship"
  install -Dm644 "$srcdir/manifest-v1.json" "$srcdir/ship/setup/manifest-v1.json"
}

package() {
  local lb="$srcdir/lastbook-$_lb" opt="$pkgdir/opt/lastbook"
  install -d "$opt"/{app,engine/setup,setup,lib,llm,gst}
  install -m644 "$lb"/app/{main,bridge,shell_linux}.py "$opt/app/"
  install -m644 "$lb"/{server,retrieval,assistant,llm_local}.py "$opt/"
  install -m644 "$lb"/engine/{__init__,paths,library,saved_pages,address_search,update}.py "$opt/engine/"
  install -m644 "$lb"/engine/setup/*.py "$lb/engine/setup/roots.pem" "$opt/engine/setup/"
  cp -R "$srcdir/ship/web" "$srcdir/ship/content" "$opt/"
  install -m644 "$srcdir/ship/setup/manifest-v1.json" "$opt/setup/"
  install -m644 "$lb/packaging/linux/lastbook.py" "$opt/"
  echo "$pkgver" > "$opt/VERSION"

  local f
  for f in "$srcdir"/*.whl; do python -m zipfile -e "$f" "$opt/lib"; done
  rm -rf "$opt/lib/bin" "$opt/lib/pypdfium2_cli"
  find "$opt/lib" -depth \( -name tests -o -name testing -o -name __pycache__ \) -type d -exec rm -rf {} +

  install -m755 "$srcdir/build/bin/lastbook-llm" "$opt/llm/"
  for f in libllama libggml libggml-base; do
    cp -L "$srcdir/build/bin/$f.so" "$opt/llm/$(objdump -p "$srcdir/build/bin/$f.so" | sed -n 's/^ *SONAME *//p')"
  done
  cp "$srcdir"/build/bin/libggml-vulkan.so "$srcdir"/build/bin/libggml-cpu*.so "$opt/llm/"
  strip -s "$opt"/llm/*.so* "$opt/llm/lastbook-llm"
  install -m644 "$srcdir/llama.cpp-$_llama/LICENSE" "$opt/llm/LICENSE-llama.cpp.txt"
  install -m755 "$srcdir/build/libgstlastbooksrc.so" "$opt/gst/"

  python -I -m compileall -q -s "$pkgdir" -p / "$opt" > /dev/null

  install -d "$pkgdir/usr/bin" "$pkgdir/usr/share/applications"
  printf '#!/bin/sh\nexec /usr/bin/python -I -B /opt/lastbook/lastbook.py "$@"\n' > "$pkgdir/usr/bin/lastbook"
  chmod 755 "$pkgdir/usr/bin/lastbook"
  install -m644 "$lb/packaging/linux/lastbook.desktop" "$pkgdir/usr/share/applications/lastbook.desktop"
  python -I - "$lb/packaging/icon/tile-1024.png" "$pkgdir/usr/share/icons/hicolor" <<'PY'
import sys
from pathlib import Path
from PIL import Image
src, out = Image.open(sys.argv[1]).convert('RGBA'), Path(sys.argv[2])
for n in (16, 24, 32, 48, 64, 128, 256, 512):
    d = out / f'{n}x{n}' / 'apps'
    d.mkdir(parents=True, exist_ok=True)
    src.resize((n, n), Image.LANCZOS).save(d / 'lastbook.png', optimize=True)
PY
  install -Dm644 "$lb/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 "$lb/THIRD_PARTY_NOTICES.md" "$pkgdir/usr/share/licenses/$pkgname/THIRD_PARTY_NOTICES.md"
}
