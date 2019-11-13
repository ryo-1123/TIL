## ------------AUTO LOGIN の基本,実装方法----------------
- 通常のログインの場合、デフォルトでsessionの時間は２時間
- オートログインを実装すると、ユーザがログアウトしない限り、約5年維持することが出来る。

#### 実装方法
>zzzz
>ssss
>sssa
>
>
>

## ------------laravel S3の接続方法----------------
- AWSのアカウント作成済み
- 

#### 実装方法
 - ` composer require league/flysystem-aws-s3-v3` をインストール  
 - `AWS_ACCESS_KEY_ID= Access key ID`  
`AWS_SECRET_ACCESS_KEY= Secret access key`  
`AWS_DEFAULT_REGION=ap-northeast-1`  
`AWS_BUCKET=　bucket名`  
を.envファイルに記入  
 - Controllerの作成  
  - ポストで受け取ってS3に挿入 下記の転載  
  `public function upload(Request $request){`  
    `$file = $request->file('file');`  
    `// 第一引数はディレクトリの指定`  
    `// 第二引数はファイル`  
    `$path = Storage::disk('s3')->putFile('/', $file, 'public');`  
    `// hogeディレクトリにアップロード`  
    `// $path = Storage::disk('s3')->putFile('/hoge', $file, 'public');`  
    `// ファイル名を指定する場合はputFileAsを利用する`  
    `// $path = Storage::disk('s3')->putFileAs('/', $file, 'hoge.jpg', 'public');`  
    `return redirect('/');`  
 `}`  
 - 表示方法  
  - `public function disp(){`  
    `$path = Storage::disk('s3')->url('hoge.jpg');`  
    `return view('disp', compact('path'));`  
 `}`  
 
