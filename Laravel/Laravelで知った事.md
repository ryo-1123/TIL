
#### ------------laravel S3の接続方法----------------
- AWSのアカウント作成済み
- 

##### 実装方法
 - ` composer require league/flysystem-aws-s3-v3` をインストール  
 - `AWS_ACCESS_KEY_ID= Access key ID`  
`AWS_SECRET_ACCESS_KEY= Secret access key`  
`AWS_DEFAULT_REGION=ap-northeast-1`  
`AWS_BUCKET=　bucket名`  
を.envファイルに記入  
 - Controllerの作成  
    - ポストで受け取ってS3に挿入 下記転載  
    `public function upload(Request $request){`  
    ` $file = $request->file('file');`  
    ` // 第一引数はディレクトリの指定`  
    ` // 第二引数はファイル`  
    ` $path = Storage::disk('s3')->putFile('/', $file, 'public');`  
    ` // hogeディレクトリにアップロード`  
    ` // $path = Storage::disk('s3')->putFile('/hoge', $file, 'public');`  
    ` // ファイル名を指定する場合はputFileAsを利用する`  
    ` // $path = Storage::disk('s3')->putFileAs('/', $file, 'hoge.jpg', 'public');`  
    ` return redirect('/');`  
    `}`   
 - 表示方法  
    - `public function disp(){`  
    ` $path = Storage::disk('s3')->url('hoge.jpg');`  
    ` return view('disp', compact('path'));`  
    `}`  
 
#### ------------Imagickを使って画像の回転を修正してstorageに保存----------------
1. imageのインストール `composer require intervention/image `
1. config/app.php の provider & aliases に imageを追加
1. imagickを使用するために
    1. `php artisan vendor:publish`
    1. 上記を実行するとconfig/image.phpが出来るので、その中身を修正（ gd -> imagick ）
1. store処理を書いて終了
    1. postされた画像を変数に入れる `$file = $request->file('name名');`
    1. DBに保存するpathの生成 `$store_path = 'public/'.$file->hashName();`
    1. 実際にストレージに入れるフルパス指定 `$full_path = storage('app/'.$store_path);`
    1. imagickを使用して画像の生成から保存まで
        1. 修正するためにImageで画像を読み込む `$store_image = Image::make($file);`
        1. 回転の修正 `$store_image->orientate();`
        1. 画像の保存 `$store_image->save($store_path);`
    1. DBにpathを保存 (Table -> $users->user_image) `$users->user_image = $image_path;`
    1. DB保存 `$users->save();`
1. 表示方法 `{{asset(str_replace('public/', '/storage/', $host->host_image))}}`
#### ---------------------------　おわり　---------------------------------
#### ------------paginateとinfinite Scrollを使用してもっと見るボタンの実装-----------------
1. Infinite Scroll(バージョン3)を読み込む
1. jsを書く(下記参照)
    $(function(){
        let infScroll = new InfiniteScroll( ".host-review__list", {
            path : ".more a",
            append : ".host-review__item",
            button : ".moreBtn",
            scrollThreshold: false,
        });
    });
1. 
1. 
1. 
1. 
#### ---------------------------　おわり　---------------------------------
