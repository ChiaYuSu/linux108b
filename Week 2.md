# 第二週筆記
## OpenFlow 是什麼
<img src="Week 1\SDN_architecture.jpg" width="550px" />

- OpenFlow 是一種網路通訊協定，屬於**資料鏈路層**，能夠控制網路交換器或路由器的轉發平面（Forwarding plane），藉此改變網路封包所走的網路路徑
- OpenFlow 被認為是第一個軟體定義網路（SDN）標準之一。它最初在 SDN 環境中定義了通信協定，使 SDN **控制器**能夠與物理和虛擬的交換機或路由器等**網路裝置**的轉發平面直接進行互動，從而更好地適應不斷變化的業務需求

## OpenFlow 協定重要元件
<img src="Week 2\openflow.png" width="300px" />

* 在 OpenFlow 交換器中處理或轉送封包皆由 Flow Table 中的規則決定，而網路管理者可利用控制器（Controller）新增、刪除或修改來管理交換器中的 Flow Table 的規則內容
* 當控制器與 OpenFlow 交換器連接成功後，彼此間透過 Control Channel 建立安全通道（Secure Channel）確保 OpenFlow 協定溝通的機密性
* OpenFlow 交換器基本上包含三個部分
    * OpenFlow Protocol：控制器和 OpenFlow 交換器彼此溝通的開放協定
    * Flow Table：規則表（Flow Rule）
    * Control Channel（Secure Channel）：交換器和控制器之間建立的安全通道，用來讓控制器下達命令給交換器，或者傳遞訊息以及轉送封包

### Flow Table 簡介
<img src="Week 2\flow_table.png" width="550px" />

- 當網路封包抵達網路交換機時，就會開始根據 Flow Table 中的內容嘗試去比對符合的條件，**Flow Table 中的每一筆資料是有前後優先順序**的，而 **Flow Table 各個表格之間也會有優先順序**。一旦找到相符的 Flow Entry（封包轉送規則），就執行 Flow Entry 中所設定好的動作指令

<img src="Week 2\flow_processing.png" width="500px" />

- 並不是每個封包都可以找到相符的 Flow Entry，所以每張 Flow Table 都必須要有 Table-miss Flow Entry，其目的就是用來告訴交換器該如何處理這些為比對到 Flow Entry 的封包
    - 如果在某一個 Flow Table 中都沒有找到符合的 Flow Entry，則會根據預先設定好的預設值執行動作指令，這預先設定好的動作可能是尋找下一個 Flow Table 的內容，或是直接丟棄（這是預設動作），或者回傳給 Controller 來處理

### Flow Entry
- Flow Entry 也就是我們所定義的轉發規則，在規則中我們會對符合條件的規則（Match）做相應的動作（Action）
- Flow Entry 的條目中，帶有以下六種欄位
    - Match fields（比對欄位）：紀錄該筆 Flow Entry 要比對的封包特徵，若封包的標頭檔與表格上的特徵相符，則代表該封包將被歸類至該筆 Flow Entry
    - Priority（優先序）：代表該筆 Flow Entry 在此 Flow Table 中的匹配順序，優先權較高的 Flow Entry 將被優先比對
    - Counters（計數器）：用來記錄符合該筆 Flow Entry 的封包數與資料大小
    - Instructions（指令集）：交換器對於隸屬於該筆 Flow Entry 的所有封包應採取的動作，常見的基本動作有 Drop 及 Forward
    - Timeouts（逾時時間）：代表該筆 Flow Entry 存在的最大逾時時間
    - Cookie（附屬屬性）：額外標記

## 延伸學習
1. [以 SDN 為基礎之網路邊界存取控制系統實作](http://ir.lib.nchu.edu.tw/bitstream/11455/96881/1/nchu-106-5096056021-1.pdf)
2. [區分 OpenFlow 定義各 Port 弄懂軟體定義網路交換器](https://www.netadmin.com.tw/netadmin/zh-tw/technology/FA061FD873454FB2934151DB0A6C89D1?page=1)
3. [深入 OpenFlow 協定精髓 看懂 Pipeline 處理流程](https://www.netadmin.com.tw/netadmin/zh-tw/technology/8244EA0CF13646ECB964DFC4B0700961?page=1)
4. [深入 OpenFlow 協定 詳解 Flow Table 比對機制](https://www.netadmin.com.tw/netadmin/zh-tw/technology/8842ED71637B430A90E72A173B566D36?page=1)