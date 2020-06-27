# 第二週筆記
## OpenFlow 是什麼
<img src="Week 1\SDN_architecture.jpg" width="550px" />

- OpenFlow 是一種網路通訊協定，屬於**資料鏈路層**，能夠控制網路交換器或路由器的轉發平面（Forwarding plane），藉此改變網路封包所走的網路路徑
- OpenFlow 被認為是第一個軟體定義網路（SDN）標準之一。它最初在 SDN 環境中定義了通信協定，使 SDN **控制器**能夠與物理和虛擬的交換機或路由器等**網路裝置**的轉發平面直接進行互動，從而更好地適應不斷變化的業務需求

## OpenFlow 協定重要元件
<img src="Week 2\openflow.png" width="250px" />

* 一般來說，一個支援 OpenFlow 協定的交換機包含幾個重要元件
* 如上圖所示，這些包含元件有一個 Group Table 以及多個 Flow Table；OpenFlow Switch 會與上方的 Controller 溝通，這個 Controller 再透過 OpenFlow 協定與 OpenFlow Channel 溝通

### Flow Table
<img src="Week 2\flow_table.png" width="550px" />
<br>
<img src="Week 2\flow_processing.png" width="500px" />

- 當網路封包抵達網路交換機時，就會開始根據 Flow Table 中的內容嘗試去比對符合的條件，**Flow Table 中的每一筆資料是有前後優先順序**的，而 **Flow Table 各個表格之間也會有優先順序**。一旦找到相符的 Flow Entry（封包轉送規則），就執行 Flow Entry 中所設定好的動作指令
- 如果在某一個 Flow Table 中都沒有找到符合的 Flow Entry，則會根據預先設定好的預設值執行動作指令，這預先設定好的動作可能是尋找下一個 Flow Table 的內容，或是直接丟棄，或者轉給 OpenFlow Channel 來處理

#### Flow Entry
- Flow Entry 也就是我們所定義的轉發規則，在規則中我們會對符合條件的規則（Match）做相應的動作（Action）
- Flow Entry 的條目中，帶有以下六種欄位
    - Match fields（比對欄位）：包含 Ingress Port、網路封包的 Header 及可能從上個 Flow Table 傳過來的 Metadata
    - Priority（優先序）：Flow Entry 的優先順序
    - Counters（計數器）：一旦有相符合的網路封包，這個計數器就會被更新
    - Instructions（指令集）：針對符合比對的網路封包的動作
        | Instructions   | 處理方法                                             |
        | -------------- | ---------------------------------------------------- |
        | Meter          | 對匹配到 Flow Entry 的封包進行限速                   |
        | Apply-Actions  | 立即執行 Action                                      |
        | Clear-Actions  | 清除動作集（Action Set）中的所有 Action              |
        | Write-Actions  | 更改動作集（Action Set）中的所有 Action              |
        | Write-Metadata | 更改 Flow Entry 間資料，在支援多個 Flow Table 時使用 |
        | Goto-Table     | 進入下一個 Flow Table                                |
    - Timeouts（逾時時間）：代表這筆 Flow Entry 存在的最大逾時時間
    - Cookie（附屬屬性）：Controller 所使用的資料

### 動作指令
- 剛才有提到每一筆 Flow Entry 都會有相對應的動作指令，一旦找到符合的 Flow Entry，就會執行相對應的動作指令。而動作指令內容可以是一般的動作（Action）或是所謂的 Pipeline Processing
- **一般的動作指令**可能是**轉發網路封包**，或是**修改網路封包內容**，而 **Pipeline Processing** 代表的是**讓下一個 Flow Table 來處理**，當然網路封包相關的資訊也都會被傳到下一個 Flow Table
- **如果 Pipeline Processing 並沒有指定下一個要處理的 Flow Table，就不會丟到下一個 Flow Table**，通常這種時候都會直接針對這個網路封包做處理

### Group Table
- 和 Flow Table 一樣，Group Table 包含許多 Group Entry。而每一個 Group Entry 包含多個 Action Bucket，這些 Action Bucket 會組成一個清單，而這些 Action Bucket 會根據不同的Group 種類而不同
- 一旦網路封包送到指定的 Group，一個或多個相對應的 Action Bucket 中的動作就會被執行

### 埠（port）的處理
- Flow Entry 的動作中可能會指定要轉發到指定的「埠」（Port），這裡的埠可以是**實體在網路交換機上面的埠**，也有可能是**虛擬的埠**，或是**保留的埠**
- **保留的埠**可能可以執行網路封包的轉發，或是丟到 Controller，或是**轉發到所有其他的埠**，也就是所謂的 **Flooding**，或甚至是透過不是使用 OpenFlow 協定的方式來處理

## OpenFlow Port 的種類介紹
### OpenFlow Port

## 延伸學習
1. []()