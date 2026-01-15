# Ansible Fundamentals

## Requirements

### 1. What is Ansible?

Research and answer:

- What is Ansible?
**Ansible** là một công cụ CNTT mã nguồn mở tự động hóa việc triển khai ứng dụng, cung cấp dịch vụ đám mây, điều phối nội bộ dịch vụ và các công cụ CNTT khác. 

- What problems does Ansible solve?
  - Triển khai phần mềm trên nhiều máy chủ cùng một lúc mà không cần sự can thiệp của con người
  - Sử dụng cấu hình máy chủ và tạo tài khoản người dùng
  - Không cần agent, không cần các phần mềm trên các node, sử dụng SSH để kết nối với các node và thực hiện các thao tác cần thiết trên máy chủ
  
- How Ansible works at a high level ?
  - Control Node: máy đã cài Ansible và chạy các lệnh tự động hóa
  - Inventory: danh sách các máy sẽ được quản lý
  - Playbooks: File `yaml` định nghĩa các quy trình tự động
  - Task & Modules: các bước tự động hóa (tasks) được thực thi bằng các module nhỏ
  - Execution: Ansible kết nối qua SSH tới máy đích và chạy các module để thực hiện nhiệm vụ

### 2. Why Ansible?

Research and explain:

- Why Ansible is used in system administration.
  - Trong hệ thống quản trị, Ansible được sử dụng nhằm tự động hóa các tác vụ quản trị lặp đi lặp lại như cài đặt phần mềm, cấu hình server, quản lý người dùng, triển khai ứng dụng và bảo trì hệ thống.
    - **Agent-less architecture**: Ansible không yêu cầu cài đặt agent trên các máy được quản lý, sử dụng SSH (linux/ Unix) hoặc WinRM (Windows) để kết nối, giúp giảm độ phức tạp trong vận hành
    - **Centralized management**: có thể quản lý nhiều máy chủ từ một control node duy nhất
    - **Consistency**: các cấu hình hệ thống được mô tả dưới dạng code (plaubooks) giúp đảm bảo tất cả các server có cùng trạng thái cấu hình.
    - **Scalability**: Ansible cho phép quản lý từ vài server đến hàng trăm hoặc hàng nhìn server cùng lúc.

- Advantages of Ansible compared to manual configuration.
   | Ansible | Manual configuration |
   |---------------------|------------------------------|
   | Tự động hóa hoàn toàn các tác vụ hệ thống | Thực hiện thủ công từng lệnh trên từng máy|
   | Sử dụng playbooks được định nghĩa sẵn, giúp thực hiện các bước một cách chính xác và lặp lại được | Dễ đẫn đến sai sót do nhập lệnh nhầm hoặc bỏ sót bước |
   | Cùng 1 playbook được áp dụng cho nhiều host, đảm bảo trạng thái đồng nhất | các server có thể bị cấu hình khác nhau, không đồng nhất|
   | Quản lý số lượng host lớn cùng lúc | Khó mở rộng khi server tăng |
   | Hỗ trợ idempotency (chạy nhiều lần không gây lỗi) | Chạy lại lệnh có thể gây xung đột|
   | Dễ tích hợp với Git (version control) | Khó theo dõi lịch sử thay đổi cấu hình|
   |Tiết kiệm thời gian và công sức vận hành | Tốn nhiều thời gian cho các tác vụ lặp lại|
   | Có thể tái sử dụng cấu hình cho nhiều môi trường |Mỗi môi trường cần cấu hình lại từ đầu |

=> Tóm lại: So với cấu hình thủ công, Ansible giúp giảm lỗi, tiết kiệm thời gian và dễ bảo trì.
   
- Use cases where Ansible is appropriate.
  - **Configuration Management**: Quản lý cấu hình HDH, user, package, service trên server.
  - **Application Deployment**: Triển khai ứng dụng web, api, microservices trên nhiều môi trường (dev, test, production)
  - **Infrastructure as Code (IaC)**: Quản lý hạ tầng dưới dạng mã, giúp dễ bảo trì và mở rộng.
  - **Continuous Integration / Continuous Deployment (CI/CD)**: Tự động hóa các bước triển khai sau khi build ứng dụng
  - **Security & Compliance**: Áp dụng chính sách bảo mật, cập nhật bản vá , kiểm tra tuân thủ chuẩn cấu hình.
  - **Cloud & Network Automation**: Cấu hình tài nguyên cloud hoặc thiết bị mạng như router, switch.

### 3. Core Ansible Terms

Terms to research:

- Inventory: tập tin hoặc nguồn dữ liệu chứa danh sách các host mà Ansible sẽ quản lý: Inventory có thể là:
  - Static inventory: (file `INI` hoặc `Yaml`)
  - Dynamic inventory: (lấy dữ liệu từ cloud, API)
=> Inventory cho phép nhóm các host theo vai trò hoặc môi trường.


- Host: một máy đích được Ansible quản lý, có thể là:
  - Server vật lý
  - Máy ảo 
  - Container
  - Thiết bị mạng
=>Mỗi host được định nghĩa trong inventory.

- Playbook: là file `yaml` mô tả toàn bộ quy trình tự động hóa. Một playbook có thể bao gồm nhiều play được dùng để triển khai, cấu hình hoặc quản lý hệ thống. Playbook đóng vai trò trung tâm trong Ansible automation

- Play: đơn vị logic trong playbook, xác định:
  - Nhóm host cần áp dụng
  - Danh sách các task sẽ được thực thi trên nhóm host đó
=> Một playbook có thể chứa nhiều play

- Task: một hành động cụ thể trong play, ví dụ:
  - Cài đặt package
  - Sao chép file
  - Khởi đôngj service
=> Mỗi task thường gọi một module để thục hiện công việc

- Module: các chương trình nhỏ được Ansible sử dụng để thực thi task. 
Ví dụ:
  - `ap`, `yum`: cài package
  - `copy`: sao chép file
  - `service`: quản lý service
=> Ansible gửi module đến host, thực thi và nhận kết quả trả về.

- Library: tập hợp các module mà Ansible sử dụng
  - Bao gồm các module core có sẵn
  - Có thể mở rộng bằng custom modules do người dùng tự viết
=> Library cung cấp các công cụ cần thiết để thực hiện các task trong playbook.