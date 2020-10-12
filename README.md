Các bước làm khi chạy dự án Laravel:

Bước 1: Copy khung web Laravel (Thường là 5.7 nhé)

Bước 2: Tạo tên các bảng bằng câu lệnh

php artisan make:migration create_tênBảng_table --create=tênBảng

Chú ý: tênBảng thường là tb_abc

Bước 3: Tạo 1 Cơ sở dữ liệu rỗng trong Local nhé:
Mình ví dụ tên CSDL là DatabaseApi, sau đó chúng ta chuyển sang bước 4 để congfig file .env

Bước 4.1: Config file .env nhé

Các bạn chỉ cần chú ý cho mình mấy dòng sau (3 dòng cuối thôi nhé):

DB_CONNECTION=mysql

DB_HOST=127.0.0.1

DB_PORT=3306

DB_DATABASE=DatabaseApi

DB_USERNAME=root

DB_PASSWORD=

Trong đó: DatabaseApi là tên CSDL mà các bạn tạo ở bước 3 đó.

          DB_USERNAME: Tên người dùng là root (gốc), tức là có toàn quyền với DB
          
          DB_PASSWORD: Chúng ta để rỗng, tức là không có password
  
  
Bước 4.2: Vào folder config -> file database.php ->  sửa tên database theo tên database rỗng và DB_USERNAME thành root trong local, trong phần sau nhé:
'mysql' => [

            'driver' => 'mysql',
            
            'url' => env('DATABASE_URL'),
            
            'host' => env('DB_HOST', '127.0.0.1'),
            
            'port' => env('DB_PORT', '3306'),
            
            'database' => env('DB_DATABASE', 'demo01'), -->> chính là phần này nè
            
            'username' => env('DB_USERNAME', 'root'), -->> sửa chỗ này nữa nhé
            
            'password' => env('DB_PASSWORD', ''),
            
            'unix_socket' => env('DB_SOCKET', ''),
            
            'charset' => 'utf8mb4',
            
            'collation' => 'utf8mb4_unicode_ci',
            
            'prefix' => '',
            
            'prefix_indexes' => true,
            
            'strict' => true,
            
            'engine' => null,
            
            'options' => extension_loaded('pdo_mysql') ? array_filter([
            
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
                
            ]) : [],
            
        ],
          
Bước 5: Tạo fieds cho các bảng:
Vào folder database -> folder migrations -> chỉnh sửa các file php (chú ý là đã mặc định 2 file 2014_10_12_000000_create_users_table.php và file 2014_10_12_100000_create_password_resets_table.php), ví dụ nhé

Schema::create('product', function (Blueprint $table) {

            $table->increments('product_id');
            
            $table->string('product_name');
            
            $table->integer('category_id');
            
            $table->integer('brand_id');
            
            $table->double('product_price');
            
            $table->double('promotion_price');
            
            $table->string('product_desc');
            
            $table->timestamp('product_date');
            
            $table->integer('promotion_amount');
            
        });

Schema::create('brand', function (Blueprint $table) {

            $table->increments('brand_id');
            
            $table->string('brand_name');
            
        });
        
        Schema::create('category', function (Blueprint $table) {
            $table->increments('category_id');
            $table->string('category_name');
            $table->timestamp('category_date');
        });



Bước 6: Sau khi tạo fields cho các bảng chúng ta lệnh

php artisan migrate

**ALERT**

Một số chú ý ngoài lề nhé
Trong trường hợp, chúng ta chưa tạo fields cho cho các bảng mà đã chạy lệnh php artisan migrate, thì như vậy sẽ lỗi, ta cần phải chạy lệnh sau để RollBack lại:

php artisan migrate:refresh
Trường hợp khi tải 1 project Laravel về từ git thì ta phải setup thì nó mới chạy được, bằng lệnh: composer update

Sau đó ta tạo 1 CSDL rỗng ở trong local và file .env (chú ý là phải chỉnh sửa tên DB_DATABASE giống với file database.php trong folder config)

Cuối cùng khi xong xuôi các bước thì các bạn mới được chạy: php artisan migrate

Nếu migrate lỗi thì đọc kỹ xem lỗi ở đâu nhé, còn nếu khi chạy xong câu lệnh: php artisan migrate không thấy lỗi gì cả mà ta cần chỉnh sửa gì trước đó thì phải dùng lệnh roll back lại: php artisan migrate:refresh

**WARNING**

Sau đây mình sẽ đưa ra một số lưu ý khi các bạn làm dự án thực tế nhé:
Thường đối với website bán hàng, trong table product thì có 1 field khá đặc biệt là field: product_date: field này dùng để lưu lại thời gian nhập sản phẩm, field này sẽ được hệ thống tự ghi lại, không cần nhập. Muốn thế ta phải chạy câu query sau: <b>ALTER TABLE mytable CHANGE `product_date` `product_date` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP</b>
