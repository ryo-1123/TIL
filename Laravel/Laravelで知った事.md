
#### ------------laravel S3の接続方法----------------
- AWSのアカウント作成済み
- 

##### 実装方法
    ```composer require league/flysystem-aws-s3-v3をインストール  
    AWS_ACCESS_KEY_ID= Access key ID
    AWS_SECRET_ACCESS_KEY= Secret access key  
    AWS_DEFAULT_REGION=ap-northeast-1 
    AWS_BUCKET=　bucket名  
を.envファイルに記入  
 - Controllerの作成  
    - ポストで受け取ってS3に挿入 下記転載  
    ```public function upload(Request $request){ 
     $file = $request->file('file');
     // 第一引数はディレクトリの指定  
     // 第二引数はファイル  
     $path = Storage::disk('s3')->putFile('/', $file, 'public');  
     // hogeディレクトリにアップロード 
     // $path = Storage::disk('s3')->putFile('/hoge', $file, 'public');
     // ファイル名を指定する場合はputFileAsを利用する
     // $path = Storage::disk('s3')->putFileAs('/', $file, 'hoge.jpg', 'public');
     return redirect('/');
    }   
 - 表示方法  
    ```public function disp(){ 
         $path = Storage::disk('s3')->url('hoge.jpg');  
         return view('disp', compact('path'));`  
       } 
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
#### ----------------Imagickを使って画像のリサイズ-------------------
一方のpxに合わせて縦横比のそのまま縮小する方法    
->resize(width, height)
``` 
    $host_image->resize(null, 525, function ($constraint) {
        $constraint->aspectRatio();
    }); 
 ```
#### ---------------------------　おわり　---------------------------------
#### ------------paginateとinfinite Scrollを使用してもっと見るボタンの実装-----------------
1. Infinite Scroll(バージョン3)を読み込む
1. controllerを書く(下記参照)
   ```
    public function index(){
      $data['posts'] = Data::orderBy('created_at', 'desc')
          ->paginate(5);
      return view('index', $data);
    }
1. bladeを書く（下記参照）
    ```
     <ul class="host-review__list">
         @foreach ($data as $d)
             <li class="host-review__item"> // append
                 <div class="host-review__content">
                      <ul class="host-review__meta">
                          <li class="data-like data-like--profile"><span>{{$d->data}}</span></li>
                          <li class="host-review__date">
                              {{$d->data}}
                           </li>
                       </ul>
                       <p class="host-review__detail">{{$d->data}}</p>
                   </div>
               </li>
          @endforeach
       </ul>
       @if($comment->hasMorePages())
            <div class="text-center more">
                <a href="{{ $comment->nextPageUrl() }}" style="display:none;"></a> //これがpath
                <button class="btn btn-addiction moreBtn">もっと見る</button> // button部分
            </div>
        @endif
1. jsを書く(下記参照）  
   ``` $(function(){  
        let infScroll = new InfiniteScroll( ".host-review__list", {  // foreachの外側のclass名
            path : ".more a",  // urlを入れているclass名の指定
            append : ".host-review__item",  // foreachで回す中身のclass名
            button : ".moreBtn",  // ボタンのclass名
            scrollThreshold: false,  //  スクロールでinfiteが動作しないようにfalse
        }); 
    });
#### ---------------------------　おわり　---------------------------------

#### 　　　　　------------ scssの導入方法 -----------------
1. 前提として、npmがインストールされていること。


#### ---------------------------　おわり　---------------------------------
#### 　　　　　------------ leftjoinとjoinの違い -----------------
 - leftjoinについて
 
 - joinについて

#### ---------------------------　おわり　---------------------------------
