---
title: "Ứng dụng phân tán"
date: "2025-10-26"
updated: "2025-10-26"
categories:
  - "sveltekit"
  - "markdown"
  - "ứng dụng phân tán"
coverImage: "/images/jefferson-santos-fCEJGBzAkrU-unsplash.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Check out how heading links work with this starter in this post.
---

## Ứng dụng phân tán là gì 
-- Hệ thống phân tán (Distributed System) là tập hợp các máy tính độc lập được kết nối qua mạng, phối hợp với nhau để cung cấp dịch vụ như một hệ thống thống nhất. Người dùng tương tác với hệ thống như thể nó là một hệ thống đơn lẻ, mặc dù các thành phần có thể nằm ở nhiều vị trí khác nhau.​

## Các ứng dụng của hệ thống phân tán.
Hệ thống phân tán được ứng dụng rộng rãi trong nhiều lĩnh vực:
Điện toán đám mây: Cung cấp dịch vụ lưu trữ và xử lý dữ liệu trên nhiều máy chủ.​
Mạng xã hội: Xử lý và phân phối nội dung đến hàng triệu người dùng.​
Thương mại điện tử: Quản lý giao dịch và dữ liệu khách hàng trên nhiều máy chủ.​
Ứng dụng tài chính: Xử lý giao dịch ngân hàng và dữ liệu thị trường tài chính.​
Hệ thống IoT: Quản lý và xử lý dữ liệu từ các thiết bị cảm biến phân tán.
## Các khái niệm chính trong hệ thống phân tán
Scalability (Khả năng mở rộng): Khả năng hệ thống xử lý hiệu quả khi số lượng người dùng hoặc khối lượng công việc tăng lên.​
Fault Tolerance (Khả năng chịu lỗi): Khả năng hệ thống tiếp tục hoạt động đúng dù một số thành phần gặp sự cố.​

Availability (Tính sẵn sàng): Khả năng hệ thống luôn sẵn sàng phục vụ người dùng.​

Transparency (Tính trong suốt): Người dùng không cần biết về vị trí, trạng thái hoặc sự tồn tại của các thành phần trong hệ thống.​

Concurrency (Tính đồng thời): Khả năng nhiều tiến trình hoạt động đồng thời mà không gây xung đột.​

Parallelism (Tính song song): Khả năng thực hiện nhiều tác vụ cùng lúc để tăng hiệu suất.​

Openness (Tính mở): Khả năng hệ thống tương tác và tích hợp với các hệ thống khác.​

Vertical Scaling (Mở rộng theo chiều dọc): Tăng cường tài nguyên (CPU, RAM) cho một máy chủ.​

Horizontal Scaling (Mở rộng theo chiều ngang): Thêm nhiều máy chủ vào hệ thống để chia sẻ tải.​

Load Balancer (Cân bằng tải): Phân phối công việc đều giữa các máy chủ để tối ưu hiệu suất.​

Replication (Sao chép dữ liệu): Tạo bản sao dữ liệu trên nhiều máy chủ để tăng độ tin cậy và hiệu suất.​

## Ví dụ minh họa: Hệ thống thương mại điện tử
Trong một hệ thống thương mại điện tử:​

Scalability: Hệ thống có thể mở rộng để phục vụ hàng triệu người dùng đồng thời.​

Fault Tolerance: Nếu một máy chủ gặp sự cố, các máy chủ khác vẫn đảm bảo dịch vụ không bị gián đoạn.​

Availability: Hệ thống luôn sẵn sàng để người dùng mua sắm bất kỳ lúc nào.​

Transparency: Người dùng không nhận thấy sự thay đổi khi hệ thống chuyển đổi giữa các máy chủ.​

Concurrency: Nhiều người dùng có thể đặt hàng cùng lúc mà không gây xung đột dữ liệu.​

Parallelism: Xử lý nhiều đơn hàng cùng lúc để tăng tốc độ phục vụ.​

Openness: Hệ thống tích hợp với các cổng thanh toán và dịch vụ vận chuyển bên ngoài.​

Vertical Scaling: Nâng cấp máy chủ cơ sở dữ liệu để xử lý nhiều giao dịch hơn.​

Horizontal Scaling: Thêm nhiều máy chủ ứng dụng để phân phối tải.​

Load Balancer: Phân phối yêu cầu người dùng đến các máy chủ khác nhau để tránh quá tải.​

Replication: Dữ liệu sản phẩm và đơn hàng được sao chép trên nhiều máy chủ để đảm bảo an toàn và truy cập nhanh chóng.
## Kiến trúc của hệ thống phân tán
Các mô hình kiến trúc phổ biến trong hệ thống phân tán bao gồm:​
Users Soict HUST

Kiến trúc phân tầng (Layered Architecture): Chia hệ thống thành các tầng như trình bày, logic nghiệp vụ và dữ liệu.​

Kiến trúc hướng đối tượng (Object-Based Architecture): Sử dụng các đối tượng để tổ chức và quản lý các thành phần trong hệ thống.​

Kiến trúc hướng dữ liệu (Data-Centric Architecture): Tập trung vào việc quản lý và phân phối dữ liệu.​

Kiến trúc hướng sự kiện (Event-Driven Architecture): Các thành phần giao tiếp với nhau thông qua các sự kiện.​

Kiến trúc Microservices: Chia hệ thống thành các dịch vụ nhỏ, độc lập, có thể triển khai và mở rộng riêng biệt.
## Kết luận  
