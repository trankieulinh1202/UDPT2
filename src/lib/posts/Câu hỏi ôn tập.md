---
title: "CÂU HỎI ÔN TẬP CUÔÍ KỲ"
date: "2025-05-29"
updated: "2025-05-29"
categories:
  - "sveltekit"
  - "markdown"
  - "Deliverable_4: Phát triển website bán hàng sử dụng thư viện Dragonfly" 
coverImage: "/images/images.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Phát triển website bán hàng sử dụng thư viện Dragonfly
---
# Ôn Luyện thi cuối kỳ

## 1. Hệ thống phân tán, tập trung, phi tập trung khác nhau như thế nào, nêu ví dụ về mỗi loại, làm thế nào để xác định sự khác biệt chính?

* Hệ thống tập trung (Centralized System): Mọi xử lý và lưu trữ dữ liệu được thực hiện tại một máy chủ trung tâm. Máy khách chỉ gửi yêu cầu và nhận phản hồi.
Ví dụ: Máy chủ cơ sở dữ liệu đơn lẻ như MySQL server.


* Hệ thống phi tập trung (Decentralized System): Không có nút trung tâm duy nhất. Nhiều nút có thể chia sẻ trách nhiệm xử lý và lưu trữ, có thể tồn tại nhiều nút chủ tương đối.
Ví dụ: Blockchain, mạng P2P như BitTorrent.

* Hệ thống phân tán (Distributed System): Một tập hợp các máy tính độc lập được kết nối mạng, phối hợp để đạt mục tiêu chung, nhưng người dùng thấy như đang sử dụng một hệ thống thống nhất.
Ví dụ: Google File System, hệ thống Microservices.

* Khác biệt chính: Cách quản lý tài nguyên, tính chịu lỗi, mức độ phân quyền và mức độ liên kết giữa các nút.

## 2. Các đặc tính của hệ phân tán là gì? giải thích cho từng đặc điểm thật kỹ.

- Tính minh bạch (Transparency):

     - Truy cập: người dùng không cần biết dữ liệu nằm ở đâu.

     - Định vị: vị trí tài nguyên được ẩn đi.

     - Di chuyển: tài nguyên có thể di chuyển mà không ảnh hưởng đến ứng dụng.

     - Sao chép: sử dụng bản sao mà không thay đổi hành vi.

- Tính đồng nhất: giao diện giống nhau bất kể vị trí.

- Tính mở (Openness): Hệ thống sử dụng các chuẩn mở, dễ mở rộng.

- Tính mở rộng (Scalability): Khả năng mở rộng theo số lượng người dùng hoặc quy mô hệ thống.

- Tính chịu lỗi (Fault tolerance): Hệ thống vẫn hoạt động khi một số thành phần gặp lỗi.

- Hiệu năng (Performance): Hệ thống cần tối ưu truyền thông và xử lý phân tán.

- Bảo mật (Security): Xác thực, phân quyền, mã hóa cần thiết để đảm bảo an toàn.

## 3. Mục đích của nút chủ trong một hệ phân tán là để làm gì? Điều gì sẽ xảy ra nếu nút chủ gặp sự cố?
- Mục đích của nút chủ (Master Node):

    - Điều phối các nút khác (ví dụ: trong MapReduce).

    - Quản lý metadata (ví dụ: GFS).

    - Phân công công việc.

- Khi nút chủ gặp sự cố:

    - Toàn bộ hệ thống có thể ngừng hoạt động nếu không có cơ chế dự phòng.

    - Cần có chiến lược như bầu lại leader (ví dụ thuật toán Bully, Ring) hoặc dùng master replica.

## 4. Trong một mạng không gian, tại sao các máy thường giao tiếp với nhau qua gossip protocol mà không gửi thông tin đến tất cả các máy khác hoặc gửi về một nút trung tâm?

- Lý do sử dụng gossip protocol:

    - Khả năng mở rộng: không cần mỗi nút gửi đến tất cả các nút khác.

    - Tính chịu lỗi: ngay cả khi một số nút không nhận được thông tin thì các nút khác vẫn truyền lại.

    - Tối ưu chi phí mạng: giới hạn lượng thông điệp, tránh tắc nghẽn.

    - Lan truyền dần dần nhưng bền vững, tương tự như lan truyền tin đồn.

## 5. Các yếu tố cốt lõi của một hệ phân tán là gì?
- Các nút độc lập: Máy tính riêng biệt có thể xử lý riêng.

- Mạng truyền thông: Kết nối các nút để trao đổi dữ liệu.

- Tính minh bạch trong sử dụng: Người dùng thấy như hệ thống thống nhất.

- Giao thức truyền thông chuẩn hóa: Cho phép tương tác hiệu quả.

- Cơ chế đồng bộ hóa và chịu lỗi: Giữ cho hệ thống ổn định khi có lỗi xảy ra.

## 6. Nêu những lí do sử dụng hệ phân tán.

- Khả năng mở rộng: Hệ phân tán cho phép thêm tài nguyên (máy chủ, lưu trữ) dễ dàng.

- Tăng hiệu năng xử lý: Công việc được phân chia và xử lý song song.

- Tính sẵn sàng cao: Khi một nút hỏng, các nút khác có thể tiếp tục hoạt động.

- Chia sẻ tài nguyên: Tài nguyên được chia sẻ và truy cập từ nhiều vị trí.

- Giảm chi phí: Sử dụng nhiều máy tính giá rẻ thay vì một siêu máy chủ.

- Địa lý phân tán: Phù hợp cho tổ chức toàn cầu với máy chủ đặt ở nhiều nơi.

## 7. Nêu định nghĩa hệ phân tán.

Hệ phân tán là một tập hợp các máy tính độc lập, phối hợp với nhau để xuất hiện với người dùng như một hệ thống thống nhất. Chúng liên kết thông qua mạng truyền thông và cùng thực hiện các tác vụ chung.

## 8. Chụp và để lên blog các hình của thuật toán Cristian, Berkeley, RBS, Lamport, Bully, Ring.

Minh họa thuật toán Cristian
![Minh họa thuật toán Cristian](/images/Cristian.jpg)

Minh họa thuật toán Berkeley
![Minh họa thuật toán Berkeley](/images/Berkeley.jpg)

Minh họa thuật toán Lamport
![Minh họa thuật toán Lamport](/images/Lamport.jpg)

Minh họa thuật toán Bully
![Minh họa thuật toán Bully](/images/Bully.jpg)

Minh họa thuật toán Ring
![Minh họa thuật toán Ring](/images/Ring.jpg)

## 9. Kỹ thuật phân tán nào hỗ trợ lập trình thủ tục, lập trình web, hướng đối tượng, liệt kê.

- Lập trình thủ tục (Procedural Programming):
Dùng RPC (Remote Procedure Call): Gọi hàm từ xa giống như gọi nội bộ.

- Lập trình web:
Dùng RESTful API, gRPC, WebSockets, microservices.

- Lập trình hướng đối tượng:
Dùng RMI (Java Remote Method Invocation), CORBA, .NET Remoting.

## 10. Tiến trình nhẹ, tiến trình, luồng có những ưu điểm và nhược điểm gì, liệt kê. Khi một lời gọi hệ thống dừng thì đối với 3 loại như thế nào. Tiến trình nhẹ, tiến trình và luồng có mối quan hệ như thế nào với nhau và với chính nó?

##### Tiến trình (Process):
- Ưu điểm:
   - Cách ly tốt giữa các tiến trình
   - Bảo mật cao do không chia sẻ bộ nhớ
   - Lỗi trong một tiến trình không ảnh hưởng đến tiến trình khác
- Nhược điểm:
   - Tốn tài nguyên hệ thống (mỗi tiến trình có không gian địa chỉ riêng)
   - Giao tiếp giữa các tiến trình phức tạp và chậm
   - Chi phí chuyển đổi ngữ cảnh cao
##### Luồng (Thread):
- Ưu điểm:
  - Nhẹ hơn tiến trình, tốn ít tài nguyên
  - Chia sẻ tài nguyên trong cùng tiến trình (bộ nhớ, tệp đã mở)
  - Chuyển đổi ngữ cảnh nhanh hơn
  - Giao tiếp giữa các luồng dễ dàng
- Nhược điểm:
  - Dễ gây lỗi nếu không đồng bộ hóa tốt
  - Một luồng gặp sự cố có thể ảnh hưởng đến cả tiến trình
  - Khó gỡ lỗi hơn
##### Tiến trình nhẹ (Lightweight Process - LWP):
- Đặc điểm: Là khái niệm trung gian giữa thread và process
- Ưu điểm:
  - Được lên lịch bởi hệ điều hành như process riêng biệt
  - Chia sẻ tài nguyên của tiến trình cha như thread
- Nhược điểm:
  - Phụ thuộc vào cài đặt cụ thể của hệ điều hành
  - Phức tạp trong quản lý
#### Khi một lời gọi hệ thống dừng:
- Tiến trình: Toàn bộ tiến trình dừng lại cùng với tất cả các luồng bên trong
- Luồng: Chỉ dừng luồng thực hiện lời gọi, các luồng khác trong cùng tiến trình vẫn tiếp tục
- Tiến trình nhẹ: Phụ thuộc vào cài đặt hệ thống, thường chỉ dừng LWP đó
##### Mối quan hệ:
- Một tiến trình chứa ít nhất một luồng (luồng chính)
Nhiều luồng có thể tồn tại trong cùng một tiến trình
Các luồng trong một tiến trình chia sẻ không gian địa chỉ và tài nguyên
Tiến trình nhẹ là cách triển khai của luồng ở mức hệ điều hành

## 11. Mô hình client server là gì, vai trò của máy chủ và máy khách là gì.

- Mô hình client-server là mô hình trong đó:

   - Client: gửi yêu cầu.
   - Server: nhận, xử lý và trả kết quả.
   - Ví dụ: trình duyệt là client, máy chủ web là server.

## 12. Quá trình gọi thủ tục từ xa là gì.
Là phương pháp cho phép gọi hàm từ một máy khác thông qua mạng như gọi cục bộ. Quá trình này gồm:

- Đóng gói tham số (marshalling)
- Truyền dữ liệu qua mạng
- Giải mã (unmarshalling) và thực thi hàm trên server
- Trả kết quả về cho client
## 13. Nêu mục đích và lợi ích của mô hình phân tầng giao thức, hướng thông điệp bền vững.

ục đích:
- Tách biệt các chức năng xử lý
- Chuẩn hóa giao tiếp giữa các thành phần
- Đơn giản hóa thiết kế hệ thống

Lợi ích:
- Dễ phát triển và bảo trì
- Tái sử dụng các tầng
- Hỗ trợ mở rộng
- Dễ dàng thay thế các thành phần

Hướng thông điệp bền vững:
- Đảm bảo truyền tin không bị mất mát
- Xử lý đúng thứ tự các thông điệp
- Hỗ trợ hoạt động offline và khắc phục lỗi

## 14. Sharding là gì.

Là kỹ thuật chia nhỏ cơ sở dữ liệu lớn thành nhiều phần nhỏ (shard), mỗi shard lưu ở một server khác nhau để tăng hiệu năng và khả năng mở rộng.

## 15. Các gói luồng có thể làm những nhiệm vụ gì?

- Tạo và quản lý vòng đời luồng
- Đồng bộ hóa luồng
- Quản lý tài nguyên chia sẻ
- Giao tiếp giữa các luồng
- Quản lý lịch trình thực thi
- Hỗ trợ tạo thread pool


## 16. Phân loại sự khác nhau giữa luồng kiểu người dùng và luồng kiểu nhân.
##### User-level Thread:
- Quản lý bởi thư viện người dùng
- Không cần gọi hệ thống để chuyển đổi ngữ cảnh
- Không tận dụng được đa nhân CPU
- Độc lập với hệ điều hành
- Hiệu suất cao trong chuyển đổi ngữ cảnh
##### Kernel-level Thread:
- Quản lý trực tiếp bởi hệ điều hành
- Tận dụng được đa nhân CPU
- Tốn tài nguyên hơn user-level thread
- Chuyển đổi ngữ cảnh chậm hơn
- Cung cấp khả năng lập lịch song song thực sự

## 17. Nêu các hàm chính ở trong RPC và giải thích chức năng của nó.

##### Client Stub:
- Đóng gói tham số (marshalling)
- Gửi yêu cầu đến server
- Nhận và giải mã kết quả trả về
##### Server Stub:
- Nhận và giải mã yêu cầu từ client
- Gọi hàm thực tế trên server
- Đóng gói kết quả và gửi lại client
#### RPC Runtime:
- Quản lý kết nối mạng
- Xử lý lỗi truyền thông
- Đảm bảo độ tin cậy của lời gọi
- Hỗ trợ tìm kiếm dịch vụ (service discovery)




