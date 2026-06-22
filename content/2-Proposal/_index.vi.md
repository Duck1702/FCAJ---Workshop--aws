---

title: "2. Bản đề xuất"
date: 2026-06-15
weight: 2
chapter: false
--------------

# Bản đề xuất dự án

## 1. Tên dự án

**Hệ thống API quản lý tài chính cá nhân và giám sát trên AWS**

Tên tiếng Anh: **Serverless Personal Finance API & Monitoring System on AWS**

## 2. Tổng quan dự án

Dự án xây dựng một hệ thống API quản lý tài chính cá nhân sử dụng kiến trúc serverless trên Amazon Web Services. Hệ thống cho phép người dùng quản lý các giao dịch thu chi, phân loại giao dịch, theo dõi ngân sách cá nhân và nhận cảnh báo khi tổng chi tiêu vượt quá giới hạn đã thiết lập.

Project được triển khai theo hướng cloud-native, tận dụng các dịch vụ AWS như Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon Cognito, Amazon CloudWatch và Amazon SNS. Mục tiêu của dự án là xây dựng một hệ thống có khả năng mở rộng, chi phí thấp, dễ triển khai, dễ giám sát và phù hợp với một use case thực tế.

## 3. Lý do chọn đề tài

Quản lý tài chính cá nhân là nhu cầu phổ biến trong đời sống hằng ngày. Tuy nhiên, nhiều người dùng vẫn gặp khó khăn trong việc theo dõi chi tiêu, phân loại giao dịch và kiểm soát ngân sách. Bên cạnh đó, khi xây dựng một hệ thống backend truyền thống, người phát triển thường phải quản lý máy chủ, cấu hình môi trường, theo dõi lỗi và tối ưu chi phí vận hành.

Vì vậy, dự án lựa chọn kiến trúc serverless trên AWS nhằm giảm bớt công việc quản trị hạ tầng, tối ưu chi phí và tăng khả năng mở rộng. Thông qua project này, sinh viên có thể thực hành các dịch vụ AWS quan trọng, hiểu rõ cách thiết kế một hệ thống cloud-native và xây dựng tài liệu workshop theo quy trình triển khai thực tế.

## 4. Vấn đề cần giải quyết

Dự án tập trung giải quyết các vấn đề sau:

* Người dùng cần một hệ thống để lưu trữ và quản lý giao dịch tài chính cá nhân.
* Dữ liệu giao dịch cần được lưu trữ an toàn, có thể truy vấn nhanh và dễ mở rộng.
* API cần có cơ chế xác thực để bảo vệ dữ liệu người dùng.
* Hệ thống cần có log và metric để theo dõi hoạt động.
* Cần có cảnh báo khi chi tiêu vượt quá ngân sách.
* Cần hạn chế chi phí vận hành bằng cách sử dụng mô hình serverless.
* Cần có tài liệu hướng dẫn chi tiết để người khác có thể triển khai lại project.

## 5. Mục tiêu dự án

Dự án hướng đến các mục tiêu chính sau:

* Xây dựng REST API cho chức năng quản lý giao dịch tài chính cá nhân.
* Cho phép người dùng thêm, xem, cập nhật và xóa giao dịch.
* Lưu trữ dữ liệu giao dịch bằng Amazon DynamoDB.
* Xác thực người dùng bằng Amazon Cognito.
* Xử lý logic nghiệp vụ bằng AWS Lambda.
* Công khai API thông qua Amazon API Gateway.
* Ghi log, theo dõi metric và tạo cảnh báo bằng Amazon CloudWatch.
* Gửi email cảnh báo bằng Amazon SNS khi chi tiêu vượt ngân sách.
* Xây dựng tài liệu workshop song ngữ hướng dẫn triển khai hệ thống từng bước.
* Thực hiện kiểm thử API và cleanup resource sau khi hoàn thành để tránh phát sinh chi phí.

## 6. Phạm vi dự án

Trong phạm vi project, hệ thống tập trung vào các chức năng chính:

* Đăng ký và đăng nhập người dùng.
* Tạo giao dịch thu hoặc chi.
* Xem danh sách giao dịch.
* Cập nhật thông tin giao dịch.
* Xóa giao dịch.
* Lưu trữ dữ liệu giao dịch theo từng người dùng.
* Thiết lập giới hạn ngân sách.
* Gửi cảnh báo khi tổng chi tiêu vượt quá ngân sách.
* Theo dõi log và metric của hệ thống.

Project không tập trung xây dựng giao diện người dùng hoàn chỉnh, mà ưu tiên vào phần backend, kiến trúc AWS, khả năng giám sát, kiểm thử và tài liệu workshop.

## 7. Các dịch vụ AWS sử dụng

Các dịch vụ AWS dự kiến sử dụng trong project gồm:

* **Amazon API Gateway:** tạo REST API để client hoặc Postman gửi request đến backend.
* **AWS Lambda:** xử lý logic nghiệp vụ như thêm, sửa, xóa và truy vấn giao dịch.
* **Amazon DynamoDB:** lưu trữ dữ liệu giao dịch tài chính.
* **Amazon Cognito:** quản lý đăng ký, đăng nhập và xác thực người dùng.
* **Amazon CloudWatch:** theo dõi log, metric, lỗi và trạng thái hoạt động của hệ thống.
* **Amazon SNS:** gửi email cảnh báo khi chi tiêu vượt ngân sách.
* **AWS IAM:** phân quyền cho Lambda truy cập đúng tài nguyên cần thiết.

## 8. Kiến trúc giải pháp

Luồng hoạt động chính của hệ thống như sau:

Người dùng đăng ký và đăng nhập thông qua Amazon Cognito. Sau khi xác thực thành công, người dùng nhận được token để gọi API. Request từ người dùng được gửi đến Amazon API Gateway. API Gateway kiểm tra xác thực và chuyển request đến AWS Lambda. Lambda xử lý logic nghiệp vụ, sau đó đọc hoặc ghi dữ liệu vào Amazon DynamoDB. Trong quá trình xử lý, log và metric được ghi lại trên Amazon CloudWatch. Khi tổng chi tiêu vượt quá giới hạn ngân sách, Lambda gửi thông báo đến Amazon SNS để gửi email cảnh báo cho người dùng.

Kiến trúc tổng quát:

![Kiến trúc tổng quát của hệ thống](/FCAJ---Workshop--aws/images/2-Proposal/architecture-overview_vi.png)

Luồng chính của hệ thống:
1. Người dùng đăng nhập qua Amazon Cognito.
2. Người dùng gọi API thông qua Amazon API Gateway.
3. API Gateway chuyển request đến AWS Lambda.
4. Lambda xử lý nghiệp vụ và lưu dữ liệu vào DynamoDB.
5. CloudWatch dùng để ghi log và giám sát hệ thống.
6. Khi chi tiêu vượt ngưỡng, SNS gửi email cảnh báo.

## 9. Mô hình dữ liệu dự kiến

Bảng DynamoDB dự kiến: **Transactions**

Các thuộc tính chính:

```text
userId
transactionId
type
category
amount
note
createdAt
updatedAt
```

Ý nghĩa:

* **userId:** định danh người dùng.
* **transactionId:** định danh giao dịch.
* **type:** loại giao dịch, gồm income hoặc expense.
* **category:** danh mục giao dịch, ví dụ food, transport, salary, shopping.
* **amount:** số tiền giao dịch.
* **note:** ghi chú giao dịch.
* **createdAt:** thời gian tạo giao dịch.
* **updatedAt:** thời gian cập nhật giao dịch.

## 10. Kế hoạch triển khai

Dự án được triển khai theo 12 tuần:

| Tuần    | Công việc dự kiến                                                  |
| ------- | ------------------------------------------------------------------ |
| Tuần 1  | Tìm hiểu yêu cầu, chọn đề tài và chuẩn bị repository workshop      |
| Tuần 2  | Viết proposal và xác định các dịch vụ AWS sử dụng                  |
| Tuần 3  | Thiết kế kiến trúc hệ thống và mô hình dữ liệu                     |
| Tuần 4  | Tạo DynamoDB table và cấu hình IAM role                            |
| Tuần 5  | Xây dựng Lambda function cho chức năng tạo giao dịch               |
| Tuần 6  | Xây dựng các API còn lại và tích hợp API Gateway                   |
| Tuần 7  | Cấu hình Amazon Cognito để xác thực người dùng                     |
| Tuần 8  | Kiểm thử API bằng Postman                                          |
| Tuần 9  | Cấu hình CloudWatch Logs, Metrics và Alarm                         |
| Tuần 10 | Cấu hình Amazon SNS để gửi cảnh báo                                |
| Tuần 11 | Hoàn thiện tài liệu workshop và thêm hình ảnh minh họa             |
| Tuần 12 | Kiểm tra, tối ưu chi phí, viết self-evaluation và cleanup resource |

## 11. Kiểm thử và đánh giá

Dự án sẽ được kiểm thử bằng Postman và AWS Console. Một số test case dự kiến gồm:

* Gọi API tạo giao dịch thành công.
* Gọi API lấy danh sách giao dịch.
* Gọi API cập nhật giao dịch.
* Gọi API xóa giao dịch.
* Gọi API khi thiếu token xác thực.
* Gọi API với dữ liệu không hợp lệ.
* Kiểm tra dữ liệu được lưu trong DynamoDB.
* Kiểm tra log Lambda trong CloudWatch.
* Kiểm tra cảnh báo khi chi tiêu vượt ngân sách.
* Kiểm tra email cảnh báo được gửi qua Amazon SNS.

## 12. Kết quả mong đợi

Sau khi hoàn thành, project sẽ đạt được các kết quả sau:

* Một hệ thống REST API serverless hoạt động trên AWS.
* Người dùng có thể quản lý giao dịch tài chính cá nhân.
* Dữ liệu được lưu trữ trong Amazon DynamoDB.
* API được bảo vệ bằng Amazon Cognito.
* Hệ thống có log, metric và alarm thông qua Amazon CloudWatch.
* Có chức năng gửi email cảnh báo bằng Amazon SNS.
* Có tài liệu workshop song ngữ hướng dẫn triển khai từng bước.
* Có phần cleanup để xóa tài nguyên AWS sau khi hoàn thành.

## 13. Rủi ro và hướng xử lý

Một số rủi ro có thể gặp trong quá trình triển khai:

* Cấu hình IAM sai quyền làm Lambda không thể truy cập DynamoDB.
* API Gateway chưa tích hợp đúng với Lambda.
* Cognito token không hợp lệ khi test API.
* CloudWatch Alarm chưa kích hoạt đúng điều kiện.
* Dữ liệu đầu vào không đúng định dạng.
* Phát sinh chi phí nếu không xóa resource sau khi demo.

Hướng xử lý:

* Kiểm tra CloudWatch Logs để debug lỗi Lambda.
* Phân quyền IAM theo nguyên tắc least privilege.
* Test từng API riêng bằng Postman.
* Kiểm tra cấu hình Cognito Authorizer trong API Gateway.
* Tạo AWS Budget hoặc theo dõi Billing Dashboard.
* Thực hiện cleanup toàn bộ resource sau khi hoàn thành project.

## 14. Ước tính chi phí

Project sử dụng chủ yếu các dịch vụ serverless nên chi phí dự kiến thấp. Với quy mô demo và số lượng request nhỏ, các dịch vụ như AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon CloudWatch và Amazon SNS có thể được sử dụng trong giới hạn miễn phí hoặc phát sinh chi phí rất thấp.

Sau khi hoàn thành demo, toàn bộ tài nguyên không cần thiết sẽ được xóa để tránh phát sinh chi phí ngoài ý muốn.

## 15. Kết luận

Dự án “Hệ thống API quản lý tài chính cá nhân và giám sát trên AWS” giúp sinh viên thực hành xây dựng một hệ thống backend serverless hoàn chỉnh trên AWS. Project không chỉ tập trung vào việc tạo API mà còn chú trọng đến bảo mật, giám sát, cảnh báo, kiểm thử, tối ưu chi phí và cleanup tài nguyên. Đây là một use case thực tế, phù hợp với mục tiêu học tập và yêu cầu của chương trình First Cloud AI Journey.
