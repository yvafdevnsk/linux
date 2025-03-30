# シェルスクリプトで連想配列を使ってファイル名とファイルサイズのリストを出力する

## 環境

- Windows 11 Pro, 24H2, 26100.3476
- Windows Subsystem for Linux (WSL2), Ubuntu 24.04.2 (LTS), 5.15.167.4-microsoft-standard-WSL2

## 1. シェルスクリプトを用意する

```
$ touch bin/file_size_list.sh
$ chmod +x bin/file_size_list.sh
```

## 2. 任意のサイズのファイルを用意する

```
$ fallocate -l 1MB resource/sample_file_size_001.zip
$ fallocate -l 2MB resource/sample_file_size_002.zip
$ fallocate -l 3MB resource/sample_file_size_003.zip
```

## 3. シェルスクリプトでディレクトリ配下のファイルのリストを取得する

bin/file_size_list.sh

```
#!/bin/bash

# リストを取得するディレクトリを指定
directory="/home/mizuki/workspace/shell/resource"
# フィルタする拡張子を指定
extension="zip"

# ファイルのリストを配列に保存
file_list=($(find "${directory}" -type f -name "*.${extension}"))

# 配列の内容を表示
echo "ファイルリスト:"
for file in "${file_list[@]}"
do
    echo "${file}"
done
```

シェルスクリプトを実行する。

```
$ ./bin/file_size_list.sh
```

実行結果。

```
ファイルリスト:
/home/mizuki/workspace/shell/resource/sample_file_size_001.zip
/home/mizuki/workspace/shell/resource/sample_file_size_002.zip
/home/mizuki/workspace/shell/resource/sample_file_size_003.zip
```

## 4. シェルスクリプトでディレクトリ配下の拡張子のないファイル名のリストを取得する

bin/file_size_list.sh

```
#!/bin/bash

# リストを取得するディレクトリを指定
directory="/home/mizuki/workspace/shell/resource"
# フィルタする拡張子を指定
extension="zip"

# ファイルのリストを配列に保存
file_list=($(find "${directory}" -type f -name "*.${extension}"))

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
$ ./bin/file_size_list.sh
```

実行結果。

```
拡張子のないファイル名のリスト:
sample_file_size_001
sample_file_size_002
sample_file_size_003
```

## 5. シェルスクリプトで連想配列を使ってファイル名とファイルサイズのリストを出力する

bin/file_size_list.sh

```
#!/bin/bash

# リストを取得するディレクトリを指定
directory="/home/mizuki/workspace/shell/resource"
# フィルタする拡張子を指定
extension="zip"

# ファイルのリストを配列に保存
file_list=($(find "${directory}" -type f -name "*.${extension}"))

# 拡張子のないファイル名とファイルサイズを連想配列に保存
declare -A file_name_and_size_hash
for file in "${file_list[@]}"
do
    file_name_without_extension=$(basename "${file%.*}")
    file_size=$(stat -c%s "${file}")

    file_name_and_size_hash["${file_name_without_extension}"]="${file_size}"
done

# 連想配列のキーをソート
sorted_key_list=($(for key in "${!file_name_and_size_hash[@]}"; do echo "${key}"; done | sort))

# 連想配列の内容をタブ区切りで表示
echo "ファイルサイズのリスト:"
for key in "${sorted_key_list[@]}"
do
    echo -e "${key}\t${file_name_and_size_hash[${key}]}"
done
```

シェルスクリプトを実行する。

```
$ ./bin/file_size_list.sh
```

実行結果。

```
ファイルサイズのリスト:
sample_file_size_001    1000000
sample_file_size_002    2000000
sample_file_size_003    3000000
```
