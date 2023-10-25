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

Oh hòm hòm rồi nhưng làm sao để gọi được controller?
Đừng lo, vào `routes\api.php` khởi tạo route
```
Route::prefix('/product')->group(function () {
    Route::get('/', [ProductController::class, 'index']);
})
```
Để test kết quả hãy nhập `{{base_url}}/api/v1/product` với `{{base_url}}` là domain của website

Haha xong chức năng lấy danh sách sản phẩm có phân trang rồi :laughing::laughing:.

<div id="create_product"></div>

## 2. Xây dựng chức năng tạo sản phẩm

Ở trên ta đã tạo `ProductController` và `ProductService` rồi nên không cần thực hiện tạo lại nữa và sẽ viết thêm các chức năng liên quan trong này.

Khi tạo sản phẩm bạn thường viết validate `Request` trong controller đúng không.

Oh shit :face_with_spiral_eyes::face_with_spiral_eyes:, ở đây chúng tôi không làm thế. Tôi muốn chia nhỏ chức năng, cái nào ra cái đó.

Yeah bắt đầu nào :wink:

* Bước 1: Validate Request
Tạo file validate `StoreProductRequest.php`. Tạo như thế nào?
```
php artisan make:request StoreProductRequest
```
oh sau khi chạy command này bạn sẽ nhận được một file trong folder `App\Http\Requests`

lalala xử lý thôi.

- Trong function `authorize()` đổi thành `return true;`
- Trong function `rules()` validate tham số truyền vào. Dưới đây là ví dụ nho nhỏ.

```
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
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, \Illuminate\Contracts\Validation\ValidationRule|array|string>
     */
    public function rules(): array
    {
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

* Bước 2: Xử lý trong controller
<br></br>

Vào ProductController thêm đoạn mã sau

```
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
            $productService = new ProductService();
            $data = $productService->storeProduct($request);
            return $this
                ->setData($data)
                ->setMessage('Create product success')
                ->successResponse();
        }catch(Exception $e){
            Log::error("[ProductController][store] error " . $e->getMessage());
            return $this->throwInternalError($e);
        }
    }
    ...
}
```

Trước đây ta viết `public function store(Request $request)` và 1 đống validation bên trong controller. Bây giờ chỉ còn như đoạn mã trên, bạn thấy sao hehe.

* Bước 3: xử lý logic thêm mới trong service

Chúng ta đang thêm mới sản phẩm vậy ta sẽ xử lý logic thêm mới sản phẩm trong file `ProductService.php`
```
<?php

namespace App\Services\Product;

...

class ProductService
{
    ...
    public function storeProduct($request){
        DB::beginTransaction();
        try {
            $params = [
                'name' => $request->input('name'),
                'code' => $request->input('code'),
                'price' => $request->input('price'),
                'description' => $request->input('description'),
            ];

            $product = Product::create($params);
            // update sort_order = product_id when create new record
            $product->update(['sort_order' => $product->id]);

            DB::commit();
            return $product;
        } catch (Exception $e) {
            DB::rollBack();
            Log::error("[ProductService][storeProduct] error " . $e->getMessage());
            throw new Exception('[ProductService][storeProduct] error ' . $e->getMessage());
        }
    }
    ...
}
```

Đối với những đoạn code phức tạp xử lý logic nhiều bạn có thể `comment` ghi chú để sau này maintain dễ dàng hơn. Đoạn trên mình chỉ ví dụ comment thôi chứ logic đơn giản vãi chưởng nhìn phát hiểu ngay.

> **Note**:
> - Logic khó, xử lý phức tạp => comment
> - Logic đơn giản, dễ đọc => không cần comment


## Note utiles: 
* Tại mỗi function phải đặt `try{} catch(){}` để bắt ngoại lệ và phải ghi log lỗi với cú pháp `[tên class][tên function] error: nối với message lỗi`.
* Đối với các chức năng thêm, sửa, xóa blabla... nói chung là thay đổi dữ liệu trong database ta cần thêm `DB::beginTransaction()`, `DB::commit()`, `DB::rollback()` để phòng những trường hợp lỗi và có thể rollback lại mà không gây ảnh hưởng đến database.