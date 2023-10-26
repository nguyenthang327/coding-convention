# Module Sản phẩm (Product)

*******
Danh sách chức năng:  
 1. [Danh sách sản phẩm (phân trang)](#list_product)
 2. [Thêm mới sản phẩm](#create_product)
 3. [Chi tiết sản phẩm](#show_product)
 4. [Cập nhật sản phẩm](#update_basic_product)
 5. [Xóa sản phẩm](#delete_product)

*******

<div id='list_product'/> 

## 1. Danh sách sản phẩm (phân trang)

### Bước 1: Tạo controller

Tạo controller có tên là ProductController
```bash
php artisan make:controller Api\ProductController --api
```
Thêm trait `RESTResponse` cho controller

File controller có dạng
```php
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Http\Traits\RESTResponse;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    use RESTResponse; // trait phục vụ cho xử lý dữ liệu trả về của api
    ...
}
```	

### Bước 2: Tạo file service và function lấy danh sách sản phẩm

Tạo file `ProductService.php` trong folder `App\Services\Product`.

```php
<?php

namespace App\Services\Product;

class ProductService
{
    ...
}
```

Tạo function `list()` lấy danh sách của sản phẩm như sau.
```php
<?php

namespace App\Services\Product;
use App\Models\Product;
use Exception;
use Illuminate\Support\Facades\Log;

class ProductService
{
    const LIMIT = 30; # hằng số cấu hình phân trang

    public function list($request)
    {
        try {
            $q = $request->query('q'); // tham số tìm kiếm "keySearch" theo tên hoặc mã sản phẩm
            $type = $request->query('type'); // tham số lọc theo loại sản phẩm
            $limit = $request->query('limit', self::LIMIT); // tham số truyền vào khi phân trang nếu không có mặc định là 30

            # lấy danh sách sản phẩm từ model Product
            $products = Product::query();
            $q = str_replace(' ', '%', $q); // thay thế ký tự % thành khoảng trắng trong chuỗi $q
            // Nếu "keySearch" có giá trị thì thêm điều kiện truy vấn
            if ($q) {
                $products = $products->where(function ($query) use ($q) {
                    $query->where('products.code',  "like", "%$q%")
                        ->orWhere("products.name", "like", "%$q%");
                });
            }
            // thêm điều kiện lọc theo loại sản phẩm nếu $type khác null
            $products->when($type, function($query, $type){
                return $query->where('products.type', $type);
            });
            $products = $products->orderBy('products.sort_order', 'asc')
                ->paginate($limit);

            # trả về dữ liệu sản phẩm
            return $products;
        } catch (Exception $e) {
            Log::error("[ProductService][list] error " . $e->getMessage()); // Log khi xảy ra lỗi
            throw new Exception('[ProductService][list] error ' . $e->getMessage());
        }
    }
    ...
}
```

### Bước 3: Thêm service trong controller

File controller có dạng như sau

```php
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
            $productService = new ProductService(); // khởi tạo class ProductService để sử dụng các function
            $data = $productService->list($request); // gọi đến function list() trong ProductService để lấy dữ liệu sản phẩm với tham số $request

            # trả về dữ liệu json
            return $this
                ->setData($data) // set giá trị $data trả về
                ->setMessage('Get list product success') // set Message trả về
                ->successResponse(); // set status trả về
        } catch (Exception $e) {
            Log::error("[ProductController][index] error " . $e->getMessage()); // log khi xảy ra lỗi
            return $this->throwInternalError($e);
        }
    }
    ...
}
```

### Bước 4: Tạo route lấy dữ liệu

Vào `routes\api.php` khởi tạo route
```php
Route::prefix('/product')->group(function () {
    Route::get('/', [ProductController::class, 'index']);
})
```
Test trên postman:
```
- URL: {{base_url}}/api/v1/product
- Method: GET
- Param: các tham số truyền vào
```

<div id="create_product"></div>

## 2. Thêm sản phẩm

### Bước 1: Validate Request
Tạo file xử lý validate
```bash
php artisan make:request StoreProductRequest
```
Thay đổi nội dung file
```php
<?php

namespace App\Http\Requests;

use App\Models\Product;
use Illuminate\Database\Query\Builder;
use Illuminate\Foundation\Http\FormRequest;

class StoreProductRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     */
    public function authorize(): bool
    {
        return true; // Chuyển giá trị từ false thành true
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, \Illuminate\Contracts\Validation\ValidationRule|array|string>
     */
    public function rules(): array
    {
        # Viết các điều kiện validate nếu lỗi trả về 422
        $rules = [
            'code' => 'required|string|max:255|unique:products,code', 
            'name' => 'required|string|max:255',
            'price' => 'nullable|numeric|min:0',
            'description' => 'nullable|string',
        ];
        return $rules;
    }
}
```

### Bước 2: Thêm function store() trong controller

Vào `ProductController.php` thêm đoạn mã sau

```php
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Http\Requests\StoreProductRequest;
use App\Http\Traits\RESTResponse;
use App\Services\Product\ProductService;
use Exception;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Log;

class ProductController extends Controller
{
    ...
    /**
     * Store a newly created resource in storage.
     */
    public function store(StoreProductRequest $request)
    {
        try{
            $productService = new ProductService(); // khởi tạo class ProductService
            $data = $productService->storeProduct($request); // gọi đến function storeProduct() trong ProductService để xử lý thêm mới sản phẩm với tham số $request truyền vào

            # trả về dữ liệu json
            return $this
                ->setData($data) // set giá trị $data trả về
                ->setMessage('Create product success') // set Message trả về
                ->successResponse(); // set status trả về
        }catch(Exception $e){
            Log::error("[ProductController][store] error " . $e->getMessage()); // log lỗi
            return $this->throwInternalError($e);
        }
    }
    ...
}
```

### Bước 3: Thêm logic thêm mới sản phẩm trong ProductService

Tại file `ProductService.php` thêm function `storeProduct()`
```php
<?php

namespace App\Services\Product;

...

class ProductService
{
    ...
    public function storeProduct($request){
        DB::beginTransaction(); // Khởi tạo Database transaction
        try {
            # xử lý request truyền vào
            $params = [
                'name' => $request->input('name'), // tên
                'code' => $request->input('code'), // mã code
                'price' => $request->input('price'), // giá
                'description' => $request->input('description'), // mô tả
            ];

            # thêm mới sản phẩm với $params đã xử lý
            $product = Product::create($params);
            $product->update(['sort_order' => $product->id]); // cập nhật sort_order = id của sản phẩm vừa tạo

            DB::commit(); // thực hiện lưu dữ liệu vào DB nếu không xảy ra exception

            # Trả về dữ liệu
            return $product;
        } catch (Exception $e) {
            DB::rollBack(); // rollback khi xảy ra lỗi
            Log::error("[ProductService][storeProduct] error " . $e->getMessage()); // log lỗi
            throw new Exception('[ProductService][storeProduct] error ' . $e->getMessage());
        }
    }
    ...
}
```

### Bước 4: Tạo route

Vào `routes\api.php` khởi tạo route lưu sản phẩm

```php
Route::prefix('/product')->group(function () {
    ...
    Route::post('/store', [ProductController::class, 'store']);
})
```
Test trên postman:
```
- URL: {{base_url}}/api/v1/product/store
- Method: POST
- Param: các tham số cần truyền
```

<div id="show_product"></div>

## 3. Chi tiết sản phẩm

### Bước 1: Thêm function show() trong controller
```php

```

## Note utiles: 
* Tại mỗi function phải đặt `try{} catch(){}` để bắt ngoại lệ và phải ghi log lỗi với cú pháp `[tên class][tên function] error: nối với message lỗi`. Điều này giúp chúng ta dễ dàng phát hiện nguyên nhân lỗi và debug lỗi đó.
* Đối với các chức năng thêm, sửa, xóa blabla... nói chung là thay đổi dữ liệu trong database ta cần thêm `DB::beginTransaction()`, `DB::commit()`, `DB::rollback()` để phòng những trường hợp lỗi và có thể rollback lại mà không gây ảnh hưởng đến database.
* Khi tạo hay cập nhật dữ liệu mình sẽ tạo file trong `App\Http\Requests`
