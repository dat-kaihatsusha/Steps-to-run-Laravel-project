Các bước làm khi chạy dự án Laravel:

Bước 1: Copy khung web Laravel (Thường là 5.7 nhé)

Bước 2: Tạo tên các bảng bằng câu lệnh

php artisan make:migration create_tênBảng_table --create=tênBảng

Chú ý: tênBảng thường là tb_abc

Bước 3: Tạo 1 Cơ sở dữ liệu rỗng trong Local nhé:
Mình ví dụ tên CSDL là DatabaseApi, sau đó chúng ta chuyển sang bước 4 để congfig file .env

Bước 4: Config file .env nhé

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
  
  
Bước 4.2: Vào folder config -> file database.php ->  sửa tên database theo tên database rỗng trong local, trong phần sau nhé:
'mysql' => [

            'driver' => 'mysql',
            
            'url' => env('DATABASE_URL'),
            
            'host' => env('DB_HOST', '127.0.0.1'),
            
            'port' => env('DB_PORT', '3306'),
            
            'database' => env('DB_DATABASE', 'demo01'), -->> chính là phần này nè
            
            'username' => env('DB_USERNAME', 'root'),
            
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
          
Bước 5: Tạo fieds cho các bảng (cái này chưa biết nè)





Bước 6: Sau khi tạo fields cho các bảng chúng ta lệnh

php artisan migrate

Trong trường hợp, chúng ta chưa tạo fields cho cho các bảng mà đã chạy lệnh php artisan migrate, thì như vậy sẽ lỗi, ta cần phải chạy lệnh sau để RollBack lại:

php artisan migrate:refresh
