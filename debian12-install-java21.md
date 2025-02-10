# Debian12にJava21(LTS)をインストールする

## 環境
- VirtualBox 7.1.4 (2024/10/15)
- Debian 12.9 ([2025/01/11](https://www.debian.org/News/2025/20250111))
- Linux Kernel 6.1.0-30-amd64

## 1. VirtualBoxにDebianをインストールする

### 1-1. Debianをインストールする
1. VirtualBoxの仮想マシンメニューの新規でダウンロードしたDebianのISOイメージを選択する。
2. 自動インストールのユーザー名とパスワードを "mizuki" に変更する。
3. メモリサイズを8,192MBに変更する。
4. インストール後にキーボードを追加して "Japanese" に変更する。GNOMEデスクトップ / Activities / Settings / Keyboard / Input Sources / [+]

### 1-2. Guest Additionsをインストールする
rootユーザーでカーネルヘッダーをインストールする。

```
su -
apt update
apt install build-essential dkms linux-headers-$(uname -r)
```

rootユーザーでGuest Additionsをインストールする。  
VirtualBoxウインドウ / デバイスメニュー / Guest Additions CDイメージの挿入

```
mkdir /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom
sh ./VBoxLinuxAdditions.run --nox11
reboot
```

再起動後の確認。

```
lsmod | grep vboxguest
```

これで VirtualBoxウインドウ / デバイスメニュー / クリップボードの共有 / 双方向 が有効になる。

### 参考情報
- [How to check Debian version: the quick and easy way](https://www.ionos.com/digitalguide/server/know-how/how-to-check-debian-version/)
- [How to check os version in Linux command line](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)
- [Chapter 4. Guest Additions](https://www.virtualbox.org/manual/ch04.html#additions-linux)
- [How to Install VirtualBox Guest Additions on Debian 12: An Easy Approach](https://greenwebpage.com/community/how-to-install-virtualbox-guest-additions-on-debian-12/)

## 2. DebianにJavaをインストールする

### 2-1. Java21(LTS)をブラウザでダウンロードする
Adoptium / Other platforms and versions / [Eclipse Temurin Latest Releases](https://adoptium.net/temurin/releases/)
- Operating System: Linux
- Architecture: x64
- Package Type: JDK
- Version: 21 - LTS

21.0.5+11-LTS Temurin October 17, 2024

### 2-2. Java21(LTS)をインストールする
通常ユーザーでダウンロードしたパッケージを展開する。

```
cd /home/mizuki/Downloads
tar xvf OpenJDK21U-jdk_x64_linux_hotspot_21.0.5_11.tar.gz
```

rootユーザーで展開したパッケージを/usr/localに移動する。

```
su -
cd /home/mizuki/Downloads
mv jdk-21.0.5+11/ /usr/local/
```

rootユーザーで移動したパッケージの所有者とグループをrootにする。

```
cd /usr/local
chown -R root jdk-21.0.5+11/
chgrp -R root jdk-21.0.5+11/
```

rootユーザーでjavaコマンドが実行できることを確認する。

```
/usr/local/jdk-21.0.5+11/bin/java --version

openjdk 21.0.5 2024-10-15 LTS
OpenJDK Runtime Environment Temurin-21.0.5+11 (build 21.0.5+11-LTS)
OpenJDK 64-Bit Server VM Temurin-21.0.5+11 (build 21.0.5+11-LTS, mixed mode, sharing)
```

### 2-3. javaコマンドに別名を付けて実行できるようにする
通常ユーザーで作業を行う。bashのエイリアス定義ファイルを作成する。

```
cd /home/mizuki
touch .bash_aliases
```

VSCodeでエイリアス定義ファイルを開いてコマンドを追加する。

```
alias java21='/usr/local/jdk-21.0.5+11/bin/java'
```

ターミナルを開きなおしてエイリアス定義したコマンドが実行できることを確認する。

```
java21 --version

openjdk 21.0.5 2024-10-15 LTS
OpenJDK Runtime Environment Temurin-21.0.5+11 (build 21.0.5+11-LTS)
OpenJDK 64-Bit Server VM Temurin-21.0.5+11 (build 21.0.5+11-LTS, mixed mode, sharing)
```

### 参考情報
- [ファイルやディレクトリの所有者やグループを変更するには (2001/05/10)](https://atmarkit.itmedia.co.jp/flinux/rensai/linuxtips/078chown.html)
