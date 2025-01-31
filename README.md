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
+ meta-rockchip - Yocto BSP layer for the Rockchip SOC boards.
2. Дополнительные слои:
+ meta-firefly-dev - Кастомный слой для разработки под платформу из ветки https://github.com/sgunin;
+ meta-hikvision - Кастомный слой c SDK камеры HikRobot.

Внимание: сборка возможна в операционной системе сборки Yocto в Ubuntu 20.04 и 22.04. Сборка не работает в Ubuntu 24, а также Ubuntu любых версий в WSL. Для возможности сборки под Ubuntu 18 необходимо использовать repo из репозитория FireFly (repo init --repo-url https://gitlab.com/firefly-linux/git-repo.git).

# Установка системы сборки Yocto в Ubuntu 20.04 и 22.04
```
$ sudo apt install -y repo
$ sudo apt install repo git ssh make gcc libssl-dev liblz4-tool expect g++ patchelf chrpath gawk texinfo chrpath diffstat binfmt-support qemu-user-static live-build bison flex fakeroot cmake gcc-multilib g++-multilib unzip device-tree-compiler ncurses-dev
$ sudo apt install wget texinfo build-essential socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm zstd
$ sudo locale-gen en_US.UTF-8
```

Дополнительные настойки Git, если они не сделаны ранее (где user - имя пользователя GitHub, github_pat_token - токен доступа к репозиторию https://github.com/sgunin/meta-firefly-dev)
```
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
$ git config --global credential.helper store
$ touch ~/.git-credentials
$ echo "https://user:github_pat_token@github.com" >> ~/.git-credentials
```

Внимание! Остальные действия выполняются не от имени привилегированного пользователя.

Создаем каталог для сборки и делаем его текущим:
```
$ mkdir <SomeDir>
$ cd <SomeDir>
```

Возможные варианты сборки зависят от выбранного файла конфигурации:
1. default.xml - в сборке используется мета слой meta-rockchip из персональной публичной ветки разработчика [JeffyCN](https://github.com/JeffyCN). Не имеет работающих dts под roc-rk3588rt.
2. scarthgap.xml - в сборке используется мета слой meta-rockchip из публичной ветки производителя RockChip - firefly-linux. На данный момент это предпочтительный и самый стабильный вариант сборки образа.
3. orig_scarthgap.xml - оригинальная сборка от производителя. Включает много лишних слоев и зависимостей, сильно увеличивающих размер и время сборки.

Инициализируем каталог сборки вариантом конфигурации №2 (для вновь созданного каталога):
```
$ repo init --no-clone-bundle -u https://github.com/sgunin/roc-rk3588-rt-bsp.git -m scarthgap.xml -b scarthgap
```

Если каталог был инициализирован ранее, или в ветки репозиториев вносились изменения, выполняем синхронизацию:
```
$ repo sync
```

Выполняем настройку переменных среды окружения (если не было сделано ранее)
```
$ source setup-environment build
```

Конфигурация scarthgap.xml через слой meta-firefly-dev предоставляет для сборки следующие варианты образов
1. rk3588-core-image-minimal - образ с минимальным количеством компонентов;
2. rk3588-core-image-minimal-x11 - образ с поддержкой Х11;
3. rk3588-core-image-minimal-x11-dev - образ с поддержкой средств разработки.

Cборка необходимого образа выполняется командой
```
$ MACHINE=roc-rk3588rt bitbake rk3588-core-image-minimal-x11-dev
```

В результате сборки в каталоге tmp/deploy/images/roc-rk3588rt будут сформированы следующие файлы:
```
-rw-r--r-- 1 sg sg       5156 янв 31 12:45 rk3588-core-image-minimal-x11-dev.env
-rw-r--r-- 1 sg sg        795 янв 31 12:45 rk3588-core-image-minimal-x11-dev-generic-gptdisk.wks
-rw-r--r-- 1 sg sg 1598046208 янв 31 12:45 rk3588-core-image-minimal-x11-dev-roc-rk3588rt-20250131094328.rootfs.ext4
-rw-r--r-- 1 sg sg      37825 янв 31 12:45 rk3588-core-image-minimal-x11-dev-roc-rk3588rt-20250131094328.rootfs.manifest
-rw-r--r-- 1 sg sg  356673188 янв 31 12:45 rk3588-core-image-minimal-x11-dev-roc-rk3588rt-20250131094328.rootfs.tar.gz
-rw-r--r-- 1 sg sg 2446575616 янв 31 12:46 rk3588-core-image-minimal-x11-dev-roc-rk3588rt-20250131094328.rootfs.wic
-rw-r--r-- 1 sg sg     346586 янв 31 12:45 rk3588-core-image-minimal-x11-dev-roc-rk3588rt-20250131094328.testdata.json
-rw-r--r-- 1 sg sg        275 янв 31 12:46 rk3588-core-image-minimal-x11-dev-roc-rk3588rt.package-file
-rw-r--r-- 1 sg sg        378 янв 31 12:46 rk3588-core-image-minimal-x11-dev-roc-rk3588rt.parameter
-rw-r--r-- 1 sg sg 2037686858 янв 31 12:46 rk3588-core-image-minimal-x11-dev-roc-rk3588rt.update.img
...
```

Для загрузки прошивки в устройство необходимо перевести его в режим загрузки. Возможно 2 варианта - аппаратно, на 2 секунды зажать кнопку Recovery и подать питание на устройство или использовать adb.
При первом использовании необходимо устранить проблемы с привилегиями для adb
```
$ adb kill-server
$ sudo adb start-server
$ sudo adb devices
```

Перезагружаем устройство в режим загрузки
```
$ sudo adb shell
$ reboot loader
```

После перезагрузки будет доступен режим обновления
```
$ sudo upgrade_tool LD
```

Загружаем прошивку в устройство
```
$ sudo upgrade_tool wl 0 rk3588-core-image-minimal-x11-dev-roc-rk3588rt-20250131094328.rootfs.wic
$ sudo upgrade_tool uf rk3588-core-image-minimal-x11-dev-roc-rk3588rt.update.img
```

На устройстве создан пользователь root/firefly.

Для правки dts можно воспользоваться devshell
```
$ MACHINE=roc-rk3588rt bitbake virtual/kernel
$ MACHINE=roc-rk3588rt bitbake virtual/kernel -c devshell
# vi arch/arm64/boot/dts/rockchip/roc-rk3588-rt.dts
# make roc-rk3588-rt.dts
# exit
```

Для запуска процесса перекомпиляции необходимо изменить md5 сумму файла, проверить её можно командой
```
# md5sum $KBUILD_OUTPUT/arch/arm64/boot/dts/rockchip/roc-rk3588-rt.dts
```

Перекомпиляция запускается командой
```
$ MACHINE=roc-rk3588rt bitbake core-image-minimal -c cleanall
$ MACHINE=roc-rk3588rt bitbake core-image-minimal
```

Правка ядра запускается командой
```
$ MACHINE=roc-rk3588rt bitbake linux-rockchip -c menuconfig
```

Проверить состояние сервиса Android ADB на стороне платы можно командой
```
# systemctl status android-tools-adbd.service
```
