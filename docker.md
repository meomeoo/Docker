
# Vấn đề mà Docker giải quyết
* Sử dụng và cài đặt phần mềm là một vấn đề phức tập: ứng dựng hoạt động trên hệ điều nào, tài nguyên mà nó yêu cầu, các dụng khác mà nó phụ thuộc vào,..   
Ngày nay một ứng dụng không chỉ hoạt động riêng lẻ riêng mình nó mà nó còn cần nhiều thứ phụ thuộc đi kèm. Vì vậy trên một máy tính việc sử dụng, cài đặt, nâng cấp một phần mềm sẽ là một vấn đề phức tạp và rắc rối.   
<img src="https://i.imgur.com/j5UAM6t.png">
và Docker giả quyết được vấn đề này với các container.
<img src="https://i.imgur.com/JHXejrZ.png">
Docker tạo riêng cho từng ứng dụng những thứ nó cần để hoạt động độc lập mà không ảnh hưởng tới các ứng dụng khác. 

* Các phần mềm phụ thuộc của một ứng dụng đôi khi yêu cầu hoạt động trên những hoạt động nhất định và môi trường làm việc hiện tại lại không đáp ứng được nhu cầu đó

  Tính di động mà docker mang đến giải quyết nhiều vấn đề như: chạy những phần mềm như nhau trên bất kì hệ thống nào, tạo ra một môi trường hoạt động thuần nhất trên nhiều hệ thống máy tính, những người bảo trì phần mềm, những nhà phát triển có thể tập chung viết phần mềm với chỉ một bộ ứng dụng phụ thuộc (mà không cần quan tâm nó có thể hoạt động được trên các hệ thống khác nhau hay không,...

# Một số lệnh cơ bản 
1. Xem thông tin và liệt kê các container và image 
* docker version: xem phiên bản docker
* docker info: xem thông tin chi tiết cơ bản của phiên bản docker
* docker ps: Liệt kê các container đang chạy
* docker ps -a : hiển thị các container đang chạy và đã tắt
* docker ps -s: giống docker ps tuy nhiên nó còn hiển thị thêm 1 field là dung lượng container được tạo ra
* docker images: xem các image đã cài đặt
* docker network ls: Xem các loại network đang có trong docker
* docker inspect { container_id }: Xem chi tiết thông tin của container được tạo ra
* docker network inspect bridge: Xem các ip của container
* docker inspect { container_id || container_name } | grep -C2 Binds: Xem các volume trong container
* docker logs { container_id }: xem lịch sử container  
2. Xóa 1 container, images:
* docker rm <id hoặc Name>:  Xóa 1 container
* docker rm –f <id hoặc Name>: Giống lệnh trên nhưng xóa được cả container đang chạy (f ó force)
* docker rm $(docker ps -a -q): xóa toàn bộ các container đang tồn tại
* docker kill <id hoặc Name>: Tắt 1 container
* docker rmi <id hoặc Name>: Xóa 1 image
* docker rmi –f <id hoặc Name>: Xóa 1 image kể cả khi đang chạy
3. Bật, chạy container
* docker start <id hoặc Name>: Star 1 docker container
* docker stop <id hoặc Name> : Dừng lại container được chỉ định với id hoặc tên đang chạy
* docker stop $(docker ps –a –q): Dừng tất cả các container đang chạy
* docker exec –it <id hoặc Name> /bin/bash: start container đã bị tắt (lệnh xem container bị tắt ở trên)
* docker run --name {container_name} -p {host_port}:{container_port} -v {/host_path}:{/container_path} -it {image_name} /bin/bas.
4.   Tìm hiểu chi tiết lệnh docker run ...
    - Với lệnh trên docker sẽ tạo 1 cotainer từ image có sẵn và chạy với tùy chọn cổng với volume       
      (chú ý: mỗi lần run là tương đương với việc tạo ra 1 container).   
      Với các tùy chọn như sau.   
–it chỉ định image sử dụng (nếu image ko có ẵn nó sẽ tự động pull về, câu lệnh này ó -i -t). image ko chỉ định được nhiều chỉ chỉ định được duy nhất 1 image khi tạo container.   
-i (-interactive): giữ cho stdin mở kể cả khi không attach   
-t (-tty ): Allocate a pseudo-TTY. Ý nghĩa của nó là cho phép phân bổ 1 giả lệnh tty (tty là 1 khái niệm dòng lệnh tương tự như shell command trong linux và các hệ điều hành unix khác, nó giúp ta nhập các dòng lệnh tương tác hoặc chạy 1 kịch bản ‘script’. Đích đến của nó là một chương trình hoặc máy in).  
-it hay –ti trong docker giúp ta mở cửa số dòng lệnh stdin và tạo 1 tty giả lập để cho người dùng (chính là ta) sử dụng và tương tác với môi trường docker  
-d (-detach): chạy ứng dụng dưới chế độ nền. Nghĩa là khi bạn start một container với cờ này thì ứng dụng sẽ không chạy thằng vào dòng lệnh bên trong container mà nó sẽ chạy ngầm và trả lại cho bạn một container Id. Để làm việc với container mà bạn chạy dưới cờ -d thì bạn sử dụng docker attach hoặc docker exec … như ở trên đã đề cập  
exit: thoát khoải môi trường ảo container đang chạy  
Ctrl + P sau đó Ctrl + Q : deatch khỏi cửa sổ thực thi trong cotainer, nó khác exit là nó không thoát hoàn toàn ứng dụng mà các ứng dụng bạn đang chạy vẫn chạy bình thường  
docker attach { container_id }: chui lại vào container bạn vừa detach, (chú ý: phải gõ enter 2 lần)  

5. Run mysql server image và tạo, sửa xóa database trong container.  
 'sudo docker pull mysql/mysql-server:latest' 

Có tác dụng pull image mysql-server với phiên bản mới nhất về (tag=latest)  
<img src="https://i.imgur.com/08m80VD.png"> 
`sudo docker run --name=hihi -d mysql/mysql-server:latest`                
Chạy và đặt tên (tên container là 'hihi') một container dựa trên image vừa tải     
-d là tùy chọn để chạy container *dưới nền* ( chạy ẩn - không thể hiện gì trên màn hình)   
<img src="https://i.imgur.com/Xg881kR.png">  
`docker logs hihi 2>&1 | grep GENERATED`   
Nhận mật khẩu mặc định cho root user   
`sudo docker exec -it hihi mysql -uroot -p`   
Kết nối tới mysql server từ bên trong container chứa nó. Cụ thể là di chuyển đến mysql client.    
`mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'conmeocon';`  
Thay đổi mật khẩu cho root user   
<img src="https://i.imgur.com/iNkB55K.png">  
`mysql> CREATE DATABASE pets;`  
Tạo database mới là: pets  
<img src="https://i.imgur.com/ush5vVG.png">
`mysql> CREATE TABLE cats..;`  
Tạo bảng cats của pets   
Và một số tao tác với bảng.  
<img src="https://i.imgur.com/Xi2hjz8.png">
<img src="https://i.imgur.com/ahl5UIt.png">
<img src="https://i.imgur.com/ec6ath2.png">
<img src="https://i.imgur.com/e8vRNar.png">



# Grafana 
`sudo docker run -d -p 3000:3000 grafana/grafana`  
Lệnh dùng để run 1 container grafana dựa dên image (được pull từ docker hub về)  
<img src="https://i.imgur.com/6uQpC72.png">

<img src="https://namlee.net/wp-content/uploads/2018/07/huong-dan-cai-dat-he-thong-monitor-voi-grafana-influxdb-va-telegraf-tren-centos-7.png">
Grafana là một Platform mã nguồn mở sử dụng trong việc phân tích các dữ liệu thu thập được từ server và hiện thị một các trực quan dữ liệu thu thập được ở nhiều dạng khác nhau.  
Influxdb sử dụng cho time-series database và Grafana cho việc visualizing metrics.  
<img src="https://s3-ap-southeast-1.amazonaws.com/kipalog.com/1kgrulwe43_cluster.png">



