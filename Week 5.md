- [第五週筆記](#第五週筆記)
  - [第一個實驗 -- 將主機配置為路由器](#第一個實驗----將主機配置為路由器)
    - [拓樸架構](#拓樸架構)
    - [程式碼](#程式碼)
    - [執行結果](#執行結果)
  - [延伸學習](#延伸學習)
---
# 第五週筆記
## 第一個實驗 -- 將主機配置為路由器
### 拓樸架構
h1 <-> h2 <-> h3
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
<img src="Week 5\h2_ifconfig.PNG" width="550px" />


## 延伸學習
1. [Mininet Operations](http://csie.nqu.edu.tw/smallko/sdn/mininet-operations.htm)