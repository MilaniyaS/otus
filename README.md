### Сборка ядра на CentOS Linux release 7.6.1810 (Core)

1. Отключаем selinux на всякий случай.
В /etc/sysconfig/selinux меняем SELINUX=enforcing на SELINUX=disabled
2. Устанавлиаем необоходимые пакеты для сборки ядра из исходников:

    `yum group install "Development Tools" -y`

    `yum install -y wget ncurses-devel make gcc bc bison flex elfutils-libelf-devel openssl-devel`

3. Скачиваем тарболл ядра

    `cd  /usr/src/kernels/ && wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.63.tar.xz`
    
4. Распаковываем и переходим в директорию с исходниками
    `tar -xf linux-4.19.63.tar.xz && cd linux-4.19.63`
   
5. Формируем .config,  небходимый для сборки. Есть два варинта:

    Включаем необходимые модули через    `make menuconfig`
    
    Или можно взять текущий 

    `cp -v /boot/config-3.10.0-957.27.2.el7.x86_64 .config`

 6. Приступаем к сборке
 
    `make -j4 bzImage`
 
    `make -j4 modules`

    `make -j4`

    -j4 означает, что будем собирать в 4 потока, что существенно увеличит скорость сборки.

7. Устанавливаем ядро в систему

    `make modules_install && make install`
    

PS Очень важно иметь не менее 20GB дискового пространства.


