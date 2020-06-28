- [第二週筆記](#第二週筆記)
  - [本學期上課內容](#本學期上課內容)
  - [期中考、期末考評量方式](#期中考期末考評量方式)
  - [什麼是 SDN](#什麼是-sdn)
  - [SDN 各層元件、功能及介面基本介紹](#sdn-各層元件功能及介面基本介紹)
    - [網路設備（Network Devices）](#網路設備network-devices)
    - [南向介面（Southbound Interface）](#南向介面southbound-interface)
    - [控制器（Controller）](#控制器controller)
    - [北向介面（Northbound Interface）](#北向介面northbound-interface)
    - [應用服務（Service）](#應用服務service)
  - [SDN 的優點及缺點](#sdn-的優點及缺點)
  - [延伸學習](#延伸學習)
---
# 第二週筆記
## 本學期上課內容
- SDN、mininet、ryu controller、p4 switch

## 期中考、期末考評量方式
- 由於老師不知道本科該如何考試比較好，預計期中考的成績由**筆記**來取代；至於期末考的部分將由報告（內容為**報告一篇論文**）取代

## 什麼是 SDN
<img src="Week 2\SDN_architecture.jpg" width="550px" />

- 軟體定義網路（Software-Defined Networking，SDN）是一種新型網路架構
- 它利用 OpenFlow 協定**將**路由器的**控制平面（control plane）從資料平面（data plane）中分離**，改以**軟體方式實作**，從而使得將分散在各個網路裝置上的控制平面進行集中化管理成為可能
- 該架構可使網路管理員在**不更動硬體裝置**的前提下，以中央控制方式用程式重新規劃網路，為控制網路流量提供了新方案，也為核心網路和應用創新提供了良好平台
- Facebook 與 Google 都在他們的資料中心中使用 OpenFlow 協定，並成立了開放網路基金會來推動這個技術

## SDN 各層元件、功能及介面基本介紹
### 網路設備（Network Devices）
- 指的是**交換器、路由器、虛擬交換器或抽象的轉接面（Forwarding / Data plane）**
- 所有轉發規則，都儲存在網路設備裡，使用者資料封包在這裡被處理及轉發
- **網路設備**透過南向介面（Southbound interface）接收控制器發過來的指令，同時也透過南向介面主動上報事件給控制器

### 南向介面（Southbound Interface）
- 指的是控制面跟資料轉發層之間的介面
- 傳統網路的南向介面存在各格設備商的私有程式，並沒有標準化；但在**新型的 SDN 架構中，希望南向介面是標準化的**，也就是我們常聽到的 **OpenFlow** 標準介面

### 控制器（Controller）
- **SDN 網路中的核心元素**，向上提供應用程式的程式設計介面，向下控制硬體設備
- 通常運行在一台獨立的伺服器上，例如一台 x86 的 linux 伺服器

### 北向介面（Northbound Interface）
- **傳統網路**的北向介面指的是**交換器控制面與網管軟體之間的介面**
- **新型的 SDN 架構**中北向介面指的是**控制器與應用程式之間的介面**

### 應用服務（Service）
- 以軟體應用程式的方式，對網路進行控制及管理，例如：負載平衡（Load Balancing）、安全（Security）、壅塞延時等網路性能的管理（Monitoring）、拓樸偵測（LLDP）等

## SDN 的優點及缺點
- 優點
    - 集中式網路配置
    - 路由可編程性
    - 更好的安全性
    - 實現網路自動化的能力
    - 節省硬體設備支出
- 缺點
    - 若單一節點（Controller）遭到攻擊，可能導致整個網路癱瘓
    - 不容易擴展網路架構

## 延伸學習
1. [軟體定義網路（SDN）架構之應用與探討](https://www.taifex.com.tw/file/taifex/CHINESE/10/moth/P18-21%20%E9%97%9C%E9%8D%B5%E7%9C%8B%E6%B3%95(3).pdf)