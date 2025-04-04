# WSL2上のUbuntu24.04.2(LTS)にApache Maven 3.9.9をインストールする

## 環境

- Windows 11 Pro, 24H2, 26100.3476
- Windows Subsystem for Linux (WSL2), Ubuntu 24.04.2 (LTS), 5.15.167.4-microsoft-standard-WSL2
- Java 17 (LTS)

## 全体図

```
/
    +-home
    |     +-mizuki
    |           +-download
    |                 |-apache-maven-3.9.9-bin.tar.gz
    |
    +-opt
          +-apache-maven-3.9.9
          |     +-bin
          |           |-mvn
          |
          +-java
                +-jdk-17.0.14+7
                      +-bin
                            |-java
```

## 1. アーカイブファイルをダウンロードする

Apache >> [Maven](https://maven.apache.org/index.html) >> Downloads

```
Files

  Binary tar.gz archive | apache-maven-3.9.9-bin.tar.gz
```

apache-maven-3.9.9-bin.tar.gz

## 2. アーカイブファイルをWSL2上のUbuntuにコピーする

WSL2上のUbuntuからエクスプローラーを起動してファイルをコピーする。

```
$ explore.exe .
```

## 3. アーカイブファイルをインストールする

アーカイブファイルを展開する。

```
$ tar xzvf apache-maven-3.9.9-bin.tar.gz
```

展開したファイルを移動する。

```
$ sudo mv apache-maven-3.9.9/ /opt
$ sudo chown -R root:root /opt/apache-maven-3.9.9
```

Mavenのバージョンを確認する。

```
$ export JAVA_HOME=/opt/java/jdk-17.0.14+7
$ /opt/apache-maven-3.9.9/bin/mvn -v
```

実行結果

```
Apache Maven 3.9.9 (8e8579a9e76f7d015ee5ec7bfcdc97d260186937)
Maven home: /opt/apache-maven-3.9.9
Java version: 17.0.14, vendor: Eclipse Adoptium, runtime: /opt/java/jdk-17.0.14+7
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "5.15.167.4-microsoft-standard-wsl2", arch: "amd64", family: "unix"
```

参考情報

- Apache >> Maven >> [Installation](https://maven.apache.org/install.html)
