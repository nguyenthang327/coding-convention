# Tổng quan Frontend

*******
Các mục cần chú ý:  
[1. Cấu trúc thư mục](#folder_structure)
[2. Chi tiết cấu trúc file](#file_structure_detail)
- [2.1. Layouts master](#file_structure_detail)

*******

<div id='folder_structure'/>

## 1. Cấu trúc thư mục

Chia thư mục lưu trữ như sau

```bash
├── resources
    ├── views
        ├── layouts
        │   ├── master.blade.php
        │   ├── header.blade.php
        │   ├── footer.blade.php
        │   └── sidebars
        │       ├── sidebar-left.blade.php
        │       └── sidebar-right.blade.php
        ├── pages
        │   ├── product
        │   │   ├── index.blade.php
        │   │   ├── detail.blade.php
        │   │   ├── scripts
        │   │   │   ├── script-product.blade.php
        │   │   │   ├── script-product-detail.blade.php
        │   │   │   └── ...
        │   │   └── partials
        │   │       ├── modals
        │   │       │   ├── modal-add.blade.php
        │   │       │   ├── modal-edit-product.blade.php
        │   │       │   ├── modal-edit-product-detail.blade.php
        │   │       │   └── modal-delete.blade.php
        │   │       ├── tables
        │   │       │   ├── table-list-product.blade.php
        │   │       │   ├── table-abc.blade.php
        │   │       │   └── ...
        │   │       ├── searchs
        │   │       │   ├── form-filter-product.blade.php     
        │   │       │   └── ...          
        │   │       └── ...
        │   └── ...
        ├── layoutStatus
        │   ├── 404.blade.php
        │   └── ...
        └── libaryGroup
            ├── script-libary.blade.php
            └── style-libary.blade.php     
```