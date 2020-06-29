- [第七週筆記](#第七週筆記)
  - [P4 Switch + Mininet + Docker Host](#p4-switch--mininet--docker-host)
    - [拓樸架構](#拓樸架構)
    - [前情提要](#前情提要)
  - [延伸學習](#延伸學習)
---
# 第七週筆記
## P4 Switch + Mininet + Docker Host
### 拓樸架構
- H1 <-> P4switch <-> H2（Docker Host）
### 前情提要
1. 安裝 docker：`sudo apt-get install docker.io`
2. 開啟 docker：`systemctl enable docker`
3. 確認 docker 是否有被開啟（若有開啟會出現綠色 active 字樣）：`service docker status`


## 延伸學習
1. [Ubuntu Linux 安裝 Docker 步驟與使用教學](https://blog.gtwang.org/virtualization/ubuntu-linux-install-docker-tutorial/)