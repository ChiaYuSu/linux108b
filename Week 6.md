- [第六週筆記](#第六週筆記)
  - [Iperf 是什麼](#iperf-是什麼)
  - [Iperf 安裝](#iperf-安裝)
  - [Iperf 參數說明](#iperf-參數說明)
    - [Iperf 測試 TCP](#iperf-測試-tcp)
    - [最簡單參數實例](#最簡單參數實例)
    - [雙向頻寬測試（`-r`、tradeoff）](#雙向頻寬測試-rtradeoff)
    - [同步雙向頻寬測試（`-d`、dualtest）](#同步雙向頻寬測試-ddualtest)
  - [Gnuplot 畫圖工具](#gnuplot-畫圖工具)
  - [Iperf 結合 Gnuplot 將測試結果畫圖表示](#iperf-結合-gnuplot-將測試結果畫圖表示)
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
- gnuplot 是一套跨平臺的數學繪圖自由軟體。使用命令列介面，可以繪製數學函數圖形，也可以從純文字檔讀入簡單格式的座標資料，繪製統計圖表等等
- 它可以提供多種輸出格式，例如 PNG，SVG，PS，HPGL，供文書處理、簡報、試算表使用。它並不是統計軟體或數學軟體
    <img src="Week 6\gnuplot.PNG" width="700px" />

## Iperf 結合 Gnuplot 將測試結果畫圖表示
1. 初始化 mininet 最小拓樸結構：`mn`
2. 在 mininet 終端打開 h1、h2 主機 xterm：`xterm h1 h2`
3. 將 h1 主機當作 Server 端，並將結果保存到文件 result 中：`iperf -s -i 1 > result`
4. 將 h2 主機當作 Client 端，連接到 h1（IP 為 10.0.0.1）：`iperf -c 10.0.0.1`
5. 經過 10 秒後，在 h2 自動生成了文件 result，儲存此次連接的訊息，在 h2 終端中查看 result 內容：`cat result`
    <img src="Week 6\result.PNG" width="700px" />
6. 將 result 中我們感興趣的訊息提取到新的文件 new_result 中：`cat result | grep sec | head -10 | tr - " " | awk '{print $4,$8}' > new_result`
7. 查看 new_result 內容：`cat new_result`
    <img src="Week 6\new_result.PNG" width="700px" />
8. 接下來使用 gnuplot 畫圖（如果尚未安裝 gnuplot，可以使用以下指令安裝：`sudo apt-get install gnuplot-x11`）
9. 在 xterm h2 進入 gnuplot：`gnuplot`
10. 在 gnuplot 命令行中，將剛才得到的文件 new_result 畫圖：`plot "new_result" title "tcp flow" with linespoints`
    <img src="Week 6\tcp_flow.PNG" width="700px" />
11. 將縱座標範圍改爲40-60，添加橫縱座標標籤，並重新作圖
    ```python
    set yrang [40:60]
    set xlabel "time (sec)"
    set ylabel "tcp throughput (Mbps)"
    replot
    ```

## 延伸學習
1. [网络性能测试工具 Iperf 介绍](https://www.sdnlab.com/2961.html)
2. [Iperf 使用說明](https://m1016c.pixnet.net/blog/post/145780230)
3. [在 mininet 中測試 TCP、UDP 帶寬並作圖](https://www.twblogs.net/a/5b7eaae32b717767c6ab0db0)