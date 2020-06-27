# 期末報告
## 主題
- 如何預防 Nmap 掃描的 DDoS 攻擊

## Nmap 是什麼（What is Nmap）
- Nmap 是⼀款用於網路探測掃描的網路安全工具， 核心功能有**主機發現、埠掃描、主機版本偵測、作業系統偵測、防火牆 / IDS 規避和哄騙、NSE 指令碼引擎**等
    - 主機發現：用於發現目標主機是否處於活動狀態
    - 埠掃描：用於掃描主機上的埠狀態
    - 主機板本偵測：用於辨識埠上執行的應用程式與程式版本
    - 作業系統偵測：用於便是目標主機的作業系統詳細資訊
    - 防火牆 / IDS 規避和哄騙：秘密地探查目標主機的狀況
    - NSE 指令碼引擎：可以用於增強以上幾種功能

## Nmap 指令介紹（Intro to Nmap command）
- nmap 指令：`nmap [Scan Types] [Options] {Target}`
    - Scan Types：掃描類型（e.g.: TCP SYN port scan）
    - Options：選項（e.g.: scan 80 port）
    - Target：目標主機 IP 位置（e.g.: 192.168.1.1）
    - Example：
        | 範例 | Scan Types | Options | target |
        | ---- | ---- | ---- | ---- |
        | TCP SYN port scan / scan port 80 / 192.168.1.1 | `-sS` | `-p 80` | `192.168.1.1` |
        | TCP connect port scan / insane speed scan / 192.168.1.2 | `-sT` | `-T5` | `192.168.1.2` |
        | UDP port scan / nse script scan / 192.168.1.3 | `-sU` | `-sC` | `192.168.1.3` |
        | TCP ACK port scan / detect OS / 192.168.0.0/16 | `-sA` | `-O` | `192.168.0.0/16` | 


## 背景知識（Background knowledge）
### TCP / IP 旗標介紹
| Flag | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 | Decimal |
| ---- | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | ----: |
| CWR（Congestion Window Reduced） | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 128 |
| ECE（ECN-Echo） | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 64 |
| URG（Urgent） | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 32 |
| ACK（Acknowledgement） | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 16 |
| PSH（Push） | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 8 |
| RST（Reset） | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 4 |
| SYN（Synchronization） | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 2 |
| FIN（Finish） | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 |

- URG（Urgent）：通知接收方此為緊急封包，應優先處理
    - 旗標值（flags）：32
- ACK（Acknowledgement）：回應對方封包已收到
    - 旗標值（flags）：16
- PSH（Push）：請求接收方將 buffer 中資料送交應用程式處理
    - 旗標值（flags）：8
- RST（Reset）終止此回合的連線
    - 旗標值（flags）：4
- SYN（Synchronize）：每回合連線請求時，第一個請求的封包
    - 旗標值（flags）：2
- FIN（Finish）：本回合連線傳送完成
    - 旗標值（flags）：1

### 正常情況 TCP / IP 三向交握示意圖
- 架構示意圖 
    <br>
    <img src="project\nmap_presentation_15.jpg" width="550px" />
- 步驟（B 主機端口有開啟）
    1. A 主機 -> **SYN** -> B 主機
    2. B 主機 -> **SYN + ACK** -> A 主機
    3. A 主機 -> **ACK** -> B 主機
    4. A 主機 <- **建立連線** -> B 主機

### 不同模式的端口掃描之回應結果
<img src="project\nmap_presentation_16.jpg" width="550px" />

- Stealth Scan：隱密掃描（又稱秘密掃描），可規避 **IDS（入侵檢測系統 Intrusion-detection system）**

## 如何預防攻擊（Prevent DDoS attack）
### TCP RST scan
<img src="project\nmap_presentation_18.jpg" width="550px" />

- 步驟（B 主機端口有開啟）
    1. A 主機 -> **SYN** -> B 主機
    2. B 主機 -> **SYN + ACK** -> A 主機
    3. A 主機 -> **RST** -> B 主機
- 規則檔（controller.py）
    ```python
    # anti SYN port scan 
    if (val1-val2>=3) and (TCP in pkt) and pkt[TCP].flags==2:   
      src = pkt.sprintf('{IP:%IP.src%}')   
      if src not in blockip:   
      self.controllers["s1"].table_add("block_pkt", "_drop", [str(src)], [])   
      blockip.append(src)
    ```
- [Demo 影片]()

### TCP FIN scan
<img src="project\nmap_presentation_21.jpg" width="550px" />

- 步驟（B 主機端口有開啟）
    1. A 主機 -> **FIN** -> B 主機
- 規則檔（controller.py）
    ```python
    # anti FIN port scan 
    if (val3>=3) and (vals<3) and (TCP in pkt) and pkt[TCP].flags==1:          
      src = pkt.sprintf('{IP:%IP.src%}')   
      if src not in blockip:   
      self.controllers["s1"].table_add("block_pkt", "_drop", [str(src)], [])   
      blockip.append(src)
    ```
- [Demo 影片]()

### TCP XMAS scan
<img src="project\nmap_presentation_24.jpg" width="550px" />

- 步驟（B 主機端口有開啟）
    1. A 主機 -> **XMAS（URG + PSH + FIN）** -> B 主機
- 規則檔（controller.py）
    ```python
    # anti XMAS port scan = FIN + PSH + URG 
    if (val4>=3) and (vals<3) and (TCP in pkt) and pkt[TCP].flags==41:    
      src = pkt.sprintf('{IP:%IP.src%}')   
      if src not in blockip:   
      self.controllers["s1"].table_add("block_pkt", "_drop", [str(src)], [])   
      blockip.append(src)
    ```
- [Demo 影片]()

### TCP NULL scan
<img src="project\nmap_presentation_27.jpg" width="550px" />

- 步驟（B 主機端口有開啟）
    1. A 主機 -> **NULL（No Flags）** -> B 主機
- 規則檔（controller.py）
    ```python
    # anti NONE port scan 
    if (val5>=3) and (TCP in pkt) and pkt[TCP].flags=="":    
      src = pkt.sprintf('{IP:%IP.src%}')   
      if src not in blockip:   
      self.controllers["s1"].table_add("block_pkt", "_drop", [str(src)], [])   
      blockip.append(src)
    ```
- [Demo 影片]()

### TCP Custom scan
<img src="project\nmap_presentation_30.jpg" width="550px" />

- 步驟（B 主機端口有開啟）
    1. A 主機 -> **Custom（All Flags）** -> B 主機
- 規則檔（controller.py）
    ```python
    # anti ALL(FSPAU) port scan 
    if (val6>=3) and (TCP in pkt) and pkt[TCP].flags==59:    
      src = pkt.sprintf('{IP:%IP.src%}')   
      if src not in blockip:   
      self.controllers["s1"].table_add("block_pkt", "_drop", [str(src)], [])   
      blockip.append(src)
    ```
- [Demo 影片]()

## 簡報參考
- [Nmap 網路安全工具 / 網路分析模擬期末報告]()

## 延伸學習
1. [NMAP.ORG / Nmap Security Scanner](https://nmap.org/)
2. [ edureka! / A Complete Guide to Nmap – Nmap Tutorial](https://www.edureka.co/blog/nmap-tutorial/)
3. [sdn-smallko / Anti TCP SYN Port Scan](http://csie.nqu.edu.tw/smallko/sdn/anti_tcp_syn_port_scan.htm)
4. [Noun Project / Icons for everything](https://thenounproject.com/)
5. [新精讚 / NMAP 掃描⽅式說明](http://n.sfs.tw/content/index/10505)
6. [軟體品管思維 / 網路偵察與弱點偵測 NMAP 常⽤指令](https://www.qa-knowhow.com/?p=3717)
7. [Kali-Linux 滲透測試⼯具 (Ver. III)](https://www.books.com.tw/products/0010843942)
8. [網管⼈雜誌-胡凱智 / 無類別區隔路由 CIDR 技術依需求善用有限 IP 位址](https://www.netadmin.com.tw/netadmin/zh-tw/technology/0B9B631F987A45439061B6629F63DD07)
9. [每⽇頭條 / Nmap的SYN、Connect、Null、FIN、Xmas、Maimon、ACK掃描](https://kknews.cc/zh-tw/science/66grpqv.html)
