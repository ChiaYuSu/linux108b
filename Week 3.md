- [第三週筆記](#第三週筆記)
  - [安裝 Ubuntu 16.04 LTS](#安裝-ubuntu-1604-lts)
    - [環境介紹](#環境介紹)
    - [環境安裝](#環境安裝)
  - [安裝 mininet-wifi](#安裝-mininet-wifi)
  - [mininet-wifi 介紹](#mininet-wifi-介紹)
  - [延伸學習](#延伸學習)
---
# 第三週筆記
## 安裝 Ubuntu 16.04 LTS
### 環境介紹
- 本學期課程所需要的環境
- Ubuntu 16.04 LTS 並非目前官網上最新版本，但老師過去都使用這個版本做實驗，我們用這個版本去做實驗之後比較不會有太多問題
    - 目前官網最新版本為 20.04 LTS

### 環境安裝
1. 安裝在 VirtualBox 虛擬機
    - 我下載的版本：VirtualBox 6.0.20
2. 下載 VirtualBox Extension Pack
    - 我下載的版本：6.0.20
3. 安裝完 Ubuntu 環境後，插入 Guest Additions CD，可以提升我們使用虛擬機的效率
    1. 可以讓畫面自動等比例縮放（自適應螢幕）
    2. 可以讓 Ubuntu 和 Windows 做無縫相接（雙向共用剪貼簿、雙向共用資料夾、雙向拖放資料）
    3. 全螢幕模式
      <img src="Week 3\guest_additions_CD.png" width="400px" />

## 安裝 mininet-wifi
1. Open VirtualBox
2. Open Ubuntu 16.04 LTS
3. Open terminal
4. Switch to Super User: `su`
5. Enter Super User Password
6. Clone the project of mininet-wifi: `git clone https://github.com/intrig-unicamp/mininet-wifi.git`
7. Change Directory: `cd mininet-wifi`
8. Install: `sudo util/install.sh -Wnfv`

## mininet-wifi 介紹
- mininet-wifi 是一個無線網路仿真器（Emulator），允許使用者採用軟體的方式建置所需要的無線網路，可以建構成具有基礎架構的無線網路（Infrastructure Mode，包含基地台和行動主機）或者是無線隨意網路（AdHoc Mode，只有行動主機）非常的方便，只需要全軟體的操作，不需要額外再購買其他的硬體

## 延伸學習
1. [ubuntu 正體中文站](https://www.ubuntu-tw.org/modules/tinyd0/)
2. [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
3. [Github / mininet](https://github.com/intrig-unicamp/mininet-wifi)