- [第二週筆記](#第二週筆記)
  - [OpenFlow 是什麼](#openflow-是什麼)
  - [OpenFlow 架構圖](#openflow-架構圖)
  - [OpenFlow 協定](#openflow-協定)
  - [Pipeline 處理流程](#pipeline-處理流程)
    - [Goto 指令](#goto-指令)
    - [Table Miss 情況](#table-miss-情況)
  - [Flow Table 的比對過程](#flow-table-的比對過程)
    - [Table Miss 情況](#table-miss-情況-1)
  - [了解 Flow Entry 內容](#了解-flow-entry-內容)
  - [延伸學習](#延伸學習)
---
# 第二週筆記
## OpenFlow 是什麼
<img src="Week 1\SDN_architecture.jpg" width="550px" />

- OpenFlow是一種網路協定，運作於網路七層的第二層，也就是資料鏈結層（Data Link Layer）
- 這個網路協定主要允許遠端地去控制交換機（Switch）或路由器（Router）的轉發平面（Forwarding Plane），改變網路封包轉送表格，進而遠端地改變網路封包的網路路徑

## OpenFlow 架構圖
<img src="Week 2\openflow.png" width="300px" />

- Controller：控制器
- OpenFlow Switch：OpenFlow 交換器
    - OpenFlow Channel：OpenFlow 通道
    - Flow Table：規則表
        - Flow Entry：規則表項

## OpenFlow 協定
- 在 OpenFlow 交換器中處理或轉送封包皆由 Flow Table 中的規則決定，而網路管理者可利用控制器（Controller）新增、刪除或修改來管理交換器中的 Flow Table 的規則內容
- 當控制器與 OpenFlow 交換器連接成功後，彼此間透過 Control Channel 建立安全通道（Secure Channel）確保 OpenFlow 協定溝通的機密性

## Pipeline 處理流程
<img src="Week 2\flow_table.png" width="550px" />

- 每個 OpenFlow 協定的網路交換機的 OpenFlow Pipeline 至少會包含一個或是以上的 Flow Table，而 OpenFlow Pipeline 的目的在於定義網路封包如何與這些 Flow Table 做互動
- 這些 Flow Table 從零開始編號，每個 Flow Table 都有編號，而每個網路封包的 Pipeline 處理都會從 Flow Table = 0 開始做處理

### Goto 指令
- 如果找到符合規則的 Flow Entry，其包含的指令可能會是 Goto 這樣的動作指令。其代表的意義是要直接跳往指定的 Flow Table，這裡必須指明下一個要跳往的 Flow Table 的編號，但必須要注意的是，只能「往後跳」不能「往前跳」
- 舉例來說，如果包含 Goto 這個 Flow Entry 是位於 Flow Table 0，然後 Goto 指令希望跳到 Flow Table 4，則代表接下來會直接跳過編號為 1、2 以及 3 這些 Flow Table，直接跳往 Flow Table 4 繼續尋找符合規則的 Flow Entry

### Table Miss 情況
- 如果在某個 Flow Table 尋找符合規則的 Flow Entry 時，都找不到相對應的 Flow Entry，這種情況就稱為 Table Miss
- 當發生 Table Miss 時，會執行原本設定好的 Table Miss 指定動作（預設為丟棄封包），這個設定是每個 Flow Table 分開的。這樣的動作有可能是要直接丟棄網路封包，或是到另外一個指定的 Flow Table 繼續 Pipeline 處理過程，也有可能是送往 Controller

## Flow Table 的比對過程
<img src="Week 2\flow_processing.png" width="500px" />

- 基本上，整個 Flow Table 的比對過程可以用上圖來解釋
- 當收到一個網路封包的時候，會先從第一個 Flow Table 開始執行比對動作，這是剛才提到 Pipeline 處理過程的一部分。而第一個 Flow Table 編號是從 0 開始，比對之前會先從網路封包中擷取出需要比對的資訊，而會擷取出來的資訊，根據不同的網路封包而有所不同
- 封包所需比對的資訊反映出當前這個封包的狀態，因為有可能在找到符合的 Flow Entry 時，會需要執行 Apply 的動作，而這樣的動作有可能改變封包中被比對的部分
- 從上圖的流程圖中可以看得出來，如果從某個 Flow Table 之中找到符合的 Flow Entry，將會先**更新計數器**，接著可能**更新 Action Set**，**更新封包中的比對資料和 Metadata**。然後，如果沒有定義 Goto 指令的話，就會執行原本已經定義好的動作，否則就會跑到想要跳過去的 Flow Table，並且繼續比對

### Table Miss 情況
- 並不是每個封包都可以找到相符的 Flow Entry，所以每張 Flow Table 都必須要有 Table-miss Flow Entry，其目的就是用來告訴交換器該如何處理這些為比對到 Flow Entry 的封包
    - 如果在某一個 Flow Table 中都沒有找到符合的 Flow Entry，則會根據預先設定好的預設值執行動作指令，這預先設定好的動作可能是尋找下一個 Flow Table 的內容，或是直接丟棄（這是預設動作），也可能回傳給 Controller 來處理

## 了解 Flow Entry 內容
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