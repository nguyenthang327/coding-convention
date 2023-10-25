# This is API Product
Do you want to get data from the api? Follow me hihi @@
> **Note**: Sản phẩm chỉ có 2 cấp. Các api response có key `related_product_ids` mặc định là null vì đang lưu dữ liệu sản phẩm liên quan tại bảng related_products.

<br>
## 1. API lấy danh sách sản phẩm (có phân trang)

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

<br>
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
| thumbnail | https://.... | nullable | URL ảnh thumbnail của sản phẩm|


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

<br>
## 3. API cập nhật sản phẩm (thông tin cơ bản)

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
| thumbnail | https://.... | nullable | URL ảnh thumbnail của sản phẩm|


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

<br>
## 4. API xóa sản phẩm
> **Note**: Chỉ xóa được sản phẩm khi sản phẩm đó không có child

URL:
```
Method DELETE: {{base_url}}/api/v1/product/delete/{id}
```
{id}: id của sản phẩm muốn xóa

Dữ liệu trả về định dạng như sau
```
{
    "message": "Destroy product success",
    "data": {
        "id": 10,
        "name": "Sản phẩm 9",
        "code": "SP9",
        "price": 200,
        "description": null,
        "thumbnail": "https://.../storage/report/24-10-2023/1698140184_xe.jpg",
        "type": 1,
        "branch": null,
        "status": 1,
        "parent_id": null,
        "related_product_ids": null,
        "created_at": "2023-10-24T09:38:30.000000Z",
        "updated_at": "2023-10-24T09:54:22.000000Z",
        "sort_order": 10,
        "file_attachment": null,
        "gallery": null,
        "children": []
    },
    "status": 200
}
```

<br>
## 5. API lấy danh sách sản phẩm cha

URL:
```
Method GET: {{base_url}}/api/v1/parent-product
```

Dữ liệu trả về định dạng như sau
```
{
  "message": "Get list parent product success",
  "data": [
    {
      "id": 1,
      "name": "ô tô",
      "code": "SP01",
      "price": 100,
      "description": "xxx",
      "thumbnail": "http://.../1697613053_hang rao.png",
      "type": 1,
      "branch": null,
      "status": 1,
      "parent_id": null,
      "related_product_ids": null,
      "created_at": "2023-10-18T07:10:57.000000Z",
      "updated_at": "2023-10-23T02:42:05.000000Z",
      "sort_order": 1,
      "file_attachment": null,
      "gallery": null
    }
  ],
  "status": 200
}
```

<br>
## 6. API lấy danh sách sản phẩm (không phân trang)

URL:
```
Method GET: {{base_url}}/api/v1/product-no-paginate
```

Dữ liệu trả về định dạng như sau
```
{
    "message": "Get list product success",
    "data": [
        {
            "id": 1,
            "name": "ô tô",
            "code": "SP01"
        },
        {
            "id": 3,
            "name": "sdsf",
            "code": "sdf"
        },
        {
            "id": 4,
            "name": "Sp con sp 1",
            "code": "SP01-child"
        }
    ],
    "status": 200
}
```

<br>
## 7. API cập nhật sản phẩm (cập nhật thông tin chi tiết của sản phẩm)
API này sử dụng khi cập nhật sản phẩm trong modal chỉnh sửa sản phẩm tại màn chi tiết sản phẩm.

URL:
```
Method PUT: {{base_url}}/api/v1/product/update-specification/{id}
```
{id}: id của sản phẩm

Tham số truyền vào
Dữ liệu dưới đây tôi truyền vào dạng raw (json)
```
{
    "name" : "sản phẩm víp",
    "price" : "500",
    "description": "Sản phẩm này pro lắm",
    "specification" : [
        {
            "id" : "7",
            "info": "ay"
        },
        {
            "id" : "8",
            "info": "ay i"
        }
    ],
    "related_product_ids": [
        1,
        4
    ],
    "file_attachment": [
        {
            "file_path": "https://.../1698053063_4 tính chất của oop.docx",
            "file_name": "4 tính chất của oop.docx"
        },
        {
            "file_path": "https://.../1698053064_Câu trả lời.docx",
            "file_name": "Câu trả lời.docx"
        }
    ],
    "gallery": [
        {
            "file_path": "http://.../1697854960_Screenshot (4) - Copy - Copy - Copy.png",
            "file_name": "Screenshot (4) - Copy - Copy - Copy.png"
        },
        {
            "file_path": "https://.../1698053562_Screenshot (2) - Copy.png",
            "file_name": "Screenshot (2) - Copy.png"
        }
    ]
}
```
> **Note**: Bạn cũng có thể truyền dữ liệu dạng form-data (Phải custom lại). Tôi để dạng raw cho ae dễ nhìn

Dữ liệu trả về định dạng như sau
```
{
    "message": "Update product success",
    "data": {
        "id": 9,
        "name": "sản phẩm víp",
        "code": "sp10",
        "price": "500",
        "description": "Sản phẩm này pro lắm",
        "thumbnail": "https://.../storage/report/24-10-2023/1698140184_xe.jpg",
        "type": 1,
        "branch": null,
        "status": 1,
        "parent_id": null,
        "related_product_ids": null,
        "created_at": "2023-10-24T09:36:26.000000Z",
        "updated_at": "2023-10-25T02:05:22.000000Z",
        "sort_order": 9,
        "file_attachment":"[{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/23-10-2023\\/1698053063_4 t\ính ch\ất c\ủa oop.docx\",\"file_name\":\"4 t\ính ch\ất c\ủa oop.docx\"},{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/23-10-2023\\/1698053064_C\âu tr\ả l\ời.docx\",\"file_name\":\"C\âu tr\ả l\ời.docx\"}]",
        "gallery": "[{\"file_path\":\"http:\\/\\/...\\/storage\\/report\\/21-10-2023\\/1697854960_Screenshot (4) - Copy - Copy - Copy.png\",\"file_name\":\"Screenshot (4) - Copy - Copy - Copy.png\"},{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/23-10-2023\\/1698053562_Screenshot (2) - Copy.png\",\"file_name\":\"Screenshot (2) - Copy.png\"}]"
    },
    "status": 200
}
```