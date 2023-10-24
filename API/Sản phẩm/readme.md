# This is API Product
Do you want to get data from the api? Follow me hihi @@
> **Note**: Sản phẩm chỉ có 2 cấp

## 1. API get danh sách sản phẩm (có phân trang)

URL:
```
Method GET: {{base_url}}/api/v1/product
```

Tham số truyền vào
| KEY        | VALUE      | REQUIRED | DESCRIPTION |
| ------|-----|-----|-----|
| q | san phẩm 1 | nullable | Truy vấn theo điều kiện name hoặc mã của sản phẩm |
| type | 1 hoặc 2 | nullable |  Sản phẩm là cha hoặc biến thể |
| limit | 10 ( tùy ý) | nullable | Số lượng bản ghi trên 1 trang|

Dữ liệu trả về định dạng như sau
```
{
    "message": "Get list product success",
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 1,
                "name": "ô tô",
                "code": "SP01",
                "price": 100,
                "type": 1,
                "parent_id": null,
                "thumbnail": "url của file",
                "parent": null
            }
        ],
        "first_page_url": "http://127.0.0.1:8000/api/v1/product?page=1",
        "from": 1,
        "last_page": 1,
        "last_page_url": "http://127.0.0.1:8000/api/v1/product?page=1",
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "active": false
            },
            {
                "url": "http://127.0.0.1:8000/api/v1/product?page=1",
                "label": "1",
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "active": false
            }
        ],
        "next_page_url": null,
        "path": "http://127.0.0.1:8000/api/v1/product",
        "per_page": 30,
        "prev_page_url": null,
        "to": 1,
        "total": 1
    },
    "status": 200
}
```

## 2. API tạo sản phẩm
URL:
```
Method POST: {{base_url}}/api/v1/product/store
```

Tham số truyền vào
| KEY        | VALUE      | REQUIRED | DESCRIPTION |
| ------|-----|-----|-----|
| code | SP1 | required | Mã sản phẩm |
| name | Sản phẩm 1 | required | Tên sản phẩm |
| type | 1 hoặc 2 | required | Loại sản phẩm `type = 1 (Sản phẩm), type = 2 (Biến thể)` |
| price | 100 | nullable | Giá bán sản phẩm |
| parent_id | 1 | nullable `(required nếu type = 2)` | Mã của sản phẩm cha |
| thumbnail | https://.... | nullable | Ảnh thumbnail của sản phẩm|


Dữ liệu trả về định dạng như sau
```
{
    "message": "Create product success",
    "data": {
        "name": "Sản phẩm 1",
        "code": "SP1",
        "price": 100,
        "thumbnail": "https:/..../storage/report/24-10-2023/1698140184_xe.jpg",
        "type": "1",
        "status": 1,
        "parent_id": null,
        "updated_at": "2023-10-24T09:38:30.000000Z",
        "created_at": "2023-10-24T09:38:30.000000Z",
        "id": 10,
        "sort_order": 10
    },
    "status": 200
}
```

## 3. API cập nhật sản phẩm

URL:
```
Method PUT: {{base_url}}/api/v1/product/update/{id}
```
{id}: id của sản phẩm

Tham số truyền vào
| KEY        | VALUE      | REQUIRED | DESCRIPTION |
| ------|-----|-----|-----|
| code | SP12 | required | Mã sản phẩm |
| name | Sản phẩm 12 | required | Tên sản phẩm |
| type | 1 hoặc 2 | required | Loại sản phẩm `type = 1 (Sản phẩm), type = 2 (Biến thể)` |
| price | 200 | nullable | Giá bán sản phẩm |
| parent_id | 1 | nullable `(required nếu type = 2)` | Mã của sản phẩm cha |
| thumbnail | https://.... | nullable | Ảnh thumbnail của sản phẩm|


Dữ liệu trả về định dạng như sau
```
{
    "message": "Update product success",
    "data": {
        "id": 10,
        "name": "Sản phẩm 12",
        "code": "SP12",
        "price": "200",
        "description": null,
        "thumbnail": "https://.../storage/report/24-10-2023/1698140184_xe.jpg",
        "type": "1",
        "branch": null,
        "status": 1,
        "parent_id": null,
        "related_product_ids": null,
        "created_at": "2023-10-24T09:38:30.000000Z",
        "updated_at": "2023-10-24T09:54:22.000000Z",
        "sort_order": 10,
        "file_attachment": null,
        "gallery": null
    },
    "status": 200
}
```
