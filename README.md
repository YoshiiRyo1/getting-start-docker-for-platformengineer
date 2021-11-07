# はじめてのDocker for インフラエンジニア

Docker に触れたことがないインフラエンジニア向けに勉強会を開催しました。  
ローカルで Docker を動かし、インフラっぽい動作確認を行い、Amazon ECS で動かすところまでを紹介します。  


## Cloud9 ロールの作成

EC2 インスタンスプロファイルです。Cloud9 のインスタンスで使用します。   
ロール名は EC2Cloud9Role としました。(任意に変更してOK)  

マネジメントコンソール <a href="https://console.aws.amazon.com/iam/home?region=ap-northeast-1#/roles" target="_blank">IAM ロール</a> を開きます。  

**ロールの作成** をクリックします。  

**ユースケースの選択** → **一般的なユースケース** → **EC2** を選択して、**次のステップ** へ進みます。  

Attach アクセス権限ポリシー画面で割り当てるポリシーは以下です。  

 * AmazonEC2ContainerRegistryFullAccess
 * ElasticLoadBalancingFullAccess
 * AmazonECS_FullAccess
 * IAMFullAccess

このあとはロール名を入力してロールを作成します。  

## Cloud9 インスタンスの作成

<a href="https://ap-northeast-1.console.aws.amazon.com/cloud9/home" target="_blank">Cloud9 画面</a> を開きます。  

**Create environment** をクリックします。  

任意の名称を入力して、次へ進みます。  

Environment Type は **Create a new EC2 instance for environment (direct access)** を選択します。  

**Network Setting** を展開して、任意の VPC とパブリックサブネットを選択します。  

## インスタンスプロファイルの作成
<a href="https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Instances:" target="_blank">EC2 インスタンス画面</a> を開くと **aws-cloud9-xxxx** というインスタンスが起動しているはずです。  
そのインスタンスに前の手順で作成したインスタンスプロファイルを関連付けます。    

## AMTC 無効
AWS Managed Temporary Credentials (AMTC) を無効にしておきます。  
無効にする方法は以下に詳しいです。  

[Cloud9からIAM Roleの権限でAWS CLIを実行する](https://hatenablog-parts.com/embed?url=https://dev.classmethod.jp/articles/execute-aws-cli-with-iam-role-on-cloud9/)

## Dockerのインストール

**これ以降は Cloud9 で手順を実行してください。**  

まずは docker がインストールされていることの確認をします。

```
docker -v
```

万が一インストールされていなければインストールします。

```
sudo yum install -y docker
```

## Nginxを動かしてみよう

[bash]
docker run --name web -d -p 80:80 nginx
[/bash]


```
curl http://localhost/
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```