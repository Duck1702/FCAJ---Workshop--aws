---

title: "2. Bản đề xuất"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

## 1. Tên dự án

**Tên tiếng Việt:** Hệ thống quản lý tài chính cá nhân, phân tích AI và cảnh báo vượt ngân sách trên AWS

**Tên tiếng Anh:** AI-Powered Personal Finance Budget Alert System on AWS

---

## 2. Tổng quan dự án

Dự án xây dựng một hệ thống quản lý tài chính cá nhân trên nền tảng Amazon Web Services theo kiến trúc serverless. Hệ thống cho phép người dùng đăng ký, đăng nhập, quản lý giao dịch thu chi, thiết lập ngân sách cá nhân theo tháng, nhận cảnh báo khi chi tiêu vượt ngân sách và sử dụng AI để hỗ trợ gợi ý danh mục giao dịch cũng như tạo nhận xét chi tiêu theo tháng.

Hệ thống bao gồm frontend web đơn giản để người dùng tương tác với các chức năng chính, backend REST API xử lý nghiệp vụ, cơ sở dữ liệu NoSQL để lưu trữ dữ liệu, dịch vụ xác thực người dùng, dịch vụ gửi thông báo email, hệ thống ghi log và giám sát, cùng với dịch vụ AI hỗ trợ phân tích dữ liệu chi tiêu.

Project được triển khai bằng các dịch vụ AWS như Amazon S3, Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon SNS, Amazon CloudWatch, Amazon Bedrock, AWS IAM và AWS Budgets. Mục tiêu của dự án là xây dựng một hệ thống cloud-native có tính thực tế, chi phí thấp, dễ mở rộng, có khả năng giám sát và có yếu tố AI hỗ trợ người dùng ra quyết định tài chính tốt hơn.

---

## 3. Mục đích dự án

Mục đích chính của dự án là hỗ trợ người dùng theo dõi và kiểm soát tài chính cá nhân một cách chủ động. Thay vì chỉ ghi nhận giao dịch thu chi, hệ thống còn giúp người dùng nhận biết khi chi tiêu vượt quá ngân sách đã thiết lập, từ đó có thể điều chỉnh hành vi chi tiêu kịp thời.

Bên cạnh đó, hệ thống sử dụng AI để hỗ trợ người dùng trong quá trình quản lý tài chính. AI có thể gợi ý danh mục phù hợp dựa trên mô tả giao dịch và tạo nhận xét chi tiêu ngắn gọn theo từng tháng. Điều này giúp người dùng dễ hiểu hơn về thói quen chi tiêu của mình.

Về mặt kỹ thuật, dự án giúp thực hành triển khai một hệ thống serverless hoàn chỉnh trên AWS, bao gồm frontend web, xác thực người dùng, REST API, xử lý nghiệp vụ bằng Lambda, lưu trữ dữ liệu bằng DynamoDB, gửi thông báo qua email bằng SNS, giám sát hệ thống bằng CloudWatch, tích hợp AI thông qua Amazon Bedrock và kiểm soát chi phí bằng AWS Budgets.

---

## 4. Đối tượng sử dụng

Hệ thống hướng đến các nhóm người dùng sau:

* Sinh viên muốn theo dõi chi tiêu hằng tháng.
* Người đi làm muốn kiểm soát thu nhập và chi phí cá nhân.
* Người dùng cá nhân muốn nhận cảnh báo khi chi tiêu vượt ngân sách.
* Người muốn có phân tích AI đơn giản về tình hình chi tiêu.
* Người học AWS muốn thực hành xây dựng một ứng dụng cloud-native có frontend, authentication, API, database, notification, monitoring và AI.

---

## 5. Vấn đề cần giải quyết

Trong thực tế, nhiều người dùng thường không theo dõi chi tiêu thường xuyên, không phân loại giao dịch rõ ràng và không biết thời điểm chi tiêu đã vượt quá ngân sách. Điều này có thể dẫn đến mất kiểm soát tài chính cá nhân, đặc biệt với sinh viên hoặc người mới bắt đầu quản lý thu chi.

Ngoài ra, các hệ thống truyền thống thường cần máy chủ, cơ sở dữ liệu quan hệ và chi phí vận hành cố định. Với phạm vi project học tập, việc sử dụng kiến trúc serverless giúp giảm chi phí, giảm công việc vận hành hạ tầng và vẫn đảm bảo khả năng mở rộng khi số lượng người dùng tăng lên.

Dự án giải quyết các vấn đề chính:

* Quản lý giao dịch thu chi cá nhân.
* Thiết lập và theo dõi ngân sách theo tháng.
* Cảnh báo khi tổng chi tiêu vượt ngân sách.
* Gợi ý danh mục giao dịch bằng AI.
* Tạo nhận xét chi tiêu bằng AI.
* Cung cấp giao diện web đơn giản để người dùng thao tác.
* Ghi log, giám sát lỗi và hỗ trợ debug hệ thống.
* Kiểm soát chi phí triển khai trên AWS.
* Cleanup tài nguyên AWS sau khi hoàn thành project.

---

## 6. Mục tiêu dự án

Dự án hướng đến các mục tiêu sau:

* Xây dựng frontend web đơn giản để người dùng sử dụng các chức năng chính.
* Triển khai frontend dưới dạng static website trên Amazon S3.
* Xây dựng REST API cho hệ thống quản lý tài chính cá nhân.
* Cho phép người dùng đăng ký và đăng nhập bằng Amazon Cognito.
* Cho phép người dùng tạo, xem, cập nhật và xóa giao dịch.
* Cho phép người dùng thiết lập ngân sách theo tháng.
* Tự động tính tổng chi tiêu và so sánh với ngân sách.
* Gửi email cảnh báo khi tổng chi tiêu vượt ngân sách.
* Sử dụng AI để gợi ý danh mục giao dịch từ mô tả giao dịch.
* Sử dụng AI để tạo nhận xét chi tiêu theo tháng.
* Lưu trữ dữ liệu bằng Amazon DynamoDB.
* Xử lý logic nghiệp vụ bằng AWS Lambda.
* Công khai API thông qua Amazon API Gateway.
* Ghi log và theo dõi hệ thống bằng Amazon CloudWatch.
* Phân quyền bằng AWS IAM theo nguyên tắc least privilege.
* Kiểm soát chi phí trong phạm vi tối đa 200 USD cho toàn bộ thời gian triển khai.
* Viết tài liệu workshop song ngữ hướng dẫn triển khai từng bước.
* Đánh giá hệ thống theo 6 trụ cột AWS Well-Architected Framework.

---

## 7. Phạm vi dự án

Trong phạm vi project, hệ thống tập trung vào các chức năng chính sau:

* Đăng ký và đăng nhập người dùng.
* Hiển thị giao diện web đơn giản cho người dùng.
* Tạo giao dịch thu hoặc chi.
* Xem danh sách giao dịch.
* Cập nhật thông tin giao dịch.
* Xóa giao dịch.
* Thiết lập ngân sách tháng.
* Kiểm tra tổng chi tiêu so với ngân sách.
* Gửi email cảnh báo khi vượt ngân sách.
* AI gợi ý danh mục giao dịch.
* AI tạo nhận xét chi tiêu.
* Ghi log và giám sát hệ thống.
* Kiểm thử API bằng Postman.
* Kiểm thử luồng người dùng cơ bản trên frontend web.
* Ghi lại minh chứng triển khai và kết quả kiểm thử.

Frontend của project không tập trung vào giao diện phức tạp, nhưng cần đảm bảo các luồng sử dụng cơ bản như đăng ký, đăng nhập, thêm giao dịch, xem danh sách giao dịch, thiết lập ngân sách, kiểm tra cảnh báo vượt ngân sách và xem nhận xét chi tiêu từ AI.

---

## 8. Các dịch vụ AWS sử dụng

Các dịch vụ AWS dự kiến sử dụng gồm:

| Dịch vụ AWS        | Vai trò trong hệ thống                                                    |
| ------------------ | ------------------------------------------------------------------------- |
| Amazon S3          | Lưu trữ và triển khai frontend web tĩnh cho người dùng truy cập hệ thống. |
| Amazon Cognito     | Quản lý đăng ký, đăng nhập và xác thực người dùng.                        |
| Amazon API Gateway | Tạo REST API endpoint để frontend hoặc Postman gọi đến hệ thống.          |
| AWS Lambda         | Xử lý logic nghiệp vụ cho giao dịch, ngân sách, cảnh báo và AI.           |
| Amazon DynamoDB    | Lưu trữ giao dịch, ngân sách và lịch sử cảnh báo.                         |
| Amazon SNS         | Gửi email cảnh báo khi chi tiêu vượt ngân sách.                           |
| Amazon CloudWatch  | Ghi log, theo dõi metric và hỗ trợ giám sát lỗi.                          |
| Amazon Bedrock     | Hỗ trợ AI gợi ý danh mục giao dịch và tạo nhận xét chi tiêu.              |
| AWS IAM            | Quản lý quyền truy cập giữa Lambda và các dịch vụ AWS.                    |
| AWS Budgets        | Theo dõi chi phí và cảnh báo khi chi phí vượt ngưỡng.                     |

Các dịch vụ như Amazon RDS, NAT Gateway, ECS, Load Balancer và EC2 không được sử dụng trong phạm vi chính của project để hạn chế chi phí và giảm độ phức tạp khi triển khai.

---

## 9. Kiến trúc giải pháp

Hình dưới đây mô tả kiến trúc tổng quát của hệ thống:

![Kiến trúc tổng quát của hệ thống](/FCAJ---Workshop--aws/images/2-Proposal/architecture-overview_vie.png)

Luồng hoạt động chính của hệ thống:

1. Người dùng truy cập frontend web được triển khai trên Amazon S3.
2. Người dùng đăng ký hoặc đăng nhập thông qua Amazon Cognito.
3. Sau khi đăng nhập thành công, người dùng nhận được JWT token.
4. Frontend gửi request đến Amazon API Gateway kèm token xác thực.
5. API Gateway xác thực request và chuyển request hợp lệ đến AWS Lambda.
6. Lambda xử lý các logic giao dịch, ngân sách, cảnh báo và AI.
7. Lambda đọc và ghi dữ liệu vào Amazon DynamoDB.
8. Khi người dùng nhập mô tả giao dịch, Lambda có thể gọi Amazon Bedrock để gợi ý danh mục.
9. Khi người dùng yêu cầu phân tích chi tiêu, Lambda tổng hợp dữ liệu và gọi Amazon Bedrock để tạo insight.
10. Nếu tổng chi tiêu vượt ngân sách, Lambda gửi thông báo đến Amazon SNS.
11. Amazon SNS gửi email cảnh báo đến người dùng.
12. Amazon CloudWatch ghi log, metric và hỗ trợ giám sát lỗi.
13. AWS IAM Role cấp quyền cần thiết cho Lambda để truy cập DynamoDB, SNS, CloudWatch và Bedrock.
14. AWS Budgets theo dõi chi phí trong quá trình triển khai project.

---

## 10. Mô hình dữ liệu dự kiến

Hệ thống dự kiến sử dụng ba bảng DynamoDB chính: Transactions, Budgets và AlertEvents.

### 10.1. Bảng Transactions

Bảng `Transactions` lưu thông tin giao dịch thu chi của người dùng.

**Key schema:**

| Thành phần    | Thuộc tính    |
| ------------- | ------------- |
| Partition key | userId        |
| Sort key      | transactionId |

**Các thuộc tính chính:**

| Thuộc tính    | Ý nghĩa                                  |
| ------------- | ---------------------------------------- |
| userId        | Định danh người dùng.                    |
| transactionId | Định danh giao dịch.                     |
| type          | Loại giao dịch, gồm income hoặc expense. |
| category      | Danh mục giao dịch.                      |
| amount        | Số tiền giao dịch.                       |
| description   | Mô tả giao dịch.                         |
| createdAt     | Thời gian tạo giao dịch.                 |
| updatedAt     | Thời gian cập nhật giao dịch.            |

Thiết kế này giúp hệ thống truy vấn danh sách giao dịch theo từng người dùng mà không cần scan toàn bảng.

### 10.2. Bảng Budgets

Bảng `Budgets` lưu ngân sách theo tháng của từng người dùng.

**Key schema:**

| Thành phần    | Thuộc tính |
| ------------- | ---------- |
| Partition key | userId     |
| Sort key      | month      |

**Các thuộc tính chính:**

| Thuộc tính     | Ý nghĩa                                 |
| -------------- | --------------------------------------- |
| userId         | Định danh người dùng.                   |
| month          | Tháng áp dụng ngân sách, ví dụ 2026-06. |
| budgetLimit    | Giới hạn ngân sách trong tháng.         |
| currentExpense | Tổng chi tiêu hiện tại trong tháng.     |
| updatedAt      | Thời gian cập nhật gần nhất.            |

Thiết kế này giúp mỗi người dùng có thể thiết lập ngân sách riêng theo từng tháng.

### 10.3. Bảng AlertEvents

Bảng `AlertEvents` lưu lịch sử cảnh báo vượt ngân sách.

**Key schema:**

| Thành phần    | Thuộc tính |
| ------------- | ---------- |
| Partition key | userId     |
| Sort key      | alertId    |

**Các thuộc tính chính:**

| Thuộc tính     | Ý nghĩa                                          |
| -------------- | ------------------------------------------------ |
| alertId        | Định danh cảnh báo.                              |
| userId         | Định danh người dùng.                            |
| month          | Tháng phát sinh cảnh báo.                        |
| budgetLimit    | Ngân sách đã thiết lập.                          |
| currentExpense | Tổng chi tiêu tại thời điểm cảnh báo.            |
| status         | Trạng thái gửi cảnh báo, ví dụ SENT hoặc FAILED. |
| createdAt      | Thời gian tạo cảnh báo.                          |

Bảng này giúp hệ thống ghi nhận lại các lần gửi cảnh báo để phục vụ kiểm tra và debug.

---

## 11. API dự kiến

Các API chính của hệ thống gồm:

| Method | Endpoint           | Chức năng                   |
| ------ | ------------------ | --------------------------- |
| POST   | /transactions      | Tạo giao dịch mới           |
| GET    | /transactions      | Lấy danh sách giao dịch     |
| GET    | /transactions/{id} | Xem chi tiết giao dịch      |
| PUT    | /transactions/{id} | Cập nhật giao dịch          |
| DELETE | /transactions/{id} | Xóa giao dịch               |
| POST   | /budgets           | Thiết lập ngân sách         |
| GET    | /budgets           | Xem ngân sách hiện tại      |
| POST   | /budgets/check     | Kiểm tra vượt ngân sách     |
| POST   | /ai/categorize     | AI gợi ý danh mục giao dịch |
| GET    | /ai/insights       | AI tạo nhận xét chi tiêu    |

Tất cả API chính cần được bảo vệ bằng Amazon Cognito Authorizer. Người dùng phải đăng nhập và gửi JWT token hợp lệ khi gọi API.

---

## 12. Giao diện người dùng

Frontend web được xây dựng ở mức đơn giản để phục vụ demo và kiểm thử luồng nghiệp vụ end-to-end. Frontend có thể được phát triển bằng React hoặc tái sử dụng các component giao diện có sẵn, sau đó điều chỉnh để tích hợp với backend serverless mới trên AWS.

Các màn hình dự kiến gồm:

* Màn hình đăng ký người dùng.
* Màn hình đăng nhập.
* Màn hình danh sách giao dịch.
* Form thêm giao dịch.
* Form cập nhật giao dịch.
* Màn hình thiết lập ngân sách tháng.
* Màn hình kiểm tra cảnh báo vượt ngân sách.
* Màn hình hiển thị nhận xét chi tiêu từ AI.

Frontend sẽ gọi API Gateway endpoint và gửi JWT token nhận được từ Amazon Cognito. Frontend không lưu AWS Access Key, Secret Key hoặc thông tin nhạy cảm trong source code.

---

## 13. Tính năng AI

Dự án sử dụng AI cho hai chức năng chính.

### 13.1. AI gợi ý danh mục giao dịch

Khi người dùng nhập mô tả giao dịch, hệ thống có thể sử dụng AI để gợi ý danh mục phù hợp.

Ví dụ:

| Mô tả giao dịch     | Danh mục gợi ý |
| ------------------- | -------------- |
| ăn trưa cơm gà      | Food           |
| đổ xăng xe máy      | Transport      |
| lương tháng 6       | Salary         |
| mua sách học AWS    | Education      |
| trả tiền điện thoại | Utilities      |

Tính năng này giúp người dùng tiết kiệm thời gian khi nhập giao dịch và hỗ trợ phân loại dữ liệu nhất quán hơn.

### 13.2. AI tạo nhận xét chi tiêu

Dựa trên dữ liệu giao dịch và ngân sách, hệ thống tạo nhận xét ngắn gọn cho người dùng.

Ví dụ:

* Tháng này bạn chi tiêu nhiều nhất cho Food và Shopping.
* Chi tiêu hiện tại đã vượt 12% so với ngân sách.
* Bạn nên giảm các khoản chi tiêu không cần thiết trong tuần tiếp theo.
* Tỷ lệ chi tiêu cho ăn uống đang cao hơn các danh mục còn lại.

Trong phạm vi project, AI chỉ đóng vai trò hỗ trợ phân tích đơn giản. Hệ thống không thực hiện huấn luyện mô hình riêng, không fine-tuning và không sử dụng batch inference để tránh phát sinh chi phí không cần thiết.

---

## 14. Định hướng theo AWS Well-Architected Framework

Dự án được thiết kế dựa trên các nguyên tắc của AWS Well-Architected Framework nhằm đảm bảo hệ thống không chỉ hoạt động đúng chức năng mà còn có khả năng vận hành, bảo mật, mở rộng, kiểm soát chi phí và duy trì hiệu quả.

### 14.1. Operational Excellence

Hệ thống sử dụng Amazon CloudWatch để ghi log Lambda, theo dõi lỗi API và hỗ trợ quá trình debug. Các bước triển khai, kiểm thử và cleanup được ghi lại trong tài liệu workshop để dễ theo dõi và tái thực hiện.

### 14.2. Security

Hệ thống sử dụng Amazon Cognito để xác thực người dùng. API Gateway kiểm tra JWT token trước khi chuyển request đến Lambda. AWS IAM Role được cấu hình theo nguyên tắc least privilege. Source code không chứa AWS Access Key, Secret Key hoặc token cố định.

### 14.3. Reliability

Các Lambda function cần có xử lý lỗi rõ ràng khi DynamoDB, SNS hoặc Bedrock phát sinh lỗi. API cần trả về mã lỗi phù hợp như 400, 401, 404 hoặc 500. Nếu AI hoặc SNS gặp lỗi, hệ thống cần ghi log để debug và không làm ảnh hưởng đến các chức năng chính khác.

### 14.4. Performance Efficiency

Hệ thống sử dụng kiến trúc serverless để tự động mở rộng theo số lượng request. DynamoDB được thiết kế với partition key và sort key phù hợp để truy vấn theo người dùng, hạn chế scan toàn bảng. Lambda timeout và memory cần được cấu hình ở mức phù hợp với quy mô demo.

### 14.5. Cost Optimization

Project sử dụng AWS Budgets để theo dõi chi phí và gửi cảnh báo khi vượt ngưỡng. Hệ thống ưu tiên các dịch vụ serverless như Lambda, API Gateway, DynamoDB và S3 static hosting. Project không sử dụng NAT Gateway, RDS, ECS, Load Balancer hoặc Bedrock Provisioned Throughput nếu chưa cần thiết.

### 14.6. Sustainability

Hệ thống hạn chế tài nguyên chạy liên tục bằng cách sử dụng Lambda và frontend tĩnh trên S3. Các tài nguyên demo sẽ được cleanup sau khi hoàn thành project. CloudWatch log retention cần được cấu hình hợp lý để tránh lưu trữ log quá lâu.

---

## 15. Kiểm thử và đánh giá

Hệ thống sẽ được kiểm thử bằng frontend web, Postman và AWS Console.

### 15.1. Kiểm thử xác thực

* Đăng ký người dùng bằng Amazon Cognito.
* Đăng nhập và lấy JWT token.
* Gọi API khi có token hợp lệ.
* Gọi API khi không có token.
* Gọi API với token không hợp lệ.

### 15.2. Kiểm thử giao dịch

* Tạo giao dịch thu.
* Tạo giao dịch chi.
* Lấy danh sách giao dịch.
* Xem chi tiết giao dịch.
* Cập nhật giao dịch.
* Xóa giao dịch.
* Kiểm tra dữ liệu lưu trong DynamoDB.

### 15.3. Kiểm thử ngân sách và cảnh báo

* Thiết lập ngân sách tháng.
* Tạo nhiều giao dịch chi tiêu để vượt ngân sách.
* Gọi API kiểm tra vượt ngân sách.
* Kiểm tra email cảnh báo từ Amazon SNS.
* Kiểm tra dữ liệu cảnh báo trong bảng AlertEvents.

### 15.4. Kiểm thử AI

* Gọi API AI gợi ý danh mục giao dịch.
* Gọi API AI tạo nhận xét chi tiêu.
* Kiểm tra trường hợp Bedrock lỗi hoặc không phản hồi.
* Giới hạn số lần gọi AI trong quá trình demo để kiểm soát chi phí.

### 15.5. Kiểm thử frontend

* Kiểm tra luồng đăng ký và đăng nhập.
* Kiểm tra thêm, sửa, xóa giao dịch từ giao diện web.
* Kiểm tra thiết lập ngân sách từ giao diện web.
* Kiểm tra hiển thị thông báo lỗi khi API lỗi.
* Kiểm tra hiển thị AI insights trên giao diện.

### 15.6. Kiểm thử vận hành

* Kiểm tra log trong CloudWatch.
* Kiểm tra Lambda error log.
* Kiểm tra API response khi dữ liệu đầu vào không hợp lệ.
* Kiểm tra AWS Budget và cảnh báo chi phí.
* Kiểm tra cleanup sau khi hoàn thành demo.

---

## 16. Kết quả mong đợi

Sau khi hoàn thành, project sẽ đạt được các kết quả sau:

* Một frontend web đơn giản để người dùng sử dụng hệ thống.
* Frontend được triển khai bằng Amazon S3 Static Website Hosting.
* Một hệ thống REST API serverless hoạt động trên AWS.
* Người dùng có thể đăng ký và đăng nhập bằng Amazon Cognito.
* Người dùng có thể quản lý giao dịch thu chi cá nhân.
* Người dùng có thể thiết lập ngân sách và nhận cảnh báo khi vượt ngân sách.
* Hệ thống có AI gợi ý danh mục giao dịch.
* Hệ thống có AI tạo nhận xét chi tiêu theo tháng.
* Dữ liệu được lưu trữ trong DynamoDB.
* API được bảo vệ bằng Amazon Cognito.
* Email cảnh báo được gửi thông qua Amazon SNS.
* CloudWatch ghi nhận log và hỗ trợ giám sát hệ thống.
* IAM Role được cấu hình theo nguyên tắc least privilege.
* Có tài liệu workshop song ngữ hướng dẫn triển khai từng bước.
* Có ảnh chụp minh chứng các dịch vụ AWS, kết quả test API, giao diện web, log và email cảnh báo.
* Có phần cleanup để xóa tài nguyên AWS sau khi hoàn thành.
* Có đánh giá hệ thống theo 6 trụ cột AWS Well-Architected.

---

## 17. Rủi ro và hướng xử lý

### 17.1. Rủi ro kỹ thuật

Một số rủi ro có thể gặp trong quá trình triển khai:

* Cấu hình IAM chưa đúng làm Lambda không thể truy cập DynamoDB, SNS hoặc Bedrock.
* API Gateway chưa tích hợp đúng với Lambda.
* Cognito token không hợp lệ khi test API.
* Frontend gọi sai API endpoint hoặc thiếu token.
* Logic tính tổng chi tiêu chưa chính xác.
* SNS email subscription chưa được xác nhận.
* Bedrock trả lỗi hoặc không phản hồi.
* CloudWatch Logs phát sinh chi phí nếu lưu log quá lâu.
* Phát sinh chi phí nếu không cleanup tài nguyên sau khi demo.

### 17.2. Hướng xử lý

* Kiểm tra CloudWatch Logs để debug lỗi Lambda.
* Cấu hình IAM theo nguyên tắc least privilege.
* Test từng API bằng Postman trước khi tích hợp với frontend.
* Xác nhận email SNS trước khi test cảnh báo.
* Kiểm tra JWT token trước khi gọi API Gateway.
* Giới hạn số lần gọi AI trong quá trình demo.
* Cấu hình CloudWatch log retention 7 hoặc 14 ngày.
* Tạo AWS Budget để theo dõi chi phí.
* Cleanup toàn bộ tài nguyên sau khi hoàn thành project.
* Không sử dụng Bedrock Provisioned Throughput.
* Không sử dụng NAT Gateway nếu không có yêu cầu bắt buộc.

---

## 18. Ước tính chi phí

Project sử dụng chủ yếu các dịch vụ serverless và pay-as-you-go nên chi phí dự kiến thấp. Mục tiêu chi phí tối đa cho toàn bộ quá trình triển khai trong khoảng 3 tháng là 200 USD.

Kế hoạch kiểm soát chi phí:

* Sử dụng Lambda, API Gateway và DynamoDB ở quy mô demo.
* Deploy frontend bằng Amazon S3 Static Website Hosting.
* Chỉ gọi Amazon Bedrock khi cần phân loại hoặc tạo insight.
* Không sử dụng RDS, ECS, NAT Gateway hoặc Load Balancer.
* Không sử dụng Bedrock Provisioned Throughput, batch inference hoặc fine-tuning.
* Đặt AWS Budget khoảng 60 USD/tháng.
* Cấu hình cảnh báo chi phí ở mức 50%, 80% và 100%.
* Cấu hình CloudWatch log retention hợp lý.
* Cleanup toàn bộ resource sau khi hoàn thành.

**Lưu ý:** AWS Budgets chỉ gửi cảnh báo chi phí, không tự động tắt tài nguyên. Vì vậy project cần có cleanup checklist rõ ràng.

---

## 19. Lộ trình triển khai

Dự án được triển khai theo 12 tuần như sau:

| Tuần    | Nội dung triển khai                                             | Kết quả mong đợi                                                           |
| ------- | --------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Tuần 1  | Phân tích yêu cầu và xác định phạm vi dự án                     | Hoàn thành scope và mục tiêu dự án                                         |
| Tuần 2  | Thiết kế kiến trúc AWS, API và data model                       | Có sơ đồ kiến trúc, API list và data model                                 |
| Tuần 3  | Tạo repo code, cấu trúc project và tài liệu kỹ thuật ban đầu    | Có cấu trúc source code và tài liệu requirements, architecture, data model |
| Tuần 4  | Cấu hình Amazon Cognito                                         | Người dùng có thể đăng ký, đăng nhập và lấy token                          |
| Tuần 5  | Tạo DynamoDB tables và IAM Role                                 | Có bảng Transactions, Budgets, AlertEvents và IAM Role cho Lambda          |
| Tuần 6  | Xây dựng Lambda và API cho giao dịch                            | API giao dịch hoạt động                                                    |
| Tuần 7  | Xây dựng Lambda và API cho ngân sách                            | API ngân sách hoạt động                                                    |
| Tuần 8  | Tích hợp SNS và cảnh báo vượt ngân sách                         | Email cảnh báo được gửi thành công                                         |
| Tuần 9  | Tích hợp Amazon Bedrock cho AI                                  | AI gợi ý danh mục và tạo insight                                           |
| Tuần 10 | Xây dựng frontend web và tích hợp với API Gateway               | Người dùng có thể thao tác chức năng chính trên giao diện web              |
| Tuần 11 | Cấu hình CloudWatch, kiểm thử hệ thống bằng Postman và frontend | Có test case, kết quả test, log và ảnh minh chứng                          |
| Tuần 12 | Hoàn thiện workshop, video demo, cleanup và tối ưu chi phí      | Hoàn thành báo cáo, video demo và xóa tài nguyên không cần thiết           |

---

## 20. Kế hoạch cleanup tài nguyên

Sau khi hoàn thành demo và báo cáo, các tài nguyên AWS cần được kiểm tra và cleanup để tránh phát sinh chi phí.

Các tài nguyên cần kiểm tra:

* Amazon S3 bucket dùng cho frontend.
* Amazon Cognito User Pool.
* API Gateway REST API.
* AWS Lambda functions.
* DynamoDB tables.
* SNS topics và subscriptions.
* CloudWatch log groups.
* IAM roles và policies tạo riêng cho project.
* AWS Budgets nếu không còn cần sử dụng.

Các tài nguyên không sử dụng trong project như NAT Gateway, RDS, ECS, Load Balancer, EC2 hoặc Bedrock Provisioned Throughput không nên tạo nếu chưa thật sự cần.

---

## 21. Kết luận

Dự án “Hệ thống quản lý tài chính cá nhân, phân tích AI và cảnh báo vượt ngân sách trên AWS” là một use case thực tế, phù hợp để thực hành xây dựng ứng dụng trên AWS theo hướng serverless. Project không chỉ xây dựng API quản lý tài chính mà còn có frontend web đơn giản cho người dùng, xác thực bằng Cognito, lưu trữ dữ liệu bằng DynamoDB, cảnh báo qua email bằng SNS, giám sát bằng CloudWatch và AI hỗ trợ phân tích chi tiêu thông qua Amazon Bedrock.

Dự án phù hợp với mục tiêu học tập và yêu cầu triển khai ứng dụng trên nền tảng cloud. Bên cạnh chức năng nghiệp vụ, project cũng chú trọng đến kiểm soát chi phí, bảo mật, khả năng vận hành, độ tin cậy, hiệu quả hiệu năng và cleanup tài nguyên theo định hướng AWS Well-Architected Framework.
