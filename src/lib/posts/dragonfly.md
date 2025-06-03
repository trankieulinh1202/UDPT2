---
title: "Deliverable_4: Phát triển website bán hàng sử dụng thư viện Dragonfly"
date: "2025-06-01"
updated: "2025-06-01"
categories:
  - "sveltekit"
  - "markdown"
  - "Deliverable_4: Phát triển website bán hàng sử dụng thư viện Dragonfly" 
coverImage: "/images/dragonfly-OG.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Phát triển website bán hàng sử dụng thư viện Dragonfly

---
**File PDF:** [Báo cáo bài tập lớn](/images/baocaobaitaplon.pdf)
**Link GitHub:** [https://github.com/trankieulinh1202/GiuaKy-](https://github.com/trankieulinh1202/GiuaKy)


- Lê Minh Quân- 22014077
- Trần Kiều Linh-22014532

# Deliverable 4: Triển khai hệ thống phân tán với Dragonfly và Docker

## 1. Mục tiêu

Triển khai hệ thống website bán hàng phân tán sử dụng Python, triển khai bằng Docker, giao tiếp thông qua Redis Dragonfly để đáp ứng các tiêu chí:
- Fault tolerance
- Distributed communication
- Sharding hoặc Replication
- Logging
- Stress testing

## 2. Mô tả hệ thống triển khai

### Kiến trúc hệ thống
Hệ thống được chia thành các service nhỏ theo kiến trúc microservices:
- **Product Service**: Quản lý sản phẩm
- **Cart Service**: Xử lý giỏ hàng
- **Order Service**: Xử lý đơn hàng
- **Frontend Web**: Giao diện web cho người dùng
- **Redis Dragonfly**: Làm vai trò caching, message queue và lưu trữ trạng thái tạm thời

Tất cả service được container hóa bằng Docker và giao tiếp với nhau qua HTTP hoặc Redis.

## 3. Đáp ứng các tiêu chí hệ thống phân tán

### 3.1 Fault Tolerance

- **Cách triển khai**: Sử dụng Docker để đảm bảo khi một container bị dừng, hệ thống có thể khởi động lại tự động (`restart: always`). Redis Dragonfly giúp lưu cache để không mất dữ liệu tạm thời khi service gián đoạn.

- **Ví dụ cấu hình Docker Compose**:
```yaml
depends_on:
  redis:
    condition: service_healthy
restart: always
```

- **Đáp ứng**: ✅ Đáp ứng cơ bản. Khi một service tạm thời lỗi, các service khác vẫn hoạt động được. Tuy nhiên, chưa triển khai load balancer hoặc service replication nâng cao.

---

### 3.2 Distributed Communication

- **Cách triển khai**: Giao tiếp giữa các service qua HTTP REST API và message queue Redis Pub/Sub. Ví dụ, khi `Order Service` xử lý xong đơn hàng, sẽ publish sự kiện để các service khác như `Inventory Service` (nếu có) cập nhật.

- **Ví dụ code Python với Redis Pub/Sub**:
```python
# Publish message
redis.publish("order_completed", json.dumps(order_data))

# Subscribe ở service khác
pubsub = redis.pubsub()
pubsub.subscribe("order_completed")
for msg in pubsub.listen():
    handle_order_completed(msg)
```

- **Đáp ứng**: ✅ Đáp ứng cơ bản. Có sử dụng Pub/Sub và API nội bộ giữa các service. Tuy nhiên, chưa có discovery service hoặc RPC nâng cao như gRPC.

---

### 3.3 Sharding / Replication

- **Cách triển khai**: Redis Dragonfly hỗ trợ clustering để chia shard dữ liệu theo key. Trong triển khai hiện tại sử dụng một instance Dragonfly nhưng có cấu hình sẵn sàng cho mở rộng sharding nếu cần.

- **Ví dụ code lưu trữ dữ liệu phân mảnh (theo key)**:
```python
# Set key-value trong Redis theo pattern sharding
redis.set("product:123", json.dumps(product_info))
```

- **Đáp ứng**: ⚠️ Mới đáp ứng ở mức cấu hình đơn giản. Chưa thực sự triển khai cluster nhiều node Dragonfly do tài nguyên giới hạn, nhưng có thể mở rộng.

---

### 3.4 Logging

- **Cách triển khai**: Sử dụng module `logging` của Python để ghi log nội bộ trong từng service. Log được lưu trong Docker volume. Tuy nhiên chưa triển khai hệ thống log tập trung.

- **Ví dụ code logging Python**:
```python
import logging
logging.basicConfig(filename='logs/order.log', level=logging.INFO)
logging.info("Đơn hàng mới đã được xử lý")
```

- **Đáp ứng**: ❌ Chưa đáp ứng đầy đủ.

- **Lý do**: Hiện tại mới ghi log ở mức cơ bản trong từng container, chưa có hệ thống tập trung hóa logs hoặc phân tích log chuyên sâu do hạn chế về thời gian và tài nguyên triển khai ELK Stack hoặc các giải pháp tương đương.

- **Kế hoạch tương lai**: Sẽ tích hợp hệ thống ELK Stack hoặc Grafana Loki để thu thập và phân tích log tập trung, hỗ trợ giám sát và phát hiện sự cố hiệu quả hơn.

---

### 3.5 Stress Testing

- **Cách triển khai dự kiến**: Sử dụng `Locust` hoặc `Apache Bench (ab)` để gửi lượng lớn request đến hệ thống, đo thời gian phản hồi và độ ổn định.

- **Ví dụ cấu hình Locust**:
```python
from locust import HttpUser, task

class WebsiteUser(HttpUser):
    @task
    def load_homepage(self):
        self.client.get("/")
```

- **Đáp ứng**: ❌ Chưa thực hiện.

- **Lý do**: Hệ thống cần được kiểm tra trong môi trường giả lập có lượng lớn kết nối đồng thời, nhưng hiện chưa có điều kiện chuẩn bị máy chủ test riêng để triển khai stress testing hiệu quả.

- **Kế hoạch tương lai**: Sẽ thực hiện stress test trong giai đoạn tiếp theo để đánh giá khả năng chịu tải và điều chỉnh cấu hình hệ thống phù hợp hơn.

---

## 4. Kết luận

Hệ thống đã được triển khai thành công theo mô hình phân tán. Các tiêu chí đề ra đã được đáp ứng ở mức cơ bản và có thể mở rộng về sau:

- Sử dụng Docker giúp dễ dàng triển khai và mở rộng
- Redis Dragonfly đóng vai trò quan trọng trong caching và giao tiếp
- Logging và kiểm thử tải sẽ được tiếp tục cải thiện và hoàn thiện trong các giai đoạn tiếp theo

Hệ thống có tiềm năng mở rộng thêm các tính năng như Load balancing, Service registry, Auto-scaling nếu được triển khai trên môi trường cloud như AWS hoặc GCP.


