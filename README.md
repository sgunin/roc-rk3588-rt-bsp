Hardware platform board support files (BSP) for ROC-RK3588-RT.
Yocto release 5.0 (scarthgap)

Страница продукта [ROC-RK3588-RT](https://en.t-firefly.com/product/industry/rocrk3588rt)

Описание [WiKi](https://wiki.t-firefly.com/en/ROC-RK3588-RT/index.html)

Файлы для скачивания [Downloads](https://en.t-firefly.com/doc/download/207.html)

Репозиторий FireFly [GitHub T-FireFly](https://github.com/T-Firefly)

Репозиторий FireFly linux [GitHub FireFly-linux](https://gitlab.com/firefly-linux)

Репозиторий FireFly JeffyCN [GitHub JeffyCN](https://github.com/JeffyCN)

# Используемые источники

1. FireFly linux: Git https://gitlab.com/firefly-linux/yocto. Layers:
+ poky - Poky Build Tool and Metadata;
+ meta-openembedded - Collection of OpenEmbedded layers;
+ meta-clang - Clang C/C++ cross compiler and runtime for OpenEmbedded/Yocto Project;
+ meta-python2 - Layer enabling legacy python2 support after EOL;
+ meta-qt5 - QT5 layer for openembedded;
+ meta-rockchip - Yocto BSP layer for the Rockchip SOC boards.
2. FireFly dev: Git https://github.com/sgunin. Layers:
+ meta-firefly-dev - Кастомный слой для разработки под платформу.

Внимание: сборка возможна в операционной системе сборки Yocto в Ubuntu 20.04 и 22.04. Сборка не работает в Ubuntu 24, а также Ubuntu любых версий в WSL. Для возможности сборки под Ubuntu 18 необходимо использовать repo из репозитория FireFly (repo init --repo-url https://gitlab.com/firefly-linux/git-repo.git).

# Установка системы сборки Yocto в Ubuntu 20.04 и 22.04
```
$: sudo apt install -y repo
$: sudo apt install repo git ssh make gcc libssl-dev liblz4-tool expect g++ patchelf chrpath gawk texinfo chrpath diffstat binfmt-support qemu-user-static live-build bison flex fakeroot cmake gcc-multilib g++-multilib unzip device-tree-compiler ncurses-dev
$: sudo apt install wget texinfo build-essential socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm zstd
$: sudo locale-gen en_US.UTF-8
```

Дополнительные настойки Git, если они не сделаны ранее (где user - имя пользователя GitHub, github_pat_token - токен доступа к репозиторию https://github.com/sgunin/meta-firefly-dev)
```
$: git config --global user.email "you@example.com"
$: git config --global user.name "Your Name"
$: git config --global credential.helper store
$: touch ~/.git-credentials
$: echo "https://user:github_pat_token@github.com" >> ~/.git-credentials
```

Внимание! Остальные действия выполняются не от имени привилегированного пользователя.

Создаем каталог для сборки и делаем его текущим:
```
$: mkdir <SomeDir>
$: cd <SomeDir>
```

Возможные варианты сборки зависят от выбранного файла конфигурации:
1. default.xml - в сборке используется мета слой meta-rockchip из персональной публичной ветки разработчика JeffyCN
2. scarthgap.xml - в сборке используется мета слой meta-rockchip из публичной ветки производителя firefly-linux
3. orig_scarthgap.xml - 

Вариант №1. Инициализация сборки из персональной публичной ветки разработчика https://github.com/JeffyCN
```
$: repo init --no-clone-bundle -u https://github.com/sgunin/roc-rk3588-rt-bsp.git -m default.xml -b scarthgap
```

Вариант №3. Инициализация сборки из публичной ветки производителя https://gitlab.com/firefly-linux
```
$: repo init --no-clone-bundle -u https://github.com/sgunin/roc-rk3588-rt-bsp.git -m scarthgap.xml -b scarthgap
```

Вариант №3. В случае, если предполагается использовать оригинальную сборку, предоставляемую производитилем, необходимо выполнить
```
$: repo init --no-clone-bundle -u https://github.com/sgunin/roc-rk3588-rt-bsp.git -m orig_scarthgap.xml -b scarthgap
```

Выполняем синхронизацию с внешними репозиториями
```
$: repo sync
```

Выполняем настройку переменных среды окружения и сборку образа core-image-minimal для аппаратной конфигурации roc-rk3588rt
```
$: source setup-environment build
$: MACHINE=roc-rk3588rt bitbake core-image-minimal
```

Возможна сборка исходного образа Rockchip командой
```
$: MACHINE=roc-rk3588-rt bitbake core-image-minimal
```

Для удаления лишних зависимостей исходного образа Rockchip возможно внести изменения в следующие файлы слоя meta-rockchip:
1. sources/meta-rockchip/conf/machine/roc-rk3588-rt.conf закоментировав следующие строки:
+ #require conf/machine/include/common.conf;
+ #require conf/machine/include/demo.conf;
2. sources/meta-rockchip/conf/machine/firefly-rk3588.conf закоментировав следующие строки:
+ #BB_NUMBER_THREADS = "4";
+ #PARALLEL_MAKE = "-j 4";
3. sources/meta-rockchip/conf/machine/include/firefly.inc удалить строчку rktoolkit из IMAGE_INSTALL:append.
