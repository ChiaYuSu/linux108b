- [第六週筆記](#第六週筆記)
  - [Iperf 是什麼](#iperf-是什麼)
  - [Iperf 安裝](#iperf-安裝)
  - [Iperf 參數說明](#iperf-參數說明)
    - [Iperf 測試 TCP](#iperf-測試-tcp)
    - [最簡單參數實例](#最簡單參數實例)
    - [雙向頻寬測試（`-r`、tradeoff）](#雙向頻寬測試-rtradeoff)
    - [同步雙向頻寬測試（`-d`、dualtest）](#同步雙向頻寬測試-ddualtest)
  - [Gnuplot 畫圖工具](#gnuplot-畫圖工具)
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
- Iperf Client 端連接 Iperf 伺服器的 TCP 默認端口為 5001，若需要更改埠號可以使用 `-p` 參數做修改
- 下圖結果顯示的頻寬是從 Client 端到 Server 端之間的頻寬，可以從圖中得知 xterm h1 及 xterm h2 Banwidth 值是一樣的
- xterm h1：`iperf -s`
- xterm h2：`iperf -c 10.0.0.1`<br>
    <img src="Week 6\iperf_simple.PNG" width="700px" />

### 雙向頻寬測試（`-r`、tradeoff）
- xterm h1：`iperf -s`
- xterm h2：`iperf -c 10.0.0.1 -r`
    - `-r`：可以測量雙向頻寬，Iperf 伺服器會主動向 Client 端發起連接，過程為先測試傳送再測試接收頻寬<br>
        <img src="Week 6\bidirect.PNG" width="700px" />

### 同步雙向頻寬測試（`-d`、dualtest）
- xterm h1：`iperf -s`
- xterm h2：`iperf -c 10.0.0.1 -d`
    - `-d`：可以同步測量雙向頻寬，過程為雙方同時測試傳送與接收頻寬<br>
        <img src="Week 6\dualtest.PNG" width="700px" />

## Gnuplot 畫圖工具

## 延伸學習
1. [网络性能测试工具 Iperf 介绍](https://www.sdnlab.com/2961.html)
2. [Iperf 使用說明](https://m1016c.pixnet.net/blog/post/145780230)