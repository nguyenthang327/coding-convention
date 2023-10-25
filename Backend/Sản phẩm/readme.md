# This is Code logic backend Product

*******
Danh sách các logic mẫu  
 1. [Xây dựng chức năng lấy danh sách sản phẩm (có phân trang)](#list_product)
 2. [Xây dựng chức năng tạo sản phẩm](#create_product)
 3. [Chi tiết sản phẩm](#show_product)
 4. [Cập nhật sản phẩm](#update_basic_product)
 5. [Xóa sản phẩm](#delete_product)

*******

<div id='list_product'/> 

## 1. Xây dựng chức năng lấy danh sách sản phẩm (có phân trang)

* Bước 1: Tạo controller
<br></br>

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

* Bước 2: Tạo file service và function lấy danh sách cho sản phẩm
<br></br>

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
<br></br>

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
    ...
}
```

* Bước 3: Trở lại controller thêm service cho function `index()`
<br></br>

Ở bước 2 ta có nói sẽ tạo service lấy danh sách sản phẩm có phân trang.
Vậy ở đâu sẽ gọi service này để lấy dữ liệu?
Tất nhiên là trong `ProductController`.
```
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Http\Traits\RESTResponse;
use App\Services\Product\ProductService;
use Exception;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Log;

class ProductController extends Controller
{
    use RESTResponse;

    /**
     * Display a listing of the resource.
     */
    public function index(Request $request)
    {
        try {
            $productService = new ProductService();
            $data = $productService->list($request);
            return $this
                ->setData($data)
                ->setMessage('Get list product success')
                ->successResponse();
        } catch (Exception $e) {
            Log::error("[ProductController][index] error " . $e->getMessage());
            return $this->throwInternalError($e);
        }
    }
    ...
}
```

Trong hàm `index()` khởi tạo class `new ProductService()` ở service vừa viết để lấy dữ liệu và trả về cho người dùng.

* Bước 4: Tạo route để lấy dữ liệu
<br></br>

Oh hòm hòm rồi nhưng làm sao để gọi được controller?
Đừng lo, vào `routes\api.php` khởi tạo route
```
Route::prefix('/product')->group(function () {
Route::get('/', [ProductController::class, 'index']);
})
```
Để test kết quả hãy nhập `{{base_url}}/api/v1/product` với `{{base_url}}` là domain của website
<br></br>

Haha xong chức năng lấy danh sách sản phẩm có phân trang rồi :laughing::laughing:.

* Biến `$request` chứa những tham số truyền vào cho mục đích lọc dữ liệu

<div id="create_product"></div>

## 2. Xây dựng chức năng tạo sản phẩm
<br></br>

Ở trên ta đã tạo `ProductController` và `ProductService` rồi nên không cần thực hiện bước 


## Note utiles: 
* Tại mỗi function phải đặt `try{} catch(){}` để bắt ngoại lệ và phải ghi log lỗi với cú pháp `[tên class][tên function] error: nối với message lỗi`.
* Đối với các chức năng thêm, sửa, xóa blabla... nói chung là thay đổi dữ liệu trong database ta cần thêm `DB::beginTransaction()`, `DB::commit()`, `DB::rollback()` để phòng những trường hợp lỗi và có thể rollback lại mà không gây ảnh hưởng đến database.