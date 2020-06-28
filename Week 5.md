- [第五週筆記](#第五週筆記)
  - [第一個實驗 -- 將主機配置為路由器](#第一個實驗----將主機配置為路由器)
    - [拓樸架構](#拓樸架構)
    - [程式碼](#程式碼)
    - [執行結果](#執行結果)
  - [第二個實驗 -- 啟動 DHCP 伺服器](#第二個實驗----啟動-dhcp-伺服器)
    - [拓樸架構](#拓樸架構-1)
    - [程式碼](#程式碼-1)
    - [執行結果](#執行結果-1)
  - [第三個實驗 -- 在 h2 添加 NAT 功能](#第三個實驗----在-h2-添加-nat-功能)
    - [拓樸架構](#拓樸架構-2)
    - [程式碼](#程式碼-2)
    - [執行結果](#執行結果-2)
  - [延伸學習](#延伸學習)
---
# 第五週筆記
## 第一個實驗 -- 將主機配置為路由器
### 拓樸架構
h1 <-> h2 <-> h3（h2 將被配置為路由器）
### 程式碼
```python
#!/usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink,Intf

if '__main__' == __name__:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  Link(h1, h2)
  Link(h2, h3, intfName1='h2-eth1')
  net.build()
  h2.cmd('ifconfig h2-eth0 192.168.10.1 netmask 255.255.255.0')
  h2.cmd('ifconfig h2-eth1 192.168.20.1 netmask 255.255.255.0')
  h2.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward")
  h1.cmd("ifconfig h1-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  h1.cmd("ip address add 192.168.10.2/24 dev h1-eth0")
  h1.cmd("ip route add default via 192.168.10.1 dev h1-eth0")
  h3.cmd("ip address add 192.168.20.2/24 dev h3-eth0")
  h3.cmd("ip route add default via 192.168.20.1 dev h3-eth0")
  CLI(net)
  net.stop()
  ```
### 執行結果
<img src="Week 5\h2_ifconfig.PNG" width="550px" /><br>
<img src="Week 5\h1_ping_h2.PNG" width="550px" />

## 第二個實驗 -- 啟動 DHCP 伺服器
### 拓樸架構
h1 <-> h2 <-> h3（h2 將被配置為路由器。另外，DHCP 伺服器在 h2 上運行）
### 程式碼
- 實驗之前，請使用 `sudo apt-get install isc-dhcp-server` 指令在 Ubuntu 中安裝 DHCP 伺服器
```python
#!/usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink,Intf
 
if '__main__' == __name__:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  Link(h1, h2, intfName1='h1-eth0', intfName2='h2-eth0')
  Link(h2, h3, intfName1='h2-eth1', intfName2='h3-eth0')
  net.build()
  h2.cmd('ifconfig h2-eth0 192.168.10.1 netmask 255.255.255.0')
  h2.cmd('ifconfig h2-eth1 192.168.20.1 netmask 255.255.255.0')
  h2.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward")
  h2.cmd("service isc-dhcp-server restart &")
  h1.cmd("ifconfig h1-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  h1.cmd("dhclient h1-eth0")
  h3.cmd("dhclient h3-eth0")
  CLI(net)
  net.stop()
```
- 在運行 mininet 腳本之前，我們必須配置 DHCP 伺服器。在 `/etc/dhcp` 下編輯 `dhcpd.conf`：`gedit /etc/dhcp/dhcpd.conf &`，編譯內容如下
```python
#!/usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink,Intf

if '__main__' == __name__:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  Link(h1, h2, intfName1='h1-eth0', intfName2='h2-eth0')
  Link(h2, h3, intfName1='h2-eth1', intfName2='h3-eth0')
  net.build()
  h2.cmd('ifconfig h2-eth0 192.168.10.1 netmask 255.255.255.0')
  h2.cmd('ifconfig h2-eth1 192.168.20.1 netmask 255.255.255.0')
  h2.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward")
  h2.cmd("service isc-dhcp-server restart &")
  h1.cmd("ifconfig h1-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  h1.cmd("dhclient h1-eth0")
  h3.cmd("dhclient h3-eth0")
  CLI(net)
  net.stop()
```
### 執行結果
<img src="Week 5\dhcp.jpg" width="550px" />

## 第三個實驗 -- 在 h2 添加 NAT 功能
### 拓樸架構
h1 <-> h2 <-> h3（h2 將被配置為路由器。此外，使用 iptables 使 h2 具有 NAT 功能）
### 程式碼
```python
#!/usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink,Intf

if '__main__' == __name__:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  Link(h1, h2, intfName1='h1-eth0', intfName2='h2-eth0')
  Link(h2, h3, intfName1='h2-eth1', intfName2='h3-eth0')
  net.build()
  h2.cmd('ifconfig h2-eth0 192.168.10.1 netmask 255.255.255.0')
  h2.cmd('ifconfig h2-eth1 192.168.20.1 netmask 255.255.255.0')
  h2.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward")
  h2.cmd("iptables -t nat -A POSTROUTING -o h2-eth1 -s 192.168.10.0/24 -j MASQUERADE")
  h2.cmd("service isc-dhcp-server restart &")
  h1.cmd("ifconfig h1-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  h1.cmd("dhclient h1-eth0")
  h3.cmd("dhclient h3-eth0")
  CLI(net)
  net.stop()
```

### 執行結果
<img src="Week 5\nat.jpg" width="550px" />

## 延伸學習
1. [Mininet Operations](http://csie.nqu.edu.tw/smallko/sdn/mininet-operations.htm)