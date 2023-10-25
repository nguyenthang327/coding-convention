# This is Code logic backend Product

## 1. Xây dựng chức năng lấy danh sách sản phẩm

* Bước 1: Tạo controller<br>
Tạo controller có tên là ProductController
```bash
php artisan make:controller Api\ProductController --api
```
Sau đó add trait `RESTResponse` cho controller
File controller sẽ có dạng sau
```
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Http\Traits\RESTResponse;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    use RESTResponse;
    ...
}
```	

* Bước 2: Tạo file service cho sản phẩm
Ở bước này, chúng ta tạo một file `ProductService.php` trong folder `App\Services\Product`.

```
<?php

namespace App\Services\Product;

class ProductService
{
    ...
}
```

Thêm `const LIMIT = 30;` để cấu hình danh sách phân trang nếu không có tham số truyền vào.
<br>
Tạo mới một function. Ở đây tôi đặt tên tên là `list()` để lấy ra danh sách của sản phẩm. Function này sẽ được gọi phía controller.
```
<?php

namespace App\Services\Product;
use App\Models\Product;
use Exception;
use Illuminate\Support\Facades\Log;

class ProductService
{
    const LIMIT = 30;
    ...
    public function list($request)
    {
        try {
            $q = $request->query('q');
            $type = $request->query('type');
            $limit = $request->query('limit', self::LIMIT);
            $products = Product::query();
            $q = str_replace(' ', '%', $q);
            if ($q) {
                $products = $products->where(function ($query) use ($q) {
                    $query->where('products.code',  "like", "%$q%")
                        ->orWhere("products.name", "like", "%$q%");
                });
            }
            $products->when($type, function($query, $type){
                return $query->where('products.type', $type);
            });
            $products = $products->orderBy('products.sort_order', 'asc')
                ->paginate($limit);
            return $products;
        } catch (Exception $e) {
            Log::error("[ProductService][list] error " . $e->getMessage());
            throw new Exception('[ProductService][list] error ' . $e->getMessage());
        }
    }
}
```

> **Note**: Tại mỗi function phải đặt `try{} catch(){}` để bắt ngoại lệ và phải ghi log lỗi với cú pháp `[tên class][tên function] error: nối với message lỗi`.

