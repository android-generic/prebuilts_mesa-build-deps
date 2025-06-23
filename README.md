# Prebuilts mesa programs for building mesa on platforms like AOSP

Currently these include:
- mesa_clc
- vtn_bindgen2

All of these are programs are built for Ubuntu 24.04.x LTS. To be able to run, get these packages

```
libllvmspirvlib-19.1 llvm-19 libclang1-19 libclang-cpp19 libtinfo6 libzstd1
```

## Building

To be able to build, you need to get

```
# Add lunarng repo to download newer version of spirv-tools, which mesa required
wget -qO - https://packages.lunarg.com/lunarg-signing-key-pub.asc | sudo apt-key add -
sudo wget -qO /etc/apt/sources.list.d/lunarg-vulkan-1.4.313-noble.list https://packages.lunarg.com/vulkan/1.4.313/lunarg-vulkan-1.4.313-noble.list
sudo apt update

# Download the required packages
sudo apt install libwayland-egl-backend-dev libxcb-randr0-dev libx11-xcb-dev libxcb-dri3-dev libxcb-present-dev libxcb-shm0-dev libxshmfence-dev clang-19-tools libclang-19-dev spirv-tools clang-19 libclang-cpp19-dev spirv-tools llvm-19 libllvmspirvlib-19-dev
```

Then start to build

```
meson setup . _build -Dgallium-drivers=iris -Dvulkan-drivers= -Dglx=disabled -Dintel-bvh-grl=true -Dintel-clc=enabled -Dinstall-intel-clc=true -Dmesa-clc=enabled -Dinstall-mesa-clc=true -Dprecomp-compiler=enabled -Dinstall-precomp-compiler=true -Dstatic-libclc=all

meson configure --no-pager _build

ninja $NINJAFLAGS -C _build
```

Once done, you can find the programs at

`mesa_clc` : `_build/src/compiler/clc/mesa_clc` <br>
`vtn_bindgen2` : `_build/src/compiler/spirv/vtn_bindgen2`
