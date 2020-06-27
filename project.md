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

| Flag | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 | Decimal |
| ---- | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | ----: |
| CWR（Congestion Window Reduced） | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 128 |
| ECE（ECN-Echo） | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 64 |
| URG（Urgent） | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 32 |
| ACK（Acknowledgement） | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 16 |

## 簡報參考
* [Saltstack 期末報告](https://drive.google.com/open?id=1IhvsjYjrs7-ePL-La_P9rcQAjdg67VOU)

## 延伸學習
1. [NMAP.ORG / Nmap Security Scanner](https://bit.ly/3dkrVnR)
2. [ edureka! / A Complete Guide to Nmap – Nmap Tutorial](bit.ly/3dlovBm)
3. [sdn-smallko / Anti TCP SYN Port Scan](bit.ly/3elYOlu)
4. [Noun Project / Icons for everything](bit.ly/3euco6E)
5. [新精讚 / NMAP 掃描⽅式說明](bit.ly/2zSYagv)
6. [軟體品管思維 / 網路偵察與弱點偵測 NMAP 常⽤指令](bit.ly/2zXi224)
7. [Kali-Linux 滲透測試⼯具 (Ver. III)](bit.ly/3hJBeBr)
8. [網管⼈雜誌-胡凱智 / 無類別區隔路由 CIDR 技術依需求善用有限 IP 位址](bit.ly/3eqQ9OE)
9. [每⽇頭條 / Nmap的SYN、Connect、Null、FIN、Xmas、Maimon、ACK掃描](bit.ly/3hU57PE)
