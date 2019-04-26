# Service là gì:
- Trong thực thế một ứng dụng có thể được chia nhỏ ra các thành phần và được chạy trên các máy chủ riêng biệt, các thành phần liên kết với nhau tạo thành một ứng dụng hoàn chỉnh mà người dùng thấy trên màn hình máy tính, mỗi thành phần như vậy được gọi là "service". 
  - Các service giống như những "container" hoạt động riêng rẽ nhưng có liên kết với nhau
  - Mỗi service chỉ chạy được một image, nhưng mỗi service có thể chạy một hoặc cùng lúc nhiều container từ imgae đó

# Docker-compose là gì?

* Là một công cụ có thể tạo, quản lí, cài đặt cấu hình, giới hạn tài nguyên.. cho các service chỉ với một dòng lệnh. Với docker-compose bạn sẽ chỉ cần sử dụng một YAML file để  định hình cho tất các service. Nhưng các service lại để phục vụ chạy một app nào đó nên Docker-compose là công cụ để chạy một app nhiều service hoặc chỉ đơn giản là tạo ra các container đọc lập một cách đơn giản.
* Docker-compose có thể làm gì chỉ với một dòng lệnh:
  - Tạo image được xây dựng trong Dockerfile rồi tự động chạy các container từ image đó
  - Tự động pull các image có sẵn trên dockerhub về rồi tự động chạy các container
  - Chạy các service được định nghĩa trong YAML file và chạy toàn bộ một app
* Các tính năng khác của Docker-compose:
  - Chạy, dừng hoặc tái xây dựng một app một cách nhanh chóng 
  - Xem trạng thái của các service đang chạy
  - Run a one-off command trong một service
* Những đặc điểm của Docker-compose: 
  - Tạo nhiều môi trường độc lập trên một máy chủ
    - Chạy một service mà trong đó nó sẽ chạy nhiều bản sao của một container từ một imgae mà nó sử dụng
    - Trên một máy có thể đặt tên khác nhau cho các service giống nhau về cấu trúc để chúng không can thiệp lẫn nhau
  - Bảo toàn dữ liệu khi các container được tạo sử dụng 
    - Khi bạn chạy `docker-compose up` (chạy lại một app nào đó) Docker-compose sẽ tìm kiếm các volume  dữ liệu từ các container trước sau đó copie chúng đến các container mới được chạy
  - Chỉ tạo lại các container đã được thay đổi
    - Docker-compose lư dữ các configuration của các container khi được tạo. Chỉ các service nào có sự thay đổi configuration mới được chạy lại container của nó. Với các service có configuration giữ nguyên các container cũ sẽ được giữ nguyên để sử dụng( không xóa đi và tạo mới lại)
  - Biến mỗi trường giữa các môi trường
    - Docker-compose hỗ chợ các biến môi trường (giống biến toàn cục được nhiều chương trình con sử dụng ) có thể được nhiều container trong các service khác nhau sử dụng

# Tại sao phải dùng Docker-compose 
* Việc dùng Docker-compose giúp ta đơn giản hóa nhiều công việc:
  * Pull hoặc xây dựng một lúc nhiều image đồng thời tự động chạy các container tương ứng chỉ với một dòng lệnh
  * Giới hạn được tài nguyên phần cứng để chạy các container một cách dễ dàng
  * Chạy toàn bộ service, toàn độ một app, re-build nhanh chóng
* Là một công cụ tốt để phát triển, test các phần mềm với các tính năng như:
  * Chạy, dừng, re-build nhanh chóng
  * Kiểm tra trạng tháng của các service khi đang chạy
  * Tạo các service độc lập giống với thực tế triển khai sản phẩm (thực tế các service sẽ chạy trên các máy chủ khác nhau) chỉ trên một máy local


# Những lệnh Docker-compose cơ bản:

Ví dụ ta có một folder như trong hình:

<img src = "https://i.imgur.com/XMcpvsv.png">

với file yml để thiết lập các service như hình: 

<img src = "https://i.imgur.com/Yqr87t0.png">


* `docker-compose up` để khởi chạy toàn bộ service thêm flag `-d` để chạy các service bên trong background

<img src = "https://i.imgur.com/pFol6LX.png">    

* `docker-compose run` cho phép ta chạy 1 lần nhiều câu lệnh cho các services. Ví dụ để thấy các environment variables trên web service:  
`$ docker-compose run web env`

<img src = "https://i.imgur.com/rRwL2bb.png">

* `docker-compose --help` để xem các câu lệnh có sẵn khác.

<img src = "https://i.imgur.com/TezAZDC.png">

* `docker-compose ps` hiển thị các service 

<img src = "https://i.imgur.com/7LrOJ4s.png">

* `docker-compose stop` để dừng các service

<img src = "https://i.imgur.com/Car1Cdz.png">

* `docker-compose build` để build hoặc re-build các service 

<img src = "https://i.imgur.com/PTX3ESo.png">

* `docker-compose config` xác nhận hoặc hiện thị config

<img src = "https://i.imgur.com/vXkS3t6.png">




  


    
    

 



