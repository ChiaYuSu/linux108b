- [第六週筆記](#第六週筆記)
  - [Iperf 是什麼](#iperf-是什麼)
  - [Iperf 安裝](#iperf-安裝)
  - [Iperf 參數說明](#iperf-參數說明)
    - [Iperf 測試 TCP](#iperf-測試-tcp)
    - [最簡單參數實例](#最簡單參數實例)
  - [延伸學習](#延伸學習)
---
# 第六週筆記
## Iperf 是什麼
- Iperf 是一個 TCP/IP 和 UDP/IP 的性能測量工具，能夠提供網路吞吐率信息，以及震動、丟包率、最大段和最大傳輸單元大小等統計信息；從而能夠幫助我們測試網路性能，定位網路瓶頸

## Iperf 安裝
- 前面幾週其實就有用到了，mininet 裡面自帶的工具，所以不需要另外安裝

## Iperf 參數說明
- `-s`：主機以 Server 模式啟動
- `-c`：主機以 Client 模式啟動
- 
### Iperf 測試 TCP
- 範例：假設有兩台主機 h1 及 h2，h1 當作 Server、h2 當作 Client，從 Client 端到 Server 端做網路頻寬測試
    - xterm h1：`iperf -s`
    - xterm h2：`iperf -c -i 1`
        - `-i`：sec 以秒為單位顯示報告間隔
        - `1`：1 秒<br>
          <img src="Week 6\iperf.PNG" width="700px" />

### 最簡單參數實例
- xterm h1：`iperf -s`
- xterm h2：`iperf -c 10.0.0.1`<br>
    <img src="Week 6\iperf_simple.PNG" width="700px" />



## 延伸學習
1. [网络性能测试工具 Iperf 介绍](https://www.sdnlab.com/2961.html)