# CloudFormation
  * 

## 主な機能
  * 

## CloudFormation ヘルパースクリプト

### cfn-init

  * AWS CloudFormatinのメタデータの取得と解析
  * パッケージのインストール
  * ディスクへのファイル書き込み
  * サービスの有効化/無効化と開始/停止

#### 構文
```
cfn-init --stack|-s stack.name.or.id \
    --resource|-r logical.resource.id \
    --region region
    --access-key access.key \
    --secret-key secret.key \
    --role rolename\
    --credential-file|-f credential.file \
    --configsets|-c config.sets \
    --url|-u service.url \
    --http-proxy HTTP.proxy \
    --https-proxy HTTPS.proxy \
    --verbose|-v
```

#### インストール参考記事
  * パッケージでインストールする方穂が記載
    * https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/deploying.applications.html#deployment-walkthrough-lamp-install
  * コマンドでインストールする方法が記載
    * https://qiita.com/tomoki_s/items/f2c49bf8fdf8f2ffdfe5

### cfn-signal

### cfn-get-metadata

### cfn-hup

## 見積もり(料金)

## 所感
