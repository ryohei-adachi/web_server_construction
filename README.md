# アクセス制限設定をしたWebサーバの作り方

## ①EC2インスタンスの準備

+ AWSにログインしてください。

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/4c8cec3b-b46a-4e04-a90c-f5378e9f63a6" width="70%" />

<br><br>

+ AWSのマジメントコンソールを開き、リージョンを選択してください。
  + 本手順書では、「東京」リージョンに選択したうえで進めます。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/a5490b76-9d0d-4306-a100-3adc8b9fc3b4" width="70%" />

<br><br>

+ マネジメントコンソールの検索部分に「EC2」と入力・検索を行い、EC2サービスを選択してください。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/2704af17-6537-492f-bd55-61f00c010825" width="70%" />

<br><br>

+ 左メニューの「インスタンス」を選択して、「インスタンスを起動」をクリックしてください。

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/17c66e10-3095-41e4-9baf-dd799a6c5549" width="70%" />

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/6025e6ac-d8d2-4207-a74b-96a10b8a4af5" width="70%" />

<br><br><br>

+ 「Launch an instance」では下記の内容を入力・選択する
  + 名前: 好きなサーバ名を入力してください
  + Amazon マシンイメージ (AMI): Amazon Linux 2023
  + インスタンスタイプ: t2.micro
  + ネットワークの設定: パブリックIPの自動割り当てを「無効」から「有効」に変えてください
  + キーペア (ログイン) : 新しいキーペアの作成をクリックして、キーペアを取得してください(ログイン時に使用)
    +  キーペア名: 好きな名前を入力してください
    +  キーペアのタイプ: RSA
    +  プライベートキーファイル形式: 「.pem」

<br><br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/05e292b4-55e6-48bd-addd-05e5a4ee682a" width="70%" />

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/26f81d98-07d2-482e-b37f-545b5e72b1cb" width="70%" />

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/ecc10718-aa70-4d39-b9fa-bd925ce0855b" width="70%" />

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/71602f1e-f76d-49af-9ba6-c0a4c516c517" width="70%" />

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/fa4d93e0-3cdd-4857-861f-9e032e38ca76" width="70%" />

<br><br>

必要項目を入力をした後、「キーペアを作成」をクリックしてください。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/63254121-0875-47eb-a8c0-6c0e7958e3bd" width="70%" />

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/0a398555-165a-44e3-a2e3-99fe68664a05" width="70%" />

<br><br>

ネットワークの設定ボタンをクリックし、パブリックIPの自動割り当てを「有効」に変えてください

+ 上記の内容の入力が出来ましたら、「インスタンスを起動」をクリックしてください。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/027944c3-56a1-4af5-b2fb-926cc96ef7a5" width="70%" />

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/84c14c41-10c4-4426-8f9f-7f7cee0712c2" width="70%" />

<br><br>

+ 「すべてインスタンスを表示」を選択して、EC2インスタンスが作成されていることを確認しましょう。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/96175ed0-8179-4b81-b0be-32a5dbdd3809" width="70%" />

<br><br>

## ② セキュリティグループの設定

+ 作成したインスタンスを選択した状態で、「セキュリティタブ」を開き、セキュリティグループを選択してください。
  + 「sg-XXXXXXXXX」というセキュリティグループIDがEC2インスタンスに付与されております。   

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/c163515d-366b-4ae7-a47d-61923e0f4bad" width="70%" />

+ セキュリティグループの設定画面を開き、「インバウンドのルールを編集」をクリックしてください。

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/cfcaf86d-227c-4f50-90c6-7172533f248a" width="70%" />

<br><br>

+ 以下のルールを追加して、「ルールを保存」ボタンをクリックしてください。

    + タイプ: SSH
    + ソース: Anywhere-IPv4 か マイIP (マイIPのほうが望ましい。)
    + タイプ: HTTP
    + ソース: Anywhere-IPv4

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/bc998a8c-580f-4b45-97e2-cb4e502a502d" width="70%" />

<br><br>

## ③EC2インスタンスにssh接続する

+ EC2インスタンスのパブリックIPアドレスを確認してください。

<br><br>

 <img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/6a27c26e-1eb6-49d9-b2f4-053dac0427c5" width="70%" />

 + Tera Term5を開き、ホストにEC2インスタンスのパブリックIPアドレスを入力して、OKをクリックする。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/1f36595f-3aec-4143-8288-d1e1b2609e94" width="50%" />

<br><br>

+ 続行をクリックする。

<br><br>

 <img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/3070e9e4-f7dd-497b-807a-747d28b307b9" width="50%" />

<br><br>

+ ユーザ名に「ec2-user」、認証方式は「RSA/DSA/ECDSA/ED25519鍵を使う」を選択してください。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/7a691b46-2d8f-47bc-8bbb-70d53c1e29a9" width="50%" />

<br><br>
+ 秘密鍵の個所に、EC2インスタンス作成時に生成したキーペア(.pemファイル)を指定してください。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/4e289e7a-a83b-4b6f-a47d-58174764999e" width="50%" />

<br><br>

 <img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/e7aca950-e189-40d5-b891-4b692663c529" width="50%" />

<br><br>

 <img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/67ca8b9b-dd7f-4dff-b31a-87c053732fea" width="50%" />

+ 「OK」をクリックして、EC2インスタンスに入れることを確認してください。

<br><br>

<img src="https://github.com/ryohei-adachi/aws_lecture/assets/75190594/f8dfc541-f812-4cff-8cfa-a1c0a4065b10" width="50%" />

<br><br>

+ ec2-userからrootユーザに切り換えるため、以下のコマンドを入力してください

```
sudo su -
```

<img src="https://github.com/user-attachments/assets/703389fa-2da9-4176-8d8b-56d5413256e6" width="50%" />


## ④Apacheのインストール

+ ターミナルに下記のコマンドを実行してください。

<br><br>

```
dnf update
dnf install -y httpd 
```

<br><br>
