## 1. Bảng so sánh VM và Container
| Tiêu chí            | Virtual Machine (VM)                                                                     | Docker Container                                                                                     |
|---------------------|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Dung lượng Image    | Thường lớn do bao gồm hệ điều hành khách và các thành phần cần thiết.                    | Thường nhỏ hơn vì chia sẻ kernel của hệ điều hành máy chủ và sử dụng các layer của image.            |
| Thời gian khởi động | Thường lâu hơn do phải khởi động cả hệ điều hành khách.                                  | Thường nhanh hơn vì chủ yếu khởi chạy tiến trình ứng dụng.                                           |
| Hao phí OS          | Mỗi VM thường có hệ điều hành khách riêng, tiêu tốn thêm CPU, RAM và dung lượng lưu trữ. | Không cần hệ điều hành khách riêng cho từng container nên thường tiết kiệm tài nguyên hơn.           |
| Mức độ cô lập       | Có mức cô lập mạnh nhờ hypervisor và kernel riêng của từng VM.                           | Cô lập ở cấp hệ điều hành; mức độ an toàn phụ thuộc cấu hình, quyền truy cập và các cơ chế bảo vệ.   |
| Khả năng mở rộng    | Có thể mở rộng nhưng việc tạo và khởi động VM thường tốn thời gian và tài nguyên hơn.    | Có thể tạo, thay thế và nhân bản container nhanh chóng, thuận tiện cho tự động hóa và mở rộng ngang. |

## 1. Vai trò của Namespaces và Control Groups (cgroups)

### 1.1. Namespaces

Namespaces là cơ chế của Linux giúp cô lập góc nhìn về tài nguyên giữa các nhóm tiến trình. Docker sử dụng các loại namespace để tạo môi trường hoạt động riêng cho container.

- **PID Namespace:** Cô lập không gian Process ID, giúp tiến trình trong container có góc nhìn riêng về các tiến trình.
- **Network Namespace:** Cô lập giao diện mạng, bảng định tuyến và ngăn xếp mạng, giúp container có môi trường mạng riêng.
- **Mount Namespace:** Cô lập góc nhìn về các điểm gắn kết hệ thống tệp, giúp container sử dụng cách bố trí hệ thống tệp riêng.

Nhờ Namespaces, các ứng dụng trong container có môi trường hoạt động tách biệt hơn với máy chủ và các container khác.

### 1.2. Control Groups (cgroups)

Control Groups là cơ chế của Linux dùng để theo dõi, phân loại và kiểm soát tài nguyên mà các nhóm tiến trình sử dụng.

- **Giới hạn CPU:** Kiểm soát lượng thời gian xử lý mà container được phép sử dụng, tránh một ứng dụng chiếm quá nhiều CPU.
- **Giới hạn RAM:** Kiểm soát lượng bộ nhớ container được sử dụng, giảm nguy cơ một ứng dụng tiêu thụ quá nhiều RAM của máy chủ.

Namespaces tập trung vào việc cô lập tài nguyên, trong khi cgroups tập trung vào việc quản lý và giới hạn mức sử dụng tài nguyên. Hai cơ chế này góp phần giúp Docker vận hành nhiều ứng dụng trên cùng một máy chủ hiệu quả hơn.

## 2. Trích xuất thông số từ Docker Info

### 2.1. Các lệnh kiểm tra

Thực hiện bằng tài khoản người dùng thông thường, không sử dụng `sudo`:

```bash
docker version
docker info
```

Lệnh `docker version` hiển thị thông tin phiên bản Docker Client và Docker Server. Lệnh `docker info` cung cấp thông tin tổng quan về Docker Engine, cấu hình hệ thống và các container, image hiện có.

### 2.2. Kết quả thực tế

| Thông số       | Giá trị thực tế |
|----------------|-----------------|
| Server Version | 29.9.0          |
| Cgroup Driver  | systemd         |
| Containers     | 2               |
| Images         | 2               |
