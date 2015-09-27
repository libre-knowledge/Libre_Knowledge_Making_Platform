# 如何於 GNU⁄Linux 作業系統中建立 Wifi Direct 連線<br />How to Establish Wifi Direct Connection in a GNU/Linux Operating System
Wi-Fi Direct，之前曾被稱為Wi-Fi 點對點（Wi-Fi Peer-to-Peer），是一套軟體協定，讓 wifi 裝置可以不必透過無線網路基地台（Access Point），以點對點的方式，直接與另一個 wifi 裝置連線，進行高速資料傳輸。這個協定由 Wi-Fi 聯盟發展、支援與授與認證，通過認證的產品將可獲得Wi-Fi 認證 Wi-Fi Direct 標誌。
（內容引用自 [Alfredo ougaowen](http://zh.wikipedia.org/wiki/User:Alfredo_ougaowen) 等人撰寫之維基百科條目：[Wi-Fi直連 - 維基百科，自由的百科全書](http://zh.wikipedia.org/wiki/Wi-Fi%E7%9B%B4%E8%BF%9E)）

本教材的目的是在 GNU/Linux 作業系統中嘗試建立此類連線。

## 搜尋關鍵字<br />Search keywords
Wi-fi Direct, connection, GNU/Linux

## 實測環境<br />Tested Environments
* 一台安裝 Ubuntu 14.04 GNU/Linux 作業系統（Intel x86（32 位元）處理器架構版本）的個人電腦、基於聯發科(Mediatek)（被併購前為雷凌(Ralink)） RT3070 802.11n 無線網路解決方案的外接 USB 無線網路介面卡

## 本教材會使用到的東西<br />Preliminaries
* 一個有支援 Wi-Fi Direct 協定的 Wi-Fi 無線網路介面卡
* 支援 Wi-Fi 無線網路介面卡的 Linux 作業系統核心驅動模組
* 用來設定無線網路的 iw 命令列介面工具
* libnl(?)
* wpa_supplicant
* wpa_cli 跟 wpa_supplicant 接觸的命令列介面工具

## 1. 檢查 Wi-fi 無線網路介面卡是否支援 Wi-Fi Direct

## 2. 讓 Network Manager 不管理 Wi-Fi 無線網路介面卡
![Network Manager 連線設定頁面](./圖片/由於 Network Manager 目前(2015⁄6⁄1)不支援 Wi-Fi Direct 連線.png)

由於 Network Manager 目前(2015/6/1)不支援 Wi-Fi Direct 連線，所以我們要讓 Network Manager 不管理 Wi-Fi 無線網路介面卡以避免發生衝突問題。

## 參考資料<br />Reference Data
* [Wi-Fi直連 - 維基百科，自由的百科全書](http://zh.wikipedia.org/wiki/Wi-Fi%E7%9B%B4%E8%BF%9E)
* [Wi-Fi Direct™: Connect with the possibilities - YouTube](https://www.youtube.com/watch?v=je2lWjfpywQ)
* [Wi-Fi Direct | Wi-Fi Alliance](http://www.wi-fi.org/discover-wi-fi/wi-fi-direct)
* [How to Setup Wi-Fi Direct on Android/Ubuntu Terminal – Part1 | அருண் வலைப்பூ - Arun's Blog](https://thangamaniarun.wordpress.com/2013/03/03/how-to-use-wi-fi-direct-on-androidubuntu-part1/)
* [wireless - How to get wifi direct( wifi p2p) on my HP DM1 laptop? - Ask Ubuntu](http://askubuntu.com/questions/258027/how-to-get-wifi-direct-wifi-p2p-on-my-hp-dm1-laptop)

## 本目錄下的項目說明<br />Description of the items under this directory
### [目錄說明文件.md<br />README.md](README.md)
本目錄的說明文件
