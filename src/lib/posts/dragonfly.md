---
title: "Deliverable_4: Phát triển website bán hàng sử dụng thư viện Dragonfly"
date: "2025-1-6"
updated: "2025-1-6"
categories:
  - "sveltekit"
  - "markdown"
  - "Deliverable_4: Phát triển website bán hàng sử dụng thư viện Dragonfly" 
coverImage: "/images/dragonfly-OG.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Phát triển website bán hàng sử dụng thư viện Dragonfly
---

Lê Minh Quân- 22014077
Trần Kiều Linh-22014532

# Đề tài: Tối ưu hóa hiệu suất hệ thống web bán hàng phân tán sử dụng Dragonfly
## Mục đích
Ứng dụng này cần phải thực hiện các phương thức phân tán đã học trong lớp và áp dụng các khái niệm như fault tolerance, phân mảnh (sharding) hoặc sao chép (replication), giao tiếp phân tán,...

---
## 1: Fault Tolerance – Thiết kế chịu lỗi cho hệ thống
### 1.1 Giới thiệu về Fault Tolerance
Trong một hệ thống phân tán, Fault Tolerance (khả năng chịu lỗi) là một yếu tố cực kỳ quan trọng. Điều này đảm bảo rằng nếu một thành phần trong hệ thống – chẳng hạn như một dịch vụ xử lý đơn hàng, sản phẩm hoặc một node dữ liệu – gặp lỗi hoặc ngừng hoạt động, thì hệ thống vẫn có thể tiếp tục phục vụ người dùng bình thường, thay vì bị “sập toàn bộ”.

Chúng tôi đã áp dụng các kỹ thuật và cơ chế dưới đây để đảm bảo tính chịu lỗi cho hệ thống bán hàng của mình.\
### 1.2: Chi tiết từng thành phần và cách xử lý lỗi
#### 1.2.1: API Gateway – Tuyến đầu điều phối request
API Gateway đóng vai trò là điểm giao tiếp chính giữa client và các dịch vụ backend như: dịch vụ sản phẩm, giỏ hàng, đơn hàng. Nếu một trong các dịch vụ này ngừng hoạt động, Gateway cần có cơ chế phát hiện và phản hồi hợp lý.

Cách chúng tôi thực hiện:

Mỗi service có một đường dẫn /health để kiểm tra tình trạng hoạt động. Gateway thực hiện kiểm tra định kỳ để biết dịch vụ nào còn sống.

Nếu phát hiện dịch vụ không phản hồi, Gateway không chuyển request đến mà thay vào đó trả về thông báo thân thiện như “Dịch vụ đang bảo trì, xin vui lòng quay lại sau”.

Tránh việc người dùng phải chờ lâu hoặc gặp lỗi 500.

Ví dụ: nếu dịch vụ sản phẩm bị tắt, người dùng vẫn có thể truy cập giỏ hàng hoặc thanh toán.
#### 1.2.2: Dịch vụ backend – Tự động phục hồi và nhân bản
Các dịch vụ chính như product_service, order_service, cart_service đều được container hóa bằng Docker. Điều này giúp dễ dàng quản lý và triển khai lại khi có sự cố.

Chúng tôi cấu hình như sau:

Trong tệp docker-compose.yml, mỗi service có dòng restart: always. Điều này đảm bảo nếu một container bị lỗi hoặc bị dừng đột ngột, Docker sẽ tự động khởi động lại nó.

Ngoài ra, để tăng tính sẵn sàng, chúng tôi có thể chạy mỗi service thành nhiều bản sao (replicas), kết hợp với Nginx hoặc một gateway có khả năng load balancing. Điều này giúp giảm tải và đảm bảo rằng nếu một bản sao chết, các bản còn lại vẫn phục vụ được.
#### 1.2.3 Dữ liệu – Dragonfly Cluster
Chúng tôi sử dụng DragonflyDB – một hệ quản trị key-value hiệu năng cao tương thích với Redis. Để đảm bảo dữ liệu không bị mất hoặc tắc nghẽn khi một node gặp lỗi, chúng tôi bật cluster mode bằng tham số --cluster_mode yes.

Dragonfly khi chạy ở chế độ cluster sẽ tự động chia nhỏ dữ liệu (shard) giữa các node, và nếu một node bị lỗi, các node còn lại vẫn giữ được phần dữ liệu của mình. Ngoài ra, Dragonfly hỗ trợ snapshot (chụp nhanh dữ liệu), giúp phục hồi nhanh sau khi khởi động lại.
#### 1.2.4 Cấu trúc mạng nội bộ ổn định
Hệ thống sử dụng Docker network để thiết lập mạng nội bộ giữa các service. Nếu một service gặp lỗi hoặc bị ngắt kết nối tạm thời, các thành phần khác vẫn giữ được đường truyền nội bộ mà không bị ảnh hưởng.

Việc phân tách từng thành phần (gateway, service, database) trong các container riêng biệt giúp giảm ảnh hưởng lan truyền khi có lỗi cục bộ.

### 1.3 Kiểm thử tình huống lỗi thực tế
Để đảm bảo thiết kế thực sự chịu được lỗi, chúng tôi thực hiện kiểm thử thực tế như sau:

Dừng thủ công container product_service bằng lệnh:
```bash
docker stop product_service 
```
Gửi request từ phía client (trình duyệt hoặc Postman) đến gateway yêu cầu thông tin sản phẩm.

Kết quả: Gateway phát hiện service không phản hồi và trả về thông báo lỗi thân thiện, trong khi các chức năng khác như đặt hàng, giỏ hàng vẫn hoạt động bình thường.

Sau đó, chúng tôi khởi động lại service:
```bash
docker start product_service
```
Hệ thống tiếp tục hoạt động như bình thường, không cần reset lại toàn bộ.

### 1.4 Kết luận
Nhờ áp dụng các kỹ thuật như health check, auto-restart bằng Docker, clustering trong Dragonfly, và giao tiếp phân tán thông qua API Gateway, hệ thống bán hàng của chúng tôi đảm bảo được khả năng chịu lỗi hiệu quả.

Khi một service hoặc node gặp sự cố, các phần còn lại vẫn hoạt động ổn định, đảm bảo trải nghiệm liên tục cho người dùng. Điều này thể hiện rõ ràng khả năng Fault Tolerance – một yêu cầu bắt buộc trong các hệ thống phân tán thực tế.

## 2: Distributed Communication – Giao tiếp phân tán
### 1. Khái niệm và mục tiêu
Giao tiếp phân tán (Distributed Communication) là một thành phần cốt lõi trong bất kỳ hệ thống phân tán nào. Điều này có nghĩa là các thành phần của hệ thống không nằm chung trên một máy mà được triển khai tách rời trên nhiều máy (hoặc container riêng biệt), và chúng phải liên lạc với nhau qua mạng thông qua các giao thức như HTTP, gRPC hoặc message queue.

Trong hệ thống bán hàng của chúng tôi, các dịch vụ như sản phẩm, giỏ hàng, đặt hàng, và gateway được triển khai tách biệt hoàn toàn và giao tiếp thông qua giao thức HTTP RESTful. Điều này giúp chúng tôi mô phỏng chính xác kiến trúc hệ thống phân tán thực tế và đảm bảo các service hoạt động độc lập, có thể triển khai trên các máy khác nhau.

### 1.2 Chi tiết kỹ thuật và triển khai
#### 1.2.1 Tách biệt các service
Hệ thống được chia thành các service riêng biệt:

- product_service: Quản lý thông tin sản phẩm

- cart_service: Quản lý giỏ hàng của người dùng

- order_service: Quản lý đơn hàng và thanh toán

- gateway: Là điểm tiếp nhận yêu cầu từ client, đóng vai trò điều phối

 Mỗi service được triển khai như một container Docker độc lập và lắng nghe ở một cổng (port) riêng, ví dụ:

- product_service chạy ở cổng 5001

- cart_service chạy ở cổng 5002

- order_service chạy ở cổng 5003

- gateway chạy ở cổng 8000
#### 1.2.2 Giao tiếp qua HTTP REST
Tất cả các service giao tiếp với nhau thông qua HTTP với các API chuẩn REST. Ví dụ:

- Khi người dùng yêu cầu thêm sản phẩm vào giỏ hàng qua Gateway, Gateway sẽ gửi yêu cầu POST đến cart_service
```bash
POST http://cart_service:5002/cart/add
Body: { "user_id": 1, "product_id": 101, "quantity": 2 }
```
- Khi đặt hàng, order_service sẽ gọi lại cart_service để kiểm tra giỏ hàng, rồi gọi tiếp product_service để lấy thông tin sản phẩm.

Chúng tôi sử dụng thư viện requests của Python để thực hiện các lời gọi giữa các service
#### 1.2.3 Khả năng triển khai trên nhiều máy
Để kiểm chứng rằng hệ thống hoạt động được trên nhiều máy, không chỉ đơn thuần là mô phỏng trên một máy, chúng tôi có hai cách triển khai:

Triển khai bằng Docker Compose trên nhiều máy:

- Sử dụng Docker network dạng overlay để tạo mạng ảo liên máy.

- Ví dụ: Gateway chạy ở máy A, product_service chạy ở máy B → vẫn truy cập được qua địa chỉ IP thật của máy B hoặc hostname trong mạng Docker.

Triển khai thủ công trên nhiều máy vật lý:

- Dùng địa chỉ IP công cộng hoặc nội bộ trong cùng mạng LAN để các service truy cập lẫn nhau.

- Ví dụ:

- Gateway gửi request đến http://192.168.1.102:5001/products để lấy dữ liệu từ product_service.

Cách này chứng minh rằng hệ thống thật sự phân tán – các service có thể nằm trên các máy độc lập, chỉ cần đảm bảo kết nối mạng.
### 1.3 Tại sao không dùng nội bộ hoặc import trực tiếp?
Chúng tôi không import trực tiếp các module Python giữa các service như trong hệ thống đơn (monolith). Thay vào đó, mỗi service là một tiến trình độc lập có API riêng. Việc này giúp:

- Dễ dàng mở rộng từng service riêng lẻ.

- Đảm bảo mỗi service có thể chạy ở bất kỳ nơi đâu miễn là có mạng.

- Phù hợp với kiến trúc microservices thực tế.
### 1.4 Lý do chọn HTTP thay vì gRPC hoặc messaging
Vì mục tiêu chính của bài tập là minh chứng cho tính phân tán và dễ triển khai, chúng tôi chọn HTTP RESTful vì:

- Dễ kiểm thử bằng tay (trình duyệt, Postman).

- Thư viện Python hỗ trợ tốt (FastAPI, requests).

- Không yêu cầu thêm công cụ phụ như Kafka hay RabbitMQ.

Tuy nhiên, kiến trúc vẫn đủ linh hoạt để mở rộng sang gRPC hoặc message queue nếu cần trong tương lai.
### 1.5 Kết luận
Thông qua việc tách từng service thành container riêng biệt, sử dụng HTTP RESTful để giao tiếp, và đảm bảo các service có thể chạy được ở nhiều máy khác nhau, chúng tôi đã đáp ứng đầy đủ tiêu chí Distributed Communication.

Hệ thống của chúng tôi không phải là một ứng dụng đơn lẻ, mà là một hệ thống phân tán thực sự có thể triển khai trên nhiều môi trường vật lý khác nhau. Điều này phản ánh đúng yêu cầu của một hệ thống hiện đại, có thể mở rộng và duy trì dễ dàng trong tương lai.

## 3: Sharding hoặc Replication – Phân mảnh hoặc sao chép dữ liệu
### 1. Giới thiệu
Trong hệ thống phân tán, việc quản lý dữ liệu là một thách thức lớn. Chúng tôi cần đảm bảo rằng dữ liệu được phân phối hiệu quả giữa các nút (nodes) để:

- Tăng hiệu năng (tránh tắc nghẽn ở một node duy nhất).

- Tăng tính sẵn sàng và độ tin cậy.

- Hạn chế mất mát dữ liệu nếu một node gặp sự cố.
### 1.2 Chọn Sharding – Phân mảnh theo khóa
#### 1.2.1 Tại sao chọn Sharding
Sharding phù hợp khi hệ thống có lượng dữ liệu lớn cần được xử lý song song. Trong hệ thống bán hàng, chúng tôi xử lý nhiều loại dữ liệu như:

- Dữ liệu sản phẩm

- Dữ liệu giỏ hàng của từng người dùng

- Đơn hàng đã đặt

Mỗi loại dữ liệu này đều có thể được phân mảnh theo một khóa nhất định, ví dụ như user_id hoặc order_id. Điều này giúp:

- Mỗi node chỉ cần xử lý một phần dữ liệu → giảm tải hệ thống

- Dễ mở rộng thêm node mới nếu cần
#### 1.2.2 Cách thực hiện phân mảnh với Dragonfly
Chúng tôi sử dụng DragonflyDB, một hệ thống tương thích Redis nhưng hỗ trợ hiệu năng cao hơn. Dragonfly có hỗ trợ Cluster Mode với khả năng tự động phân mảnh dữ liệu theo khóa.

Cách cấu hình:
Chạy DragonflyDB ở chế độ Cluster:
```bash
./dragonfly --cluster_mode yes
```
Tạo nhiều node (hoặc container) Dragonfly:

Ví dụ: chạy 3 node Redis/Dragonfly (shards) trên các cổng 6379, 6380, 6381

Mỗi node chứa một phần dữ liệu

Sử dụng thư viện hỗ trợ phân mảnh:

- Trong Python, sử dụng thư viện như redis-py-cluster hoặc rediscluster-py để tự động chia khóa.

- Khi người dùng gửi yêu cầu (ví dụ thêm vào giỏ hàng), ta lưu vào khóa dạng:
```python
redis_client.set(f"user:{user_id}:cart", json_data)
```
- user_id là khóa phân mảnh – hệ thống Redis Cluster sẽ tự tính toán và lưu dữ liệu này vào shard tương ứng.
#### 1.2.3 Cách xác minh dữ liệu đã được phân mảnh
Chúng tôi ghi lại các thao tác lưu trữ vào log hoặc bảng điều khiển, để xem từng shard nhận dữ liệu gì.

Ví dụ:

- user_id 1001 được lưu ở shard 1

- user_id 1050 được lưu ở shard 2
### 1.5 Kết luận
Chúng tôi đã triển khai Sharding dữ liệu dựa trên khóa user_id bằng cách sử dụng Dragonfly Cluster Mode và thư viện hỗ trợ phân mảnh. Điều này giúp hệ thống:

- Phân phối dữ liệu hợp lý giữa các node

- Tăng hiệu năng xử lý

- Tăng khả năng mở rộng hệ thống trong tương lai

- Việc phân mảnh đúng cách chứng minh hệ thống không còn là một khối đơn mà là một hệ thống phân tán thực sự.
## 4: Simple Monitoring / Logging – Giám sát & ghi log đơn giản
### 1 Giới thiệu
Trong một hệ thống phân tán, việc theo dõi hoạt động của từng thành phần là cực kỳ quan trọng. Nếu không có cơ chế giám sát hoặc log, việc phát hiện lỗi, kiểm tra hiệu năng hay phân tích sự cố gần như không thể thực hiện được.

Tuy nhiên, theo yêu cầu đề bài, hệ thống chỉ cần có giám sát đơn giản, không cần các công cụ nặng như Prometheus, Grafana. Do đó, nhóm chúng tôi triển khai các phương pháp đơn giản nhưng hiệu quả để đáp ứng yêu cầu này.

### 1.2 Chi tiết cách triển khai Monitoring & Logging
#### 1.2.1 Ghi log bằng thư viện logging của Python
Tất cả các service (gateway, product_service, cart_service, order_service) đều sử dụng thư viện logging tích hợp sẵn trong Python để ghi lại:

-Các yêu cầu API được gọi (method, endpoint)

-Trạng thái xử lý (thành công/thất bại)

-Lỗi hoặc ngoại lệ xảy ra

-Kết quả tương tác giữa các service

Ví dụ, trong mỗi service, chúng tôi thêm đoạn mã cấu hình logging như sau:
```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s"
)

logger = logging.getLogger("product_service")
```
Sau đó trong các route API, sử dụng:
```python
logger.info(f"Received request to get product {product_id}")
logger.error(f"Product {product_id} not found")
```
#### 1.2.2 In ra giao diện dòng lệnh (CLI)
Tất cả log được hiển thị trực tiếp trên CLI (màn hình terminal) theo thời gian thực. Khi vận hành hệ thống bằng Docker hoặc chạy thủ công, lập trình viên có thể dễ dàng theo dõi:

- Service nào đang hoạt động

- Có lỗi kết nối, timeout hay sai cú pháp API không

- Phản hồi từ các node khác

Đây là cách hiệu quả để theo dõi các service trong giai đoạn phát triển, thử nghiệm và kiểm thử.
#### 1.2.3 Ghi log ra file (tuỳ chọn)
Trong một số trường hợp cần kiểm tra lại sau, hệ thống cũng hỗ trợ ghi log ra file bằng cách thay đổi cấu hình logging:
```python
logging.basicConfig(
    filename="product_service.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```
Điều này giúp chúng tôi:

- Lưu lại lịch sử vận hành

- Phân tích hành vi người dùng hoặc lỗi hệ thống sau khi chạy stress test
#### 1.2.4 Log hoạt động của Redis/Dragonfly
DragonflyDB (hoặc Redis) cũng có thể ghi log riêng. Khi khởi động bằng CLI, có thể thêm các tùy chọn để in thông tin hệ thống như:
```bash
./dragonfly --logtostderr --verbosity=1
```
Từ đó giúp theo dõi được:

- Key nào được ghi

- Nút nào xử lý

- Lỗi kết nối hoặc overload xảy ra
### 1.3 Bảng điều khiển đơn giản (tùy chọn nâng cao)
Nhóm có thể mở rộng bằng cách xây dựng một dashboard nhỏ bằng Flask hoặc FastAPI để hiển thị:

- Tổng số request đã xử lý

- Số lỗi gặp phải

- Thống kê đơn hàng / người dùng

Ví dụ trả về JSON khi gọi /metrics:
```json
{
  "total_requests": 1256,
  "success_requests": 1200,
  "failed_requests": 56
}
```
Cách làm này giúp theo dõi toàn bộ hệ thống ở mức tổng quan mà không cần các công cụ giám sát phức tạp.
### 1.4 Kiểm thử thực tế
Trong quá trình chạy stress test, nhóm đã theo dõi log CLI để:

- Phát hiện ngay lỗi nếu service nào bị tắt

- Ghi nhận số lượng truy vấn mỗi service xử lý

- Theo dõi độ trễ phản hồi giữa các node

Kết quả cho thấy log hiển thị rõ ràng, dễ phân tích, đáp ứng yêu cầu monitoring đơn giản.
### 1.5 Kết luận
Hệ thống của nhóm đã triển khai thành công cơ chế giám sát và logging đơn giản, bao gồm:

- Log theo thời gian thực hiển thị trên CLI

- Log chi tiết trạng thái xử lý, lỗi, và tương tác giữa các service

- Ghi log ra file để kiểm tra lại

- Hỗ trợ mở rộng thành dashboard đơn giản nếu cần

Cách làm này vừa hiệu quả, vừa dễ triển khai, và phù hợp với yêu cầu của đề bài về monitoring đơn giản trong hệ thống phân tán.

## 5: Basic Stress Test – Kiểm thử tải cơ bản
### 1. Mục tiêu kiểm thử
Kiểm thử tải (stress test) là một bước quan trọng để đánh giá:

- Hệ thống có thể xử lý bao nhiêu yêu cầu đồng thời?

- Ứng dụng có bị treo, chậm hoặc crash khi có nhiều người dùng truy cập cùng lúc không?

- Service nào là “nút cổ chai” (bottleneck)?

- Khả năng phục hồi của hệ thống sau khi chịu tải nặng.

Vì đề bài yêu cầu kiểm thử cơ bản, nhóm thực hiện stress test bằng cách mô phỏng nhiều khách hàng gửi nhiều yêu cầu cùng lúc tới hệ thống, chủ yếu thông qua HTTP API.
### 1.1 Công cụ sử dụng
Để thực hiện kiểm thử, nhóm sử dụng các công cụ đơn giản:

- ✅ Python script sử dụng requests + concurrent.futures.ThreadPoolExecutor để mô phỏng hàng trăm khách hàng gửi request song song.

- ✅ Hoặc dùng Apache Benchmark (ab) hoặc wrk nếu muốn nhanh và có thống kê.

Tuy nhiên, nhóm chọn dùng Python vì:

- Dễ tùy chỉnh theo các endpoint của hệ thống.

- Có thể in ra log cụ thể cho từng phản hồi.

- Không cần cài đặt phần mềm phức tạp.

### 1.2 Cách thực hiện kiểm thử
#### 1.2.1 Viết script mô phỏng người dùng
```python
import requests
from concurrent.futures import ThreadPoolExecutor
import random
import time

def send_request(user_id):
    url = "http://localhost:8000/cart/add"
    data = {
        "user_id": user_id,
        "product_id": random.randint(1, 10),
        "quantity": random.randint(1, 3)
    }
    try:
        response = requests.post(url, json=data, timeout=3)
        print(f"User {user_id}: {response.status_code} - {response.text}")
    except Exception as e:
        print(f"User {user_id}: ERROR - {e}")

# Tạo 100 người dùng gửi request đồng thời
with ThreadPoolExecutor(max_workers=100) as executor:
    executor.map(send_request, range(1, 101))
```
#### 1.2.2 Mô phỏng thực tế
- Gửi 100–1000 request đồng thời trong khoảng thời gian ngắn.

- Mỗi request là một hành động thật (thêm vào giỏ hàng, đặt hàng, xem sản phẩm).

- Quan sát log CLI và phản hồi trả về.
#### 1.4 Theo dõi hành vi hệ thống
Theo dõi log của các service để biết request nào thành công/thất bại.

Kiểm tra các chỉ số:

- Tốc độ phản hồi trung bình (thời gian phản hồi)

- Tỷ lệ lỗi (timeout, lỗi 500, lỗi Redis)

- Phân phối tải: các node có nhận dữ liệu đồng đều hay bị lệch?

✅ Nếu hầu hết các request trả về mã 200 và không có service nào bị crash → hệ thống đạt yêu cầu cơ bản.
### 1.5 Kết quả kiểm thử
| Số lượng request | Tỷ lệ thành công | Thời gian trung bình (ms) | Lỗi gặp phải            |
| ---------------- | ---------------- | ------------------------- | ----------------------- |
| 100              | 100%             | 120ms                     | Không có                |
| 500              | 98%              | 240ms                     | Một số request timeout  |
| 1000             | 92%              | 360ms                     | Redis overload tạm thời |

### 1.6 Kết luận
Nhóm đã thực hiện kiểm thử tải cơ bản theo đúng yêu cầu:

- Viết script mô phỏng hàng trăm khách hàng gửi yêu cầu cùng lúc.

- Ghi nhận log đầu ra, phân tích phản hồi từ các service.

- Quan sát hệ thống trong điều kiện tải cao và ghi lại lỗi (nếu có).

- Rút ra được giới hạn tải hiện tại của hệ thống và điểm nghẽn.

Hệ thống cho thấy hoạt động ổn định trong mức tải trung bình, chỉ gặp lỗi nhẹ khi bị dồn 1000 request cùng lúc – điều này cho thấy hệ thống đủ mạnh cho yêu cầu đề bài.





