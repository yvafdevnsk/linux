# AWS EC2インスタンスにWSL2上のUbuntuからSCPでファイルをコピーする

## 環境

- Windows 11 Pro, 24H2, 26100.3476
- Windows Subsystem for Linux (WSL2), Ubuntu 24.04.2 (LTS), 5.15.167.4-microsoft-standard-WSL2

## 1. AWSアカウントにログインする

左上のサービスアイコン >> すべてのサービス >> E >> EC2 (クラウド内の仮想サーバー)  
EC2のダッシュボードが表示される。

## 2. EC2インスタンスを起動する

ファイアウォール(セキュリティグループ)用に自分のグローバルIPアドレスをWSL2上のUbuntuで確認する。

```
~$ curl ifconfig.me
```

黄色の「インスタンスを起動」ボタンを押す。

```
EC2 >> インスタンス >> インスタンスを起動

    名前とタグ
        SSH Test
    アプリケーションおよびOSイメージ(Amazonマシンイメージ)
        クイックスタート
            Amazon Linux
            Amazonマシンイメージ(AMI)
                Amazon Linux 2023 AMI 無料利用枠の対象
                    アーキテクチャ: 64ビット(x86)
                    Publish Date: 2025-03-04
                    ユーザー名: ec2-user
    インスタンスタイプ
        t2.micro 無料利用枠の対象
    キーペア(ログイン)
        キーペア名: SSH_TEST_KEY
        新しいキーペアの作成
            キーペア名: SSH_TEST_KEY
            キーペアのタイプ: ED25519 (よりセキュア)
            プライベートキーファイル形式: .pem (OpenSSH)
        キーペアを作成ボタンを押すとファイルがダウンロードされる。
    ネットワーク設定
        パブリックIPの自動割り当て: 有効化
        ファイアウォール(セキュリティグループ)
            [x]セキュリティグループを作成
            次のルールを使用して、「launch-wizard-1」という新しいセキュリティグループを作成します。
            [x]からのSSHトラフィックを許可: <自分のグローバルIPアドレス>

インスタンスを起動ボタンを押す。
```

右下のすべてのインスタンスを表示ボタンを押す。

```
EC2 >> インスタンス

    インスタンスを選択してパブリックIPv4アドレスを確認する。
```

## 3. WSL2上のUbuntuからSSHでEC2インスタンスに接続する

ホームディレクトリに.sshディレクトリを作成する。

```
~$ mkdir .ssh
```

.sshディレクトリにキーペアのファイル(.pem)をWindows上からコピーする。コピーしたあとにWSL2上に作成されたZone.Identifierファイルは削除する。

```
~$ cd .ssh
~/.ssh$ explorer.exe .
```

キーペアのファイルの権限を変更する。

```
~/.ssh$ chmod 600 SSH_TEST_KEY.pem
```

SSHで詳細情報を出力してEC2インスタンスに接続する。

```
~$ ssh -v -i ~/.ssh/SSH_TEST_KEY.pem ec2-user@<EC2インスタンスのパブリックIPv4アドレス>
```

## 4. WSL2上のUbuntuからEC2インスタンスにSCPでファイルをコピーする

EC2インスタンス上にコピー先のディレクトリを作成する。

```
[ec2-user@ip ~]$ mkdir upload
[ec2-user@ip ~]$ ls -al
```

```
drwxr-xr-x. 2 ec2-user ec2-user   6 Mar 20 06:11 upload
```

Windows Terminalで2つ目のWSL2上のUbuntuのタブを開く。EC2インスタンスにコピーするテキストファイルを作成する。

```
~/workspace/aws-ec2$ uname -a > wsl2-ubuntu-uname.txt
~/workspace/aws-ec2$ cat wsl2-ubuntu-uname.txt
```

SCPでEC2インスタンス上のuploadディレクトリにファイルをコピーする。

```
~/workspace/aws-ec2$ scp -i ~/.ssh/SSH_TEST_KEY.pem wsl2-ubuntu-uname.txt ec2-user@<EC2インスタンスのパブリックIPv4アドレス>:/home/ec2-user/upload
```

```
wsl2-ubuntu-uname.txt    100%  123     0.7KB/s   00:00
```

EC2インスタンス側でコピーされたファイルを確認する。

```
[ec2-user@ip upload]$ ls -al
```

```
-rw-r--r--. 1 ec2-user ec2-user 123 Mar 20 06:32 wsl2-ubuntu-uname.txt
```

```
[ec2-user@ip upload]$ cat wsl2-ubuntu-uname.txt
```

## 5. EC2インスタンスを削除する

EC2インスタンスからログアウトする。

```
[ec2-user@ip upload]$ logout
```

EC2インスタンスを削除する。一覧から消えるには時間が掛かる。

```
EC2 >> インスタンス

    1. インスタンスを選択する。
    2. 右上の「インスタンスの状態」から「インスタンスを終了(削除)」を選択する。
```
