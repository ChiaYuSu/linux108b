# 第一週筆記
## 本學期上課內容
- SDN、mininet、ryu controller、p4 switch

## 期中考、期末考評量方式
- 由於老師不知道本科該如何考試比較好，預計期中考的成績由**筆記**來取代；至於期末考的部分將由報告（內容為**報告一篇論文**）取代

## 什麼是 SDN
<img src="Week 1\SDN_architecture.jpg" width="550px" />

- 軟體定義網路（Software-Defined Networking，SDN）是一種新型網路架構
- 它利用 OpenFlow 協定**將**路由器的**控制平面（control plane）從資料平面（data plane）中分離**，改以**軟體方式實作**，從而使得將分散在各個網路裝置上的控制平面進行集中化管理成為可能
- 該架構可使網路管理員在**不更動硬體裝置**的前提下，以中央控制方式用程式重新規劃網路，為控制網路流量提供了新方案，也為核心網路和應用創新提供了良好平台
- Facebook 與 Google 都在他們的資料中心中使用 OpenFlow 協定，並成立了開放網路基金會來推動這個技術