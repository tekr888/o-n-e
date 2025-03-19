# Implement DHCPv4  

## Задачи:  
 1. Создать сеть на основе топологии и произвести базовые настройки;  
 2. Настроить 2 DHCPv4 сервера на R1;  
 3. Настроить DHCP Relay на R2 и проверить.  

# Решение:  
  1. Топология из задания:  
  ![](topology3.png)  
  Топология созданная в EVE-NG:  
  ![](eve-ng3.png)  

   Таблица адресов:  
| Device     | Interface Topology - EVE-NG  | IP Address   | Subnet Mask     | Default Gateway  |
|:----------:|:----------------------------:|:------------:|:---------------:|:----------------:|
| R1         | G0/0/0/ - E0/0               | 10.0.0.1     | 255.255.255.252 | N/A              |
|            | G0/0/1 - E0/1                | N/A          | N/A             | N/A              |
|            | G0/0/1.100 - E0/1.100        | N/A          | N/A             | N/A              |
|            | G0/0/1.200 - E0/1.200        | N/A          | N/A             | N/A              |
|            | G0/0/1.1000 - E0/1.1000      | N/A          | N/A             | N/A              |
| R2         | G0/0/0 - E0/0                | 10.0.0.2     | 255.255.255.252 | N/A              |
| R2         | G0/0/1 - E0/1                | N/A          | N/A             | N/A              |
| S1         | VLAN200 - VLAN200            | N/A          | N/A             | N/A              |
| S2         | VLAN1 - VLAN1                | N/A          | N/A             | N/A              |
| PC-A       | NIC - NIC                    | DHCP         | DHPC            | DHCP             |
| PC-B       | NIC - NIC                    | DHCP         | DHCP            | DHCP             |  

VLAN Таблица:  

| VLAN | NAME       | Interface Assigned Topology - EVE-NG                              |
|:----:|:----------:|:-----------------------------------------------------------------:|
| 1    | N/A        | S2: F0/18 - E4/1                                                  |
| 100  | Clients    | S1: F0/6 - E1/0                                                   |
| 200  | Management | S1: VLAN200 - VLAN200                                             |
| 999  | Parking_Lot| S1: F0/1-4 - E0/0-3, F0/7-24 - E1/2-5/3                           |
| 1000 | Native     | N/A                                                               |  

Trunk Таблица:  

| Device |Trunk Interface Topology - EVE-NG |
|:------:|:--------------------------------:|
| S1     | F0/5 - E1/0                      |
| S2     | F0/5 - E1/0                      |  

