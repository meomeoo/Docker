# Đôi điều lịch sử
Vitualization là công nghệ có trước containerlization.
Ban đầu Vitualization giải quyết được các vấn đề đa năng hóa khả năng của một máy tính mà không cần thêm tài nguyên vật lí. Nhưng sau khi công nghê containerlization xuất hiện nó giải quyết được các vấn mà Vitualization đang làm nhưng nó tốt hơn Vitualization ở những điểm như: Tận dụng tối đa tài nguyên vật lí, giảm thiểu thời gian về mặt khởi động, shutdown,..
Và thế là nhưng một lẽ tự nhiên :))  công nghệ containerlization ngày càng được ưa chuộng.
Thủa ban đầu việc dùng công nghệ containerlization để tạo ra các container khá là phức tập nên không khả thi với các tổ chức nhỏ hay người dùng cá nhân. Khi đó công nghệ này chỉ khả thi với các ông lớn ngành công nghệ nhưng cách họ làm thì đương nhiên được private source code. Và buồn thay công nghệ containerlization khi đó chưa được áp dụng rộng rãi.
Nhưng rồi đến một ngày nọ có một công ty đã public source code của họ về công nghệ này và họ tung ra sản phầm mang tên là Docker và nhận được nhiều sự chú ý, sau đó công ty cũng đổi tên thành Docker luôn.
Vì là công nghê containerlization nên nó yêu cầu can thiệp vào phần lõi, nhân kernel của hệ điều hành và đương nhiên linux - một OS mã nguồn mở là phương án khả thi để ấp dụng công nghệ này và đó là tại sao ban đầu Docker được viết cho linux. và Microsoft cũng có sản phẩm tương tự của mình đó là Windows Container.
Đến khi thấy Docker hay quá, Microsoft ngỏ lời, thế là công ty Docker và công ty Microsoft hợp tác với nhau nhưng có vẻ chưa khả quan lắm bởi vì nhân Windows có nhưng thứ không public được (bản quyền mà ). Theo như em tìm hiểu: 
Docker trên Windows có 2 phiên bản:

Docker for Windows yêu cầu OS là Windows 10 bật Hyper-V (không phải máy nào cũng hỗ trợ và chưa ổn định)
Docker Toolbox có thể cài trên bất kỳ OS Windows nào do dựa trên VirtualBox (bị giới hạn bởi các thiết lập mặc định của VirtualBox)
Về cơ bản em thấy Docker hoạt động trên window vẫn cần những công cụ máy ảo để có thể chạy được nên em vẫn hiểu nó là không dành do Window (hay không chạy trên nền thuần túy của window).
Đây là các bài viết em tham khảo được post vào 2 thời điểm 17/11/2016 và Jun 30th, 2017 8:16 AMe.
Nhưng một bài viết Posted on Tháng Bảy 3, 2018 nói răng: kể từ phiên bản Windows 10 64bit Pro, Enterprise and Education (1607 Anniversary Update, Build 14393 or later) chúng ta có thể cài đặt docker một cách dễ dàng như các phần mềm khác. Nên -_- em nghĩ giơ Docker là cho cả linux và windows.

# Vấn đề mà Docker giải quyết
* Sử dụng và cài đặt phần mềm là một vấn đề phức tập: ứng dựng hoạt động trên hệ điều nào, tài nguyên mà nó yêu cầu, các dụng khác mà nó phụ thuộc vào,.. 
Ngày nay một ứng dụng không chỉ hoạt động riêng lẻ riêng mình nó mà nó còn cần nhiều thứ phụ thuộc đi kèm. Vì vậy trên một máy tính việc sử dụng, cài đặt, nâng cấp một phần mềm sẽ là một vấn đề phức tạp và rắc rối. 
<img src="https://lh3.googleusercontent.com/71PeETEaRnNkGGxzmFM0xMnvHesSs7QSdiBbZZi8D9qcZMLpS1Tu7_mVjypxUjJhvaNrk3I2qo5gG7Cv17-_DGTLGTjPfnqyJ5uzan6WpdknqVfjkKcZ-N92B5nPcncodJShzmVFCe3UVedrAUFRb3fKqQw03btg_6gvBvtjGGPqZpK5sSGiR0CNQer8sTZyaLoWs0qpQmc0Ypi5lNgFmWf_l9mUB84uCmpPL2O4OLi06MxDjjSyMBk3b63zQ68SEen8su6Y73qvhqPENXQWY-iFDAf92NwT-Y8aO-mXEpbtdGT_zwqZtlzgq0-0QVoGKgsId316HjCqDb1CFpega5tSJCEHpjaP7LacWKogQ6eZLAz6oVrj8JZliC-4l57TDi_Oz1NcjJuC86Nl2FJEqFAfdGcVXeMedBSRr3pNMTnWFCgQPYOBI0iU4GY1X5ST5p7Hpc70pKZeTD47QNkZYpz3vZDIzqrLIqk3HAnUkjgGt_pE4rikt265jcdm4BDVbd1prkrr5wnTl6znQFZyUmsGpT2m6TRy_on5FoabDDDqowGA3K3TsGdq6tXGjiEaa9AQkJxj9AvQaJt5tbcpDb_MmqVd7UrmrBZcMokF7b4d4tOWxOXIPOcU6HiJaFZFV8AdxObkEJvVwv0ZXSrBTHfgoguo5Ow=w506-h248-no">
và Docker giả quyết được vấn đề này với các container.
<img src="https://lh3.googleusercontent.com/Bs3IYo8H7Zdb7rfm2wdEZoKW85fpJwMT3ZbgWU_1mmjLoWN8hrtVDBW8OFy2P2GxjY0_gPtxe-0Sk5kaiX-fanZHpJQZomoankRCoHbie7zfnvSE9gcLqPdvW7c67IHaE7LE2rGID_5_lBNXo3rom9flz4Dbo3r3GOfWqGgq6_-Hn3vEgyOaQqymkIzOlYtXytaGpt4KKEELwXjxzitgZTK6B2YOpHx0GHy6LP5gHSGAvA7ZyNKH5unhnP0hjBn2ncAQ6uxrU6jje-BkfPhSeys9CTzF-9e2WNQyUIQX-7SNztgaCczmMTXWRCzXwvesVkcOeRAMO7G_BytlMYwC-8nqX3CnIGZ9TpwbStN1799SwQq8bG3c1bFHVh_k8m0K37biGLk3Jv0hXy41gChreZw7Eg1tnYfvcSjcX_9wO7gwVrefTJfRFX-f4HVpxCudPirgL67mytnEXu9GXqZXbgCPjVwTQeKPEq3MvppkgNJqMRlsPkdyePdFPOU5WGCSkuf-8qF71mRB7vqJ73x8kkEm2NmhuPw_lxeq1ichKX794cA72fngArQW2R0zvp8pRiSyl1Ta2jZOKA7T6UY3ddrSoiapWf-SWKVC4T6d4uF1jeJwwaYok7At57c09S_VMHjTDC7nl0orTZmnRAuTlcnGs5Gqpac=w847-h343-no">
Docker tạo riêng cho từng ứng dụng những thứ nó cần để hoạt động độc lập mà không ảnh hưởng tới các ứng dụng khác. 

* Một vấn đề khác của phần mềm đó chính là các các ứng dụng phụ thuộc của nó yêu cầu hoạt động trên những hệ điều hành nhất định và nó là một vấn đề lớn của người sử dụng phần mềm. Có thể có sự tương đồng (có thể hoặt động được) của một số ựng dụng phụ thuộc trên 2 hệ điều hành là Linux và Mac OS X, nhưng dùng những hệ điều hành tương tự trên Windown lại không được. Điều đó yêu cầu phải xây dựng lại toàn bộ những ứng dụng phụ thuộc của từng phiên bản để dành cho việc hoặt động trên WinDows, nhưng nó chỉ có thể khi những phụ thuộc được xây dựng có thể hoạt động trên WinDows.
*Như vậy nếu hoạt động trên Windows hoặc MAC OS X chúng ta chỉ cần chạy một máy ảo để chạy Docker sau đó dùng docker để tạo ra các container -> chi phí chạy máy chủ được cố định trong khi đó chúng ta có thể tăng số container tạo được.*(phần này em không rõ có đúng không, em nghĩ cái này đúng với các bản window trước bản 1607 ).
Sự di động mà docker mang đến giải quyết nhiều vấn đề như: chạy những phần mềm như nhau trên bất kì hệ thống nào, tạo ra một môi trường hoạt động thuần nhất trên nhiều hệ thống máy tính, những người bảo trì phần mềm, những nhà phát triển có thể tập chung viết phần mềm với chỉ một bộ ứng dụng phụ thuộc (mà không cần quan tâm nó có thể hoạt động được trên các hệ thống khác nhau hay không,...
*  Trong quyển Docker in Actions có viết "This new portability helps users in a few ways. First, it unlocks a whole world of soft-ware that was previously inaccessible." em không hiểu rõ nghĩ của câu này và lợi ích thứ nhất lắm.

# Một số lệnh cơ bản 
<img src="https://lh3.googleusercontent.com/dpE1Rb15ExIP-3LRaYo-SgLQdG2I0WpsROUVU_OS7Vj-sBjyjKAtbe6paWmwSeGMNK_CWZGp2dwsdlvyw0YKYMuL4dWk4LpgcjbiaQcldO6g9ve1VCrlP4qlANuTs2IvQNDukBcSmTde29U9IavTqWaWVS8I0PhdMGwXOn930IkFOcLJ-2FQf1zDn44nrxhWlq5fGQhe39iKIcMKEy6HtiiZVNIAWkxdTi8eQ0RbY0pfRUmjAktKwhh9Mfrh-OFPVcP9DNiOJiqCgnBhTXOzbDzF9Qjc9ScnrR4sFuRuggU8ZGhKh14zN9SM9gW50twhnBxjw-jZJYyqFOyww9YdGm8i34dgTYZFIDf3MLxq7C3MUNM8BGixnhLNP74qliZSifMAJ2UVL6oCsn-vM4BUJq1oKIaWXPN_E3MzLlwQnkDZAH_C41XDjcHoK5Zg3HEHDytQl-buMjZ5srgwaxtd8W52TOpLs6kQc89RhnbyuY4E0Yf1gGYkGTdll-uiLlTTSRPGqqJ_DCqa9kaoXc-GUD2iEAyQS4BWyu7NEcDD70S-LAT3pcJ7t0ZUuZH_VZCkhfyLOBJ5YOIujhtCoUo7V0TD8wyFbCGFLk97bn_8Ye3VKPywguEjlMEBGMHMJ0Xwn2YZwUHNDQ1cnakggMkZoQoxBIIKUHY=w786-h525-no">
Nhưng em chạy một số lệnh thì nó lại gặp lỗi.
<img src="https://lh3.googleusercontent.com/xyHSwlVLZCjwGJedZ9QDAZ8JnZAAAcyZoqWrPRUIeHjpMon1zpyiDYQAMeNZDryx_ecFoqURSjqeWeR_xE5lEoKAl57EfgoYBGpe1Q3Sr0dsr1D2QqGt_6Cv_tb349JLX17mr-VQpjvLkWDgmVrxHHeyR45sdvJb2UupMySukfRXH0DieBK-bQgjpH7pIslqac5VIYH6mEbknNaApGuy7D9KjmFhtdSglF-AtDrZewBtvbATbjdwXfEjRmGrH80eLlu2l6z_XWRABpEVdEJWiLZ5jRwTFMVM883p1zzuKq10Q_PjbXNnF9qQdIMvKdWtkXXgYaEFWrJoU418pUdAxIr_l-pWUJ2sPd4vlu7gPU40MEw64VBHk04AkTHdwCknaP00CJIFWNFCqSWMRAp-nHTOUzgof3dkVrfGfh-KB_Ecqa09ZHdcStMM6yEnOETK4f90DhTysImGXwUo4EGcJ24s7sjs2Qf4M4c33jD-iCz2dkPEDnUJwrIFFnwRhGv8PDBF32J1FqkmYQvPQAPUP4dJKPzm9Z_Inef6FD6Js5Og5xmzhPHEZ8bGY8F9MvA6ioNcMX5H200jvOGe766Crd4_71kf7J8uXK8wtSY2717ZHP_s2gdXXrLI5w7KoZKC4Bv5H5jN8uE4gpuRy4ssVuLIJXtKT3g=w963-h204-no">
Em sẽ tìm cách để xem sửa như nào huhu!
# Grafana 
<img src="https://namlee.net/wp-content/uploads/2018/07/huong-dan-cai-dat-he-thong-monitor-voi-grafana-influxdb-va-telegraf-tren-centos-7.png">
Grafana là một bộ mã nguồn mở sử dụng trong việc phân tích các dữ liệu thu thập được từ server và hiện thị một các trực quan dữ liệu thu thập được ở nhiều dạng khác nhau.


"the web server will be available only to other programs on your compute" câu này có nghĩ là dùng web browser chỉ truy cập cập được vào các chương trình có trên máy tính của mình. *em hiểu có đúng không vậy mama*

