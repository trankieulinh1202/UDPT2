---
title: "Tiến trình và luồng"
date: "2023-10-26"
updated: "2023-10-26"
categories:
  - "sveltekit"
  - "markdown"
coverImage: "/images/tien-trinh-va-luong-1.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Check out how heading links work with this starter in this post.
---
![title](/images/CPU.jpg)
## Bài tập 1: Dựa vào bài học, kiểm tra CPU, GPU, RAM và giải thích hiệu năng máy tính em đang dùng
### 1. Thông tin cấu hình máy tính hiện tại
Thành phần	Thông số	Nhận xét
CPU	Đang sử dụng 24% tại 1.23 GHz	Mức sử dụng tương đối nhẹ, CPU tiết kiệm điện (xung nhịp thấp khi không cần thiết)
RAM	9.7 GB / 15.7 GB (~62%)	Máy có 16GB RAM vật lý, đang sử dụng gần 10GB, phù hợp cho đa nhiệm
Ổ đĩa (Disk)	SSD, đang dùng 8%	Truy cập nhanh, phù hợp lập trình, học tập
GPU	Intel(R) Iris(R) Xe Graphics (0% sử dụng)	GPU tích hợp, đủ dùng cho tác vụ nhẹ như lướt web, xem video, học tập
DirectX version	12 (Feature Level 12.1)	Hỗ trợ các API hiện đại, có thể xử lý đồ họa cơ bản

### 2. Hiệu năng tổng thể
 CPU hiệu năng tốt cho học tập, lập trình, chạy IDE như VS Code, Chrome,... Máy có thể hỗ trợ đa tiến trình nhờ có nhiều lõi và hỗ trợ Hyper-Threading.

 RAM 16GB đủ tốt để chạy cùng lúc nhiều ứng dụng (IDE, Chrome, PowerPoint, Edge…).

 GPU tích hợp (Intel Iris Xe) không phù hợp cho xử lý đồ họa nặng (game 3D, deep learning). Tuy nhiên, nó rất tốt cho công việc văn phòng, học online, dựng slide,...

 SSD giúp máy phản hồi nhanh, tốc độ mở app, khởi động máy đều mượt.

### 3. Đánh giá chung
Máy tính của bạn có hiệu năng tốt trong các tác vụ học tập, làm báo cáo, lập trình cơ bản, xử lý nhiều luồng nhẹ nhàng.
Không phù hợp để chơi game nặng, dựng video 4K hay huấn luyện mô hình AI (do GPU tích hợp).

## Bài tập 2: Liệt kê 12 bài toán phổ biến trong ngành CNTT sử dụng đa luồng, đa tiến trình
| STT | Bài toán / ứng dụng thực tế                 | Đa luồng (Thread) | Đa tiến trình (Process) | Mô tả vị trí sử dụng                                                                                                          |
| --- | ------------------------------------------- | ----------------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 1   | Trình duyệt web (Chrome, Edge)              | ✅                 | ✅                       | Mỗi tab là 1 tiến trình, bên trong mỗi tab có nhiều luồng: vẽ UI, tải nội dung, xử lý JS                                      |
| 2   | Game 3D                                     | ✅                 | ❌                       | Các luồng xử lý hình ảnh, âm thanh, điều khiển nhân vật, AI…                                                                  |
| 3   | Biên dịch mã nguồn (Compiler)               | ✅                 | ✅                       | Một số trình biên dịch chia thành các tiến trình phụ (compile từng module) và sử dụng đa luồng để phân tích ngữ pháp, sinh mã |
| 4   | IDE (Visual Studio, VS Code)                | ✅                 | ✅                       | Luồng cho UI, kiểm tra cú pháp, biên dịch nền                                                                                 |
| 5   | Web server (Apache, Nginx)                  | ✅                 | ✅                       | Thread-per-request, Thread-per-connection…                                                                                    |
| 6   | Ứng dụng xử lý ảnh (Photoshop, GIMP)        | ✅                 | ✅                       | Thread dùng để xử lý hiệu ứng, render ảnh theo từng lớp (layer)                                                               |
| 7   | Phần mềm diệt virus                         | ✅                 | ✅                       | Luồng quét file, kiểm tra hệ thống, theo dõi theo thời gian thực                                                              |
| 8   | Cơ sở dữ liệu (MySQL, PostgreSQL)           | ✅                 | ✅                       | Mỗi truy vấn là một luồng/tiến trình, hỗ trợ song song hóa để tăng hiệu suất                                                  |
| 9   | Công cụ nén/giải nén file (WinRAR, 7-Zip)   | ✅                 | ❌                       | Tách nén thành nhiều phần song song để tăng tốc                                                                               |
| 10  | AI model training (như ChatGPT)             | ✅                 | ✅                       | Dữ liệu chia theo batch, mỗi tiến trình hoặc GPU xử lý 1 phần, trong đó có nhiều luồng phục vụ tính toán                      |
| 11  | Quét dữ liệu web (web crawler)              | ✅                 | ✅                       | Mỗi tiến trình/luồng xử lý 1 trang web hoặc domain riêng biệt                                                                 |
| 12  | Video streaming platform (Netflix, YouTube) | ✅                 | ✅                       | Các luồng xử lý encode, decode, truyền tín hiệu mạng, hiển thị, cache v.v                                                     |

## Bài tập 3 – Tổng hợp: Khi nào nên dùng Thread, Process, hoặc cả hai
![title](/images/tientrinh3.jpg)

## Bài tập 4:  ChatGPT và hệ thống phân tán khi huấn luyện
### Mô hình ChatGPT sử dụng hệ thống phân tán:
- **Data Parallelism:** chia batch dữ liệu cho nhiều GPU cùng xử lý
- **Model Parallelism:** chia tầng của mạng neural ra nhiều GPU
- **Pipeline Parallelism:** phân tầng thành các giai đoạn để xử lý nối tiếp

### Công nghệ sử dụng:
- GPU cluster tốc độ cao (NVIDIA A100, TPU)
- Kết nối InfiniBand
- Framework: PyTorch Distributed, DeepSpeed, Megatron-LM

### Tài liệu tham khảo:
- [EleutherAI GPT-3 Training Blog](https://blog.eleuther.ai/gpt3-model-training/)
- [NVIDIA - Scaling Deep Learning](https://developer.nvidia.com/blog/large-language-model-training-gpu-clusters/)
- [Microsoft DeepSpeed](https://www.microsoft.com/en-us/research/project/deepspeed/)