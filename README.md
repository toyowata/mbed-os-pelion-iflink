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

クラウドサービスのアカウントは、評価用として機能制限付きの無償版アカウントが用意されています。
自分のアカウントの[コンソール](https://console.mbed.com/)の中にある、Pelion Device Managementから`Learn how to connect`と書かれている黄色のボタンを押します。

<img width="400" alt="pdm-learn-how-to-connect.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/47573f9a-5ca2-6480-11ff-59d559ae6df7.png">

画面が切り替わるので、`Activate your free access` ボタンを押します。

<img width="357" alt="pdm-activate-1.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/c64e066d-0ccc-d87c-9a7c-d46f79879a67.png">

以下の画面から、`Activate Pelion Device Management account`ボタンを押します。

<img width="476" alt="pdm-activate-2.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/165eeeb9-0d7c-874b-5572-151e48ebf690.png">

ボタンを押すと以下のようなメッセージが表示されます。

<img width="356" alt="pdm-success.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/baf53a76-f3d4-1103-87de-72ae826d97b1.png">

自分のアカウントの[コンソール](https://console.mbed.com/)に以下の項目が表示されていれば、準備完了です。

<img width="400" alt="pdm-ready.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/1c09493a-dc20-f768-b995-f97c3e7484dc.png">

`portal`ボタンを押して、Pelion Device Management のポータルサイトにアクセスします。 
初回アクセス時はライセンス条項への同意が必要になります。

### APIキーの生成

ポータルサイトにアクセス出来たので、自分のアカウントで利用するAPIキーを生成します。以下のURLにアクセスするか、左側のメニューから[Access management]-[API keys]を選択します。
https://portal.mbedcloud.com/access/keys

画面右上の`+ New API Key`ボタンをクリックします。

<img width="600" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/706c9b77-92ad-b215-1c97-14c7c4205ed6.png">

Create API Keyダイアログが開きます。`API Key name`の部分に任意の名前を設定します。Groupは、Administratorsを指定してください。`Create API Key`ボタンを押して、APIキーを生成します。

<img width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/b41f240f-1b41-3d76-7524-ef059b8194a6.png">

【注意】　ここで生成されたAPIキー（ak_で始まる文字列）は必ず、コピーして値を保存しておいてください。セキュリティの関係上、ポータルサイトからAPIキーの値は二度と取得できなくなります。もし、APIキーの値を紛失してしまった場合は、新しくキーを生成してください。

<img width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/98e24f0d-bd06-718d-dcfd-3e6a2d8bcba7.png">

### 開発者証明書の生成

次にデバイスがサービス接続時に必要な証明書を生成します。通常、プロダクションに必要なデバイス証明書の生成や書き込みは、工場出荷時にセキュアな環境で行いますが、ここでは開発時に手軽に利用できる開発証明書を使用します。
以下のURLにアクセスするか、左側のメニューから[Device identity]-[Certificates]を選択します。
https://portal.mbedcloud.com/identity/certificates/list

画面右上の`+ New certificate`ボタンをクリックし、`Create a developer certificate`を選択します。

<img width="600" alt="" src="https://dl.dropboxusercontent.com/s/zvkz7c9n6qdm8w5/portal_devcert1.png">

Nameの部分は自動入力されますが、変更することも可能です。`Create certificate`ボタンを押して、開発証明書を作成します。

<img width="400" alt="" src="https://dl.dropboxusercontent.com/s/cr0rr4lx4lpdfpl/portal_devcert2.png">

## デバイス側サンプルプログラムの作成

Pelion Device Management ClientをMbed OS対応デバイスで使用するには、[クイックスタートガイド](https://os.mbed.com/guides/connect-device-to-pelion/)が用意されています。

クイックスタートガイドには、動作確認済みのサンプルプログラムが用意されています。このサンプルをインポートして自分のアカウント用の設定を行うだけで、Pelion Device Managementサービスを簡単に体験することが出来ます。クイックスタートガイドで説明している内容は、MbedオンラインIDEを使用する方法ですが、このドキュメントではローカル環境で使用可能なMbed Studioで同じサンプルプログラムをビルドしてみます。

### サンプルプログラムのインポートと設定

Wio 3Gボードをセルラー接続で使用する手順を説明します。

まず、Mbed Studioを起動します。

<img width="160" alt="" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/4fbe1716-70d7-6d28-6657-e949ca1f8a92.png">

次に、メニュー[File]-[Import Program...]を選択し表示されたダイアログに以下のリンクを入力します。
https://github.com/toyowata/mbed-os-pelion-sensor

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

Wio 3G等のセルラー接続の場合は、使用するネットワーク回線に応じた設定が必要です。左側のプログラムのリストから、`mbed_app.json`をクリックしてオープンします。
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

[こちらの手順](https://seeedjp.github.io/Wiki/General/how_to_sim_tf-en)を参考にして、Wio 3GにSIMカードを挿入します（このサンプルプログラムでは、マイクロSDカードは使用しません）。

このサンプルプログラムでは、温湿度・気圧センサと、LEDボタンを使用します。写真のように接続して下さい（BME280センサーモジュールをI2Cに、青LEDボタンをD20に接続します）。

<img width="400" src="https://dl.dropboxusercontent.com/s/wfd5shdts0qegp7/IMG_8045.JPG">


最後にUSBケーブルでホストPCとWio 3Gボードを接続します。

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

以下のようなシリアル出力ログが表示されます。`Account ID`や`Endpoint name`が表示されれば、プログラムは正常に動作しています。
このサンプルコードでは、5秒ごとにセンサーから取得したデータ（湿度、気圧、温度）を表示し、Pelion Device Managementサービスに値をアップデートします。この値は、Pelion Device Managementのリソースに書き込まれます。また、AWSアカウント用の設定が行われていれば、AWS IoTのMQTTクライアントにもパブリッシュされます。

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
Account ID: 0160XXXXXXXXXX206ae5ecf600000000
Endpoint name: 017XXXXXXXXXX0000000000100138fbc
Device ID: 017XXXXXXXXXX0000000000100138fbc

Opening network interface...
Connecting to network
Network interface opened successfully.

[PDM] humidity = 61.56%, pressure =  994.48 hPa, temperature = 29.84 DegC
Time is now Thu Jun 11 11:36:22 2020
[AWS] Connecting to host xxxxxxxxxx-ats.iot.ap-northeast-1.amazonaws.com:8883 ...
[AWS] Connection established.
[AWS] MQTT client is trying to connect the server ...
[AWS] Client connected.

[AWS] Client is trying to subscribe a topic "cmd/area-1/device-1".
[AWS] Client has subscribed a topic "cmd/area-1/device-1".
[AWS] To send a packet, push the button on your board.

[PDM] humidity = 61.45%, pressure =  994.15 hPa, temperature = 29.84 DegC
{"humidity":61.45, "pressure": 994.15, "temperature":29.84}

[AWS] Message published.
[PDM] humidity = 61.53%, pressure =  994.21 hPa, temperature = 29.83 DegC
{"humidity":61.53, "pressure": 994.21, "temperature":29.83}
```

また、青LEDボタンを押すと、カウント値をインクリメントします。この値は、Pelion Device Managementのリソースに書き込まれるのと同時に、AWS IoTのMQTTクライアントにもパブリッシュされます。

```
[PDM] Counter 1
{"is_button_clicked":true,"count":1}

[AWS] Message published.
[AWS] Message arrived:
{"is_button_clicked":true,"count":1}

[PDM] Counter 2
{"is_button_clicked":true,"count":2}
```

## ウェブサイトでの接続確認
### Pelion Device Managementポータルサイトでの確認

ポータルサイト側からデバイスが正常に登録されているかを確認します。
ポータルサイトにログインします。メニュー [Device directory（デバイスID）] - [Devices（デバイス）] を選択し、デバイスが登録されているのを確認します。登録され、アクセス可能なデバイスは、`Status`フィールドに緑色のマークが付いています。

<img width="1358" alt="Screen Shot 2020-04-10 at 18.28.12.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/d209336e-de2f-4776-aea7-5e7c7cbb5a39.png">

登録されているデバイスをクリックすると、各種情報が表示されます。

<img width="1358" alt="Screen Shot 2020-04-10 at 18.28.22.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/9ba97570-1185-b46a-86a1-2bee8ee025a3.png">

[Resources（リソース）] タブをクリックすると、[OMA LWM2M仕様](https://omaspecworks.org/what-is-oma-specworks/iot/lightweight-m2m-lwm2m/)に準拠したデバイスのリソースパスが表示されます。
`/3200/0/5501`（サンプルコードでカウンター値にとして使用しているDigital Inputに割り当てられたリソース）をクリックします。

<img width="1358" alt="Screen Shot 2020-04-10 at 18.29.13.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/2b07f2e7-22e7-4e81-6bf0-2c2dcf8646ef.png">

`/3200/0/5501`をクリックすると、青LEDボタンを押すことでカウントアップされた数値がグラフ化され値が更新されます。

<img width="600" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/75582/9ab8107c-8a29-b0e4-e31b-00b98b452049.png">

また、センサーから取得されたデータは、OMA LWM2M規格に準拠した以下のリソースパスに書き込まれています。これらの値も、該当するリソースをクリックすることで表示することが出来ます。

|種別|リソースパス|単位|
|---|---|---|
|温度|/3303/0/5700|℃|
|湿度|/3304/0/5700|%|
|気圧|/3323/0/5700|hPa|

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
