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

После всех операций презагружаемся и грузимся с данного ядра:

     [root@localhost ~]# uname -a
     Linux localhost.localdomain 4.19.63 #1 SMP Wed Jul 31 22:15:00 MSK 2019 x86_64 x86_64 x86_64 GNU/Linux

     [root@localhost ~]# ls -la /boot/
     total 224752
     dr-xr-xr-x.  5 root root     4096 Jul 31 23:10 .
     dr-xr-xr-x. 17 root root      224 Jul 31 20:56 ..
     -rw-r--r--.  1 root root   151945 Jul 29 20:49 config-3.10.0-957.27.2.el7.x86_64
     -rw-r--r--.  1 root root   151918 Nov  9  2018 config-3.10.0-957.el7.x86_64
     drwxr-xr-x.  3 root root       17 Jul 31 20:53 efi
     drwxr-xr-x.  2 root root       27 Jul 31 20:54 grub
     drwx------.  5 root root       97 Jul 31 23:10 grub2
     -rw-------.  1 root root 57157191 Jul 31 20:55 initramfs-0-rescue-b3adf553fbda4648a7c387e8807032ec.img
     -rw-------.  1 root root 21287189 Jul 31 21:07 initramfs-3.10.0-957.27.2.el7.x86_64.img
     -rw-------.  1 root root 13533552 Jul 31 21:07 initramfs-3.10.0-957.27.2.el7.x86_64kdump.img
     -rw-------.  1 root root 21286646 Jul 31 21:06 initramfs-3.10.0-957.el7.x86_64.img
     -rw-------.  1 root root 13535153 Jul 31 21:06 initramfs-3.10.0-957.el7.x86_64kdump.img
     -rw-------   1 root root 52202233 Jul 31 23:10 initramfs-4.19.63.img
     -rw-r--r--.  1 root root   314282 Jul 29 20:50 symvers-3.10.0-957.27.2.el7.x86_64.gz
     -rw-r--r--.  1 root root   314036 Nov  9  2018 symvers-3.10.0-957.el7.x86_64.gz
     lrwxrwxrwx   1 root root       24 Jul 31 23:09 System.map -> /boot/System.map-4.19.63
     -rw-------.  1 root root  3546707 Jul 29 20:49 System.map-3.10.0-957.27.2.el7.x86_64
     -rw-------.  1 root root  3543471 Nov  9  2018 System.map-3.10.0-957.el7.x86_64
     -rw-r--r--   1 root root  3676250 Jul 31 23:09 System.map-4.19.63
     -rw-r--r--   1 root root  3676250 Jul 31 23:07 System.map-4.19.63.old
     lrwxrwxrwx   1 root root       21 Jul 31 23:09 vmlinuz -> /boot/vmlinuz-4.19.63
     -rwxr-xr-x.  1 root root  6639904 Jul 31 20:55 vmlinuz-0-rescue-b3adf553fbda4648a7c387e8807032ec
     -rwxr-xr-x.  1 root root  6648000 Jul 29 20:49 vmlinuz-3.10.0-957.27.2.el7.x86_64
     -rw-r--r--.  1 root root      171 Jul 29 20:49 .vmlinuz-3.10.0-957.27.2.el7.x86_64.hmac
     -rwxr-xr-x.  1 root root  6639904 Nov  9  2018 vmlinuz-3.10.0-957.el7.x86_64
     -rw-r--r--.  1 root root      166 Nov  9  2018 .vmlinuz-3.10.0-957.el7.x86_64.hmac
     -rw-r--r--   1 root root  7890304 Jul 31 23:09 vmlinuz-4.19.63
     -rw-r--r--   1 root root  7890304 Jul 31 23:07 vmlinuz-4.19.63.old
