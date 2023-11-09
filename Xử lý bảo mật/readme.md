# Phương án:

Tạo 1 service check người dùng có quyền truy cập hay khách hàng hay không

```

check if admin 
	=> xem hết 
check if manager
	=> xem được của nó và những thằng thuộc quản lý của nó
		Query all user thuộc quyền quản lý của nó
		check khách hàng muốn xem có hợp lệ không.
check if user
	=> chỉ xem đc khách hàng của nó
		check khách hàng muốn xem có hợp lệ không.
		
		
trạng thái ok
	status: 200
	message: null

không có quyền, ...
	status: 403
	message: bạn không có quyền ....

```

ở controller gọi service check data

```
	if status !=200
		return error
			mã lỗi và message của service
			
	else
		thực hiện công việc tiếp theo.
```