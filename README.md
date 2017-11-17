minIRum
======

A minimal implementation of infrared sender/receiver like IRKit by ESP8266

## Description

IRKitの GET,POST /messages 同様の機能**だけ**をESP8266で実現する最小限の実装です

ESP-WROOM-02で動作確認済み

Maybe it will work on other ESP* boards.

## How to use

以下の通り接続

* GPIO 12 - Infrared LED
* GPIO 14 - Infrared receiver

![schematic](https://raw.githubusercontent.com/9SQ/minIRum/master/schematic.png)

赤外線リモコン受信モジュールは[OSRB38C9AA](http://akizukidenshi.com/catalog/g/gI-04659/)などの3.3V駆動可能のものを利用します

以下の2か所を適宜変更

```C
const char* ssid = "**********"; // Wi-FiアクセスポイントのSSID
const char* password = "**********"; // パスワード
```

電源を入れると無線LANアクセスポイントに自動的に接続し、DHCPによりIPアドレスを取得します。

### See also

回路図とパーツリストはブログ記事を参照してください。

[ミニマルなIRKitクローンを作ってiOSから家電を制御する : Eleclog.](http://eleclog.quitsq.com/2016/09/minirum.html)

## Example Requests

mDNSに対応している場合 minirum-[MACアドレスの下位3オクテット].local のアドレスで接続できます。

**GET /messages**

```sh
curl -i http://minirum-a492cd.local/messages
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 383
Connection: close
Access-Control-Allow-Origin: *

{"format":"raw","freq":38,"data":[3550,1700,500,400,500,400,500,1300,500,1300,500,400,500,1300,500,400,500,400,500,400,500,1300,500,400,500,400,500,1300,500,400,500,1300,500,400,500,1300,500,400,500,400,500,1300,500,400,500,400,500,400,500,400,500,1300,500,400,500,1300,500,1300,500,400,500,1300,500,400,500,400,500,400,500,400,500,1300,500,400,500,400,500,1300,500,400,500,400,500]}
```

**POST /messages**

```sh
curl -i http://minirum-a492cd.local/messages -d'{"format":"raw","freq":38,"data":[3550,1700,500,400,500,400,500,1300,500,1300,500,400,500,1300,500,400,500,400,500,400,500,1300,500,400,500,400,500,1300,500,400,500,1300,500,400,500,1300,500,400,500,400,500,1300,500,400,500,400,500,400,500,400,500,1300,500,400,500,1300,500,1300,500,400,500,1300,500,400,500,400,500,400,500,400,500,1300,500,400,500,400,500,1300,500,400,500,400,500]}'
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 2
Connection: close
Access-Control-Allow-Origin: *
```

### simple web console

ブラウザでroot URLにアクセスすると簡易的なコンソールが利用できます。

![webconsole](https://raw.githubusercontent.com/9SQ/minIRum/master/webconsole.png)

## Requirements

* [Arduino core for ESP8266 WiFi chip](https://github.com/esp8266/Arduino)
* [aJson](https://github.com/interactive-matter/aJson)
* [IRremoteESP8266](https://github.com/markszabo/IRremoteESP8266)

## Appendix

### 動作確認環境

- Arduino 1.8.5
- IRremoteESP8266 2.3.0
- aJson 1.0

### Install

- Arduinoは公式から取得するのでOK
- ESP8266用のArduino coreはArduinoへのインストールが必要 [https://qiita.com/azusa9/items/264165005aefaa3e8d7d]
- IRremoteESP8266, aJsonのライブラリはgithubからzipを落としてきて スケッチ > ライブラリをインクルード > .ZIP形式のライブラリをインストール

### ボードへの書き込み等

まずは正しく接続できているかを調べる：

[http://deviceplus.jp/hobby/entry0033/]

- FTDIのVCOMドライバを公式からインストールする
- シリアルモニタに接続、ボーレート等を調整する
- USBで接続する
- ESPにリセットをかけて（リセットピンをGNDに接地させてもどすとリセットがかかる）、シリアルモニタに文字化け＆readyが表示されればOK

つぎに書き込む：

- ESPを書き込みモードにする（IO0を接地させると書き込みモードになる）
- Arduinoで書き込む
- リセットを書ける



