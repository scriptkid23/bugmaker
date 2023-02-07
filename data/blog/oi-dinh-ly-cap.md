---
title: Định lý CAP là gì?
date: '2022-03-28'
tags: ['Blockchain', 'CAP']
draft: false
images: ['/static/images/cap/cap.jpeg']
summary: Giới thiệu tổng quan về định lý CAP
---

# Lời mở đầu

Định lý CAP, còn được gọi là định lý Brewer, được đưa ra bởi Eric Brewer vào năm 1998 dưới dạng một phỏng đoán. Năm 2002, nó đã được chứng minh như một định lý bởi Seth Gilbert và Nancy Lynch. Định lý nói rằng bất kỳ hệ thống phân tán nào không thể đồng thời có được tính nhất quán, tính khả dụng và khả năng chịu phân vùng.

![ocean](/static/images/cap/cap.jpeg)

- Tính nhất quán - **Consistency (C)**: Tính nhất quán là thuộc tính đảm bảo rằng tất cả các node trong hệ thống phân tán đều có một bản sao dữ liệu duy nhất và giống hệt nhau.
- Tính khả dụng - **Availability (A)**: có nghĩa là các node trong hệ thống đã hoạt động, có thể truy cập để sử dụng và đang chấp nhận các yêu cầu đến cũng như phản hồi dữ liệu mà không có bất kỳ lỗi nào khi được yêu cầu. Nói cách khác, dữ liệu có sẵn tại mỗi node và các node cũng đang phản hồi các yêu cầu.
- Khả năng chịu phân vùng - **Partition tolerance (P)**: đảm bảo rằng nếu một nhóm các node không thể giao tiếp với các node khác do lỗi mạng, hệ thống phân tán vẫn tiếp tục hoạt động chính xác. Điều này có thể xảy ra do lỗi mạng.

Dựa vào sơ đồ ta có thể thấy chỉ có thể đạt được hai thuộc tính tại một thời điểm. AP, CA hoặc CP.

- Nếu chọn CP (tính nhất quán và khả năng chịu phân vùng) => hy sinh tính khả dụng.
- Nếu chọn AP (tính khả dụng và khả năng chịu phân vùng) => chúng tôi hy sinh tính nhất quán.
- Nếu chọn AC (tính khả dụng và tính nhất quán) => hy sinh khả năng chịu phân vùng.

Thông thường, không thể bỏ qua một phân vùng mạng; do đó, sự lựa chọn chủ yếu là CP và AP. Như đã trình bày trước đó, một hệ thống phân tán không thể có đồng thời tính nhất quán, tính khả dụng và khả năng chịu phân vùng. Điều này có thể được giải thích bằng ví dụ sau. Hãy tưởng tượng rằng có một hệ thống phân tán với hai node. Bây giờ, chúng ta hãy áp dụng ba tính chất lên hệ thống phân tán chỉ với hai node khi đó:

- Tính nhất quán đạt được nếu cả hai node có cùng trạng thái được chia sẻ; nghĩa là chúng có cùng một bản sao dữ liệu cập nhật.
- Tính khả dụng đạt được nếu cả hai node đều hoạt động và phản hồi bằng bản sao dữ liệu mới nhất.
- Khả năng chịu phân vùng đạt được nếu bất chấp sự cố hoặc độ trễ giao tiếp giữa các node, mạng (hệ thống phân tán) vẫn tiếp tục hoạt động.

Bây giờ hãy nghĩ về khả năng mà một phân vùng xảy ra lỗi và các node không thể giao tiếp với nhau nữa. Nếu bây giờ dữ liệu được cập nhật, nó chỉ có thể được cập nhật trên một node duy nhất. Trong trường hợp đó, nếu node chấp nhận cập nhật, thì chỉ một node trong mạng được cập nhật và do đó tính nhất quán bị mất nhưng có được tính khả dụng. Bây giờ, nếu bản cập nhật bị từ chối bởi node để có được tính nhất quán giữa các node, điều đó sẽ dẫn đến việc mất tính khả dụng. Trong trường hợp này do chọn khả năng chịu phân vùng mà tính khả dụng và tính nhất quán đều không thể đạt được đồng thời.
