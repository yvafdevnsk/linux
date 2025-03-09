# Windows11にWSL2でUbuntu24.04.2(LTS)をインストールする

## 環境

- Windows 11 Pro, 24H2, 26100.3194

## 1. WSL コマンドのインストール

コマンドプロンプトを管理者権限で起動してコマンドを実行する。  
スタートメニュー >> すべて >> Windowsツール >> コマンドプロンプト >> 右クリックして管理者として実行

```
wsl --install
```

```
ダウンロード中: Linux 用 Windows サブシステム 2.4.11
インストール中: Linux 用 Windows サブシステム 2.4.11
Linux 用 Windows サブシステム 2.4.11 はインストールされました。
Windows オプション コンポーネントをインストールしています: VirtualMachinePlatform

展開イメージのサービスと管理ツール
バージョン: 10.0.26100.1150

イメージのバージョン: 10.0.26100.3194

機能を有効にしています
[==========================100.0%==========================]
操作は正常に完了しました。
要求された操作は正常に終了しました。変更を有効にするには、システムを再起動する必要があります。
```

Windowsを再起動する。

### 参考情報

- [WSL を使用して Windows に Linux をインストールする方法 >> WSL コマンドのインストール](https://learn.microsoft.com/ja-jp/windows/wsl/install#install-wsl-command)

## 2. 実行している WSL のバージョンを確認する

コマンドプロンプトを管理者権限で起動してコマンドを実行する。  
スタートメニュー >> すべて >> Windowsツール >> コマンドプロンプト >> 右クリックして管理者として実行

```
wsl -l -v
```

```
Linux 用 Windows サブシステムにインストールされているディストリビューションはありません。
この問題を解決するには、以下の手順に従ってディストリビューションをインストールしてください:

'wsl.exe --list --online' を使用して利用可能な配布を一覧表示する
および 'wsl.exe --install <Distro>' を使用してインストールしてください。
```

まだディストリビューションはインストールされていない。利用可能なディストリビューションを表示する。

```
wsl --list --online
```

```
インストールできる有効なディストリビューションの一覧を次に示します。
'wsl.exe --install <Distro>' を使用してインストールします。

NAME                            FRIENDLY NAME
Debian                          Debian GNU/Linux
SUSE-Linux-Enterprise-15-SP5    SUSE Linux Enterprise 15 SP5
SUSE-Linux-Enterprise-15-SP6    SUSE Linux Enterprise 15 SP6
Ubuntu                          Ubuntu
Ubuntu-24.04                    Ubuntu 24.04 LTS
kali-linux                      Kali Linux Rolling
openSUSE-Tumbleweed             openSUSE Tumbleweed
openSUSE-Leap-15.6              openSUSE Leap 15.6
Ubuntu-18.04                    Ubuntu 18.04 LTS
Ubuntu-20.04                    Ubuntu 20.04 LTS
Ubuntu-22.04                    Ubuntu 22.04 LTS
OracleLinux_7_9                 Oracle Linux 7.9
OracleLinux_8_7                 Oracle Linux 8.7
OracleLinux_9_1                 Oracle Linux 9.1
```

既定では、[インストールされる Linux ディストリビューションは Ubuntu](https://learn.microsoft.com/ja-jp/windows/wsl/install#change-the-default-linux-distribution-installed) になる。2回目のインストールコマンドをディストリビューションを指定しないで実行する。

```
wsl --install
```

```
ダウンロード中: Ubuntu
インストール中: Ubuntu
ディストリビューションが正常にインストールされました。'wsl.exe -d Ubuntu' を使用して起動できます
```

インストールしたディストリビューションを起動する。

```
wsl -d Ubuntu
```

ユーザー名とパスワードを入力する。既定ではWindowsのユーザー名になっている。

```
Provisioning the new WSL instance Ubuntu
This might take a while...
Create a default Unix user account: <Windowsのユーザー名>
New password:
Retype new password:
passwd: password updated successfully
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 24.04.2 LTS (GNU/Linux 5.15.167.4-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sat Mar  8 21:36:03 JST 2025

  System load:  0.0                 Processes:             31
  Usage of /:   0.1% of 1006.85GB   Users logged in:       0
  Memory usage: 3%                  IPv4 address for eth0: <IPアドレス>
  Swap usage:   0%


This message is shown once a day. To disable it please create the
/home/<ユーザー名>/.hushlogin file.
<ユーザー名>@<パソコン名>:/mnt/c/Windows/System32$
```

WSLを終了する。

```
<ユーザー名>@<パソコン名>:/mnt/c/Windows/System32$ logout
```

実行している WSL のバージョンを確認する。

```
wsl -l -v
```

```
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
```

### 参考情報

- [WSL を使用して Windows に Linux をインストールする方法 >> 実行している WSL のバージョンを確認する](https://learn.microsoft.com/ja-jp/windows/wsl/install#check-which-version-of-wsl-you-are-running)
- [WSL 開発環境を設定する >> Linux のユーザー名とパスワードを設定する](https://learn.microsoft.com/ja-jp/windows/wsl/setup/environment#set-up-your-linux-username-and-password)

## 3. Windows TerminalでWSLを実行する

ターミナルを起動する。  
スタートメニュー >> すべて >> ターミナル  
  
タブの横のドロップダウンアイコン(▼)からUbuntuを選択する。  
  
![Windows Terminal ドロップダウンメニュー](/image/windows11-wsl-install-ubuntu24-1.png)

WSLを終了する。タブが1つの場合はWindows Terminalのウインドウが閉じる。

```
<ユーザー名>@<パソコン名>: $ logout
```

### 参考情報

- [WSL 開発環境を設定する >> Windows Terminalの設定](https://learn.microsoft.com/ja-jp/windows/wsl/setup/environment#set-up-windows-terminal)
