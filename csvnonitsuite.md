# CSVの形式について

## 概要 <a href="#h_c5a1381f66" id="h_c5a1381f66"></a>

本ページではWave ProからダウンロードできるCSVについて説明します。\
Wave Proの各ページからダウンロードできるCSVは以下の通りです。

### ダッシュボード <a href="#h_c780669f53" id="h_c780669f53"></a>

ダッシュボードからは複数のアカウント、タグを束ねた全体の金額をサービス単位で表示します。ダッシュボードからダウンロードできるCSVには以下の項目があります。

ファイル名：chart\_service\_daily\_yyyy-mm-dd\_yyyy-mm-dd

＊前後の日付は指定した期間のはじめと最後の日付が表記されます。

* date：CSVダウンロード時に設定した期間が表示されます。月別、日別の単位と設定内容によって変動します。
* Total：ドルベースの利用料
* サービス名：指定したサービス名が表示されます。

### レポート <a href="#h_f5a2c39433" id="h_f5a2c39433"></a>

**レポート全体**

ファイル名：ID-yyyy-mm-report

\* IDには請求グループID、またはアクセスグループIDが表示されます。

* AccountId：アカウントID
* ServiceCode：サービス名
* CostBeforeTax：ドル料金（税抜）
* Region：リージョン
* UsageQuantity：使用量
* InstanceType：インスタンスタイプ
* Operation：
* ItemDescription

**各アカウント、タグのレポートCSV**

ファイル名：awsAccountID-yyyymmdd-yyyymmdd

＊前後の日付は指定した期間のはじめと最後の日付が表記されます。

* AccountId：アカウントID
* ServiceCode：サービス名
* CostBeforeTax：ドル料金（税抜）
* Region：リージョン
* UsageQuantity：使用量
* InstanceType：インスタンスタイプ
* Operation：
* ItemDescription

### レポートフィルター <a href="#h_f5a2c39433" id="h_f5a2c39433"></a>

ファイル名：Report - date month dd yyyy

＊例）Report - Fri December 23 2023

レポートフィルターで生成されるCVSの項目はダウンロード時に指定することができます。

<figure><img src="https://downloads.intercomcdn.com/i/o/749253355/a6e8e43b18779586ce727dd3/Screenshot+2023-05-25+at+15.29.05.png" alt=""><figcaption></figcaption></figure>

## 操作手順 <a href="#h_c5a1381f66" id="h_c5a1381f66"></a>

#### ダッシュボード <a href="#h_5367de34a8" id="h_5367de34a8"></a>

ダッシュボードの右上にある縦「・・・」からデータをエクスポートをクリックすると指定している期間、サービスのデータがダウンロードできます。

<figure><img src="https://downloads.intercomcdn.com/i/o/749255911/23161cc6c1abf1141be494ca/Screenshot+2023-05-25+at+15.34.13.png" alt=""><figcaption></figcaption></figure>

#### レポート全体 <a href="#h_5367de34a8" id="h_5367de34a8"></a>

レポートの画面のアカウント検索の右のダウンロードアイコンをクリックし、ダウンロードしたい月を選択することでCSVをダウンロードできます。ダウンロードしたCSVはレポートで一覧で表示されているすべてのアカウントを合算した値が表示されます。

<figure><img src="https://downloads.intercomcdn.com/i/o/749256620/10ab5c831ea7074f89d94c6d/Screenshot+2023-05-25+at+15.35.40.png" alt=""><figcaption></figcaption></figure>

**各アカウント、タグのレポートCSV**

各アカウント、またはタグのレポートタブから期間を指定し、期間の右にあるダウンロードアイコンをクリックすることでCSVをダウンロードできます。

<figure><img src="https://downloads.intercomcdn.com/i/o/749257954/e33d4a4e1a81aa72a3190ac7/Screenshot+2023-05-25+at+15.37.34.png" alt=""><figcaption></figcaption></figure>

#### レポートフィルタ <a href="#h_036569d33c" id="h_036569d33c"></a>

[レポートフィルタの具体的な操作はこちらを参考にしてください。](https://docs.alphaus.cloud/v/projp/wave-pro-for-aws/wave-pro-for-aws/reportfilter)
