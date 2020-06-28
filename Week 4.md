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
3. 查看各鏈結訊息：`net`<br>
   <img src="Week 4\mininet_net.PNG" width="550px" />
4. 查看各節點的訊息：`dump`<br>
   <img src="Week 4\mininet_dump.PNG" width="550px" />
5. 測試 h1 節點 ping h2 節點：`h1 ping -c 3 h2`<br>
   <img src="Week 4\mininet_ping.PNG" width="550px" />
6. 叫出兩個節點的命令視窗：`xterm h1 h2`
   - 若無反應代表環境未安裝 xterm，需先安裝：開啟一個新的 terminal -> `su` -> `apt-get install xterm`<br>
      <img src="Week 4\mininet_xterm.PNG" width="550px" />
7. 離開 mininet：`exit`<br>
   <img src="Week 4\mininet_exit.PNG" width="550px" />


## 延伸學習
1. []()