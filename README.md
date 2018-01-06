* 自己學習Laravel的筆記，非教學手冊，僅供參考

## 安裝PHP & Laravel & MySQL (Mac OSX)
### Mac OS: El Capitan 10.11
1. Terminal安裝Oh-My-Zsh
2. 安裝Xcode套件 (Command line tool)
*   安裝xcode commmand line tool (安裝完先打開xcode確認)
```
xcode-select --install
```
*   檢查是否安裝成功
```
xcode-select -p
```

3. 安裝Homebrew (OSX套件管理系統，類似apt-get)
*   http://brew.sh/
*   注意：可能會跟macports衝突
```
/usr/bin/ruby -e "$(curl -fsSLhttps://raw.githubusercontent.com/Homebrew/install/master/install)"
```
*   檢查是否安裝成功
```
brew
```

4. 升級PHP 7(最新的Mac OS: High Sierra已經內建PHP7.1，如果作業系統是最新，可不用升級PHP 7)
*   http://php-osx.liip.ch
```
curl -s https://php-osx.liip.ch/install.sh | bash -s 7.0
```
*   自動安裝script執行完成後，如果PHP仍是5.6，則需手動修改$PATH設定環境目錄
```
php --version
```   
```
vim .zshrc
```
*   游標移到最下面，打下列兩行
```
# PHP 7
export PATH=/usr/local/php5/bin:$PATH
# Tinker
alias tinker=”php artisan tinker”
```
*   檢查路徑是不是正確
```
which php
```
5.  安裝MySQL
*   檢查系統是否健康
```
brew doctor
```
*   若發生連結錯誤，可能需要更改權限後執行(不可直接sudo brew)
```
brew update
brew install mysql
brew link mysql
```
*   若發生連結錯誤，可能需要更改權限後執行(不可直接sudo brew)
*   加強mysql安全（建議必須設定）
*   若密碼無法輸入可取消（密碼設定有規定，可上網查）
```
mysql_secure_installation
```
6.  控制mysql
*   每次重新開機啟動mysql
*   mysql.server stop (停止msyql)
*   測試連線
*   -p 是需要密碼，沒有密碼就不用打 -p
```
mysql.server start
mysql -u root -p
```

7. 安裝composer
*   PHP套件管理器
*   https://getcomposer.org/download/
*   安裝完後會得到composer.phar檔案

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === 'aa96f26c2b67226a324c27919f1eb05f21c248b987e6195cad9690d5c1ff713d53020a02ac8c217dbf90a7eacc9d141d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

8.  安裝Laravel
*   test-project是專案名稱，可更改
```
composer create-project --prefer-dist laravel/laravel test-project
```

##  Artisan & Laravel目錄結構簡介
### 如何啟動Laravel開發伺服器 (使用Artisan)
*   要先切換到Laravel專案底下
*   這裡使用編輯器來開啟
*   檢查artisan
```
php artisan
```
```
php artisan serve
```
*   以下參考用，有需要再下指令
```
php artisan serve --host=0.0.0.0
php artisan serve --port=3000
localhost:8000
```
*   如果安裝失敗，確定初始環境正確？
```
打開Laravel專案目錄夾，檢查 .env設定檔是否存在？
否，執行cp.env.example .env
```
*   開啟Laravel server 後出現錯誤，執行下列指令
```
php artisan key:gen
```
*   產生金鑰 (在.env裡面的APP_KEY可以看到)

### Laravel的入口: Artisan
*   Artisan工具可以幫助你完成許多事情，包括：
    *   遷移(migrate)資料庫
    *   開啟維修模式
    *   檢視Route
    *   自動產生程式碼

### Laravel目錄結構
1.  app
    *   App\
        *   Model.php (底線表示資料庫物件名稱 ex: user.php)
        *   Http\
            *   Controller\
            *   Middleware\
                *   防護或者權限檢查的時候會用到的
2.  bootstrap
*   啟動應用程式用，並非前端CSS套件
3.  config
*   所有設定檔，常用包括：
    *   app.php設定時區
    *   config/app.php
    *   'timezone' =>'UTC', 改成'timezone' =>'Asia/Taipei',
    *   services.php設定外部服務
    *   mail.php設定寄信服務
4.  database
    *   factories/製造資料庫(假)資料 (flicker套件)
    *   migration/定義資料表，方便artisan遷移(migrate)
    *   seeds/定義初始資料表資料
5.  public
    *   網站根目錄，整個public資料夾向網際網路公開
    *   可存放css, js和圖片等靜態資源
    *   public/index.php為Laravel進入點，不可移除
6.  resources
    *   assets/
        *   存放靜態JS, CSS Library，供gulp前端處理器讀取後編譯成app.js / app.css
    *   lang/
        *   多語系檔
    *   views/
        *   Blade HTML Template存放處，副檔名為name.blade.php
        *   "name"是可以替換的命名
7.  routes
    *   Laravel 5.3之後新增資料夾，內有：
        *   web.php處理使用者瀏覽相關的Route
        *   api.php專門處理API路由，有特殊middleware (中介層)
        *   console.php處理自訂artisan命令
8.  storage
    *   app/
        *   主要用以儲存使用者上傳的檔案，如須公開存取，可使用php artisan storage:link產生一個捷徑連至public/storage
    *   framework
        *   用以存放Laravel執行期間自動產生的資料
    *   logs
        *   用以存放laravel.log，記錄所有log::的輸出
9.  tests
    *   存放所有單元測試(unit test)
    *   使用PHPUnit，有興趣可google關鍵字unit test, TDD
10. vendor
    *   此資料夾不會被git紀錄
    *   所有composer install下載的套件會被存放在這邊，laravel框架原始檔也在這裡
11. 「.env」 and 「.env.example」
    *   .env是環境設定檔
    *   env不會被git紀錄，用以存放資料庫密碼等環境設定
    *   執行期間使用env()函數可取得設定值
12. composer.json
    *   composer會自動讀取該檔案
    *   該檔案紀錄所有必須被composer install安裝的相依套件
    *   composer.lock為composer自動產生的進度檔，如果選擇讓git紀錄該檔案，則可保證每次composer install的結果都一模一樣
13. gulpfile.js
    *   前端處理工具gulp設定檔案，用以自動化處理js, css
14. package.json
    *   nap (node package manager)設定檔案
    *   定義所有npm install需要安裝的相依套件
    *   通常使用npm來安裝gulp等前端處理工具
15. phpunit.xml
    *   PHP單元測試設定檔
16. server.php
    *   用以啟動server
    *   通常不必更改

##  處理網址及參數: Routing
### 舊網址的寫法
*   localhost:8000/admin/user.php
### 去除這種觀念，使用Routing路由的想法
*   /user   ->UserController
*   /log    ->LogController
*   /post   ->PostController

### Laravel路由分類
*   web: 負責網頁介面的routing
*   api: 負責api的routing
*   console: 負責自訂artisan命令
*   routes/web.php
### 直接回傳View
```php
Route::get('/', function () {
    return view('welcome');  
    //連接到views/welcome.blade.php
    //或者連接到錯誤訊息檔案return view('errors/503’);
});
```
```
localhost:8000
```
*   直接回傳內容
```php
Route::get('/hello', function() {
　return 'Hello, World';
});
```
```
localhost:8000/hello
```
*   傳送給Controller
```php
Route::get('/hello', 'HelloController@hello');
```
*   Controller name@Controller function

### 常用HTTP動詞
*   RESTful API實作方法(routes/api.php)
```php
Route::get('/user') //直接拿東西
Route::post('/user') //用來新增一個東西
Route::put('/user') //用來更新一個東西
Route::delete('/user') //用來刪除一個東西
```

### 處理參數
*   routes/web.php
```php
Route::get('/'user/{id}, function($id) {
    return 'Hi, User' . $id;
});
```
```
localhost:8000/user/300
localhost:8000/user/apple
```
### 傳送給Controller
*   HelloController中的hello函數就要帶一個id參數進去
```php
Route::get('/hello/{id}', 'HelloController@hello');
```
### 將參數帶入view
*   views/welcome.blade.php
```html
<div class=”title m-b-md”>
    {{ $title }}
</div>
```
*   routes/web.php
```php
Route::get('/title/{str}', function($str) {
    return view('welcome', [
        'title' => $str
    ]);
});
```
```
localhost:8000/title/Hello
```

##  資料庫基礎: Migration
*   使用Laravel的Migration來進行資料表的建立
### Why Migration?
*   Migration資料庫遷移
*   優點
    *   將資料表結構寫入Laravel PHP檔案內
    *   可用git追蹤變更
    *   確保團隊所有資料庫結構同步
*   Step 00: 先在MySQL Workbench建立資料庫，在.env設定資料庫資料，後面migration連線測試才不會出現錯誤
    *   .env設定mysql資料庫，預設 DB_DATABASE=(自己建立一個), DB_USERNAME=root, DB_PASSWORD=(如果前面沒有設定密碼，就是空的，不用打任何東西)
*   Step 01: 產生migration file
```
    php artisan make:migration create_post_table --create=posts
```
*   進到專案目錄
    *   database/migrations/2016_09_15_085513_create_post_table.php 這個檔案就是要撰寫資料庫table結構的地方，它可以代替我們執行MySQL指令，包括create table
*   Step 02: 如何編輯一個migration table
    *   進到專案目錄，database/migrations/2016_09_15_085513_create_post_table.php
    *   顯示一篇文章，資料庫table需要甚麼欄位?
```php
public function up()
{
    Schema::create('posts', function (Blueprint $print) {　
        //這是對應到指令的--create和=posts，兩者需對應，不然會migration錯誤
    　　$table->increments('id'); // Laravel預設將id設為主鍵，自動新增流水號
    　　$table->string('title');
    　　$table->string('text');
    　　$table->integer('author');
    　　$table->timestamps();
    });
}
```
*   up就是在建立整個table的時候需要執行的function
```php
public function up() {
    ...
}
```
*   down就是如果我們後悔了這個migration，要進行rollback就要執行down
```php
public function down() {
    ...
}
```
*   Step 03: 進行 migration
    *   執行順利會看見table被migration，前面兩個是內建的，後面是2016_09_15_085513_create_post_table
```
php artisan migrate
```
*   如果想知道更多，到Laravel的文件查詢
    *   https://laravel.com
    *   Documentation 查詢關鍵字 Migration 可以看到各種資料型態的列表 (Available Column Types)

## 資料庫物件: Model
*   配合Model就可以用Laravel的Model ORM來操作資料庫
*   Laravel的資料庫介面
    *   稱為Eloquent ORM
    *   ORM = Object Relational Mapping
    *   與一般傳統直接在程式碼中執行SQL不同，ORM採用物件的方式存取資料庫
*   差異比較：一般SQL
```php
$db = new PDO(資料庫設定);
$count = $db->exec("select * from posts");
```
*   差異比較：ORM
```php
Post::all(); //顯示全部筆數 = select * from posts
```
*   產生一個post model file
*   App/Post.php
```
php artisan make:model Post
```
*   設定Model屬性
    *   MySQL Table實際名稱
        *   protected $table = 'posts';
    *   Mass-Assignment大量新增許可欄位
        *   protected $fillable = ['title', 'text', 'author'];
    *   保護機密欄位(API中可用)
        *   protected $guarded=['password'];
```php
class Post extends Model
{
    protected $table = 'posts';
    /**
    * The attributes that are mass assignable.
    *
    * @var array
    */
    // fillable: math assignment的一個設定，在fillable陣列裡面的欄位才能被新增，如果新增時發現欄位寫不進來，就是因為沒有放進fillable裡面，這是一個保護機制，所以在post table中有的欄位可以寫進去
    protected $fillable = [
        'title', 'note', 'author',
    ];
    /**
    * The attributes that should be hidden for arrays.
    *
    * @var array
    */
    protected $hidden = [
        // 'password', 'remember_token', 沒有用到所以刪掉這行，但還是保留hidden，如果要顯示user，會把password隱藏起來
    ];
}
```

##  撰寫後端功能: Controller
### Step 01: 產生Controller
```
php artisan make:controller PostController--resource
```
*   app\Http\Controllers\PostController.php
### Step 02: 要寫controller之前先寫route
*   routes\web.php
```php
Route::get('/posts', 'PostController@index'); //PostController中的index function
```
### Step 03: 到controller
*   app\Http\Controllers\PostController.php
*   先測試有沒有連到route
```php
public function index()
{
　return 'post index';
}
```
```php
php artisan serve
localhost:8000/posts
```
*   如果想知道更多，到Laravel的文件查詢
    *   https://laravel.com

##  接上前端內容: View & Blade Template樣板語言
*   建立view檔案
    *   在resources/views資料夾中新增檔案
    *   檔案名稱必須為：*.blade.php
### Step 01: 新增view檔案
*   resources\views按右鍵新建立一個檔案
*   新建一個檔案　resources\views\post.blade.php
*   在post.blade.php新增前端語法
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset=”utf-8”>
        <meta http-equiv=”X-UA-Compatible” content=”IE=edge”>
        <meta name=”viewport” content=”width=device-width, initial-scale=1”>
    <title> Bootstrap Template </title>
    <link href=https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.cssrel=”stylesheet”>
    </head>
    <body>
        <h1>Hello, world! </h1>
        <script src=https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js></script>
        <script src=https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js></script>
    </body>
</html>
```
### Step 02: 來到PostController.php
*   app\Http\Controllers\PostController.php
```php
public function index()
{
    //return Post::all();
    return view('post');
}
```
```
localhost:8000/posts
```
*   如果想知道更多，到Laravel的文件查詢
    *   https://laravel.com




































