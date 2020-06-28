- [第四週筆記](#第四週筆記)
  - [mininet 指令介紹](#mininet-指令介紹)
  - [延伸學習](#延伸學習)
---
# 第四週筆記
## mininet 指令介紹
1. 開始 mininet：`mn`
   - 由下圖可看出此基本網路拓樸中包含兩個虛擬 host（h1、h2）、一個 switch（s1）與一個　controller（c0）<br>
      <img src="Week 4\mininet_mn.PNG" width="550px" />
2. 查看 mininet 環境下所有可用指令：`help`<br>
   <img src="Week 4\mininet_help.PNG" width="550px" />
3. 顯示所有網路的連接情況：`net`<br>
   <img src="Week 4\mininet_net.PNG" width="550px" />
4. 列出所有節點：`nodes`<br>
   <img src="Week 4\mininet_nodes.PNG" width="550px" />
5. 列出所有節點的連線狀態：`links`<br>
   <img src="Week 4\mininet_links.PNG" width="550px" />
6. 查看各節點的訊息：`dump`<br>
   <img src="Week 4\mininet_dump.PNG" width="550px" />
7. 查看交換機連接 ports：`ports`<br>
   <img src="Week 4\mininet_ports.PNG" width="550px" />
8. 測試 h1 節點 ping h2 節點：`h1 ping -c 3 h2`<br>
   <img src="Week 4\mininet_ping.PNG" width="550px" />
9.  叫出兩個節點的命令視窗：`xterm h1 h2`
   - 若無反應代表環境未安裝 xterm，需先安裝：開啟一個新的 terminal -> `su` -> `apt-get install xterm`<br>
      <img src="Week 4\mininet_xterm.PNG" width="550px" />
11. 安裝 wireshark 觀察封包轉發情形：開啟一個新的 terminal -> `su` -> `apt-get install wireshark`<br>
   <img src="Week 4\mininet_wireshark.PNG" width="550px" />
11. 開啟 wireshark 選擇 s1-eth1，h2 節點 ping h1 節點，觀察封包轉發情形
   - xterm h1：`ifconfig`（得知 IP 為 10.0.0.1）
   - xterm h2：`ifconfig`（得知 IP 為 10.0.0.2）
   - xterm h2：`h2 ping -c 4 h1`（h2 ping h1 四次）<br>
      <img src="Week 4\mininet_wireshark_ping.PNG" width="550px" />
11. 離開 mininet：`exit`<br>
   <img src="Week 4\mininet_exit.PNG" width="550px" />


## 延伸學習
1. [Lab 1-mininet 介紹、安裝與使用方法](https://sites.google.com/site/sdnruantidingyiwanglu/vmware-xia-zai-yu-an-zhuang/mininet)
2. [【Mininet指令介紹】](https://ting-kuan.blog/2017/11/09/%E3%80%90mininet%E6%8C%87%E4%BB%A4%E4%BB%8B%E7%B4%B9%E3%80%91/)