# シェルスクリプトで拡張子なしのファイル名を取得する

## 環境

- Windows 11 Pro, 24H2, 26100.3476
- Windows Subsystem for Linux (WSL2), Ubuntu 24.04.2 (LTS), 5.15.167.4-microsoft-standard-WSL2

## 1. シェルスクリプトを用意する

```
$ touch bin/file_name_list.sh
$ chmod +x bin/file_name_list.sh
```

## 2. ファイルを用意する

```
$ touch resource/sample_file_name_001.txt
$ touch resource/sample_file_name_002.txt
$ touch resource/sample_file_name_003.txt
```

## 3. シェルスクリプトでディレクトリ配下のファイルのリストを取得する

bin/file_name_list.sh

```
#!/bin/bash

# リストを取得するディレクトリを指定
directory="/home/mizuki/workspace/shell/resource"

# ファイルのリストを配列に保存
file_list=($(ls "${directory}"))

# 配列の内容を表示
echo "ファイルリスト:"
for file in "${file_list[@]}"
do
    echo "${file}"
done
```

シェルスクリプトを実行する。

```
$ ./bin/file_name_list.sh
```

実行結果。

```
ファイルリスト:
sample_file_name_001.txt
sample_file_name_002.txt
sample_file_name_003.txt
```

## 4. シェルスクリプトでディレクトリ配下の拡張子のないファイル名のリストを取得する

bin/file_name_list.sh

```
#!/bin/bash

# リストを取得するディレクトリを指定
directory="/home/mizuki/workspace/shell/resource"

# ファイルのリストを配列に保存
file_list=($(ls "${directory}"))

# 拡張子のないファイル名を配列に保存
file_name_without_extension_list=()
for file in "${file_list[@]}"
do
    file_name_without_extension_list+=($(basename "${file%.*}"))
done

# 配列の内容を表示
echo "拡張子のないファイル名のリスト:"
for file in "${file_name_without_extension_list[@]}"
do
    echo "${file}"
done
```

シェルスクリプトを実行する。

```
$ ./bin/file_name_list.sh
```

実行結果。

```
拡張子のないファイル名のリスト:
sample_file_name_001
sample_file_name_002
sample_file_name_003
```
