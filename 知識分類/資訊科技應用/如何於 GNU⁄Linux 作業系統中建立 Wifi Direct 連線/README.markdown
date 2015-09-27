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

## 1. 檢查 Wi-fi 無線網路介面卡是否支援 Wi-Fi Direct 模式
執行 `iw list` 命令察看 Wi-fi 無線網路介面卡是否支援 Wi-Fi Direct 模式：

* RT3070
`````
Wiphy phy0
	Band 1:
		Capabilities: 0x172
			HT20/HT40
			Static SM Power Save
			RX Greenfield
			RX HT20 SGI
			RX HT40 SGI
			RX STBC 1-stream
			Max AMSDU length: 3839 bytes
			No DSSS/CCK HT40
		Maximum RX AMPDU length 65535 bytes (exponent: 0x003)
		Minimum RX AMPDU time spacing: 2 usec (0x04)
		HT RX MCS rate indexes supported: 0-7, 32
		TX unequal modulation not supported
		HT TX Max spatial streams: 1
		HT TX MCS rate indexes supported may differ
		Frequencies:
			* 2412 MHz [1] (27.0 dBm)
			* 2417 MHz [2] (27.0 dBm)
			* 2422 MHz [3] (27.0 dBm)
			* 2427 MHz [4] (27.0 dBm)
			* 2432 MHz [5] (27.0 dBm)
			* 2437 MHz [6] (27.0 dBm)
			* 2442 MHz [7] (27.0 dBm)
			* 2447 MHz [8] (27.0 dBm)
			* 2452 MHz [9] (27.0 dBm)
			* 2457 MHz [10] (27.0 dBm)
			* 2462 MHz [11] (27.0 dBm)
			* 2467 MHz [12] (disabled)
			* 2472 MHz [13] (disabled)
			* 2484 MHz [14] (disabled)
		Bitrates (non-HT):
			* 1.0 Mbps
			* 2.0 Mbps (short preamble supported)
			* 5.5 Mbps (short preamble supported)
			* 11.0 Mbps (short preamble supported)
			* 6.0 Mbps
			* 9.0 Mbps
			* 12.0 Mbps
			* 18.0 Mbps
			* 24.0 Mbps
			* 36.0 Mbps
			* 48.0 Mbps
			* 54.0 Mbps
	max # scan SSIDs: 4
	max scan IEs length: 2257 bytes
	Coverage class: 0 (up to 0m)
	Supported Ciphers:
		* WEP40 (00-0f-ac:1)
		* WEP104 (00-0f-ac:5)
		* TKIP (00-0f-ac:2)
		* CCMP (00-0f-ac:4)
	Available Antennas: TX 0 RX 0
	Supported interface modes:
		 * IBSS
		 * managed
		 * AP
		 * AP/VLAN
		 * WDS
		 * monitor
		 * mesh point
	software interface modes (can always be added):
		 * AP/VLAN
		 * monitor
	valid interface combinations:
		 * #{ AP, mesh point } <= 8,
		   total <= 8, #channels <= 1
	Supported commands:
		 * new_interface
		 * set_interface
		 * new_key
		 * new_beacon
		 * new_station
		 * new_mpath
		 * set_mesh_params
		 * set_bss
		 * authenticate
		 * associate
		 * deauthenticate
		 * disassociate
		 * join_ibss
		 * join_mesh
		 * set_tx_bitrate_mask
		 * action
		 * frame_wait_cancel
		 * set_wiphy_netns
		 * set_channel
		 * set_wds_peer
		 * Unknown command (84)
		 * Unknown command (87)
		 * Unknown command (85)
		 * Unknown command (89)
		 * Unknown command (92)
		 * connect
		 * disconnect
	Supported TX frame types:
		 * IBSS: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * managed: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * AP: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * AP/VLAN: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * mesh point: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * P2P-client: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * P2P-GO: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * Unknown mode (10): 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
	Supported RX frame types:
		 * IBSS: 0x40 0xb0 0xc0 0xd0
		 * managed: 0x40 0xd0
		 * AP: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
		 * AP/VLAN: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
		 * mesh point: 0xb0 0xc0 0xd0
		 * P2P-client: 0x40 0xd0
		 * P2P-GO: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
		 * Unknown mode (10): 0x40 0xd0
	Device supports RSN-IBSS.
	HT Capability overrides:
		 * MCS: ff ff ff ff ff ff ff ff ff ff
		 * maximum A-MSDU length
		 * supported channel width
		 * short GI for 40 MHz
		 * max A-MPDU length exponent
		 * min MPDU start spacing
	Device supports TX status socket option.
	Device supports HT-IBSS.

`````

`Supported interface modes` 清單中有列出 `P2P-GO` 與 `P2P-client` 就代表您的 Wi-Fi 無線網路介面卡支援 Wi-Fi Direct，**沒有就代表不支援**。

## 2. 讓 Network Manager 不管理 Wi-Fi 無線網路介面卡
![Network Manager 連線設定頁面](./Pictures/%E7%94%B1%E6%96%BC%20Network%20Manager%20%E7%9B%AE%E5%89%8D%282015%E2%81%846%E2%81%841%29%E4%B8%8D%E6%94%AF%E6%8F%B4%20Wi-Fi%20Direct%20%E9%80%A3%E7%B7%9A.png)

由於 Network Manager 目前(2015/6/1)不支援 Wi-Fi Direct 連線，所以我們要讓 Network Manager 不管理 Wi-Fi 無線網路介面卡以避免發生衝突問題。參考 `NetworkManager.conf` 位於第五區的 Manpage 格式說明文件**以 `root` 身份**編輯 `/etc/NetworkManager/NetworkManager.conf` 設定檔，於 `keyfile` 區域建立一個叫作 `unmanaged-devices` 的 key 其值為 `mac:〈以冒號分隔的網路介面 MAC 地址〉`，如果有多個網路介面的話以英式分號(;)分隔，例如：

```
[keyfile]
unmanaged-devices=mac:00:22:68:1c:59:b1;mac:00:1E:65:30:D1:C4
```

然後**以 root 身份**執行 `service network-manager restart` 命令重新啟動 network-manager 服務即可，您會發現 NetworkManager 設定介面中該網路介面有關的連線都不見了，表示 NetworkManager 不再管理該網路介面。

## 3. 啟動 wpa_supplicant 幕後程式

## 4. 使用 wpa_cli 建立 Wifi Direct 連線
### 使用 PBC 法
### 使用 PIN 法

## 5. 設定 IP 地址

## 參考資料<br />Reference Data
* [Wi-Fi直連 - 維基百科，自由的百科全書](http://zh.wikipedia.org/wiki/Wi-Fi%E7%9B%B4%E8%BF%9E)
* [Wi-Fi Direct™: Connect with the possibilities - YouTube](https://www.youtube.com/watch?v=je2lWjfpywQ)
* [Wi-Fi Direct | Wi-Fi Alliance](http://www.wi-fi.org/discover-wi-fi/wi-fi-direct)
* [How to Setup Wi-Fi Direct on Android/Ubuntu Terminal – Part1 | அருண் வலைப்பூ - Arun's Blog](https://thangamaniarun.wordpress.com/2013/03/03/how-to-use-wi-fi-direct-on-androidubuntu-part1/)
* [wireless - How to get wifi direct( wifi p2p) on my HP DM1 laptop? - Ask Ubuntu](http://askubuntu.com/questions/258027/how-to-get-wifi-direct-wifi-p2p-on-my-hp-dm1-laptop)
* `NetworkManager.conf` 位於第五區的 Manpage 格式說明文件

## 本目錄下的項目說明<br />Description of the items under this directory
### [目錄說明文件.md<br />README.md](README.md)
本目錄的說明文件
