---
title: "ĐỀ TÀI GIỮA KỲ"
date: "2023-04-28"
updated: "2023-04-28"
categories:
  - "sveltekit"
  - "markdown"
  - "svelte"
coverImage: "/images/jerry-zhang-ePpaQC2c1xA-unsplash.jpg"
coverWidth: 16
coverHeight: 9
excerpt: This post demonstrates how to include a Svelte component in a Markdown post.
---

## 1. Nghiên cứu Dragonfly
**Mục đích:**  Dragonfly là hệ thống lưu trữ dữ liệu trong bộ nhớ, tập trung vào hiệu suất cao, khả năng mở rộng tốt và sử dụng tài nguyên hiệu quả hơn Redis và Memcached. Mục tiêu là tăng tốc độ truy cập, giảm độ trễ và xử lý lượng lớn dữ liệu, đồng thời tương thích với giao thức hiện có.

**Khả năng chính:** Sử dụng kiến trúc đa luồng, xử lý song song dữ liệu, hoạt động bất đồng bộ cho tác vụ nền, cấu trúc dữ liệu tối ưu và tương thích giao thức Redis/Memcached.

**Điểm mạnh:** Hiệu suất vượt trội (thường nhanh hơn 10-25 lần), sử dụng tài nguyên hiệu quả, khả năng mở rộng tốt theo CPU, dễ dàng thay thế Redis/Memcached.

**Điểm yếu:** Dự án còn mới, cộng đồng hỗ trợ có thể nhỏ hơn, có thể chưa hỗ trợ đầy đủ mọi tính năng nâng cao của Redis, giấy phép BSL có một số hạn chế thương mại.

**So sánh:** (Bảng so sánh ngắn gọn về kiến trúc, hiệu suất, tính năng chính, độ ổn định và cộng đồng so với Redis và Memcached).

**Ứng dụng tiềm năng:** Caching hiệu suất cao, thay thế Redis/Memcached, hàng đợi tin nhắn tốc độ cao, phân tích thời gian thực, bảng xếp hạng.

## 2. Đề xuất đề tài dự án và giải thích vấn đề
### Đề xuất đề tài:

### Hệ thống đếm lượt truy cập phân tán cơ bản với khả năng chịu lỗi bằng sao chép

**Vấn đề cần giải quyết:**
Theo dõi số lượt truy cập vào một trang web đơn lẻ. Nếu chỉ sử dụng một server duy nhất để đếm, server đó có thể trở thành điểm lỗi duy nhất và không chịu được tải lớn.

**Tầm quan trọng:**
Thống kê lượt truy cập là một yêu cầu cơ bản cho nhiều trang web.

**Hạn chế của giải pháp tập trung:**
Dễ bị lỗi khi server đếm gặp sự cố.
Khó mở rộng khi lượng truy cập tăng cao.
Lợi ích của hệ thống phân tán chịu lỗi:

**Tăng tính sẵn sàng:** Nếu một nút đếm lỗi, các nút khác vẫn hoạt động.
Cải thiện khả năng chịu tải: Các nút có thể cùng nhau xử lý yêu cầu.

**Các bước thực hiện chính:**

**1.Thiết lập giao tiếp cơ bản:**

Sử dụng Flask trong Python để tạo một API endpoint nhận yêu cầu tăng lượt truy cập.
Chạy nhiều instance của ứng dụng Flask này trên các cổng khác nhau (hoặc máy khác nhau nếu có).

 **2.Triển khai cơ chế sao chép (Replication) đơn giản:**

Chọn một nút làm “primary” (hoặc có thể là một cơ chế đồng thuận đơn giản nếu thư viện cho phép).
Khi một yêu cầu tăng lượt truy cập đến một nút, nút đó sẽ tăng bộ đếm cục bộ và gửi yêu cầu sao chép giá trị bộ đếm hiện tại đến các nút “secondary” khác.
Các nút secondary sẽ cập nhật bộ đếm cục bộ của chúng theo giá trị nhận được.

 **3.Thực hiện thao tác đọc lượt truy cập:**

Tạo một API endpoint khác để trả về số lượt truy cập hiện tại. Bạn có thể chọn trả về giá trị từ một nút cụ thể (ví dụ: primary) hoặc tính trung bình/lấy giá trị từ nút bất kỳ.

 **4.Triển khai Fault Tolerance cơ bản:**

Kiểm tra “liveness” đơn giản: Các nút có thể định kỳ “ping” nhau hoặc client có thể thử kết nối đến các nút khác nhau nếu một nút không phản hồi.

 **5.Logging đơn giản:**

Sử dụng logging của Flask hoặc in ra console khi có yêu cầu đến và khi bộ đếm được cập nhật.
 **6.Basic Stress Test:**

Sử dụng một công cụ như ab (Apache Benchmark) để gửi nhiều yêu cầu tăng lượt truy cập đồng thời đến một hoặc nhiều nút và quan sát xem số lượng có được cập nhật đúng không và hệ thống có lỗi không.

 **Công nghệ sử dụng (ngoài thư viện được giao):**

Ngôn ngữ lập trình: Python

Framework web: Flask (rất đơn giản để tạo API)

Thư viện logging: logging tích hợp của Python

Công cụ test tải: ab (Apache Benchmark)

**Ước tính tiến độ (cho giai đoạn giữa kỳ):**

Tuần 1: Thiết lập Flask, tạo API endpoint tăng lượt truy cập và chạy trên nhiều cổng.

Tuần 2: Triển khai cơ chế sao chép đơn giản (từ một nút đến các nút khác).

Tuần 3: Triển khai endpoint đọc lượt truy cập, thêm logging cơ bản.

Tuần 4: Viết script đơn giản để mô phỏng lỗi nút và thực hiện stress test cơ bản.

## 3. Kế hoạch dự kiến cho bài giữa kỳ
Mục tiêu bài giữa kỳ:

Cài đặt cơ bản của hệ thống với giao tiếp giữa các nút và cơ chế sao chép.
Triển khai tính năng logging đơn giản và thực hiện kiểm tra cơ bản với stress test.
Đảm bảo các nút hoạt động ổn định và có khả năng chịu lỗi khi một nút gặp sự cố.
## 4. Nộp bài
Soạn thảo và nộp kế hoạch đề tài và mô tả vấn đề dưới dạng văn bản qua website đã sử dụng trong bài tập 1.
