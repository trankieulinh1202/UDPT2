---
title: "Truyền Thông"
date: "2025-05-01"
updated: "2025-05-01"
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
### 4. Hướng dẫn cài đặt
Cách 1: Dùng Docker
```
docker run -d --hostname rabbitmq-host --name rabbitmq \
  -p 5672:5672 -p 15672:15672 \
  rabbitmq:3-management
```
Truy cập Management UI: http://localhost:15672
(Tài khoản mặc định: guest / guest)

Cách 2: Cài trực tiếp (Ubuntu)
```
sudo apt update
sudo apt install rabbitmq-server
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
```

## Bài Tập 2: Code Một Hệ Thống Sử Dụng RabbitMQ
**Mục tiêu**
Xây dựng hệ thống đơn giản với một Producer gửi thông điệp và một Consumer nhận thông điệp bằng RabbitMQ.

**Môi trường:**
Python

Thư viện: pika (client RabbitMQ cho Python)
```
pip install pika
```
**Producer – Gửi thông điệp (sender.py)**
``` 
python
import pika

# Kết nối đến RabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Tạo queue nếu chưa có
channel.queue_declare(queue='hello')

# Gửi thông điệp
channel.basic_publish(exchange='',
                      routing_key='hello',
                      body='Hello RabbitMQ!')
print(" [x] Sent 'Hello RabbitMQ!'")

connection.close()
```
**Consumer – Nhận thông điệp (receiver.py)**
```
python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')

# Xử lý khi nhận được message
def callback(ch, method, properties, body):
    print(f" [x] Received {body}")

channel.basic_consume(queue='hello',
                      on_message_callback=callback,
                      auto_ack=True)
print(' [*] Waiting for messages...')
channel.start_consuming()
```
## Bài Tập 3: Tìm Hiểu RPC với JSON - Demo Thư Viện JSON-RPC
**1. RPC là gì?**
RPC (Remote Procedure Call) là cơ chế cho phép một chương trình gọi hàm của chương trình khác trên một máy khác như thể là gọi hàm cục bộ.

Trong bài trước bạn đã dùng xmlrpc, bây giờ sẽ tìm hiểu một cách khác: JSON-RPC.
**2. JSON-RPC là gì?**
Là một chuẩn RPC sử dụng định dạng JSON.

Giao tiếp đơn giản qua HTTP hoặc TCP.

Không phụ thuộc vào ngôn ngữ lập trình.

**3. Các thư viện JSON-RPC phổ biến**

| Ngôn ngữ   | Thư viện                         |
| ---------- | -------------------------------- |
| Python     | `jsonrpcserver`, `jsonrpcclient` |
| JavaScript | `jayson`, `json-rpc-2.0`         |
| Go         | `golang-jsonrpc`                 |
| Java       | `jsonrpc4j`                      |


**4. Demo Python – jsonrpcserver và jsonrpcclient**
Cài đặt:
```
pip install jsonrpcserver jsonrpcclient flask
```
Server – server.py
```
python
from flask import Flask, request
from jsonrpcserver import method, dispatch

app = Flask(__name__)

@method
def add(x, y):
    return x + y

@app.route("/", methods=["POST"])
def handle():
    return dispatch(request.get_data().decode())

if __name__ == "__main__":
    app.run(port=5000)
```
Client – client.py
```
python
from jsonrpcclient import request

response = request("http://localhost:5000", "add", x=7, y=5)
print(response.data.result)  # Output: 12
```
**5. Ưu điểm của JSON-RPC so với XML-RPC: **

Nhẹ hơn, dễ đọc hơn.

Tương thích với REST API hiện đại.

Hỗ trợ tốt với các frontend JavaScript.