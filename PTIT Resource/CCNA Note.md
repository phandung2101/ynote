> https://itexamanswers.net/14-3-5-packet-tracer-basic-router-configuration-review-instructions-answer.html
# Cấu hình switch cơ bản

- enable: truy cập privilege mode
- show running-config: truy cập running config
	- sử dụng `show running-config | include ip route` dùng include để lọc các dòng
- enable password: set password cho privilege mode ở plaintext
- enable secret: set password cho privilege mode ở secret encrypted

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
# Cách kết nối đến switch với console line
- dùng cáp console cắm thẳng từ PC -> switch
- dùng terminal kết nối đến switch từ PC
# Cấu hình địa chỉ IP cho switch/router
```bash
interface vlan 1 
ip address 128.107.20.10 255.255.255.0 
// phải cấu hình default gateway
ip default-gateway {ip-router} {subnet-mask}
no shutdown 
end
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
# Cách kiểm tra định tuyến
Trên từng máy vật lý có thể sử dụng lệnh
```bash
# tracert {target_ip}
PC> tracert 2001:db8:f:f::10

Tracing route to 2001:db8:f:f::10 over a maximum of 30 hops: 

  1   1 ms      0 ms      0 ms      2001:DB8:1:10::1
  2   0 ms      0 ms      1 ms      2001:DB8:A:2::1
  3   13 ms     0 ms      1 ms      2001:DB8:F:F::10

Trace complete.
```
# OSPF
Là Open Shortest Path First là 1 giao thức định tuyến động (dynamic routing protocol) thuộc nhóm IGP (Interior Gateway Protocol)
> Đọc kĩ thêm phần này chứ không hiểu gì
## Cách cấu hình 
```bash
# Kích hoạt OSPF với process ID là 1 (ID này chỉ có ý nghĩa nội bộ).
Router(config)# router ospf <process-id>
# Cài đặt ID cho router hiện tại (ví dụ 1.1.1.1 chẳng hạn)
Router(config-router)# router-id <rid>

Router(config-if)# **ip ospf** process-id **area** area-id
```
# VLAN Config
## Cấu hình trên từng router
```bash
// Hiển thị cấu hình vlan
S1#show vlan brief
// Add vlan và name
S1#(config)# vlan 10
S1#(config-vlan)# name Faculty/Staff
```
Cấu hình trong mạng có sử dụng VoIP. Đơn giản là ưu tiên cho các vlan này hơn.
```bash
// Config through IP Phone
S3(config)# interface f0/11
// Bật chế độ ưu tiên
S3(config-if)# mls qos trust cos
S3(config-if)# switchport voice vlan 150
```
Có thể dùng trunk mode để cấu hình trung gian
```bash
// Config switch trung gian nhằm chuyển tiếp traffic
S3(config)# interface FastEthernet0/2 
S3(config-if)# switchport mode trunk 
S3(config-if)# switchport trunk allowed vlan 10
 
// Hiển thị trunk 
S2# show interfaces trunk
```
Native trunk vlan chẳng hạn
```bash
switchport trunk native vlan 99
```
# Wifi config
```bash
// Sử dụng command-line trên pc để renew IP
ipconfig /renew
```
