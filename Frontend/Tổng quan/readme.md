# Tổng quan Frontend

*******
Các mục cần chú ý:  
[1. Cấu trúc thư mục](#folder_structure)

[2. Cấu trúc file](#file_structure_detail)
- [2.1. File master.blade.php](#file_master)

*******

<div id='folder_structure'/>

## 1. Cấu trúc thư mục

Chia thư mục lưu trữ như sau

```bash
├── public
│   ├── assets
│       ├── css
│       │   ├── style.css
│       │   ├── variables.css
│       │   └── pages
│       │       ├── product
│       │       │   ├── index.css
│       │       │   └── ...
│       │       └── ...
│       ├── js
│       │   ├── common.js
│       │   └── pages
│       │       ├── product
│       │       │   ├── index.js
│       │       │   └── ...
│       │       └── ...
│       └── plugins
│            ├── bootstrap
│            │   ├── file.min.js
│            │   └── file.min.css
│            └── ...
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

<div id='folder_structure'/>

## 2. Cấu trúc file

<div id='file_master'/>

## 2.1 File master.blade.php
```html
<?php

date_default_timezone_set('Asia/Ho_Chi_Minh');
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>@yield('title') - {{ env('SLOGAN_URL', '') }}</title>

    <!-- Favicon -->
    <link rel="shortcut icon" href="{{ env('FAVICON_URL', '') }}">

    <!--  import các libary css chung cần thiết -->
    

     <!--  import các libary css riêng nếu cần với page đó -->
    @yield('css_library')

    <!--  Common css -->
    <link rel="stylesheet" href="{{ asset('assets/css/style.css') }}">

    <!-- Page style -->
    @yield('css_page')
</head>

<body>
    <div class="wrapper">
        <!-- Pre-Loader Page -->
        <span id="loader" class="loader"></span>
        <!-- End Pre-Loader Page -->

        <!-- layout header -->
        @include('layouts.header')

        <!-- content -->
        <div class="content-wrapper">
            @yield('content')
        </div>

        <!-- Layout footer -->
        @include('layouts.footer')
    </div>

    <!-- import các libary js chung cần thiết -->

    <!-- import các libary js riêng nếu cần với page đó -->
    @yield('js_library')

    <!-- Page script -->
    @yield('js_page')

    <!-- common js -->
    <script src="{{ asset('assets/js/common.js') }}"></script>
</body>

```