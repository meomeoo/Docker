#1.Virtualization
Virtualization là một kĩ thuật có thể giúp cài nhiều hệ điều hành khác nhau trên cùng một phần cứng(máy tính), chúng hoàn toàn riêng biệt và độc lâp với nhau.
- Ảo hóa cũng là một kĩ thuật để tạo các vùng sử dụng (làm việc với môi trường làm việc cần thiết)
Virtualization ẩn đi các đặc điểm vật lí của tài nguyên tính toán với người sử dụng của chúng và người sử dụng cuối.
Virtualization thường là:
 * Tạo ra nhiều tài nguyên ảo (OS, APP, VM, container,.) từ mội tài nguyên vật lí (1 máy tính, 1 máy chủ..).
 * Tạo ra một tài nguyên ảo (OS, APP, VM, container,.)từ một hoặc nhiều tài nguyên vật lí (1 hay nhiều máy tính)

#2.MỘt số ví dụ:
Hiện nay thuật ngữ Virtualization đã được áp dụng rộng rãi trong một số khái niệm, vd:
 * Server Virtualization: tạo nhiều máy chủ ảo(APP,OS) trên nền máy chủ thực, giúp tiết kiệm chi phí, thực hiện nhiều mục đích khác nhau mà không tốn thêm tài nguyên vật lý.
![https://www.tutorialspoint.com/virtualization2.0/images/server.jpg]
 * Client & Desktop Virtualization 
Trong một công ty có 1 máy chủ lớn chứa dữ liệu cần đề nhân viên truy cập, giải pháp là tạo ra các desktop(ảo) như những những client mỏng cho từng bằng việc chia từ trung tâm dữ liệu ở máy chủ. Các nhận viên có thể truy cập dữ liệu sử dụng phần mềm ở máy chủ (tùy thuộc vào quyền được cấp.
![https://www.tutorialspoint.com/virtualization2.0/images/client.jpg]
 * Network Virtualization
Là một phần của cơ sở hạ tầng ảo hóa, được sử dụng với múc đích đặc biết là tạo nên giao diện trực quan (có thể nhìn bằng mắt thường) với nhiều tính năng tùy chỉnh, vì nó tạo nên những  switching, Vlans, NAT-ing,.(đoạn này em không chắc lắm).
![https://www.tutorialspoint.com/virtualization2.0/images/network.jpg]
 * Services and Applications Virtualization
Là kĩ thuật mà tách biệt độc lập hoàn toàn Applicatuons với hề điều hành bên dưới nó và giữa các Application với nhau, để tăng khả năng tương thích và quản lí (em không hiểu sao lại tăng được khả năng tương thích - compatibility - tron khi tách biết các app riêng biết với nhau).Docker được dùng cho mục đích này.
![https://www.tutorialspoint.com/virtualization2.0/images/applications.jpg]

#3.Những công nghê ảo hóa

##máy ảo(VM)
là một trương trình có vai trò và tác dụng như máy tính thật. Chạy trên hệ điều hành chủ (Hosted Hypervisor) hoặc chạy thông qua Hypervisor, các máy ảo (VM) được ảo hóa cung cấp CPU,RAM được cài OS như máy tính thật,nói chung tất cả đều giống như một máy tính thật nhưng các thành phần được trừu tượng hóa dựa trên một tài nguyên vật lý có sẵn sau đó"ảo hóa"ra các thành phần giống một máy tính thật.
![https://techtalk.vn/techtalk_blog/public/picture/img/vinhle/1510815543.png]
##Container (user space)
ontainer không giống như VMs, Container không cung cấp sự ảo hóa về phần cứng. Một Container cung cấp ảo hóa ở cấp hệ điều hành bằng một khái niệm trừu tượng là “user space”. Sự khác nhau lớn nhất của Container và VMs là Container có thể chia sẻ host system’s kernel với các container khác. 
![https://techtalk.vn/techtalk_blog/public/picture/img/vinhle/1510815613.png]
Các bạn có thể thấy các gói container chỉ là một user space bao gồm ứng dụng, system binaries và libraries mà không cần guest OS hoặc ảo hóa phần cứng như VMs.  Đây là cái mà làm cho các container nhẹ hơn (lightweight). Các container sẽ chạy trên công nghệ cụ thể ở đây là Docker Engine.
theo em: contaier là một không gian ảo được tạo ra để giả lập các môi trường đủ để cho người dùng làm việc mà không cần phải yêu cầu cung cấp ảo hóa phần cứng (RAM,CPU, OS như một máy tính thật) nên hiển nhiên nó sẽ nhẽ hơn, bớt phức tạp về mức độ áo hóa.
##Hypervisor(tạo các VM)
Là một lớp phần mềm mỏng ngăn cách giữa hệ điều hành gọi tới phần cứng.cÒn được gọi là Virtual Machine Monitor (VMM). Nó tạo nền tảng trên máy chủ để Các hệ điều hành khách (guest OS) sẽ "tiếp xúc" trực tiếp với Hypervisor và sẽ được thực thi và giám sát.
có 2 kiểu: 
* Native of Bare Metal Hypervisor and
* Hosted Hypervisor
   * Native of Bare Metal Hypervisor and:
Là phần mềm hệ thống chạy trực tiếp với phần cứng và điều khiên phần cứng cũng như tạo nền tảng để giám sát (monitor - không biết em dịch có đúng không?) các guest OS (các máy ảo với các hệ điều hành hoạt động riêng biệt).Các Guest OS sẽ hoạt động riêng rẽ phía trên hypervisor.
![https://www.tutorialspoint.com/virtualization2.0/images/bare_metal.jpg]
   * Hosted Hypervisor
là phần mềm được thế kế để chạy trên một hề điều hành truyền thống có sẵn (không hoạt động trực tiếp với phần cứng như cái bên trên).Lớp gust OS sẽ là lớp phần mềm thứ 3 trên phần cứng.
![https://www.tutorialspoint.com/virtualization2.0/images/hosted.jpg]


##OS level (Isolation)(tạo các container mà k tạo các VM)
hay còn gọi là Containers Virtualization , là phương pháp ảo hóa mới cho phép nhân của hệ điều hành hỗ trợ nhiều instances (còn được gọi là container) dựa trên 1 hệ điều hành có sẵn cho nhiều users khác nhau. --> tạo, An toàn dùng chung 1 hệ điều hành. Ưu điểm của công nghệ này là bảo trì nhanh chóng nên được sử dụng nhiều trong lĩnh vực hosting. . công nghệ này chỉ tồn tại trên hệ điều hành Linux. VD: chroot, Docker,... (phần này em lấy của chị vì em tìm không thấy nó trên mạng -_-  và em xóa đoạn ở giữa và thay bằng mục container).

#4.Docker 
Docker là một nền tảng cung cấp các containers.
Như đã giải thích Docker cung cấp các containers không tạo ra máy ảo.
 * Các khái niệm quan trọng liên quan:
   * Images:Hiểu nôm na là một khuôn mẫu để tạo một container. Thường thì image sẽ base trên 1 image khác với những tùy chỉnh thêm.Bạn có thể tự build một image riêng cho mình hoặc sử dụng những image được publish từ cộng đồng Docker Hub. Một image sẽ được build dựa trên những chỉ dẫn của Dockerfile.
   * Containers: là một instance của một image. Bạn có thể create, start, stop, move or delete container dựa trên Docker API hoặc Docker CLI.
   * Registry (Docker Hub): là một kho chứa các image được publish bởi cộng đồng Docker. Nó giống như GitHub và bạn có thể tìm những image cần thiết và pull về sử dụng.
   * Docker Client: là một công cụ giúp người dùng giao tiếp với Docker host.
   * Docker Daemon: lắng nghe các yêu cầu từ Docker Client để quản lý các đối tượng như Container, Image, Network và Volumes. Các Docker Daemon cũng giao tiếp với nhau để quản lý các Docker Service.
   * Dockerfile: là một tập tin bao gồm các chỉ dẫn để build một image .

#The end. 





