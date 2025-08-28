# コマンドでテキストファイルの各行の行頭の固定桁を書き換えて上書きする

テキストファイルのサンプル
```
"apple","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"apple","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"apple","bbbb","ccc","dd","e"
```

## 行頭が指定した文字列に一致したら書き換える

行頭が '"apple"' だったら '"ume"' に書き換える。
```
sed -i 's/^"apple"/"ume"/' filename.txt
```

実行結果
```
"ume","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"ume","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"ume","bbbb","ccc","dd","e"
```

## 行頭が指定した文字列に一致した行数を出力する

行頭が大文字小文字を無視して '"apple"' だったらカウントする。
```
grep -i '^"apple"' filename.txt | wc -l
```
