---
title: "FTPログイン接続をSlackで自動通知するツールの作成"
url: "/programming/php/ftp-auto-check/"
date: 2019-07-22
categories: 
  - "php"
---

複数のFTPログイン情報の記載があるcsvファイルを参照し、ログイン接続できるかの自動チェックツールを作成しました。

通知はSlackへ通知するようにしました。

定期的にチェックしたいので、cronにセッティングしました。

Slack通知には、APIの「Webhook URL」が必要なので事前に取得しておきます。

実際に作ったスクリプトは以下になります。

```
<?php
mb_language("Japanese");
mb_internal_encoding("UTF-8");
date_default_timezone_set('Asia/Tokyo');

//接続情報を配列形式に
$connect_array = [];
$csv = file('/usr/local/bin/ftplist.csv');//FTPログインユーザーのリストファイル
foreach ($csv as $row) {
    $row = str_replace(array("\r\n", "\r", "\n"), '', $row);//改行の削除
    $row_array = explode(',', $row);
    array_push($connect_array,$row_array);
}

foreach($connect_array as $connect){
    // 設定
    $user = $connect[0];//ユーザー名
    $pass = $connect[1];//パスワード
    $host = "192.168.33.10";//接続先サーバー
    $port = 21;//Port

    // FTPサーバ接続
    $conn_id = ftp_connect($host,$port);

    // ログインを試みる
    if (!@ftp_login($conn_id, $user, $pass)) {
        $message = "ログインエラー". PHP_EOL . 
        "以下のユーザーでログインができませんでした。" . PHP_EOL . 
        "$user@$host" . "(" . gethostname() . ")" . PHP_EOL  .
        date("Y/m/d H:i:s") . PHP_EOL;

        // slackへ送信
        slack($message);
    }

    // 接続を閉じる
    @ftp_close($conn_id); 
}

// slack通知
function slack($message) {
    $icon_emoji = ":warning:";
    $data = "payload=" . json_encode(
        array(
            "text" => $icon_emoji . $message,
        )
    );
    $ch = curl_init("SlackのWebhook URL");
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_exec($ch);
    curl_close($ch);
}
?>
```
