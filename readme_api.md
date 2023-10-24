# This is API
Do you want to get data from api? Follow me hihi @@

## 1. API get

### URL of api {{base_url}}/api/v1/...
Giả sử tôi có api lấy danh sách sản phẩm với url như sau
```
Method GET: {{base_url}}/api/v1/product
```

Tham số truyền vào
| KEY        | VALUE      | DESCRIPTION |
| ------|-----|-----|
| q | tên sản phẩm | Truy vấn theo điều kiện name hoặc mã của sản phẩm |

có thể có thêm tham số tùy mục đích sử dụng

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
        "to": 7,
        "total": 7
    },
    "status": 200
}
```

## Second