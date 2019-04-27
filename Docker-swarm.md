
# Docker-swarm 

Swarm là chế độ hoạt động mà một nhóm các máy tính chạy Docker (vật lý hoặc máy ảo) tham gia vào cluster có kết nối với nhau. Đơn giản chế độ Docker-Swarm cung cấp một môi trường chung chạy Docker với nhiều tài nguyên hơn phục vụ cho việc đó và với một số tính năng tự động như: Điều phối hoạt động của tasks, theo dõi trạng thái hoạt động của tasks,...

<img src = "https://image.slidesharecdn.com/docker-swarm-mike-goelzer-mv-meetup-45min-workshop022420161-160228024416/95/docker-swarm-docker-native-clustering-5-638.jpg?cb=1456856097">


## Node

* Mỗi một máy tính chạy Docker engine (Ảo hoặc vật lí) trong cluster được gọi là một node của Swarm, có 2 loại node trong Swarm đó là: Node-manager và Node-worker. Trước tiên ta sẽ phải định nghĩa một máy tính là node-manager sau đó ta sẽ join các máy tính khác vào cluster mà node-manager quản lí các máy đó gọi là node-woker

* Node-manager:
  * Là máy tính duy nhất trong swarm để chạy các lệnh Docker 
  * Xác định chiến lược chạy các container trong swarm:
    * Emptiest node: Lấp đầy container với các node ít hoạt động  
    * Global: Mỗi node sẽ chắc chắn chạy một container nào đó được chỉ định
  * Phối hợp, giao nhiệm vụ và tự động điều phối hoạt động của các node-worker đề duy trì cấu hình mong muốn của người sử dụng
  * Hoạt động như một Worker 
* Node-Worker:
  * Có nhiệm vụ duy nhất là cung cấp tài nguyên cho việc chạy Docker trong cluster (Bảo gì làm đấy)
  * Nhận và thực hiện công việc được giao từ node-manager
  * Thông báo trạng thái thực tế của các task cho node-manager để  node-manager thực hiện những xử lí thích hợp

# Một số cơ chế hoạt động của Swarm 

## Raft consensus in swarm mode

- Chạy chế độ Docker-swarm với một node-manager duy nhất với mục địch kiểm thử là chấp nhận được, những khi triển khai ở hệ thống lớn hơn nếu một node-manager duy nhất bị chết các node-worker vẫn hoạt động mà không có sự giám sát hay điều phối từ node-manager ==>> khi đó ta sẽ lại phải tạo lại cluster.

- Để khắc phục vấn đề này Docker-swarm sử dụng cơ chế fault-tolerance thông qua việc dùng thuật toán Raft một cluster có nhiều node-manager 
<img src = "https://docs.docker.com/engine/swarm/images/swarm-diagram.png"> 

* Thuật toán Raft gồm 2 tiến trình:
  * Leader election: 
     Chọn một node mới làm Leader sau khi một Node-Leader bị chết 
     Nếu ta có một nhóm có N node quá trình chọn node-Leader vẫn có thể xảy ra khi mất tối đa là (N-1)/2 node
  * Log replication
     Đảm bảo log của node-leader giống với log của các node còn lại (còn hoạt động)

* Việc sử dụng thuật toán Raft trên nhiều node-manager:
  * Giữa các node-manager sẽ có một node-manager làm leader, log của leader sẽ có nhiệm vụ ghi lại mọi hoạt động được thự hiện trong cluster 
  * Theo tiến trình Log replication mọi node-manager cũng sẽ có log giống với log của leader, để node-manager nào cũng có thể đảm nhận vai trò của leader trong trường hợp node-leader chết.
  * Khi một node-manager chết nhờ vào tiến trình Leader election mà một leader mới sẽ được chọn ra, các node-manager còn lại dựa vào log đã được lưu trữ để thực hiện tiếp tục duy trì, điều phối hoạt động của cluster, một cluster gồm N node-manager chịu được mất tối đa (N-1)/2 node-manager
  * Docker đề suất tối da 7 node-manager cho một cluster

## Service work 

* Docker-swarm là nơi ta triển khai các app-serviecs của mình, các service sẽ được định cấu hình ví dụ: Thông qua file YML 

Khi được triển khai, Swarm-manger nhận những định hình của services và lên kế hoạch hoạt động cho các service trên các node và thực hiện chạy chực tiếp các task trên các node.   

Khi chạy service có các container, mà các container hoạt động là độc lập nên cần có tasks (gắn với từng container) để thể hiện sự liên quan của các container với service (giống như nhãn định danh).

<img src = "https://docs.docker.com/engine/swarm/images/services-diagram.png">

### Task:

Mỗi task đi liền với chính xác một container của các service, là thứ để nhận biết các container trong Swarm, nếu một task chết - tương đương với việc container đi với nó chết khi đó task và container tương ứng bị xóa để tạo task với container mới thay thế sao cho phù hợp với cấu hình mong muốn của service .

Khai một task stop thì nó sẽ bị xóa chứ không có chuyện được chạy lại.

Task có những trạng thái của từ khi được tạo cho đến khi hoàn thành hoặc kết thúc. Sẽ theo một số thự tự trạng thái nhất định. Ví dụ: Không có chuyện task đi từ trạng thái `COMPLETE` sau đó đến `RUNNING`

Task đi qua một số trạng thái theo thứ tự sau: 

<img src = "https://i.imgur.com/izzrSaA.png">

### Pending services

Cấu hình được cài đặt cho các service có thể được trển khai khi mà các node trong Swarm không có hoặc không đủ tài nguyên để thực hiện khi đó service như vậy sẽ ở thái chờ
Ví dụ một số trường hợp các service có thể ở trạng thái chờ:
  * Tất cả các node của Swarm đang ở trạng thái dừng,.. như vậy service sẽ ở trạng thái chờ cho đến khi các nút có thể hoạt động được, trong thực tế khi node đầu tiên khả thi để hoạt động, tất cả các task sẽ hoạt động trên đó ==>>> không ổn trong môi trường triển khai
  * Bạn cần một lượng tài nguyên cụ thể để lưu dữ liệu cho một service, nếu không một node có đủ tài nguyên cho bộ nhớ cần thiết service sẻ ở trạng thái chờ cho đến khi có một node khả thi cho việc lưu trữ 
  * Một service bạn có thêm ràng buộc về vị trí hoạt động của nó nhưng swarm chưa thể đáp ứng điều đó =>> Service sẽ ở trạng thái chờ xử lí 

Bạn chỉ cần cung cấp cấu hình mà mình mong muốn, node-manager sẽ tự động điều phối sự hoạt động của các task, bận không cần quan tâm đến việc cài đặt cấu hình về sự hoạt động của từng task trên Swarm.

### Replicated and global services

* Replicated: Chúng ta sẽ định cấu hình chính xác số task bạn muốn chạy và node-manager sẽ thực hiện điều đó

* Global: Đó là một service mà đảm bảo việc sẽ chạy một task (được chỉ định cụ thể  - với khả năng đặc biệt nào đó) của nó trên mọi node của swarm (khi đó bạn không cần chỉ định chính xác số task muốn chạy) 
 Khi một node mới được thêm vào cluster node-manager sẽ thực hiện điều phối và tự động thêm task được chỉ định sẵn vào node mới
 Ví dụ: Task có chức năng hiện thị trạng thái của node, task có chức năng quyét virut,...

## Phân biệt Docker-swarm và Docker-compose
* Docker-compose giúp ta chạy toàn bộ một app với một câu lệnh một cách dễ dàng thông qua Docker-compose file     chỉ trên một máy tính  ==>> không phù hợp nếu tài nguyên của máy không đáp ứng đủ tài nguyên cần sử dụng
  Phù hợp dùng để test 

  Phù hợp sử dụng trong giai đoạn phát triển một sản phẩm
  
* Docker-swarm là một chế độ mà cung cấp một môi trường chạy Docker với nhiều tài nguyên (trên nhiều máy khác nhau) với nhiều cơ chế hay tính năng tự động để đảo bảo duy trì những config mong muốn được cài đặt sẵn 

  Docker-swarm là chế độ giúp triển khai các service trong file YML trên nhiều máy : Hữu ích trong cả việc test và triển khai 

  Phù hợp với một app cần nhiều tài nguyên máy tính ( Có thể thêm node-woker để có thêm tài nguyên)

  Phù hợp ở gia đoạn triển khai một sản phẩm đã hoàn chỉnh với nhiều cơ chế để duy trì nếu gặp lỗi
  


## Thực hành một số câu lệnh

Để thực hiện một số câu lệnh cơ bản với Swarm ta cần ít nhất 2 máy, ta sẽ dùng công cụ Virtualbox và Docker Manchine để tạo 2 máy ảo, chạy Docker Engine trên 2 máy đó và quản lí các máy đó với câu lệnh `docker-machine` 

Tạo 2 máy ảo với 2 câu lệnh
<img src = "https://i.imgur.com/rhtAFiI.png">
<img src = "https://i.imgur.com/jH1SBB2.png">

Tạo swarm bằng cách chạy `docker swarm init` trên máy bạn muốn mà máy đó là node-manager 

`docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip>" `

`docker-machine ssh ` Dùng khi bạn muốn chạy câu lệnh trên máy nào trên shell của máy chủ hiện tại

<img src = "https://i.imgur.com/XJtOov0.png">

Thêm một máy vào Swarm với việc chạy câu lệnh `docker swarm join` trên máy mà bạn muốn join vào .

<img src = "https://i.imgur.com/DfE0Ec5.png">

Chạy `docker node ls` trên máy manager để xem các node có trong sluster 

<img src = "https://i.imgur.com/1DPbNen.png">

Để rời một node khỏi cluster chạy `docker swarm leave` trên node đó 

<img src = "https://i.imgur.com/k8ocG4W.png">

Trước tiên chạy 2 lệnh  
`docker-machine env myvm1`
`eval $(docker-machine env myvm1)`

Khi đó sell trên máy hiện tại sẽ hiểu là lệnh được chạy trên máy myvm1 và vẫn truy cập được vào file trên máy local 

Để triển khai một app trên cluster ta chạy lệnh `docker stack deploy` trên node-manager dùng compose file 

<img src = "https://i.imgur.com/u5oM2LV.png">


Để xem các các task của stack  

`docker stack ps getstartedlab`

<img src = "https://i.imgur.com/tuDfpXf.png" >

Dùng `docker node inspect <NODE-ID>` trên node-manager để xem thông tin chi tiết một node 

Thêm cờ `--pretty` để thông tin ở dạng ta có thể đọc được 

<img src = "https://i.imgur.com/sMSKdmE.png">

Để xem join token chạy `docker swarm join-token -q worker  ` trên node-manager 

<img src = "https://i.imgur.com/6JSgpX5.png">


# khái niệm bổ sung thêm

## Docker-volum 

* Thông thường docker-images được lưu dưới dạng read-only layers, khi ta bắt đầu chạy một container docker sẽ thêm một read-write layer phía trên read-only layer. 

<img src = "https://i.imgur.com/nJCV3sd.png">

* Khi container chạy và thay đổi bất cứ file nào có sẵn ở lớp image, file sẽ được copy lên read-write layer để thực hiện các thay đổi (chỉ tồn tải khi container đang chạy). Khi container bị xóa sự thay đổi các file sẽ mất, các file nguyên bản ở read-only layer vẫn tồn tại, không thay đổi.

=>>> Không lưu trữ được giữ liệu bị thay đổi.

=>>>>>>> Volume sinh ra để giải quyết vấn đề này, Nhằm có thể lưu trữ những file bị thay đổi ngay cả khi container bị xóa, có thể dùng để chia sẻ tài nguyên giữa các node  

Các làm: 
* Tạo một volume (là một thư mục thực) ở trong máy chủ
* Ánh xạ trực tiếp thư mục đó với /data ở trong container

### Một số cách để tạo volume và kết nối nó với container
1.  Tạo khi thực hiện câu lệnh run (dùng để chạy container)
`$ docker run -it --name vol-test -h CONTAINER -v /data debian /bin/bash`
<img src = "https://i.imgur.com/ujt47ct.png">

Ta có thể xem vị trí volume trong máy chủ với câu lệnh:
`$ docker inspect -f "{{json .Mounts}}" vol-test | jq .`
<img src = "https://i.imgur.com/re90F4F.png">

Tạo một file mới vào trong volume trực tiếp bằng host         

`sudo touch /var/lib/docker/volumes/f3891e09e21cf69b5916ab552ff8c7926b9b02a092bb68dc3f4042e93dd8e134/_data/test-file`

Và lập tức được ánh xạ vào /data trong container 
<img src = "https://i.imgur.com/0XzF1Mf.png">

2.  Ta cũng có thể tạo volume bằng cách sử dụng VOLUME trong Dokerfile
<img src = "https://i.imgur.com/1POP8N1.png">

3. Tạo volume bằng `docker volume`

`$ docker volume create --name my-vol`
<img src = "https://i.imgur.com/LgVKDXf.png">

Mount volume vừa tạo với container sẽ chạy
`docker run -d -v my-vol:/data debian`
<img src = "https://i.imgur.com/TJIpYyQ.png">

4. Ta mount một foder cụ thể trong máy chủ với container sử dụng thẻ `v`
`docker run -v /home/adrian/data:/data debian ls /data`

/home/adrian/data trong host sẽ được mount tới \data trong container

`docker run -v /home/hoc/data:/data debian ls /data`
Việc này hữu ích cho việc các container cùng chia sẻ file 
Những directory được đắt sau thẻ `-v` (như  trong trường hợp trên) sẽ không chịu sự quản lí của Docker, không thể bị xóa bởi docker-daemon  



