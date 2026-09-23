---
title: "Pythonの簡易WEBサーバの文字化けを環境変数設定してなおす"
url: "/programming/python/python-webserver/"
date: 2018-11-24
categories: 
  - "python"
---

Python勉強に使っているのが、【実践力を身につける Pythonの教科書】ってのをひたすらやってる。環境はWindows。

WEBアプリを作ってみようのところで、WEBアプリをブラウザでみると日本語が文字化けした。

まずは以下のようにコードをっと、、

```
#!/usr/bin/env python

print("Content-Type: text/html; charset=utf-8")

print("")

print("")
print("パイソンのお勉強！")
print("")
```

それで、コマンドランインからサーバを起動。

```
python -m http.server --cgi 8080
```

それでブラウザ上でURLを入れる。

```
http://localhost:8080/cgi-bin/test.py
```

ここで文字化け。

プログラムの文字コードをちゃんとUTF-8で保存されているけどなおらず...

次に、書籍に書いてある通り環境変数を新規に設定した。

変数名は「PYTHONIOENCODING」で、値は「utf-8」に。

しかし、それでも文字化けしたまま…

数時間がハマったあげく、とりあえず再起動して試すことにした。

そしたら、解決！

**新しい環境変数を追加した場合は、再起動する必要がある！**

とりあえずなにかの設定変更しときは、再起動した方が良いということです。
