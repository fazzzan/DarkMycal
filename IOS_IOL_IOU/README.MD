https://habr.com/ru/post/494504/
http://www.gns3.xyz/files/

# IOU/IOL + WebIOL
Эмулятор это Cisco IOS on UNIX – IOU и его вариация Cisco IOS on Linux – IOL. 
Представляет собой двоичный исполняемый файл, содержащий операционную систему L3 коммутатора Catalyst ```(L2IOU, да L2 – это не опечатка)``` или многофункционального маршрутизатора — ISR ```(L3IOU)```, скомпилированную производителем для запуска в UNIX/Linux (http://www.gns3.xyz/files/Downloads/IOU/ioulist.txt). 
- IOU характеризуется очень низким потреблением ресурсов (относительно других эмуляторов). И в отличии от Dynamips он может запускать ПО для коммутаторов. IOU разработан Cisco Systems для внутреннего использования, поэтому он доступен только сотрудникам и партнёрам. Несмотря на это, в Интернет есть инструкции по его скачиванию и установке. Однако следует помнить, что это нелегально.
- IOL существует официальный графический фронтенд WebIOL (не путать с неофициальным iou-web [3]), который позволяет формировать из устройств сложные сетевые топологии.

# Virtual Appliance (VA)
Cisco Systems всё больше и больше продуктов выпускает в виде т.н. Virtual Appliance или, проще говоря, обычных виртуальных машин, предназначенных для запуска в распространённых гипервизорах: QEMU, VMWare, Hyper-V и др.
```ASAv``` -  (Cisco Adaptive Security Virtual Appliance) – хорошо всем знакомый, но немножко устаревший, МСЭ Cisco ASA. Теперь и в виртуалке.
```NGFWv и NGIPSv``` (Cisco Firepower – Next Generation Firewall и Intrusion Prevention System) – новое поколение устройств безопасности.
```IOS XRv``` – версия IOS-XR для маршрутизаторов операторского класса. Такая штука может заменить Quagga или что-нибудь по мощнее.
```CSR1000v``` (Cloud Service Router) – маршрутизатор с ОС IOS-XE. Специализируется на VPN, MPLS, VXLAN, контроле трафика и т.д. Предназначена для размещения в облаке (https://habr.com/ru/company/mclouds/blog/326086/).
```NX-OSv``` – ещё один коммутатор/маршрутизатор, на этот раз под ОС IOS-NX, которая используется в «железках» Cisco Nexus. Также ориентирована на решение задач в ЦОД.
```Cisco Nexus Titanum``` – устаревшая виртуальная машина с NX-OS, предназначенная для внутреннего использования в Cisco.
```Nexus 1000v``` – специальная версия виртуального Nexus, предназначенная для обслуживания кластера виртуальных машин Hyper-V или VMWare. Причём это специфическая виртуальная машина, поддерживающая подключение распределённых и, опять же, виртуальных модулей. [4] Поставляется вместе со средствами виртуализации.

# Dynamips
программный эмулятор маршрутизаторов Cisco. Dynamips работает на большинстве Linux-систем, Mac OS X и Windows, при этом позволяет эмулировать аппаратную часть маршрутизаторов, непосредственно загружая и взаимодействуя с реальными образами Cisco IOS. Поддерживаемые платформы: 
  - 1700 (от 1710 до 1760)
  - 2600 (от 2610 до 2650XM)
  - 2691
  - 3600 (3620, 3640 и 3660)
  - 3725
  - 3745
  - 7200 (от NPE-100 до NPE-G1)

# EVE-NG
Виртуальная машина, отличающаяся от GNS3. Имеет свои особенности. Импорт и запуск в qemu образов виртуального оборудования выполняется в несколько этапов. Ниже приведены команды необходимые для импорта  
```
mkdir /opt/unetlab/addons/qemu/csr1000vng-universalk9.16.09.06.Fuji/
/opt/qemu-2.2.0/bin/qemu-system-x86_64  -nographic -drive file=virtioa.qcow2,if=virtio,bus=0,unit=0,cache=none -machine type=pc-1.0,accel=kvm -serial mon:stdio -nographic -nodefconfig -nodefaults -rtc base=utc -cdrom csr1000v-universalk9.16.09.06.iso -boot order=dc  -m 4096

/opt/qemu-2.2.0/bin/qemu-system-x86_64  -nographic -drive file=virtioa.qcow2,if=virtio,bus=0,unit=0,cache=none -machine type=pc-1.0,accel=kvm -serial mon:stdio -nographic -nodefconfig -nodefaults -rtc base=utc -boot order=dc  -m 4096
```

```
mkdir /opt/unetlab/addons/qemu/csr1000vng-universalk9.16.12.04a.Gibraltar
mv virtioa.qcow2 /opt/unetlab/addons/qemu/csr1000vng-universalk9.16.12.04a.Gibraltar
/opt/qemu-2.2.0/bin/qemu-system-x86_64  -nographic -drive file=virtioa.qcow2,if=virtio,bus=0,unit=0,cache=none -machine type=pc-1.0,accel=kvm -serial mon:stdio -nographic -nodefconfig -nodefaults -rtc base=utc -cdrom csr1000v-universalk9.16.12.04a.iso -boot order=dc  -m 4096
```

```
mkdir /opt/unetlab/addons/qemu/csr1000vng-universalk9.17.03.01a.Amsterdam
mv virtioa.qcow2 /opt/unetlab/addons/qemu/csr1000vng-universalk9.17.03.01a.Amsterdam
/opt/qemu-2.2.0/bin/qemu-system-x86_64  -nographic -drive file=virtioa.qcow2,if=virtio,bus=0,unit=0,cache=none -machine type=pc-1.0,accel=kvm -serial mon:stdio -nographic -nodefconfig -nodefaults -rtc base=utc -cdrom csr1000vng-universalk9.17.03.01a.iso -boot order=dc  -m 4096
```
# GNS3
GNS3 позволяет запускать не только образы Dynamips, но и VA/QEMU, а также взаимодействовать с IOU и другими виртуальными машинами. Приложение является «толстым», т.е. запускается непосредственно на машине, на которой находится. Для эмуляции устройств оно может использовать виртуальные машины, расположенные на этом же хосте или удалённо. Поддерживается работа в Linux, Windows и Mac OS X. Большим плюсом GNS3 является возможность использования тех же инструментов, что и для работы с реальным «железом»: PuTTY, SecureCRT, Wireshark и др.

![](https://hsto.org/webt/gr/6y/j_/gr6yj_6udk7ms9gwxbjwiwpk7-o.gif)
