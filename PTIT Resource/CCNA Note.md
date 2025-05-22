# Cấu hình switch cơ bản

- enable: truy cập privilege mode
- show running-config: truy cập running config
- enable password: set password cho privilege mode ở plaintext
- enable secret: set password cho privilege mode ở secret encrypted

Sample mẫu running config

```bash
S1#show running-config
Building configuration...

Current configuration : 1107 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname S1
!
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
!
!
!
!
!
line con 0
 password letmein
 login
!
line vty 0 4
 login
line vty 5 15
 login
!
!
!
!
end
```

Những điểm lưu ý trong phần trên:

- vty là VTY line (Virtual Terminal Line) thông qua các giao thức Telnet hoặc SSH
- line vty 0 4 login: ám chỉ 4 phiên  (chưa cấu hình)
- line vty 5 15 tương tự
- Line con 0 (console line 0) là đường kết nối vật lý dùng để truy cập trực tiếp vào thiết bị Cisco thông qua cổng console

Để truy cập priviledged mode thì phải sử dụng lệnh `enable` như trên. Nhưng như thế thì không bảo mật nên phải dùng lệnh dưới để set password cho priviledged mode
```bash
S1> enable
S1# configure terminal
S1(config)# enable password c1$c0
S1(config)# exit
%SYS-5-CONFIG_I: Configured from console by console
S1#
```

# Cấu hình secret
Mặc định thì các cấu hình secret sẽ override password ví dụ như sau:
```bash
...
hostname S1
!
!
enable secret 5 $1$mERr$ILwq/b7kc.7X/ejA4Aosn0
enable password c1$c0
```
- Khi sử dụng `enable` với password là `c1$c0` sẽ không thể truy cập được
- Sử dụng lệnh password-encryption để set mã hóa mặc định cho password
```bash
S1# config t
S1(config)# service password-encryption
S1(config)# exit
```
- Vậy tại sao lại dùng secret thay vì password trong khi đã có encryption. Câu trả lời là **mức độ bảo mật secret cao hơn nhiều so với encryption**
# Secret với username/password
```bash
Router(config)# hostname Router1
Router1(config)# ip domain-name ccna-lab.com
Router1(config)# username SSHadmin secret 55Hadm!n
Router1(config)# crypto key generate rsa modulus 1024
```
# Config static và default route
## Phân biệt default static route và static route

- static route là tuyến đường tĩnh - chỉ cho 1 số route cụ thể
- default static route là tuyến đường tĩnh cho tất cả traffic
- default ip và subnet-mask cho ipv4 là `0.0.0.0 0.0.0.0`
- default ipv6 là `::/0`
## Cấu hình cơ bản

```bash
# Structure
ip route [destination-network] [subnet-mask] [next-hop-ip/interface] [admin-distance]
# Example
# Using next hop ip
ip route 192.168.2.0 255.255.255.0 192.168.1.1
# Using interface ( meaning trafic out interface )
ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/0
```
## Directly Connected là gì?

- Là kết nối trực tiếp thông qua interface thay vì next hop ip
- trường hợp đổi next hop ip thì vẫn hoạt động bt
## Administrative distance là gì?
- giá trị ưu tiên của tuyến đường
- càng thấp càng ưu tiên
- mặc định static route là 1=
## IPv6
```bash
ipv6 route [destination-network/prefix-length] [next-hop-ipv6 | interface]
ipv6 route 2001:DB8::/64 2001:DB8:1::1
ipv6 route 2001:DB8::/64 GigabitEthernet0/0
```
## Listing route table

```bash
# Hiển thị IPv4 routing table
show ip route

# Hiển thị IPv6 routing table
show ipv6 route

# Hiển thị chi tiết static route
show ip route static

# Hiển thị route table ngắn gọn
show ip route summary

```