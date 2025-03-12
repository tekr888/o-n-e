# Configure Router-on-a-Stick Inter-VLAN Routing

## Задачи:
 1. Создать сеть на основе топологии;
 2. Создать VLAN и назначить на порты коммутаторов;
 3. Настройка транка 802.1Q между коммутаторами:
 4. Настроить маршрутизацию между VLAN на маршрутизаторе;
 5. Проверить работу маршрутизации между VLAN;  

 ## Конфигурации:  
  - [Конфигурация R1](config-R1);  
  - [Конфигурация S1](config-S1);  
  - [Конфигурация S2](config-S2);  
  
#  Решение:
 1. Топология из задания:  
 ![](topology.png)  
 Топология созданная в EVE-NG:  
 ![](eve-ng.png)    
 
 Таблица адресов:  
| Device     | Interface Topology - EVE-NG  | IP Address   | Subnet Mask   | Default Gateway  |
|:----------:|:----------------------------:|:------------:|:-------------:|:----------------:|
| R1         | G0/0/1.3 - E0/1.3            | 192.168.3.1  | 255.255.255.0 | N/A              |
|            | G0/0/1.4 - E0/1.4            | 192.168.4.1  | 255.255.255.0 | N/A              |
|            | G0/0/1.8 - E0/1.8            | N/A          | N/A           | N/A              |
| S1         | VLAN3 - VLAN3                | 192.168.3.11 | 255.255.255.0 | 192.168.3.1      |
| S2         | VLAN3 - VLAN3                | 192.168.3.12 | 255.255.255.0 | 192.168.3.1      |
| PC-A       | NIC - NIC                    | 192.168.3.3  | 255.255.255.0 | 192.168.3.1      |
| PC-B       | NIC - NIC                    | 192.168.4.3  | 255.255.255.0 | 192.168.4.1      |  

VLAN Таблица:  

| VLAN | NAME       | Interface Assigned Topology - EVE-NG                              |
|:----:|:----------:|:-----------------------------------------------------------------:|
| 3    | Management | S1: VLAN3 - VLAN3                                                 |
|      |            | S2: VLAN3 - VLAN3                                                 |
|      |            | S1: F0/6 - E1/0                                                   |
| 4    | Operations | S2: F0/18 = E4/1                                                  |
| 7    | ParkinkLot | S1: F0/2-4 - E0/1-E0/3, F0/7-24 - E1/2-E5/3, G0/1-2 - N/A         |
|      |            | S2: F0/2-17 - E0/0, E0/2-E4/0, F0/19-24 - E4/2-E5/3, G0/1-2 - N/A |
| 8    | Native     | N/A                                                               |  

Trunk Таблица:  

| Device |Trunk Interface Topology - EVE-NG |
|:------:|:--------------------------------:|
| S1     | F0/1, F0/5 - E0/0, E1/0          |
| S2     | F0/1 - E0/1                      |  


 2. Создание VLAN и их назначение на коммутаторах:  

VLAN с S1:  

```
S1#sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active
3    Management                       active    Et1/1
4    Operations                       active
7    ParkingLot                       active    Et0/1, Et0/2, Et0/3, Et1/2
                                                Et1/3, Et2/0, Et2/1, Et2/2
                                                Et2/3, Et3/0, Et3/1, Et3/2
                                                Et3/3, Et4/0, Et4/1, Et4/2
                                                Et4/3, Et5/0, Et5/1, Et5/2
                                                Et5/3
8    Native                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```  

VLAN с S2:  

```
S2#sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active
3    Management                       active
4    Operations                       active    Et4/1
7    ParkingLot                       active    Et0/0, Et0/2, Et0/3, Et1/0
                                                Et1/1, Et1/2, Et1/3, Et2/0
                                                Et2/1, Et2/2, Et2/3, Et3/0
                                                Et3/1, Et3/2, Et3/3, Et4/0
                                                Et4/2, Et4/3, Et5/0, Et5/1
                                                Et5/2, Et5/3
8    Native                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```  
3. Транк между коммутаторами S1<-->S2, а также транк для подключения S1<-->R1:  

S1:  

```
...
!
interface Ethernet0/0
 description -=To S2=-
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 8
 switchport nonegotiate
 switchport mode trunk
 !
 ...
 !
 interface Ethernet1/0
 description -=To R1=-
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 8
 switchport nonegotiate
 switchport mode trunk
 !
 ...

```  
S2:  

```
...
!
interface Ethernet0/1
 description -=To S1=-
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 8
 switchport nonegotiate
 switchport mode trunk
 !
 ...
```  
4. Настроить маршрутизацию между VLAN на маршрутизаторе:  

```
...
!
interface Ethernet0/0
 no ip address
 shutdown
!
interface Ethernet0/1
 description -=To S1=-
 no ip address
!
interface Ethernet0/1.3
 encapsulation dot1Q 3
 ip address 192.168.3.1 255.255.255.0
!
interface Ethernet0/1.4
 encapsulation dot1Q 4
 ip address 192.168.4.1 255.255.255.0
!
interface Ethernet0/1.8
!
...
```  
5. Проверка связанности:  

   - с PC-A на default gateway;  
   - с PC-A на PC-B;
   - с PC-A на S2;

```
PC-A> ping 192.168.3.1

84 bytes from 192.168.3.1 icmp_seq=1 ttl=255 time=0.791 ms
84 bytes from 192.168.3.1 icmp_seq=2 ttl=255 time=1.082 ms
84 bytes from 192.168.3.1 icmp_seq=3 ttl=255 time=1.131 ms
84 bytes from 192.168.3.1 icmp_seq=4 ttl=255 time=1.280 ms
84 bytes from 192.168.3.1 icmp_seq=5 ttl=255 time=1.478 ms

PC-A> ping 192.168.4.3

84 bytes from 192.168.4.3 icmp_seq=1 ttl=63 time=3.684 ms
84 bytes from 192.168.4.3 icmp_seq=2 ttl=63 time=2.247 ms
84 bytes from 192.168.4.3 icmp_seq=3 ttl=63 time=1.989 ms
84 bytes from 192.168.4.3 icmp_seq=4 ttl=63 time=3.349 ms
84 bytes from 192.168.4.3 icmp_seq=5 ttl=63 time=1.511 ms

PC-A> ping 192.168.3.12

84 bytes from 192.168.3.12 icmp_seq=1 ttl=255 time=0.827 ms
84 bytes from 192.168.3.12 icmp_seq=2 ttl=255 time=1.508 ms
84 bytes from 192.168.3.12 icmp_seq=3 ttl=255 time=0.690 ms
84 bytes from 192.168.3.12 icmp_seq=4 ttl=255 time=0.651 ms
84 bytes from 192.168.3.12 icmp_seq=5 ttl=255 time=0.823 ms

PC-A>
```  
# Остальное:  

1. Сменить имя устройства;  
2. Отключить разрешение имён DNS;  
3. Задать пароль для console, vty, enable;  
4. Зашифровать созданные пароли для режимов подключения;  
5. Создать предупреждающий баннер на устрйоствах;  
6. Установить время.  

Смотреть в конфигурациях:  
  - [Конфигурация R1](config-R1);  
  - [Конфигурация S1](config-S1);  
  - [Конфигурация S2](config-S2);  

  Примеры команд:  
  ```
  clock set 23:12:10 11 Mar 2025
  banner motd #Unauthorized access is prohibited!#
  copy running-config startup-config
  no ip domain-lookup
  vtp mode off
  service password-encryption
  enable algorithm-type sha256 secret you_password
  ```  
