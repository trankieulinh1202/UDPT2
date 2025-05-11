---
title: "Truyền Thông"
date: "2023-01-05"
updated: "2023-01-05"
categories:
  - "sveltekit"
  - "web"
  - "css"
  - "markdown"
coverImage: "/images/traodoithongtin.jpg"
coverWidth: 16
coverHeight: 9
excerpt: This post shows you how syntax highlighting works here.
---
## Bài 1: Tìm hiểu về RabbitMQ
### 1. Khái Niệm
RabbitMQ là một message-queuing software có thể được biết đến như là một người vận chuyển message trung gian hoặc một người quản lí các queue. Nói một cách đơn giản, nó là một phần mềm nơi các queue được định nghĩa, phục vụ cho ứng dụng với mục đích vận chuyển một hoặc nhiều message.
### 2.Cơ chế hoạt động  
#### Những khái niệm cơ bản trong RabbitMQ
Producer: Ứng dụng gửi message.

Consumer: Ứng dụng nhận message.

Queue: Lưu trữ messages.

Message: Thông tin truyền từ Producer đến Consumer qua RabbitMQ.

Connection: Một kết nối TCP giữa ứng dụng và RabbitMQ broker.

Channel: Một kết nối ảo trong một Connection. Việc publishing hoặc consuming từ một queue đều được thực hiện trên channel.

Exchange: Là nơi nhận message được publish từ Producer và đẩy chúng vào queue dựa vào quy tắc của từng loại Exchange. Để nhận được message, queue phải được nằm trong ít nhất 1 Exchange.

Binding: Đảm nhận nhiệm vụ liên kết giữa Exchange và Queue.

Routing key: Một key mà Exchange dựa vào đó để quyết định cách để định tuyến message đến queue. Routing key là địa chỉ dành cho message.

AMQP: Giao thức Advance Message Queuing Protocol, là giao thức truyền message trong RabbitMQ.

User: Để có thể truy cập vào RabbitMQ, chúng ta phải có username và password. Trong RabbitMQ, mỗi user được chỉ định với một quyền hạn nào đó. User có thể được phân quyền đặc biệt cho một Vhost nào đó.

Virtual host/Vhost: Cung cấp những cách riêng biệt để các ứng dụng dùng chung một RabbitMQ instance. Những user khác nhau có thể có các quyền khác nhau đối với vhost khác nhau. Queue và Exchange có thể được tạo, vì vậy chúng chỉ tồn tại trong một vhost.
#### Cơ chế hoạt động cơ bản:
Producer gửi thông điệp đến Exchange.

Exchange định tuyến thông điệp đến Queue dựa trên routing key.

Consumer nhận thông điệp từ Queue và xử lý.
### 3.Chức năng của RabbitMQ
Hàng đợi thông điệp: Lưu trữ và quản lý tin nhắn giữa các tiến trình.

Giao tiếp bất đồng bộ: Cho phép producer và consumer hoạt động không đồng thời.

Định tuyến thông minh: Phân phối tin nhắn đến đúng queue theo key hoặc chủ đề.

Đảm bảo tin cậy: Hỗ trợ xác nhận, lưu trữ bền vững, tránh mất dữ liệu.

Cân bằng tải: Phân phối tin nhắn đều giữa nhiều consumer.

Hỗ trợ mô hình Publish-Subscribe: Gửi tin nhắn đến nhiều subscriber cùng lúc.

Giao diện quản lý: Theo dõi, giám sát và cấu hình hệ thống dễ dàng.

Tích hợp dễ dàng: Hỗ trợ nhiều ngôn ngữ và giao thức.
