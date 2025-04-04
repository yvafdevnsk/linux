# WSL2上のUbuntu24.04.2(LTS)にJava17(LTS)のJDKをインストールする

## 環境

- Windows 11 Pro, 24H2, 26100.3476
- Windows Subsystem for Linux (WSL2), Ubuntu 24.04.2 (LTS), 5.15.167.4-microsoft-standard-WSL2

## 全体図

```
/
    +-home
    |     +-mizuki
    |           +-download
    |                 |-OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz
    |
    +-opt
    |     +-java
    |           +-jdk-17.0.14+7
    |                 +-bin
    |                       |-java
    |
    +-usr
          +-local
                +-bin
                      |-java17
```

## 1. アーカイブファイルをダウンロードする

[Adoptium](https://adoptium.net/) >> Other platforms and versions

```
Eclipse Temurin Latest Releases

    Operating System: Linux
    Architecture: x64
    Package Type: JDK
    Version: 17 - LTS

        17.0.14+7 | Temurin | January 24, 2025
        Linux
        x64
        JDK - 191MB
        [download] .tar.gz
```

OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz

## 2. アーカイブファイルをWSL2上のUbuntuにコピーする

WSL2上のUbuntuからエクスプローラーを起動してファイルをコピーする。

```
$ explore.exe .
```

## 3. アーカイブファイルをインストールする

アーカイブファイルを展開する。

```
$ tar xzf OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz
```

展開したファイルを移動する。

```
$ sudo mkdir -p /opt/java
$ sudo mv jdk-17.0.14+7/ /opt/java
$ sudo chown -R root:root /opt/java/jdk-17.0.14+7
```

Javaのバージョンを確認する。

```
$ /opt/java/jdk-17.0.14+7/bin/java --version
```

実行結果

```
openjdk 17.0.14 2025-01-21
OpenJDK Runtime Environment Temurin-17.0.14+7 (build 17.0.14+7)
OpenJDK 64-Bit Server VM Temurin-17.0.14+7 (build 17.0.14+7, mixed mode, sharing)
```

javaコマンドに別名をつける。

```
$ sudo ln -s /opt/java/jdk-17.0.14+7/bin/java /usr/local/bin/java17
```

javaコマンドの別名でJavaのバージョンを確認する。

```
$ java17 --version
```

実行結果

```
openjdk 17.0.14 2025-01-21
OpenJDK Runtime Environment Temurin-17.0.14+7 (build 17.0.14+7)
OpenJDK 64-Bit Server VM Temurin-17.0.14+7 (build 17.0.14+7, mixed mode, sharing)
```

参考情報

- [Adoptium](https://adoptium.net/) >> Documentation >> Get Temurin >> Install Temurin >> [Archive files](https://adoptium.net/installation/archives/)
