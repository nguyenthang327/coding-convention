# API module sản phẩm (Product)

> **Note**:
> Sản phẩm chỉ có 2 cấp.
> Các api response có key `related_product_ids` mặc định là null vì đang lưu dữ liệu sản phẩm liên quan tại bảng related_products.

*******
Danh sách api  
 1. [API lấy danh sách sản phẩm (phân trang)](#API_list_product)
 2. [API tạo sản phẩm](#API_create_product)
 3. [API chi tiết sản phẩm](#API_show_product)
 4. [API cập nhật sản phẩm](#API_update_basic_product)
 5. [API xóa sản phẩm](#API_delete_product)
 6. [API lấy danh sách sản phẩm cha](#API_get_list_parent_product)
 7. [API lấy danh sách sản phẩm (không phân trang)](#API_get_list_product_no_paginate)
 8. [API cập nhật sản phẩm (cập nhật thông tin chi tiết của sản phẩm)](#API_update_detail_product)

*******

<div id='API_list_product'/>  

## 1. API lấy danh sách sản phẩm (phân trang)

```
URL: {{base_url}}/api/v1/product
Method GET
```

Tham số truyền vào
| KEY        | VALUE      | REQUIRED | DESCRIPTION |
| ------|-----|-----|-----|
| q | san phẩm 1 | nullable | Truy vấn theo điều kiện name hoặc mã của sản phẩm |
| type | 1 hoặc 2 | nullable |  Sản phẩm là cha hoặc biến thể |
| limit | 10 ( tùy ý) | nullable | Số lượng bản ghi trên 1 trang|

Dữ liệu trả về
```json
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

<div id='API_create_product'/> 

## 2. API tạo sản phẩm

```
URL: {{base_url}}/api/v1/product/store
Method POST
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


Dữ liệu trả về
```json
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

<div id='API_show_product'/> 

## 3. API chi tiết sản phẩm

```
URL: {{base_url}}/api/v1/product/{id}
Method GET
```
{id}: id của sản phẩm

Dữ liệu trả về
```json
{
    "message": "success",
    "data": {
        "id": 9,
        "name": "sản phẩm xịn",
        "code": "sp10",
        "price": 500,
        "description": "Sản phẩm này víp",
        "thumbnail": "https://.../report/24-10-2023/1698140184_xe.jpg",
        "type": 1,
        "branch": null,
        "status": 1,
        "parent_id": null,
        "related_product_ids": null,
        "created_at": "2023-10-24T09:36:26.000000Z",
        "updated_at": "2023-10-25T02:05:22.000000Z",
        "sort_order": 9,
        "file_attachment":"[{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/23-10-2023\\/1698053063_4 t\ính ch\ất c\ủa oop.docx\",\"file_name\":\"4 t\ính ch\ất c\ủa oop.docx\"},{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/23-10-2023\\/1698053064_C\âu tr\ả l\ời.docx\",\"file_name\":\"C\âu tr\ả l\ời.docx\"}]",
        "gallery": "[{\"file_path\":\"http:\\/\\/...\\/storage\\/report\\/21-10-2023\\/1697854960_Screenshot (4) - Copy - Copy - Copy.png\",\"file_name\":\"Screenshot (4) - Copy - Copy - Copy.png\"},{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/23-10-2023\\/1698053562_Screenshot (2) - Copy.png\",\"file_name\":\"Screenshot (2) - Copy.png\"}]",
        "productGroup": [
            {
                "id": 9,
                "name": "sản phẩm bịp",
                "code": "sp10",
                "price": 500,
                "description": "Sản phẩm này không hề bịp",
                "thumbnail": "https://.../report/24-10-2023/1698140184_xe.jpg",
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
            }
        ],
        "specifications": [
            {
                "id": 7,
                "code": "TS2",
                "name": "Thong so 2",
                "desc": null,
                "sort_order": 7,
                "specification_group_id": 7,
                "created_at": "2023-10-19T03:46:17.000000Z",
                "updated_at": "2023-10-19T03:46:17.000000Z",
                "pivot": {
                    "product_id": 9,
                    "specification_id": 7,
                    "info": "ay",
                    "created_at": "2023-10-25T01:53:42.000000Z",
                    "updated_at": "2023-10-25T02:05:22.000000Z"
                }
            },
            {
                "id": 8,
                "code": "TS3",
                "name": "Thong so 3",
                "desc": null,
                "sort_order": 8,
                "specification_group_id": 2,
                "created_at": "2023-10-19T03:46:36.000000Z",
                "updated_at": "2023-10-19T03:46:36.000000Z",
                "pivot": {
                    "product_id": 9,
                    "specification_id": 8,
                    "info": "ay i",
                    "created_at": "2023-10-25T01:53:42.000000Z",
                    "updated_at": "2023-10-25T02:05:22.000000Z"
                }
            }
        ],
        "related_products": [
            {
                "id": 1,
                "name": "ô tô",
                "code": "SP01",
                "price": 100,
                "description": "xxx",
                "thumbnail": "http://.../report/18-10-2023/1697613053_hang rao.png",
                "type": 1,
                "branch": null,
                "status": 1,
                "parent_id": null,
                "related_product_ids": null,
                "created_at": "2023-10-18T07:10:57.000000Z",
                "updated_at": "2023-10-23T02:42:05.000000Z",
                "sort_order": 1,
                "file_attachment": null,
                "gallery": null,
                "pivot": {
                    "product_id": 9,
                    "related_product_id": 1
                }
            },
            {
                "id": 4,
                "name": "Sp con sp 1",
                "code": "SP01-child",
                "price": 150,
                "description": null,
                "thumbnail": "https://.../report/23-10-2023/1698053277_Screenshot (5).png",
                "type": 2,
                "branch": null,
                "status": 1,
                "parent_id": 1,
                "related_product_ids": null,
                "created_at": "2023-10-18T07:41:34.000000Z",
                "updated_at": "2023-10-23T09:27:59.000000Z",
                "sort_order": 4,
                "file_attachment": null,
                "gallery": null,
                "pivot": {
                    "product_id": 9,
                    "related_product_id": 4
                }
            }
        ]
    },
    "status": 200
}
```

Các key quan trọng
| KEY    |  DESCRIPTION |
| ------|-----|
| id | Tên sản phẩm |
| code | Mã sản phẩm |
| name | Tên sản phẩm |
| type | Loại sản phẩm `type = 1 (Sản phẩm), type = 2 (Biến thể)` |
| price | Giá bán sản phẩm |
| parent_id | Mã của sản phẩm cha |
| thumbnail | URL ảnh thumbnail của sản phẩm  |
| file_attachment | các tệp tin tài liệu kỹ thuật |
| gallery | Lưới ảnh |
| specifications | thông số của sản phẩm |
| productGroup | các sản phẩm cùng nhóm |
| related_products | Các sản phẩm liên quan |

<div id='API_update_basic_product'/> 

## 4. API cập nhật sản phẩm (thông tin cơ bản)

```
URL: {{base_url}}/api/v1/product/update/{id}
Method PUT
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


Dữ liệu trả về
```json
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

<div id='API_delete_product'/> 

## 5. API xóa sản phẩm
> **Note**: Chỉ xóa được sản phẩm khi sản phẩm đó không có child

```
URL: {{base_url}}/api/v1/product/delete/{id}
Method DELETE
```
{id}: id của sản phẩm muốn xóa

Dữ liệu trả về
```json
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

<div id='API_get_list_parent_product'/> 

## 6. API lấy danh sách sản phẩm cha

```
URL: {{base_url}}/api/v1/parent-product
Method GET 
```

Dữ liệu trả về
```json
{
  "message": "Get list parent product success",
  "data": [
    {
      "id": 1,
      "name": "ô tô",
      "code": "SP01",
      "price": 100,
      "description": "xxx",
      "thumbnail": "https://.../1697613053_hang rao.png",
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

<div id='API_get_list_product_no_paginate'/> 

## 7. API lấy danh sách sản phẩm (không phân trang)

```
URL: {{base_url}}/api/v1/product-no-paginate
Method GET
```

Dữ liệu trả về
```json
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

<div id='API_update_detail_product'/> 

## 8. API cập nhật sản phẩm (cập nhật thông tin chi tiết của sản phẩm)

API này sử dụng khi cập nhật sản phẩm trong modal chỉnh sửa sản phẩm tại màn chi tiết sản phẩm.<br>

```
URL: {{base_url}}/api/v1/product/update-specification/{id}
Method PUT
```
{id}: id của sản phẩm

Tham số truyền vào

Dữ liệu dưới đây tôi truyền vào dạng raw (json)
```json
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
            "file_path": "https://.../1697854960_Screenshot (4) - Copy - Copy - Copy.png",
            "file_name": "Screenshot (4) - Copy - Copy - Copy.png"
        },
        {
            "file_path": "https://.../1698053562_Screenshot (2) - Copy.png",
            "file_name": "Screenshot (2) - Copy.png"
        }
    ]
}
```
> **Note**: Bạn cũng có thể truyền dữ liệu dạng form-data (Phải custom lại). Tôi để dạng json cho dễ nhận biết

Dữ liệu trả về
```json
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
        "gallery": "[{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/21-10-2023\\/1697854960_Screenshot (4) - Copy - Copy - Copy.png\",\"file_name\":\"Screenshot (4) - Copy - Copy - Copy.png\"},{\"file_path\":\"https:\\/\\/...\\/storage\\/report\\/23-10-2023\\/1698053562_Screenshot (2) - Copy.png\",\"file_name\":\"Screenshot (2) - Copy.png\"}]"
    },
    "status": 200
}
```