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
