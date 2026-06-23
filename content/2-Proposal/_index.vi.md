---

title: "2. Bản đề xuất"
date: 2026-06-15
weight: 2
chapter: false
--------------


## 1. Tên dự án

**Hệ thống quản lý tài chính cá nhân, phân tích AI và cảnh báo vượt ngân sách trên AWS**

Tên tiếng Anh: **AI-Powered Personal Finance Budget Alert System on AWS**

## 2. Tổng quan dự án

Dự án xây dựng một hệ thống quản lý tài chính cá nhân sử dụng kiến trúc serverless trên Amazon Web Services. Hệ thống cho phép người dùng quản lý giao dịch thu chi, thiết lập ngân sách cá nhân, nhận cảnh báo khi chi tiêu vượt quá ngân sách và sử dụng AI để gợi ý danh mục giao dịch cũng như tạo nhận xét chi tiêu theo tháng.

Project được triển khai bằng các dịch vụ AWS như Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon SNS, Amazon CloudWatch, Amazon Bedrock và AWS IAM. Mục tiêu của dự án là xây dựng một hệ thống cloud-native có tính thực tế, chi phí thấp, dễ mở rộng, có khả năng giám sát và có yếu tố AI hỗ trợ người dùng ra quyết định tài chính tốt hơn.

## 3. Mục đích dự án

Mục đích chính của dự án là hỗ trợ người dùng theo dõi và kiểm soát tài chính cá nhân một cách chủ động. Thay vì chỉ ghi nhận giao dịch, hệ thống còn giúp người dùng nhận biết khi chi tiêu vượt quá ngân sách và cung cấp phân tích AI để hiểu rõ hơn về hành vi chi tiêu.

Về mặt kỹ thuật, dự án giúp thực hành triển khai một hệ thống serverless hoàn chỉnh trên AWS, bao gồm xác thực người dùng, xây dựng REST API, xử lý nghiệp vụ bằng Lambda, lưu trữ dữ liệu NoSQL, gửi thông báo qua email, giám sát hệ thống bằng CloudWatch và tích hợp AI thông qua Amazon Bedrock.

## 4. Đối tượng sử dụng

Hệ thống hướng đến các nhóm người dùng sau:

* Sinh viên muốn theo dõi chi tiêu hằng tháng.
* Người đi làm muốn kiểm soát thu nhập và chi phí cá nhân.
* Người dùng cá nhân muốn nhận cảnh báo khi chi tiêu vượt ngân sách.
* Người muốn có phân tích AI đơn giản về tình hình chi tiêu.
* Người học AWS muốn thực hành xây dựng một ứng dụng serverless có xác thực, API, database, notification, monitoring và AI.

## 5. Vấn đề cần giải quyết

Trong thực tế, nhiều người dùng thường không theo dõi chi tiêu thường xuyên, không phân loại giao dịch rõ ràng và không biết thời điểm chi tiêu đã vượt quá ngân sách. Điều này có thể dẫn đến mất kiểm soát tài chính cá nhân.

Ngoài ra, các hệ thống truyền thống thường cần máy chủ, cơ sở dữ liệu quan hệ và chi phí vận hành cố định. Với phạm vi project học tập, việc sử dụng kiến trúc serverless giúp giảm chi phí, giảm công việc vận hành hạ tầng và vẫn đảm bảo khả năng mở rộng.

Dự án giải quyết các vấn đề chính:

* Quản lý giao dịch thu chi cá nhân.
* Thiết lập và theo dõi ngân sách.
* Cảnh báo khi chi tiêu vượt ngân sách.
* Gợi ý danh mục giao dịch bằng AI.
* Tạo nhận xét chi tiêu bằng AI.
* Ghi log, giám sát lỗi và kiểm soát chi phí triển khai.

## 6. Mục tiêu dự án

Dự án hướng đến các mục tiêu sau:

* Xây dựng REST API cho hệ thống quản lý tài chính cá nhân.
* Cho phép người dùng tạo, xem, cập nhật và xóa giao dịch.
* Cho phép người dùng thiết lập ngân sách theo tháng.
* Tự động tính tổng chi tiêu và so sánh với ngân sách.
* Gửi email cảnh báo khi tổng chi tiêu vượt ngân sách.
* Sử dụng AI để gợi ý danh mục giao dịch từ mô tả giao dịch.
* Sử dụng AI để tạo nhận xét chi tiêu theo tháng.
* Xác thực người dùng bằng Amazon Cognito.
* Lưu trữ dữ liệu bằng Amazon DynamoDB.
* Xử lý nghiệp vụ bằng AWS Lambda.
* Công khai API thông qua Amazon API Gateway.
* Ghi log và theo dõi hệ thống bằng Amazon CloudWatch.
* Phân quyền bằng AWS IAM theo nguyên tắc least privilege.
* Kiểm soát chi phí trong phạm vi tối đa 200 USD cho toàn bộ thời gian triển khai.
* Viết tài liệu workshop song ngữ hướng dẫn triển khai từng bước.

## 7. Phạm vi dự án

Trong phạm vi project, hệ thống tập trung vào các chức năng chính:

* Đăng ký và đăng nhập người dùng.
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

Project không tập trung xây dựng giao diện người dùng hoàn chỉnh. Việc kiểm thử và demo có thể được thực hiện bằng Postman, AWS Console và email cảnh báo thực tế.

## 8. Các dịch vụ AWS sử dụng

Các dịch vụ AWS dự kiến sử dụng gồm:

* **Amazon Cognito:** quản lý đăng ký, đăng nhập và xác thực người dùng.
* **Amazon API Gateway:** tạo REST API endpoint để người dùng hoặc Postman gọi đến hệ thống.
* **AWS Lambda:** xử lý logic nghiệp vụ cho giao dịch, ngân sách, cảnh báo và AI.
* **Amazon DynamoDB:** lưu trữ giao dịch, ngân sách và lịch sử cảnh báo.
* **Amazon SNS:** gửi email cảnh báo khi chi tiêu vượt ngân sách.
* **Amazon CloudWatch:** ghi log, theo dõi metric và hỗ trợ giám sát lỗi.
* **Amazon Bedrock:** hỗ trợ AI gợi ý danh mục giao dịch và tạo nhận xét chi tiêu.
* **AWS IAM:** quản lý quyền truy cập giữa Lambda và các dịch vụ AWS.
* **AWS Budgets:** theo dõi chi phí và cảnh báo khi chi phí vượt ngưỡng.

## 9. Kiến trúc giải pháp

Hình dưới đây mô tả kiến trúc tổng quát của hệ thống:

![Kiến trúc tổng quát của hệ thống](/FCAJ---Workshop--aws/images/2-Proposal/architecture-overview_vie.png)

Luồng hoạt động chính của hệ thống:

1. Người dùng đăng ký hoặc đăng nhập thông qua Amazon Cognito.
2. Sau khi đăng nhập thành công, người dùng nhận được JWT token.
3. Người dùng gửi request đến Amazon API Gateway kèm token xác thực.
4. API Gateway chuyển request hợp lệ đến AWS Lambda.
5. Lambda xử lý các logic giao dịch, ngân sách và AI.
6. Lambda đọc và ghi dữ liệu vào Amazon DynamoDB.
7. Khi người dùng nhập mô tả giao dịch, Lambda có thể gọi Amazon Bedrock để gợi ý danh mục.
8. Khi người dùng yêu cầu phân tích chi tiêu, Lambda tổng hợp dữ liệu và gọi Amazon Bedrock để tạo insight.
9. Nếu tổng chi tiêu vượt ngân sách, Lambda gửi thông báo đến Amazon SNS.
10. Amazon SNS gửi email cảnh báo đến người dùng.
11. Amazon CloudWatch ghi log, metric và hỗ trợ giám sát lỗi.
12. AWS IAM Role cấp quyền cần thiết cho Lambda để truy cập DynamoDB, SNS, CloudWatch và Bedrock.

## 10. Mô hình dữ liệu dự kiến

Hệ thống dự kiến sử dụng ba bảng DynamoDB chính.

### Bảng Transactions

Lưu thông tin giao dịch thu chi.

```text
userId
transactionId
type
category
amount
description
createdAt
updatedAt
```

Ý nghĩa:

* **userId:** định danh người dùng.
* **transactionId:** định danh giao dịch.
* **type:** loại giao dịch, gồm income hoặc expense.
* **category:** danh mục giao dịch.
* **amount:** số tiền giao dịch.
* **description:** mô tả giao dịch.
* **createdAt:** thời gian tạo giao dịch.
* **updatedAt:** thời gian cập nhật giao dịch.

### Bảng Budgets

Lưu ngân sách theo tháng của người dùng.

```text
userId
month
budgetLimit
currentExpense
updatedAt
```

Ý nghĩa:

* **userId:** định danh người dùng.
* **month:** tháng áp dụng ngân sách.
* **budgetLimit:** giới hạn ngân sách.
* **currentExpense:** tổng chi tiêu hiện tại.
* **updatedAt:** thời gian cập nhật gần nhất.

### Bảng AlertEvents

Lưu lịch sử cảnh báo vượt ngân sách.

```text
alertId
userId
month
budgetLimit
currentExpense
status
createdAt
```

Ý nghĩa:

* **alertId:** định danh cảnh báo.
* **userId:** định danh người dùng.
* **month:** tháng phát sinh cảnh báo.
* **budgetLimit:** ngân sách đã thiết lập.
* **currentExpense:** tổng chi tiêu tại thời điểm cảnh báo.
* **status:** trạng thái gửi cảnh báo, ví dụ SENT hoặc FAILED.
* **createdAt:** thời gian tạo cảnh báo.

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

## 12. Tính năng AI

Dự án sử dụng AI cho hai chức năng chính.

### 12.1. AI gợi ý danh mục giao dịch

Khi người dùng nhập mô tả giao dịch, hệ thống có thể sử dụng AI để gợi ý danh mục phù hợp.

Ví dụ:

```text
Mô tả: "ăn trưa cơm gà"
Danh mục gợi ý: Food

Mô tả: "đổ xăng xe máy"
Danh mục gợi ý: Transport

Mô tả: "lương tháng 6"
Danh mục gợi ý: Salary
```

### 12.2. AI tạo nhận xét chi tiêu

Dựa trên dữ liệu giao dịch và ngân sách, hệ thống tạo nhận xét ngắn gọn cho người dùng.

Ví dụ:

```text
Tháng này bạn chi tiêu nhiều nhất cho Food và Shopping.
Chi tiêu hiện tại đã vượt 12% so với ngân sách.
Bạn nên giảm chi tiêu không cần thiết trong tuần tiếp theo.
```

## 13. Kiểm thử và đánh giá

Hệ thống sẽ được kiểm thử bằng Postman và AWS Console. Các test case dự kiến gồm:

* Đăng nhập và lấy token từ Amazon Cognito.
* Gọi API khi có token hợp lệ.
* Gọi API khi không có token.
* Tạo giao dịch thu hoặc chi.
* Lấy danh sách giao dịch.
* Cập nhật giao dịch.
* Xóa giao dịch.
* Thiết lập ngân sách.
* Tạo nhiều giao dịch chi tiêu để vượt ngân sách.
* Kiểm tra email cảnh báo từ Amazon SNS.
* Gọi API AI gợi ý danh mục.
* Gọi API AI tạo nhận xét chi tiêu.
* Kiểm tra dữ liệu lưu trong DynamoDB.
* Kiểm tra log trong CloudWatch.
* Kiểm tra phản hồi lỗi khi dữ liệu đầu vào không hợp lệ.

## 14. Kết quả mong đợi

Sau khi hoàn thành, project sẽ đạt được các kết quả sau:

* Một hệ thống REST API serverless hoạt động trên AWS.
* Người dùng có thể quản lý giao dịch thu chi cá nhân.
* Người dùng có thể thiết lập ngân sách và nhận cảnh báo khi vượt ngân sách.
* Hệ thống có AI gợi ý danh mục giao dịch.
* Hệ thống có AI tạo nhận xét chi tiêu theo tháng.
* Dữ liệu được lưu trữ trong DynamoDB.
* API được bảo vệ bằng Amazon Cognito.
* Email cảnh báo được gửi thông qua Amazon SNS.
* CloudWatch ghi nhận log và hỗ trợ giám sát hệ thống.
* Có tài liệu workshop song ngữ hướng dẫn triển khai từng bước.
* Có ảnh chụp minh chứng các dịch vụ AWS, kết quả test API, log và email cảnh báo.
* Có phần cleanup để xóa tài nguyên AWS sau khi hoàn thành.

## 15. Rủi ro và hướng xử lý

Một số rủi ro có thể gặp trong quá trình triển khai:

* Cấu hình IAM chưa đúng làm Lambda không thể truy cập DynamoDB, SNS hoặc Bedrock.
* API Gateway chưa tích hợp đúng với Lambda.
* Cognito token không hợp lệ khi test API.
* Logic tính tổng chi tiêu chưa chính xác.
* SNS email subscription chưa được xác nhận.
* Chi phí AI tăng nếu gọi Bedrock quá nhiều lần.
* CloudWatch Logs phát sinh chi phí nếu lưu log quá lâu.
* Phát sinh chi phí nếu không cleanup tài nguyên sau khi demo.

Hướng xử lý:

* Kiểm tra CloudWatch Logs để debug lỗi Lambda.
* Cấu hình IAM theo nguyên tắc least privilege.
* Test từng API bằng Postman trước khi test toàn bộ luồng.
* Xác nhận email SNS trước khi test cảnh báo.
* Giới hạn số lần gọi AI trong quá trình demo.
* Thiết lập CloudWatch log retention 7 hoặc 14 ngày.
* Tạo AWS Budget để theo dõi chi phí.
* Cleanup toàn bộ tài nguyên sau khi hoàn thành project.

## 16. Ước tính chi phí

Project sử dụng chủ yếu các dịch vụ serverless và pay-as-you-go nên chi phí dự kiến thấp. Mục tiêu chi phí tối đa cho toàn bộ quá trình triển khai trong khoảng 3 tháng là **200 USD**.

Kế hoạch kiểm soát chi phí:

* Sử dụng Lambda, API Gateway và DynamoDB ở quy mô demo.
* Chỉ gọi Amazon Bedrock khi cần phân loại hoặc tạo insight.
* Không sử dụng RDS, ECS, NAT Gateway hoặc Load Balancer.
* Đặt AWS Budget khoảng 60 USD/tháng.
* Cấu hình cảnh báo chi phí ở mức 50%, 80% và 100%.
* Cleanup toàn bộ resource sau khi hoàn thành.

## 17. Lộ trình triển khai

Dự án được triển khai theo 12 tuần như sau:

| Tuần    | Nội dung triển khai                            | Kết quả mong đợi                                     |
| ------- | ---------------------------------------------- | ---------------------------------------------------- |
| Tuần 1  | Phân tích yêu cầu và xác định phạm vi dự án    | Hoàn thành scope và mục tiêu dự án                   |
| Tuần 2  | Thiết kế kiến trúc AWS, API và data model      | Có sơ đồ kiến trúc, API list và data model           |
| Tuần 3  | Tạo repo code và cấu trúc project              | Có cấu trúc source code ban đầu                      |
| Tuần 4  | Cấu hình Amazon Cognito                        | Người dùng có thể đăng ký, đăng nhập và lấy token    |
| Tuần 5  | Tạo DynamoDB tables và IAM Role                | Có bảng Transactions, Budgets, AlertEvents           |
| Tuần 6  | Xây dựng Lambda và API cho giao dịch           | API giao dịch hoạt động                              |
| Tuần 7  | Xây dựng Lambda và API cho ngân sách           | API ngân sách hoạt động                              |
| Tuần 8  | Tích hợp SNS và cảnh báo vượt ngân sách        | Email cảnh báo được gửi thành công                   |
| Tuần 9  | Tích hợp Amazon Bedrock cho AI                 | AI gợi ý danh mục và tạo insight                     |
| Tuần 10 | Cấu hình CloudWatch Logs, Metrics và Alarm     | Có log và metric giám sát                            |
| Tuần 11 | Kiểm thử toàn bộ hệ thống bằng Postman         | Có test case, kết quả test và ảnh minh chứng         |
| Tuần 12 | Hoàn thiện workshop, cleanup và tối ưu chi phí | Hoàn thành báo cáo và xóa tài nguyên không cần thiết |

## 18. Kết luận

Dự án “Hệ thống quản lý tài chính cá nhân, phân tích AI và cảnh báo vượt ngân sách trên AWS” là một use case thực tế, phù hợp để thực hành các dịch vụ AWS serverless. Project không chỉ xây dựng API quản lý tài chính mà còn kết hợp xác thực, lưu trữ dữ liệu, cảnh báo qua email, giám sát hệ thống và AI hỗ trợ phân tích chi tiêu. Đây là hướng triển khai phù hợp với mục tiêu học tập, yêu cầu project và giới hạn chi phí trong khoảng 3 tháng.
