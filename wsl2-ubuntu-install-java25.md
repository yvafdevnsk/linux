# WSL2上のUbuntu24.04.3(LTS)にJava25(LTS)のJDKをインストールする

## 環境

- Windows 11 Pro, 24H2, 26100.7462
- Windows Subsystem for Linux (WSL2), Ubuntu 24.04.3 (LTS), 6.6.87.2-microsoft-standard-WSL2

## 全体図

```
/
    +-home
    |     +-mizuki
    |           +-download
    |                 |-OpenJDK25U-jdk_x64_linux_hotspot_25.0.1_8.tar.gz
    |
    +-opt
    |     +-java
    |           +-jdk-25.0.1+8
    |                 +-bin
    |                       |-java
    |
    +-usr
          +-local
                +-bin
                      |-java25
```

## 1. WSLの環境を最新に更新する

WSLバージョンを最新バージョンに更新する。PowerShell上で以下のコマンドを実行する。UACのダイアログが出る。実行中のUbuntuのタブは先に閉じておく。

```
wsl --update
```

実行結果。

```
更新プログラムを確認しています。
Linux 用 Windows サブシステムをバージョンに更新しています: 2.6.3。
```

WSLとそのコンポーネントに関するバージョン情報を確認する。PowerShell上で以下のコマンドを実行する。

```
wsl --version
```

実行結果。

```
WSL バージョン: 2.6.3.0
カーネル バージョン: 6.6.87.2-1
WSLg バージョン: 1.0.71
MSRDC バージョン: 1.2.6353
Direct3D バージョン: 1.611.1-81528511
DXCore バージョン: 10.0.26100.1-240331-1435.ge-release
Windows バージョン: 10.0.26100.7462
```

ディストリビューションのパッケージの更新とアップグレードを実施する。Ubuntu上で以下のコマンドを実行する。

```
sudo apt update && sudo apt upgrade
```

参考情報。

- [Update WSL | Basic commands for WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#update-wsl)
- [Check WSL version | Basic commands for WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#check-wsl-version)
- [Update and upgrade packages | Set up a WSL development environment | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/setup/environment#update-and-upgrade-packages)

## 2. Javaのアーカイブファイルをダウンロードする

[Adoptium](https://adoptium.net/) >> Other Downloads >> JDK 25 - LTS

```
Linux >> x64 >> JDK >> Temurin 25.0.1+8-LTS - 10/25/2025
```

OpenJDK25U-jdk_x64_linux_hotspot_25.0.1_8.tar.gz

## 3. JavaのアーカイブファイルをWSL2上のUbuntuにコピーする

WSL2上のUbuntuからエクスプローラーを起動してPC側からファイルをコピーする。

```
cd /home/mizuki/download
explorer.exe .
```

## 4. Javaのアーカイブファイルをインストールする

Javaのアーカイブファイルを展開する。

```
tar xzf OpenJDK25U-jdk_x64_linux_hotspot_25.0.1_8.tar.gz
```

展開したファイルを移動する。

```
sudo mkdir -p /opt/java
ls -al /opt/java
sudo mv jdk-25.0.1+8/ /opt/java
ls -al /opt/java
sudo chown -R root:root /opt/java/jdk-25.0.1+8
ls -al /opt/java
```

Javaのバージョンを確認する。

```
/opt/java/jdk-25.0.1+8/bin/java --version
```

実行結果

```
openjdk 25.0.1 2025-10-21 LTS
OpenJDK Runtime Environment Temurin-25.0.1+8 (build 25.0.1+8-LTS)
OpenJDK 64-Bit Server VM Temurin-25.0.1+8 (build 25.0.1+8-LTS, mixed mode, sharing)
```

javaコマンドに別名をつける。

```
sudo ln -s /opt/java/jdk-25.0.1+8/bin/java /usr/local/bin/java25
```

javaコマンドの別名でJavaのバージョンを確認する。

```
java25 --version
```

実行結果

```
openjdk 25.0.1 2025-10-21 LTS
OpenJDK Runtime Environment Temurin-25.0.1+8 (build 25.0.1+8-LTS)
OpenJDK 64-Bit Server VM Temurin-25.0.1+8 (build 25.0.1+8-LTS, mixed mode, sharing)
```

参考情報

- [Adoptium](https://adoptium.net/) >> Documentation >> Get Temurin >> Install Temurin >> [Archive files](https://adoptium.net/installation/archives/)
