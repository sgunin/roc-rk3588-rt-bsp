# roc-rk3588-rt-bsp
Hardware platform board support files (BSP) for ROC-RK3588-RT

Страница продукта [ROC-RK3588-RT](https://en.t-firefly.com/product/industry/rocrk3588rt)

Описание [WiKi](https://wiki.t-firefly.com/en/ROC-RK3588-RT/index.html)

Файлы для скачивания [Downloads](https://en.t-firefly.com/doc/download/207.html)

Репозиторий FireFly [GitHub T-FireFly](https://github.com/T-Firefly)

Репозиторий FireFly linux [GitHub FireFly-linux](https://gitlab.com/firefly-linux)

# Используемые источники

- Yocto: GIT https://git.yoctoproject.org/git. Layers: 
	=> poky - Poky Build Tool and Metadata
- OpenEmbedded: GIT https://github.com/openembedded. Layers: 
	=> meta-openembedded - Collection of OpenEmbedded layers
- Freescale Community: GIT https://github.com/Freescale. Layers: 
	=> base,
	=> meta-freescale - Layer containing NXP hardware support metadata, 
	=> meta-freescale-3rdparty - OpenEmbedded/Yocto BSP layer for Freescale's ARM based platforms,
	=> meta-freescale-distro - OpenEmbedded/Yocto BSP layer for Freescale's ARM based platforms
- NXP IMX Support: GIT https://github.com/nxp-imx-support. Layers: 
	=> meta-nxp-demo-experience - NXP Demo Experience Yocto Project Layer
- MYiR Dev: GIT https://github.com/sgunin. Layers: 
	=> meta-myir - i.MX Linux Yocto Project BSP 5.10.9_1.0.0
- Clang: GIT https://github.com/kraj. Layers:
	=> meta-clang - Clang C/C++ cross compiler and runtime for OpenEmbedded/Yocto Project
- Python2: GIT https://git.openembedded.org. Layers: 
	=> meta-python2 - Layer enabling legacy python2 support after EOL
- QT5: GIT https://github.com/meta-qt5. Layers: 
	=> meta-qt5 - QT5 layer for openembedded

# Установка системы сборки Yocto в Ubuntu 18 
```
$: sudo apt install -y repo
$: sudo apt-get install repo git ssh make gcc libssl-dev liblz4-tool expect g++ patchelf chrpath gawk texinfo chrpath diffstat binfmt-support qemu-user-static live-build bison flex fakeroot cmake gcc-multilib g++-multilib unzip device-tree-compiler ncurses-dev
$: sudo apt-get install wget git-core texinfo build-essential socat cpio python python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm zstd
$: sudo locale-gen en_US.UTF-8
```

Инициализация репозитория в каталог <SomeDir>, например, roc-rk3588-rt-bsp
```
$: mkdir <SomeDir>
$: cd <SomeDir>
$: repo init --no-clone-bundle --repo-url https://gitlab.com/firefly-linux/git-repo.git -u https://github.com/sgunin/roc-rk3588-rt-bsp.git -m default.xml -b main
$: repo sync
```

Настройка переменных среды и сборка образа image-minimal
```
$: MACHINE=roc-rk3588-rt source setup-environment build
$: bitbake core-image-minimal
```
