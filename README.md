# Pelion Device Management Client example for ifLink


## 準備

* Mbed開発サイト（os.mbed.com）のアカウント
* [Seeed Wio 3G](https://os.mbed.com/platforms/seeed-wio-3g/)
* [Mbed Studio IDE](https://os.mbed.com/studio/)
* パトライトLED

### Mbed開発サイトのアカウント取得

ここでは、Mbed OSデバイスとPelion Device Managementサービスを使用する方法を説明するので、最初にMbed開発サイトのアカウントが必要です。アカウントを取得していない場合は、こちらをご覧下さい。

https://github.com/toyowata/create_mbed_account/blob/master/README.md

### Mbed Studioのインストール

以下のURLから、使用するホストPCに対応したMbed Studioをインストールします。
https://os.mbed.com/studio/

## Pelion Portal Account の取得とサービス利用準備

アカウントの取得、APIキーの生成、開発者証明書の作成方法は、こちらを参照してください。
https://blog.pelion.com/post/arm-pelion-device-management-tutorial-jp-ja

## デバイス側サンプルプログラムの作成

### サンプルプログラムのインポートと設定

Wio 3Gボードをセルラー接続で使用する手順を説明します。

まず、Mbed Studioを起動します。

<img width="160" alt="" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/4fbe1716-70d7-6d28-6657-e949ca1f8a92.png">

次に、メニュー[File]-[Import Program...]を選択し表示されたダイアログに以下のリンクを入力します。
https://github.com/toyowata/mbed-os-pelion-iflink

<img width="300" src="https://dl.dropboxusercontent.com/s/aepz7g90o40wi8m/Screen%20Shot%202020-04-30%20at%2015.00.39.png">

`Add Program`ボタンを押してプログラムをインポートします。回線の状態によってはインポートに時間がかかるときがあります。画面右下のBackground Task表示が消えるまでお待ちください。

<img width="300" alt="" src="https://dl.dropboxusercontent.com/s/y4w2ete5syekf10/bk_task.png">

インポートされたサンプルプログラムのリストをクリックすると、使用しているソースファイル等の内容が表示されます。

<img width="300" src="https://dl.dropboxusercontent.com/s/240805btbc2fk5l/00_project.png">

#### 各種証明書の設定（Pelion Device Management用）

次にサービスへの接続とファームウェアアップデート用に必要な開発者証明書をAPIキーを使ってインストールします。
画面左側の雲形のアイコンをクリックします。Pelion Device Managementの右上の歯車アイコンを押してAPIキーを入力します。

<img width="300" src="https://dl.dropboxusercontent.com/s/enfsl3cnifnloft/studio_pdm.png">

`API Key`の部分に、ポータルから生成したAPIキーの値をペーストして、`Save`ボタンを押します。

<img width="400" src="https://dl.dropboxusercontent.com/s/tdi136u2halsgbi/studio_apikey.png">

次に開発者証明書を選択します。`Connect certificate`をから開発者証明書を選択します。`Enable remote firmware update`チェックボックスを有効にします。`Done`ボタンを押して、ダイアログを閉じます。これで、開発証明書のデータがプロジェクトに追加されます。

<img width="400" src="https://dl.dropboxusercontent.com/s/faru416m49s43dn/studio_devcert.png">

#### セルラー回線用の設定

このサンプルコードは、デフォルトでSORACOM Airサービスの設定が含まれています。

それ以外のSIMを使用する場合は、使用するネットワーク回線に応じた設定が必要です。左側のプログラムのリストから、`mbed_app.json`をクリックしてオープンします。
以下の画面でハイライトされている`nsapi.default-cellular-apn`等の項目を使用するSIMカードに応じた適切な値に設定します。

<img width="600" src="https://dl.dropboxusercontent.com/s/dun4gsy291wtnmx/Screen%20Shot%202020-04-30%20at%2015.13.34.png">

参考までに設定値の例を以下に示します。

* SORACOM Airの場合
```json
            "nsapi.default-cellular-apn"                : "\"soracom.io\"",
            "nsapi.default-cellular-sim-pin"            : null,
            "nsapi.default-cellular-username"           : "\"sora\"",
            "nsapi.default-cellular-password"           : "\"sora\""
```

* IIJmioの場合
```json
            "nsapi.default-cellular-apn"                : "\"iijmio.jp\"",
            "nsapi.default-cellular-sim-pin"            : null,
            "nsapi.default-cellular-username"           : null,
            "nsapi.default-cellular-password"           : null
```

* docomoの場合
```json
            "nsapi.default-cellular-apn"                : "\"spmode.ne.jp\"",
            "nsapi.default-cellular-sim-pin"            : null,
            "nsapi.default-cellular-username"           : "\"spmode\"",
            "nsapi.default-cellular-password"           : "\"spmode\""
```

<!--
|config|SORACOM Air|IIJmio|
|:---|:---|:---|
|`"nsapi.default-cellular-apn"`|`"\"soracom.io\""`|`"\"iijmio.jp\""`|
|`"nsapi.default-cellular-sim-pin"`|`null`|`null`|
|`"nsapi.default-cellular-username"`|`"\"sora\""`|`null`|
|`"nsapi.default-cellular-password"`|`"\"sora\""`|`null`|
-->

これでデバイス側のプロジェクトの設定は終わりです。

### ハードウェアの接続設定

パトライトLEDボードとWio 3GをGroveケーブルで接続します。写真のようにWio 3GボードのA6, A4コネクタをボードに接続してください。

<img width="500" src="https://dl.dropboxusercontent.com/s/t68bjsf7xg5g3vd/wio_3g_led.JPG">

パトライトLEDボードにACアダプタ（DC24V）を接続します。最後にUSBケーブルでホストPCとWio 3Gボードを接続します。

### サンプルプログラムのビルド

続いて、このサンプルプログラムをビルドします。
USBケーブルでボードをホストコンピュータを接続すると、Mbed Studioがボードを認識します。

問題なければ`Yes`をクリックします。後から`Target`のドロップダウンリストから選択することも可能です。

<img width="500" src="https://dl.dropboxusercontent.com/s/j589fxh7orugaaf/Screen%20Shot%202020-04-30%20at%2015.42.14.png">

検出されたターゲットボードが有効になりました。また、ビルド、実行、デバッグのアイコンも有効になりました。

<img width="300" src="https://dl.dropboxusercontent.com/s/dj8gya0iak3q7i0/Screen%20Shot%202020-04-30%20at%2015.50.18.png">

サンプルプログラムをビルドするには、ビルド用のアイコン（かなづちのマーク）をクリックします。Terminalウィンドウにビルドの進捗が表示されます。最後にImageのファイル名が表示されたら、ビルドは成功です。
エラーが表示された場合は、証明書のインストール方法の部分を再度確認してください。

<img width="600" src="https://dl.dropboxusercontent.com/s/hnbzcv5iytaak3r/Screen%20Shot%202020-04-30%20at%2015.56.44.png">

### サンプルコードの実行

デバイス上で実行されたプログラムのログ出力は、Mbed Studioから確認することが出来ます。

`Run Program`ボタンを押すとビルドされたコードがターゲットボードに書き込まれます。書き込みは、約30秒で終了します。書き込まれた後はボード上のリセットボタンを押してプログラムを実行して下さい。ボードの設定によっては、自動的にリセットする事も出来ます。

【注意】書き込みが正常に終了しない場合は、トラブルシューティングの項目をご覧ください。

<img width="300" src="https://dl.dropboxusercontent.com/s/j3kyp40p65ue77e/Screen%20Shot%202020-04-30%20at%2015.50.20.png">

シリアルコンソールの`Baud rate`は、115200に設定します。

<img width="600" src="https://dl.dropboxusercontent.com/s/h8tdy0lxs9z9q2q/console_out.png">

以下のようなシリアル出力ログが表示されます。Pelion Device Managementサービスに正常に登録されると、`Account ID`や`Endpoint name`が表示されます。同時に、Wio 3Gボード上の青色LEDが点灯します（接続までには30秒程度かかります）。
このサンプルコードでは、5秒ごとにパトライトLEDボードを制御するための設定値が表示されます。

```

Mbed Bootloader
No Update image
[DBG ] Active firmware up-to-date
booting...

Application ready
Connect to network
Network initialized, connected with IP 10.129.102.246

Start developer flow
Create resources
Register Pelion Device Management Client

Client registered.
Account ID: 01600xxxx058be206ae5ecf600000000
Endpoint name: 01xxxx09326200000000000100128207
Device ID: 01xxxx09326200000000000100128207

value = 0
value = 0
value = 0
value = 0
value = 0
Counter resource set to 7
value = 7
value = 7

```

## ウェブサイトでの接続確認
### Pelion Device Managementポータルサイトでの確認

ポータルサイト側からデバイスが正常に登録されているかを確認します。
ポータルサイトにログインします。メニュー [Device directory（デバイスID）] - [Devices（デバイス）] を選択し、デバイスが登録されているのを確認します。登録され、アクセス可能なデバイスは、`Status`フィールドに緑色のマークが付いています。

<img width="1358" alt="Screen Shot 2020-04-10 at 18.28.12.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/d209336e-de2f-4776-aea7-5e7c7cbb5a39.png">

登録されているデバイスをクリックすると、各種情報が表示されます。

<img width="1358" alt="Screen Shot 2020-04-10 at 18.28.22.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/9ba97570-1185-b46a-86a1-2bee8ee025a3.png">

[Resources（リソース）] タブをクリックすると、[OMA LWM2M仕様](https://omaspecworks.org/what-is-oma-specworks/iot/lightweight-m2m-lwm2m/)に準拠したデバイスのリソースパスが表示されます。
`/3200/0/5501`（サンプルコードでパトライトLEDボードを制御する値にとして使用しているDigital Inputに割り当てられたリソース）をクリックします。

<img width="1358" alt="Screen Shot 2020-04-10 at 18.29.13.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/2b07f2e7-22e7-4e81-6bf0-2c2dcf8646ef.png">

`/3200/0/5501`をクリックすると、現在の値がグラフ化され値が更新されます。エディットボタンをクリックして、PUTを先駆して値を更新することも出来ます。

<img width="600" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/9ab8107c-8a29-b0e4-e31b-00b98b452049.png">

この`/3200/0/5501`リソースの制御データは、以下のフォーマットでLEDを制御します。

|設定値|緑LED|黄LED|赤LED|
|---|---|---|---|
|0|OFF|OFF|OFF|
|1|OFF|OFF|ON|
|2|OFF|ON|OFF|
|3|OFF|ON|ON|
|4|ON|OFF|OFF|
|5|ON|OFF|ON|
|6|ON|ON|OFF|
|7|ON|ON|ON|

※ Bit 0〜2までが、各LEDに対応しています。  
※ Bit 3以上の値は無視されます。  

Pelion Service APIを使用する場合は、以下のページの「5.デバイスのリソースに書き込む」を参照してください。

https://toyowata.github.io/Pelion-device-e2e-example/

## トラブルシューティング

### Wio 3Gターゲットボードに書き込めない場合

`Run program`ボタンを押しても、`Erasing Chip  0%`と表示され、プログラムが書き込めない場合があります。その場合は、以下のいずれかの方法を試してみてください。

#### ターゲットフラッシュを全消去する

* Mbed Studioを終了する
* `erase.act`というファイル名のファイル（中身は空でかまいません）を作成する
* デバイスのマスストレージドライブ（DAPLINK）に`erase.act`をコピーする
* マスストレージドライブがアンマウントされ、再度マウントされるまで待つ
* Mbed Studioを起動し、`Run program`ボタンを押して書き込む

#### Terminalウィンドウから書き込む

* Mbed Studioを終了し、再度起動する
* `Build program`ボタンでプログラムをビルドする（`Run program`ボタンは押さない）
* プログラムがビルドされたら、メニュー[Terminal]-[New Terminal]を選択する
* Terminal上で以下のコマンドを入力し、フラッシュメモリに書き込む（Windowsの場合はcopyコマンドを使用し、マウントされたドライブ名を指定する）

```
$ cd mbed-os-pelion-sensor 
$ cp BUILD/WIO_3G/ARMC6/mbed-os-pelion-sensor.bin /Volumes/DAPLINK
```

* マスストレージドライブがアンマウントされ、再度マウントされるまで待つ
* ボードの上のリセットボタンを押し、シリアルコンソール（`__ Seeed Wio 3G`）でデバイスのログを確認する

## その他の情報

ここでは、Seeed Wio 3G（セルラー接続）を使用する手順を説明しましたが、このサンプルコードは以下のMbed OS対応ターゲットボードでも動作することを確認しています。Mbed Studioを使用する場合は、ボードを接続すると自動的にターゲットが切り替わります。
 
[NXP FRDM-K64F](https://os.mbed.com/platforms/FRDM-K64F/) (Ethernet)  
[ST NUCLEO-F429ZI](https://os.mbed.com/platforms/ST-Nucleo-F429ZI/) (Ethernet)  
