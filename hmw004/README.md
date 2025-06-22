# Большая Лабораторная работа  

## Задачи:  
1. Разработаете и задокументируете адресное пространство для лабораторного стенда.  
2. Настроите ip адреса на каждом активном порту.  
3. Настроите каждый VPC в каждом офисе в своем VLAN.  
4. Настроите VLAN/Loopbackup interface управления для сетевых устройств.  
5. Настроите сети офисов так, чтобы не возникало broadcast штормов, а использование линков было максимально оптимизировано.  
6. Используете IPv4. IPv6 по желанию.  
7. Настроите политику маршрутизации для сетей офиса.  
8. Распределите трафик между двумя линками с провайдером.  
9. Настроите отслеживание линка через технологию IP SLA.(только для IPv4).  
10. Настройте для офиса Лабытнанги маршрут по-умолчанию.  
11. Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.  
12. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.  
13. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.  
14. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.  
15. Настроите IS-IS в ISP Триада:  
- R23 и R25 находятся в зоне 2222;  
- R24 находится в зоне 24;  
- R26 находится в зоне 26;  
- Настройка осуществляется одновременно для IPv4 и IPv6.  
16. Настроить EIGRP в С.-Петербург:  
- В офисе С.-Петербург настроить EIGRP;  
- R32 получает только маршрут по умолчанию;  
- R16-17 анонсируют только суммарные префиксы;  
- Использовать EIGRP named-mode для настройки сети;
- Настройка осуществляется одновременно для IPv4 и IPv6.  
17. Настроить BGP между автономными системами. Организовать доступность между офисами Москва и С.-Петербург:  
- Настроите eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас;  
- Настроите eBGP между провайдерами Киторн и Ламас;  
- Настроите eBGP между Ламас и Триада;  
- Настроите eBGP между офисом С.-Петербург и провайдером Триада;  
- Организуете IP доступность между пограничным роутерами офисами Москва и С.-Петербург.  
- Настройка осуществляется одновременно для IPv4 и IPv6.  
18. Настроить iBGP в офисе Москва. Настроить iBGP в сети провайдера Триада  
- Настроите iBGP в офисом Москва между маршрутизаторами R14 и R15.  
- Настроите iBGP в провайдере Триада, с использованием RR.  
- Настройте офис Москва так, чтобы приоритетным провайдером стал Ламас.  
- Настройте офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.  

# Решение:  

## 1-6:  

Адресное пространство: 

Топология из задания:  
![](scheme.jpg) 

AS1001 (Москва):  
| Device     | Interface Topology | IP Address    | Subnet Mask     | Default Gateway  | LoopBack       | 
|:----------:|:------------------:|:-------------:|:---------------:|:----------------:|:--------------:|
| R12        | E0/0               | N/A           | N/A             | N/A              | 172.16.1.12/32 |
|            | E0/0.11            | 10.177.11.1   | 255.255.255.0   | N/A              |                |
|            | E0/1               | N/A           | N/A             | N/A              |                |
|            | E0/1.12            | 10.177.12.1   | 255.255.255.0   | N/A              |                |
|            | E0/2               | 100.0.10.12   | 255.255.255.240 | N/A              |                |
|            | E0/3               | 100.0.11.12   | 255.255.255.240 | N/A              |                |
|            | E1/0               | N/A           | N/A             | N/A              |                |
|            | E1/0.13            | 10.177.13.1   | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |
| R13        | E0/0               | N/A           | N/A             | N/A              | 172.16.1.13/32 |
|            | E0/0.11            | 10.177.11.2   | 255.255.255.0   | N/A              |                |
|            | E0/1               | N/A           | N/A             | N/A              |                |
|            | E0/1.12            | 10.177.12.2   | 255.255.255.0   | N/A              |                |
|            | E0/2               | 100.0.10.13   | 255.255.255.240 | N/A              |                |
|            | E0/3               | 100.0.9.13    | 255.255.255.240 | N/A              |                |
|            | E1/0               | N/A           | N/A             | N/A              |                |
|            | E1/0.13            | 10.177.13.2   | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |
| R14        | E0/0               | 100.0.10.14   | 255.255.255.240 | N/A              | 172.16.1.14/32 |
|            | E0/1               | 100.0.9.14    | 255.255.255.240 | N/A              |                |
|            | E0/2               | 109.0.13.14   | 255.255.255.224 | N/A              |                |
|            | E0/3               | 100.0.12.14   | 255.255.255.224 | N/A              |                |
|            |                    |               |                 |                  |                |
| R15        | E0/0               | 100.0.10.11   | 255.255.255.240 | N/A              | 172.16.1.15/32 |
|            | E0/1               | 100.0.11.11   | 255.255.255.240 | N/A              |                |
|            | E0/2               | 100.0.13.15   | 255.255.255.224 | N/A              |                |
|            | E0/3               | 100.0.16.15   | 255.255.255.224 | N/A              |                |
|            |                    |               |                 |                  |                |
| R19        | E0/0               | 100.0.12.19   | 255.255.255.224 | N/A              | 172.16.1.19/32 |
|            |                    |               |                 |                  |                |
| R20        | E0/0               | 100.0.16.20   | 255.255.255.224 | N/A              | 172.16.1.20    |
|            |                    |               |                 |                  |                |
| SW3        | VLAN11             | 10.177.11.3   | 255.255.255.0   | 10.177.11.254    | N/A            |
|            | VLAN12             | 10.177.12.3   | 255.255.255.0   | 10.177.11.254    | N/A            |
|            | VLAN13             | 10.177.13.3   | 255.255.255.0   | 10.177.11.254    | N/A            |
|            |                    |               |                 |                  |                |
| SW4        | VLAN11             | 10.177.11.4   | 255.255.255.0   | N/A              | N/A            |
|            | VLAN12             | N/A           | N/A             | N/A              | N/A            |
|            | VLAN13             | N/A           | N/A             | N/A              | N/A            |
|            |                    |               |                 |                  |                |
| SW5        | VLAN11             | 10.177.11.5   | 255.255.255.0   | N/A              | N/A            |
|            | VLAN12             | N/A           |                 | N/A              | N/A            |
|            | VLAN13             | N/A           |                 | N/A              | N/A            |
|            |                    |               |                 |                  |                |
| SW6        | VLAN11             | 10.177.11.6   | 255.255.255.0   | 10.177.11.254    | N/A            |
|            | VLAN12             | 10.177.12.6   | 255.255.255.0   | 10.177.11.254    | N/A            |
|            | VLAN13             | 10.177.13.6   | 255.255.255.0   | 10.177.11.254    | N/A            |
|            |                    |               |                 |                  |                |
| VPC-1      | NIC - NIC          | DHCP          | DHCP            | DHCP             |                |
| VPC-7      | NIC - NIC          | DHCP          | DHCP            | DHCP             |                |
|            |                    |               |                 |                  |                |  

VLAN Таблица:  
| VLAN | NAME       | Interface Assigned Topology - EVE-NG |
|:----:|:----------:|:------------------------------------:|
| 11   | Management | SW3-SW6: N/A                         |
| 12   | UserGroup1 | SW3: E0/2-3, E1/0-3                  |
| 13   | UserGroup2 | SW5: E0/2-3, E1/0-3                  |
| 3999 | Native     | N/A                                  |
|      |            |                                      |  


AS2042(Санкт-Питербург):  
| Device     | Interface Topology | IP Address    | Subnet Mask     | Default Gateway  | LoopBack       |
|:----------:|:------------------:|:-------------:|:---------------:|:----------------:|:--------------:|
| R16        | E0/0               | N/A           | N/A             | N/A              | 172.16.1.16/32 |
|            | E0/0.15            | 10.177.15.2   | 255.255.255.0   | N/A              |                |
|            | E0/1               | 85.85.85.16   | 255.255.255.224 | N/A              |                |
|            | E0/2               | N/A           | N/A             | N/A              |                |
|            | E0/2.14            | 10.177.14.2   | 255.255.255.0   | N/A              |                |
|            | E0/3               | 84.84.84.16   | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |
| R17        | E0/0               | N/A           | N/A             | N/A              | 172.16.1.17/32 |
|            | E0/0.14            | 10.177.14.1   | 255.255.255.0   | N/A              |                |
|            | E0/1               | 86.86.86.17   | 255.255.255.224 | N/A              |                |
|            | E0/2               | N/A           | N/A             | N/A              |                |
|            | E0/2.15            | 10.177.15.1   | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |
| R18        | E0/0               | 85.85.85.18   | 255.255.255.224 | N/A              | 172.16.1.18/32 |
|            | E0/1               | 86.86.86.18   | 255.255.255.224 | N/A              |                |
|            | E0/2               | 98.0.98.18    | 255.255.255.0   | N/A              |                |
|            | E0/3               | 99.0.99.18    | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |
| R32        | E0/0               | 84.84.84.32   | 255.255.255.0   | N/A              | 172.16.1.32/32 |
|            |                    |               |                 |                  |                |
| SW9        | VLAN14             | 10.177.14.9   | 255.255.255.0   | 10.177.14.254    | N/A            |
|            | VLAN15             | 10.177.15.9   | 255.255.255.0   | 10.177.14.254    | N/A            |
|            |                    |               |                 |                  |                |
| SW10       | VLAN14             | 10.177.14.10  | 255.255.255.0   | 10.177.14.254    | N/A            |
|            | VLAN15             | 10.177.15.10  | 255.255.255.0   | 10.177.14.254    | N/A            |
|            |                    |               |                 |                  |                |
| VPC        | NIC - NIC          | DHCP          | DHCP            | DHCP             |                |
| VPC-8      | NIC - NIC          | DHCP          | DHCP            | DHCP             |                |
|            |                    |               |                 |                  |                |  

VLAN Таблица:  
| VLAN | NAME       | Interface Assigned Topology - EVE-NG |
|:----:|:----------:|:------------------------------------:|
| 14   | Management | SW9: E0/2, E1/1-3                    |
| 15   | UserGroup1 | S10: E0/2, E1/1  -3                  |
| 3999 | Native     | N/A                                  |
|      |            |                                      |

AS301 (Ламас):
| Device     | Interface Topology | IP Address    | Subnet Mask     | Default Gateway  | LoopBack       |
|:----------:|:------------------:|:-------------:|:---------------:|:----------------:|:--------------:|
| R21        | E0/0               | 100.0.13.21   | 255.255.255.224 | N/A              | 172.16.1.21/32 |
|            | E0/1               | 110.0.110.21  | 255.255.255.224 | N/A              |                |
|            | E0/2               | 112.0.10.21   | 255.255.255.224 | N/A              |                |
|            |                    |               |                 |                  |                |  

AS101(Китрон):
| Device     | Interface Topology | IP Address    | Subnet Mask     | Default Gateway  | LoopBack       |
|:----------:|:------------------:|:-------------:|:---------------:|:----------------:|:--------------:|
| R22        | E0/0               | 109.0.13.22   | 255.255.255.224 | N/A              | 172.16.1.22/32 |
|            | E0/1               | 110.0.110.22  | 255.255.255.224 | N/A              |                |
|            | E0/2               | 111.0.10.22   | 255.255.255.224 | N/A              |                |
|            |                    |               |                 |                  |                |  

AS520(Триада):
| Device     | Interface Topology | IP Address    | Subnet Mask     | Default Gateway  | LoopBack       |
|:----------:|:------------------:|:-------------:|:---------------:|:----------------:|:--------------:|
| R23        | E0/0               | 111.0.10.23   | 255.255.255.224 | N/A              | 172.16.1.23/32 |
|            | E0/1               | 95.0.95.23    | 255.255.255.224 | N/A              |                |
|            | E0/2               | 96.0.96.23    | 255.255.255.224 | N/A              |                |
|            |                    |               |                 |                  |                |
| R24        | E0/0               | 112.0.10.24   | 255.255.255.224 | N/A              | 172.16.1.24/32 |
|            | E0/1               | 97.0.97.24    | 255.255.255.224 | N/A              |                |
|            | E0/2               | 96.0.96.24    | 255.255.255.224 | N/A              |                |
|            | E0/3               | 98.0.98.24    | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |
| R25        | E0/0               | 95.0.95.25    | 255.255.255.224 | N/A              | 172.16.1.25/32 |
|            | E0/1               | 200.20.2.1    | 255.255.255.0   | N/A              |                |
|            | E0/2               | 94.0.94.25    | 255.255.255.224 | N/A              |                |
|            | E0/3               | 92.0.92.25    | 255.255.255.192 | N/A              |                |
|            |                    |               |                 |                  |                |
| R26        | E0/0               | 97.0.97.26    | 255.255.255.224 | N/A              | 172.16.1.26/32 |
|            | E0/1               | 93.0.93.26    | 255.255.255.192 | N/A              |                |
|            | E0/2               | 94.0.94.26    | 255.255.255.224 | N/A              |                |
|            | E0/3               | 99.0.99.26    | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |  

Лабытанги:
| Device     | Interface Topology | IP Address    | Subnet Mask     | Default Gateway  | LoopBack       |
|:----------:|:------------------:|:-------------:|:---------------:|:----------------:|:--------------:|
| R27        | E0/0               | 200.20.2.27   | 255.255.255.0   | 200.20.2.1       | 172.16.1.27/32 |
|            |                    |               |                 |                  |                |  

Чокурдах:
| Device     | Interface Topology | IP Address    | Subnet Mask     | Default Gateway  | LoopBack       |
|:----------:|:------------------:|:-------------:|:---------------:|:----------------:|:--------------:|
| R28        | E0/0               | 93.0.93.28    | 255.255.255.192 | N/A              | 172.16.1.28/32 |
|            | E0/1               | 92.0.92.28    | 255.255.255.192 | N/A              |                |
|            | E0/2               | N/A           | N/A             | N/A              |                |
|            | E0/2.17            | 10.177.17.1   | 255.255.255.0   | N/A              |                |
|            | E0/2.18            | 10.177.18.1   | 255.255.255.0   | N/A              |                |
|            | E0/2.19            | 10.177.19.1   | 255.255.255.0   | N/A              |                |
|            |                    |               |                 |                  |                |
| SW29       | VLAN17             | 10.177.18.29  | 255.255.255.0   | 10.177.18.1      | N/A            |
|            | VLAN18             | N/A           | N/A             | N/A              | N/A            |
|            | VLAN19             | N/A           | N/A             | N/A              | N/A            |
| VPC-30     | NIC - NIC          | DHCP          | DHCP            | DHCP             |                |
| VPC-31     | NIC - NIC          | DHCP          | DHCP            | DHCP             |                |  

VLAN Таблица:  
| VLAN | NAME       | Interface Assigned Topology - EVE-NG |
|:----:|:----------:|:------------------------------------:|
| 17   | Management | SW29: E0/3                           |
| 18   | UserGroup1 | SW29: E0/0                           |
| 19   | UserGroup2 | SW29: E0/1                           |
| 3999 | Native     | N/A                                  |
|      |            |                                      |  


### 7-9:  

Пример PBR на офисной сети в Чокурдах (R28):

```
R28(config)#do sh run | s route-map
 ip policy route-map VLAN17
route-map VLAN17 permit 10
 match ip address VLAN17
 set ip next-hop 92.0.92.25
route-map VLAN18-19 permit 20
 match ip address VLAN18-19
 set ip next-hop 93.0.93.26
```  

```
R28(config)#do sh run | s access-list
ip access-list standard VLAN17
 permit 10.177.17.0 0.0.0.255
ip access-list standard VLAN18-19
 permit 10.177.18.0 0.0.1.255
```  

```
!
interface Ethernet0/2.17
 encapsulation dot1Q 17
 ip address 10.177.17.1 255.255.255.0
 ip policy route-map VLAN17
!
interface Ethernet0/2.18
 encapsulation dot1Q 18
 ip address 10.177.18.1 255.255.255.0
 ip policy route-map VLAN18-19
!
interface Ethernet0/2.19
 encapsulation dot1Q 19
 ip address 10.177.19.1 255.255.255.0
 ip policy route-map VLAN18-19
!
```  

```
R28(config)#do sh ip policy
Interface      Route map
Ethernet0/2.17 VLAN17
Ethernet0/2.18 VLAN18-19
Ethernet0/2.19 VLAN18-19
```  

```
R28(config)#do sh route-map all
STATIC routemaps
route-map VLAN17, permit, sequence 10
  Match clauses:
    ip address (access-lists): VLAN17
  Set clauses:
    ip next-hop 92.0.92.25
  Policy routing matches: 0 packets, 0 bytes
route-map VLAN18-19, permit, sequence 20
  Match clauses:
    ip address (access-lists): VLAN18-19
  Set clauses:
    ip next-hop 93.0.93.26
  Policy routing matches: 30 packets, 3060 bytes
DYNAMIC routemaps
Current active dynamic routemaps = 0
```  

```
R28(config)#do debug ip policy
```  
Запускаем трассировку с VPC30:  
```
VPC30> trace 93.0.93.26
trace to 93.0.93.26, 8 hops max, press Ctrl+C to stop
 1   10.177.18.1   0.571 ms  0.432 ms  0.374 ms
 2   *93.0.93.26   0.696 ms (ICMP type:3, code:3, Destination port unreachable)  *
```  
```
R28(config)#
*Apr 20 19:02:19.778: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 92, FIB policy match
*Apr 20 19:02:19.778: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 92, PBR Counted
*Apr 20 19:02:19.778: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, g=93.0.93.26, len 92, FIB policy routed
*Apr 20 19:02:20.779: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 92, FIB policy match
*Apr 20 19:02:20.779: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 92, PBR Counted
*Apr 20 19:02:20.779: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, g=93.0.93.26, len 92, FIB policy routed
```  

Запускаем ping с VPC30:  

```
VPC30> ping 93.0.93.26

84 bytes from 93.0.93.26 icmp_seq=1 ttl=254 time=0.738 ms
84 bytes from 93.0.93.26 icmp_seq=2 ttl=254 time=0.825 ms
84 bytes from 93.0.93.26 icmp_seq=3 ttl=254 time=0.905 ms
84 bytes from 93.0.93.26 icmp_seq=4 ttl=254 time=0.859 ms
84 bytes from 93.0.93.26 icmp_seq=5 ttl=254 time=0.941 ms
```  

```
R28(config)#do debug ip policy
*Apr 20 19:06:34.475: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, FIB policy match
*Apr 20 19:06:34.475: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, PBR Counted
*Apr 20 19:06:34.475: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, g=93.0.93.26, len 84, FIB policy routed
*Apr 20 19:06:35.477: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, FIB policy match
*Apr 20 19:06:35.477: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, PBR Counted
*Apr 20 19:06:35.477: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, g=93.0.93.26, len 84, FIB policy routed
R28(config)#do debug ip policy
*Apr 20 19:06:36.479: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, FIB policy match
*Apr 20 19:06:36.479: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, PBR Counted
*Apr 20 19:06:36.479: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, g=93.0.93.26, len 84, FIB policy routed
*Apr 20 19:06:37.480: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, FIB policy match
*Apr 20 19:06:37.480: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, PBR Counted
*Apr 20 19:06:37.480: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, g=93.0.93.26, len 84, FIB policy routed
R28(config)#do debug ip policy
*Apr 20 19:06:38.483: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, FIB policy match
*Apr 20 19:06:38.483: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, len 84, PBR Counted
*Apr 20 19:06:38.483: IP: s=10.177.18.11 (Ethernet0/2.18), d=93.0.93.26, g=93.0.93.26, len 84, FIB policy routed
``` 
Выключаем debug:  

```
R28(config)#do no debug ip policy
Policy routing debugging is off
```  

Пример SLA на офисной сети в Чокурдах (R28):  

```
!
ip sla 10
 icmp-echo 93.0.93.26 source-ip 93.0.93.28
 threshold 100
 timeout 100
 frequency 5
ip sla schedule 10 life forever start-time now
!
track 10 ip sla 10 reachability
 delay down 60 up 60
!
ip route 0.0.0.0 0.0.0.0 93.0.93.26 track 10
ip route 0.0.0.0 0.0.0.0 92.0.92.25
!
```  

```
R28(config)# do sh ip sla summary
IPSLAs Latest Operation Summary
Codes: * active, ^ inactive, ~ pending

ID           Type        Destination       Stats       Return      Last
                                           (ms)        Code        Run
-----------------------------------------------------------------------
*10          icmp-echo   93.0.93.26        RTT=1       OK          3 seconds ago
```  

```
R28(config)# do sh ip sla statistics
IPSLAs Latest Operation Statistics

IPSLA operation id: 10
        Latest RTT: 1 milliseconds
Latest operation start time: 19:48:39 UTC Thu Mar 20 2025
Latest operation return code: OK
Number of successes: 254
Number of failures: 0
Operation time to live: Forever
```  


10. Маршрут по умолчанию для офиса Лабытанги:  

```
R27(config)#do sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 200.20.2.1 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 200.20.2.1
      200.20.2.0/24 is variably subnetted, 2 subnets, 2 masks
C        200.20.2.0/24 is directly connected, Ethernet0/0
L        200.20.2.27/32 is directly connected, Ethernet0/0
```  
### 11-14:  

Настройка OSPF на R14:  
```
!
interface Ethernet0/0
 description -=To R12=-
 ip address 100.0.10.14 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/1
 description -=To R13=-
 ip address 100.0.9.14 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/3
 description -=To R19=-
 ip address 100.0.12.14 255.255.255.224
 ip ospf 1 area 101
!
interface Ethernet1/0
 ip address 100.100.100.14 255.255.255.0
 ip ospf 1 area 0
!
router ospf 1
 router-id 1.1.1.1
 area 10 stub
 area 101 stub no-summary
!
```  

Настройка OSPF на R15 с фильтрацией:
```
!
interface Ethernet0/0
 description -=To R13=-
 ip address 100.0.10.11 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/1
 description -=To R12=-
 ip address 100.0.11.11 255.255.255.240
 ip ospf 1 area 10
!
!
interface Ethernet0/3
 description -=To R20=-
 ip address 100.0.16.15 255.255.255.224
 ip ospf 1 area 102
!
interface Ethernet1/0
 ip address 100.100.100.15 255.255.255.0
 ip ospf 1 area 0
!
router ospf 1
 router-id 1.1.1.2
 area 102 filter-list prefix DENY_101_ZONE in
 !
ip prefix-list DENY_101_ZONE seq 5 deny 100.0.12.0/27
ip prefix-list DENY_101_ZONE seq 10 permit 0.0.0.0/0 le 32
!
```  
Настройка OSPF на R12 с маршрутом по умолчанию:  
```
!
interface Ethernet0/2
 description -=To R14=-
 ip address 100.0.10.12 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/3
 description -=To R15=-
 ip address 100.0.11.12 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet1/1
 description -=To R13=-
 ip address 100.99.99.13 255.255.255.0
 ip ospf 1 area 10
!
router ospf 1
 router-id 2.2.2.1
 area 10 stub
 network 100.0.10.12 0.0.0.0 area 10
 network 100.0.11.12 0.0.0.0 area 10
 ! 
```  

Настройка OSPF на R13 с маршрутом по умолчанию:  
```
!
interface Ethernet0/2
 description -=To R15=-
 ip address 100.0.10.13 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/3
 description -=To R14=-
 ip address 100.0.9.13 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet1/1
 description -=To R12=-
 ip address 100.99.99.12 255.255.255.0
 ip ospf 1 area 10
 !
router ospf 1
 router-id 2.2.2.2
 area 10 stub
 network 100.0.10.13 0.0.0.0 area 10
 network 100.0.11.13 0.0.0.0 area 10
 !
```  

Настройка OSPF на R19:  
```
!
interface Ethernet0/0
 description -=To R14=-
 ip address 100.0.12.19 255.255.255.224
 ip ospf 1 area 101
router ospf 1
 router-id 3.3.3.1
 area 101 stub
 passive-interface default
 no passive-interface Ethernet0/0
 network 100.0.12.19 0.0.0.0 area 101
!
```  
Настройка OSPF на R20:  
```
!
interface Ethernet0/0
 description -=To R15=-
 ip address 100.0.16.20 255.255.255.224
 ip ospf 1 area 102
 !
router ospf 1
 router-id 3.3.3.2
 network 100.0.16.20 0.0.0.0 area 102
!
```  
### 15:  

Настройки IS-IS в Триада (AS520):  

Настройки IS-IS R23:  
```
!
router isis TRIADA
 net 49.2222.0023.0000.00
 is-type level-2-only
 metric-style wide
 log-adjacency-changes
 !
 address-family ipv6
  multi-topology
 exit-address-family
!
interface Ethernet0/1
 description -=To R25=-
 ip address 95.0.95.23 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:95:0:95::23/64
 ipv6 router isis TRIADA
!
interface Ethernet0/2
 description -=To R24=-
 ip address 96.0.96.23 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:96:0:96::23/64
 ipv6 router isis TRIADA
!
```  
Настройки IS-IS R24:  
```
router isis TRIADA
 net 49.0024.0024.0000.00
 is-type level-2-only
 metric-style wide
 log-adjacency-changes
 !
 address-family ipv6
  multi-topology
 exit-address-family
!
interface Ethernet0/1
 description -=To R26=-
 ip address 97.0.97.24 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:97:0:97::24/64
 ipv6 router isis TRIADA
!
interface Ethernet0/2
 description -=To R23=-
 ip address 96.0.96.24 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:96:0:96::24/64
 ipv6 router isis TRIADA
!
```  
Настройки IS-IS R25:  
```
!
router isis TRIADA
 net 49.2222.0025.0000.00
 is-type level-2-only
 metric-style wide
 log-adjacency-changes
 !
 address-family ipv6
  multi-topology
 exit-address-family
!
interface Ethernet0/0
 description -=To R23=-
 ip address 95.0.95.25 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:95:0:95::25/64
 ipv6 router isis TRIADA
!
interface Ethernet0/2
 description -=To R26=-
 ip address 94.0.94.25 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:94:0:94::25/64
 ipv6 router isis TRIADA
!

```  
Настройки IS-IS R26:  
```
!
router isis TRIADA
 net 49.0026.0026.0000.00
 is-type level-2-only
 metric-style wide
 log-adjacency-changes
 !
 address-family ipv6
  multi-topology
 exit-address-family
!
interface Ethernet0/0
 description -=To R24=-
 ip address 97.0.97.26 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:97:0:97::26/64
 ipv6 router isis TRIADA
!
interface Ethernet0/2
 description -=To R25=-
 ip address 94.0.94.26 255.255.255.224
 ip router isis TRIADA
 ipv6 address 2001:94:0:94::26/64
 ipv6 router isis TRIADA
!
```  
Для настройки IPv6 не забыть ввести команду:  
```
ipv6 unicast-routing
```  
Пример проверки:  
```
R26# sh isis database

Tag TRIADA:
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R24.00-00             0x0000007E   0xC424        696               0/0/0
R24.01-00             0x0000006D   0xC0A9        680               0/0/0
R26.00-00           * 0x0000007D   0x08DD        584               0/0/0
R26.01-00           * 0x0000006D   0xDEA2        561               0/0/0
R26.02-00           * 0x0000006B   0xCD93        651               0/0/0
R23.00-00             0x00000081   0x296E        604               0/0/0
R25.00-00             0x00000080   0xFC99        520               0/0/0
R25.01-00             0x0000006E   0x9690        547               0/0/0
```  
```
R26#sh isis ip topology

Tag TRIADA:

IS-IS TID 0 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R24                  10         R24                  Et0/0       aabb.cc01.8010
R26                  --
R23                  20         R24                  Et0/0       aabb.cc01.8010
                                R25                  Et0/2       aabb.cc01.9020
R25                  10         R25                  Et0/2       aabb.cc01.9020
```  
```
R26#sh isis ipv6 topology

Tag TRIADA:

IS-IS TID 2 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R24                  10         R24                  Et0/0       aabb.cc01.8010
R26                  --
R23                  20         R24                  Et0/0       aabb.cc01.8010
                                R25                  Et0/2       aabb.cc01.9020
R25                  10         R25                  Et0/2       aabb.cc01.9020
```  
```
R26#sh ip route isis
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 93.0.93.28 to network 0.0.0.0

      95.0.0.0/27 is subnetted, 1 subnets
i L2     95.0.95.0 [115/20] via 94.0.94.25, 00:25:15, Ethernet0/2
      96.0.0.0/27 is subnetted, 1 subnets
i L2     96.0.96.0 [115/20] via 97.0.97.24, 00:25:15, Ethernet0/0
```  
```
R26#sh ipv6 route isis
IPv6 Routing Table - default - 7 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
I2  2001:95:0:95::/64 [115/20]
     via FE80::A8BB:CCFF:FE01:9020, Ethernet0/2
I2  2001:96:0:96::/64 [115/20]
     via FE80::A8BB:CCFF:FE01:8010, Ethernet0/0
```  
### 16: 

Настроить EIGRP в С.-Петербург:  

Конфигурация EIGRP R18:  

```
R18#sh run | s r e
router eigrp EIGRP-SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   no passive-interface
  exit-af-interface
  !
  topology base
   redistribute static
  exit-af-topology
  network 85.85.85.0 0.0.0.31
  network 86.86.86.0 0.0.0.31
  network 98.0.98.0 0.0.0.255
  network 99.0.99.0 0.0.0.255
  eigrp router-id 18.18.18.18
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
 exit-address-family
```  
Конфигурация EIGRP R17 (анонсируют суммарные префиксы):  

```
R17#sh run | s r e
router eigrp EIGRP-SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 86.86.86.0 255.255.255.224
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 86.86.86.0 0.0.0.31
  eigrp router-id 17.17.17.17
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 2001:86:0:86::/64
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
 exit-address-family
```  

Конфигурация EIGRP R16 (анонсируют суммарные префиксы):  

```
R16#sh run | s r e
router eigrp EIGRP-SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 85.85.85.0 255.255.255.224
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/3
   summary-address 0.0.0.0 0.0.0.0
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 84.84.84.0 0.0.0.255
  network 85.85.85.0 0.0.0.31
  eigrp router-id 16.16.16.16
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 2001:85:0:85::/64
   no passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/3
   summary-address ::/0
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
 exit-address-family
```  

Конфигурация EIGRP R32:  

```
R32#sh run | s r e
router eigrp EIGRP-SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 84.84.84.0 0.0.0.255
  eigrp router-id 32.32.32.32
  eigrp stub receive-only
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface default
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0
   no passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp stub receive-only
 exit-address-family
```  
- Маршрут по умолчанию на R32:  

```
R32# sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 84.84.84.16 to network 0.0.0.0

D*    0.0.0.0/0 [90/1536000] via 84.84.84.16, 00:07:38, Ethernet0/0
```  

Соведство напримере R16:  

```
R16# sh ip eigrp neighbors
EIGRP-IPv4 VR(EIGRP-SPB) Address-Family Neighbors for AS(2042)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q                                        Seq
                                                   (sec)         (ms)       Cnt                                       Num
1   85.85.85.18             Et0/1                    10 00:10:27 1602  5000  0                                        6
0   84.84.84.32             Et0/3                    14 00:10:27 1595  5000  0                                        1
```  
```
R16# sh ipv6 eigrp neighbors
EIGRP-IPv6 VR(EIGRP-SPB) Address-Family Neighbors for AS(2042)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q                                        Seq
                                                   (sec)         (ms)       Cnt                                       Num
1   Link-local address:     Et0/3                    13 00:10:30   10   100  0                                        1
    FE80::A8BB:CCFF:FE02:0
0   Link-local address:     Et0/1                    12 00:10:34 1601  5000  0                                        6
    FE80::A8BB:CCFF:FE01:2000
```  

Проверка маршрутов EIGRP на примере R16:  

```
R16#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 0.0.0.0 to network 0.0.0.0

D*    0.0.0.0/0 is a summary, 00:12:49, Null0
      86.0.0.0/27 is subnetted, 1 subnets
D        86.86.86.0 [90/1536000] via 85.85.85.18, 00:12:49, Ethernet0/1
      98.0.0.0/24 is subnetted, 1 subnets
D        98.0.98.0 [90/1536000] via 85.85.85.18, 00:12:49, Ethernet0/1
      99.0.0.0/24 is subnetted, 1 subnets
D        99.0.99.0 [90/1536000] via 85.85.85.18, 00:12:49, Ethernet0/1
```  
```
R16#sh ipv6 route eigrp
IPv6 Routing Table - default - 7 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
D   ::/0 [5/1024000]
     via Null0, directly connected
D   2001:86:0:86::/64 [90/1536000]
     via FE80::A8BB:CCFF:FE01:2000, Ethernet0/1
```  

Проверка связанности R32->R16->R18->R17:  

```
R32#ping 86.86.86.17
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 86.86.86.17, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```  

### 17:

Настроите eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас:  

- eBGP между Москва (R14 AS1001) и Киторн (R22 AS101):

```
R14#sh run | s bgp
router bgp 1001
 bgp log-neighbor-changes
 network 109.0.13.0 mask 255.255.255.224
 neighbor 109.0.13.22 remote-as 101
```  

```
R22#sh run | s bg
router bgp 101
 bgp log-neighbor-changes
 network 109.0.13.0 mask 255.255.255.224
 network 110.0.110.0 mask 255.255.255.224
 network 111.0.10.0 mask 255.255.255.224
 neighbor 109.0.13.14 remote-as 1001
 neighbor 110.0.110.21 remote-as 301
 neighbor 111.0.10.23 remote-as 520
```  

- eBGP между Москва (R15 AS1001) и Ламас (R22 AS301):  

```
R15#sh run | s bg
router bgp 1001
 bgp log-neighbor-changes
 network 100.0.13.0 mask 255.255.255.224
 neighbor 100.0.13.21 remote-as 301
```  

```
R21#sh run | s bg
router bgp 301
 bgp log-neighbor-changes
 network 100.0.13.0 mask 255.255.255.224
 network 110.0.110.0 mask 255.255.255.224
 network 112.0.10.0 mask 255.255.255.224
 neighbor 100.0.13.15 remote-as 1001
 neighbor 110.0.110.22 remote-as 101
 neighbor 112.0.10.24 remote-as 520
```  

- eBGP между Киторн (R22 AS101) и Ламас (R21 AS301):  

```
R22#sh run | s bg
router bgp 101
 bgp log-neighbor-changes
 network 109.0.13.0 mask 255.255.255.224
 network 110.0.110.0 mask 255.255.255.224
 network 111.0.10.0 mask 255.255.255.224
 neighbor 109.0.13.14 remote-as 1001
 neighbor 110.0.110.21 remote-as 301
 neighbor 111.0.10.23 remote-as 520
```  

```
R21#sh run | s bg
router bgp 301
 bgp log-neighbor-changes
 network 100.0.13.0 mask 255.255.255.224
 network 110.0.110.0 mask 255.255.255.224
 network 112.0.10.0 mask 255.255.255.224
 neighbor 100.0.13.15 remote-as 1001
 neighbor 110.0.110.22 remote-as 101
 neighbor 112.0.10.24 remote-as 520
```  

- eBGP между Ламас (R21 AS301) и Триада (R24 AS520):  

```
R21#sh run | s bg
router bgp 301
 bgp log-neighbor-changes
 network 100.0.13.0 mask 255.255.255.224
 network 110.0.110.0 mask 255.255.255.224
 network 112.0.10.0 mask 255.255.255.224
 neighbor 100.0.13.15 remote-as 1001
 neighbor 110.0.110.22 remote-as 101
 neighbor 112.0.10.24 remote-as 520
```  

```
R24#sh run | s bg
router bgp 520
 bgp log-neighbor-changes
 network 98.0.98.0 mask 255.255.255.0
 network 112.0.10.0 mask 255.255.255.224
 neighbor 98.0.98.18 remote-as 2042
 neighbor 112.0.10.21 remote-as 301
```  

- eBGP между Китор (R22 AS101) и Триада (R23 AS520):  

```
R22#sh run | s bg
router bgp 101
 bgp log-neighbor-changes
 network 109.0.13.0 mask 255.255.255.224
 network 110.0.110.0 mask 255.255.255.224
 network 111.0.10.0 mask 255.255.255.224
 neighbor 109.0.13.14 remote-as 1001
 neighbor 110.0.110.21 remote-as 301
 neighbor 111.0.10.23 remote-as 520
```  

```
R23#sh run | s bg
router bgp 520
 bgp log-neighbor-changes
 network 111.0.10.0 mask 255.255.255.224
 neighbor 111.0.10.22 remote-as 101
```  

- eBGP между С.-Петербург (R18 AS2042) и Триада (R24 AS520):  

```
R18#sh run | s bg
router bgp 2042
 bgp log-neighbor-changes
 network 98.0.98.0 mask 255.255.255.0
 network 99.0.99.0 mask 255.255.255.0
 neighbor 98.0.98.24 remote-as 520
 neighbor 99.0.99.26 remote-as 520
```  

```
R24#sh run | s bg
router bgp 520
 bgp log-neighbor-changes
 network 98.0.98.0 mask 255.255.255.0
 network 112.0.10.0 mask 255.255.255.224
 neighbor 98.0.98.18 remote-as 2042
 neighbor 112.0.10.21 remote-as 301
```  

- eBGP между С.-Петербург (R18 AS2042) и Триада (R26 AS520):  

```
R18#sh run | s bg
router bgp 2042
 bgp log-neighbor-changes
 network 98.0.98.0 mask 255.255.255.0
 network 99.0.99.0 mask 255.255.255.0
 neighbor 98.0.98.24 remote-as 520
 neighbor 99.0.99.26 remote-as 520
```  

```
R26#sh run | s bg
router bgp 520
 bgp log-neighbor-changes
 network 99.0.99.0 mask 255.255.255.0
 neighbor 99.0.99.18 remote-as 2042
```  

- Проверка доступности между пограничными роутерами Москва (R14 AS1001) и С.-Петербург (R18 AS2042):  

Посмотрим маршруты eBGP на этих пограничных роутерах:  

```
R14#sh ip bgp
BGP table version is 12, local router ID is 172.16.1.14
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  98.0.98.0/24     109.0.13.22                            0 101 301 520 i
 *>  99.0.99.0/24     109.0.13.22                            0 101 301 520 2042 i
 *>  100.0.13.0/27    109.0.13.22                            0 101 301 i
 *   109.0.13.0/27    109.0.13.22              0             0 101 i
 *>                   0.0.0.0                  0         32768 i
 *>  110.0.110.0/27   109.0.13.22              0             0 101 i
 *>  111.0.10.0/27    109.0.13.22              0             0 101 i
 *>  112.0.10.0/27    109.0.13.22                            0 101 301 i
```  

```
R18#sh ip bgp
BGP table version is 8, local router ID is 172.16.1.18
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *   98.0.98.0/24     98.0.98.24               0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *   99.0.99.0/24     99.0.99.26               0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *>  100.0.13.0/27    98.0.98.24                             0 520 301 i
 *>  109.0.13.0/27    98.0.98.24                             0 520 301 101 i
 *>  110.0.110.0/27   98.0.98.24                             0 520 301 i
 *>  111.0.10.0/27    98.0.98.24                             0 520 301 101 i
 *>  112.0.10.0/27    98.0.98.24               0             0 520 i
```  
Видим валидные и лучшие маршруты.  

Терепь проверим IP связанность между R14 МСК и R18 С.-Петербург:  

R14->R18:  

```
R14#ping 98.0.98.18
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 98.0.98.18, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
```
R18->R14:  

```
R18#ping 109.0.13.14
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 109.0.13.14, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
```  
Аналогично будет работать от R15 к R18 и наоброт с меньшим ASPath  

### 18:  

- Настроите iBGP в офисом Москва между маршрутизаторами R14 и R15.  

И  

- Настройте офис Москва так, чтобы приоритетным провайдером стал Ламас.  

Настройки iBGP на R14 и настройка приоритетного провайдера Ламас:  

```
!
router ospf 1
 router-id 1.1.1.1
 area 10 stub
 area 101 stub no-summary
 network 172.16.1.0 0.0.0.255 area 0
!
router bgp 1001
 bgp log-neighbor-changes
 network 100.100.100.0 mask 255.255.255.0
 network 109.0.13.0 mask 255.255.255.224
 redistribute connected
 neighbor 100.100.100.15 remote-as 1001
 neighbor 100.100.100.15 weight 300
 neighbor 109.0.13.22 remote-as 101
 neighbor 109.0.13.22 weight 100
 neighbor 172.16.1.15 remote-as 1001
 neighbor 172.16.1.15 update-source Loopback0
```  
Проверка маршрутов BGP:  

```
R14#sh ip bgp summary
BGP router identifier 172.16.1.14, local AS number 1001
BGP table version is 135066, main routing table version 135066
18 network entries using 2520 bytes of memory
46 path entries using 3680 bytes of memory
10/7 BGP path/bestpath attribute entries using 1440 bytes of memory
6 BGP AS-PATH entries using 144 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 7784 total bytes of memory
BGP activity 21/3 prefixes, 214/168 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
100.100.100.15  4         1001      85      87   135066    0    0 01:06:18       15
109.0.13.22     4          101    9458   27720   135066    0    0 3d15h          10
172.16.1.15     4         1001     131     134   135066    0    0 01:35:37       15  
####################################################################################  
R14#sh ip bgp
BGP table version is 135066, local router ID is 172.16.1.14
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>i 95.0.95.0/27     100.0.13.21              0    100    300 301 520 i
 * i                  100.0.13.21              0    100      0 301 520 i
 *                    109.0.13.22                          100 101 520 i
 *>i 96.0.96.0/27     100.0.13.21              0    100    300 301 520 i
 * i                  100.0.13.21              0    100      0 301 520 i
 *                    109.0.13.22                          100 101 520 i
 *>i 97.0.97.0/27     100.0.13.21              0    100    300 301 520 i
 * i                  100.0.13.21              0    100      0 301 520 i
 *                    109.0.13.22                          100 101 520 i
 *>i 98.0.98.0/24     100.0.13.21              0    100    300 301 520 i
 * i                  100.0.13.21              0    100      0 301 520 i
 *                    109.0.13.22                          100 101 520 i
 *>i 99.0.99.0/24     100.0.13.21              0    100    300 301 520 i
 * i                  100.0.13.21              0    100      0 301 520 i
     Network          Next Hop            Metric LocPrf Weight Path
 *                    109.0.13.22                          100 101 520 i
 *>  100.0.9.0/28     0.0.0.0                  0         32768 ?
 * i 100.0.10.0/28    100.100.100.15           0    100    300 ?
 * i                  172.16.1.15              0    100      0 ?
 *>                   0.0.0.0                  0         32768 ?
 r>i 100.0.11.0/28    100.100.100.15           0    100    300 ?
 r i                  172.16.1.15              0    100      0 ?
 *>  100.0.12.0/27    0.0.0.0                  0         32768 ?
 *>i 100.0.13.0/27    100.100.100.15           0    100    300 i
 * i                  172.16.1.15              0    100      0 i
 *                    109.0.13.22                          100 101 301 i
 r>i 100.0.16.0/27    100.100.100.15           0    100    300 ?
 r i                  172.16.1.15              0    100      0 ?
 * i 100.100.100.0/24 100.100.100.15           0    100    300 i
 * i                  172.16.1.15              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 * i 109.0.13.0/27    100.0.13.21              0    100    300 301 101 i
 * i                  100.0.13.21              0    100      0 301 101 i
 *                    109.0.13.22              0           100 101 i
 *>                   0.0.0.0                  0         32768 i
 *>i 110.0.110.0/27   100.0.13.21              0    100    300 301 i
 * i                  100.0.13.21              0    100      0 301 i
     Network          Next Hop            Metric LocPrf Weight Path
 *                    109.0.13.22              0           100 101 i
 *>i 111.0.10.0/27    100.0.13.21              0    100    300 301 101 i
 * i                  100.0.13.21              0    100      0 301 101 i
 *                    109.0.13.22              0           100 101 i
 *>i 112.0.10.0/27    100.0.13.21              0    100    300 301 i
 * i                  100.0.13.21              0    100      0 301 i
 *                    109.0.13.22                          100 101 301 i
 *>  172.16.1.14/32   0.0.0.0                  0         32768 ?
 *>i 172.16.1.15/32   100.100.100.15           0    100    300 ?
 * i                  172.16.1.15              0    100      0 ?
```  
Что бы Ламас(AS 301) стал приоритетным провайдером на R14 добавим атрибут weight(выше в конфигурации) в сторону R15, т.к. R15 выходит на провайдера Ламас.  
Проверим с R12 выключив интерфейс соединяющий R12 с R15 и проверим связанность с R23 (111.0.10.23):  

```
R12(config)# interface Ethernet0/3
R12(config-if)#shu
R12(config-if)#
*Jun 22 11:00:19.595: %LINK-5-CHANGED: Interface Ethernet0/3, changed state to administratively down
*Jun 22 11:00:20.600: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/3, changed state to down
R12(config-if)#do ping 111.0.10.23
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 111.0.10.23, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
R12(config-if)#do trace 111.0.10.23
Type escape sequence to abort.
Tracing the route to 111.0.10.23
VRF info: (vrf in name/id, vrf out name/id)
  1 100.0.10.14 1 msec 0 msec 0 msec
  2  *  *  *
  3  *  *  *
  4 110.0.110.22 1 msec 2 msec 1 msec
  5 111.0.10.23 1 msec *  3 msec
```  
В примере выше через трассировку может показаться что атрибут weight не работает, на самом деле мартшрут прошел следующим образом:  

R12(AS1001, 100.0.10.12/28)->R14(AS1001, 100.0.10.14/28)->R14(AS1001, 100.100.100.14/24)->R15(AS1001, 100.100.100.15/24)->R15(AS1001, 100.0.13.15/27)->R21(AS301, 100.0.13.21/27)->R21(AS301, 110.0.110.21/27)->R22(AS101, 110.0.110.22/27)->R22(AS101, 111.0.10.22/27)->R23(AS101, 111.0.10.23/27)  

Более наглядно моно увидеть это с R14:  
```
R14#trace 111.0.10.23
Type escape sequence to abort.
Tracing the route to 111.0.10.23
VRF info: (vrf in name/id, vrf out name/id)
  1 100.100.100.15 1 msec 0 msec 1 msec
  2 100.0.13.21 0 msec 1 msec 0 msec
  3 110.0.110.22 [AS 301] 0 msec 1 msec 1 msec
  4 111.0.10.23 [AS 101] 1 msec *  2 msec
```  
Настройки iBGP на R15:  

```
!
router ospf 1
 router-id 1.1.1.2
 area 102 filter-list prefix DENY_101_ZONE in
 network 172.16.1.0 0.0.0.1 area 0
!
router bgp 1001
 bgp log-neighbor-changes
 network 100.0.13.0 mask 255.255.255.224
 network 100.100.100.0 mask 255.255.255.0
 redistribute connected
 neighbor 100.0.13.21 remote-as 301
 neighbor 100.100.100.14 remote-as 1001
 neighbor 172.16.1.14 remote-as 1001
 neighbor 172.16.1.14 update-source Loopback0
```  

Проверка маршрутов BGP:  

```
R15#sh ip bgp summary
BGP router identifier 172.16.1.15, local AS number 1001
BGP table version is 161, main routing table version 161
18 network entries using 2520 bytes of memory
28 path entries using 2240 bytes of memory
7/6 BGP path/bestpath attribute entries using 1008 bytes of memory
3 BGP AS-PATH entries using 72 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 5840 total bytes of memory
BGP activity 35/17 prefixes, 31289/31261 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
100.0.13.21     4          301    5873    5879      161    0    0 3d16h          10
100.100.100.14  4         1001      90      88      161    0    0 01:08:59        6
172.16.1.14     4         1001     137     134      161    0    0 01:38:17        6  
########################################################################################  
R15#sh ip bgp
BGP table version is 161, local router ID is 172.16.1.15
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  95.0.95.0/27     100.0.13.21                          200 301 520 i
 *>  96.0.96.0/27     100.0.13.21                          200 301 520 i
 *>  97.0.97.0/27     100.0.13.21                          200 301 520 i
 *>  98.0.98.0/24     100.0.13.21                          200 301 520 i
 *>  99.0.99.0/24     100.0.13.21                          200 301 520 i
 r>i 100.0.9.0/28     100.100.100.14           0    100      0 ?
 r i                  172.16.1.14              0    100      0 ?
 * i 100.0.10.0/28    100.100.100.14           0    100      0 ?
 * i                  172.16.1.14              0    100      0 ?
 *>                   0.0.0.0                  0         32768 ?
 *>  100.0.11.0/28    0.0.0.0                  0         32768 ?
 r>i 100.0.12.0/27    100.100.100.14           0    100      0 ?
 r i                  172.16.1.14              0    100      0 ?
 *   100.0.13.0/27    100.0.13.21              0           200 301 i
     Network          Next Hop            Metric LocPrf Weight Path
 *>                   0.0.0.0                  0         32768 i
 *>  100.0.16.0/27    0.0.0.0                  0         32768 ?
 * i 100.100.100.0/24 100.100.100.14           0    100      0 i
 * i                  172.16.1.14              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 * i 109.0.13.0/27    100.100.100.14           0    100      0 i
 * i                  172.16.1.14              0    100      0 i
 *>                   100.0.13.21                          200 301 101 i
 *>  110.0.110.0/27   100.0.13.21              0           200 301 i
 *>  111.0.10.0/27    100.0.13.21                          200 301 101 i
 *>  112.0.10.0/27    100.0.13.21              0           200 301 i
 r>i 172.16.1.14/32   100.100.100.14           0    100      0 ?
 r i                  172.16.1.14              0    100      0 ?
 *>  172.16.1.15/32   0.0.0.0                  0         32768 ?
```  

