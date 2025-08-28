# コマンドでテキストファイルの各行の行頭の固定桁を書き換えて上書きする

テキストファイルのサンプル
```
"Apple","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"applE","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"apple","bbbb","ccc","dd","e"
```

## 行頭が指定した文字列に一致したら書き換える

行頭がダブルクォーテーションで括られた '"apple"' だったら '"ume"' に書き換える。
```
awk '{ if (tolower($0) ~ /^"apple"/) $0 = "\"ume\"" substr($0, 8); print }' filename.txt >> tempfile && mv tempfile filename.txt
```

実行結果
```
"ume","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"ume","bbbb","ccc","dd","e"
"mikan","bbbb","ccc","dd","e"
"ume","bbbb","ccc","dd","e"
```

IBM AIX 7.2 'awk $0'  
File Processing with Records and Fields No.3  
[https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a158c1368__title__1](https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a158c1368__title__1)  
行全体を '$0' で表す。  

IBM AI 7.2 'awk tolower(String)'  
Actions >> Built-in Functions >> String Functions >> tolower(String)  
[https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a171c125d__title__1](https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a171c125d__title__1)  
tolower(String)で大文字小文字を無視する。  

IBM AIX 7.2 'awk \"'  
Patterns >> Recognized Escape Sequences  
[https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a161c119e__title__1](https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a161c119e__title__1)  
ダブルクォーテーション(")を文字列として使用したい場合はエスケープする。  

IBM AI 7.2 'awk substr(String, Start, Count)'  
Actions >> Built-in Functions >> String Functions >> substr(String, Start, Count)  
[https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a171c125d__title__1](https://www.ibm.com/docs/en/aix/7.2.0?topic=awk-command#awk__a171c125d__title__1)  
StringのStartからCount数の文字列を取得する。Startは1から始まる。Countを省略した場合は最後まで。

## 行頭が指定した文字列に一致した行数を出力する

行頭が大文字小文字を無視してダブルクォーテーションで括られた '"apple"' だったらカウントする。
```
grep -i '^"apple"' filename.txt | wc -l
```

IBM AIX 7.2 'grep -i'  
[https://www.ibm.com/docs/en/aix/7.2.0?topic=g-grep-command#grep__row-d3e144040](https://www.ibm.com/docs/en/aix/7.2.0?topic=g-grep-command#grep__row-d3e144040)  
Red Had 'grep -i'  
[https://www.redhat.com/en/blog/find-text-files-using-grep](https://www.redhat.com/en/blog/find-text-files-using-grep)
