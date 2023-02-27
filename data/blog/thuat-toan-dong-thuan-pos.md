---
title: Thuật toán đồng thuận PoS
date: '2023-02-27'
tags: ['Blockchain', 'Consensus']
draft: false
images: ['/static/images/thuat-toan-dong-thuan-pos/thumbnail.png']
summary: Cung cấp một số thông tin về thuật toán đồng thuận PoS và tại sao nó lại ít tốn kém hơn PoW
---

# Thuật toán đồng thuận PoS

<aside>
💡 Proof of Stake là một thuật toán được thiết kế để chỉ một người khai thác có quyền nối thêm một khối mới trong một khoảng thời gian nhất định. Do đó, một khối mới chỉ được xử lý vài giây một lần trong toàn mạng, thay vì xử lý một khối hàng nghìn tỷ lần mỗi giây, giúp loại bỏ vấn đề tiêu thụ điện. Như bạn có thể tưởng tượng, thuật ngữ “thợ đào” không còn được áp dụng theo thuật toán này nữa thay vào đó là thuật ngữ "validator"

</aside>

![Untitled](/static/images/thuat-toan-dong-thuan-pos/Untitled.png)

Từ hình 1 ta có thể thấy được các `Mining node` đang cố gắng xử lý tính toán để thêm một khối mới vào hệ thống nhưng ở nhình 2 chỉ có duy nhất 1 `validator` mới có thể thêm một khối mới vào hệ thống.

Các Validator sẽ gửi một khoản ether đại diện cho cổ phần của họ trong hệ thống. Để chọn một `validator` có quyền đề xuất một khối mới cần thông qua một bộ chọn ngẫu nhiên hoặc chọn `validator` có cổ phần cao nhất. Sau đó `validator` sẽ gửi đề xuất khối mới tới các `validator` thứ cấp để họ phê duyệt. Họ bỏ phiếu cho khối đó(phiếu bầu có thể được tính trên số tiền của họ) và nếu kết quả theo hướng tích cực, khối sẽ được thêm vào blockchain và `validator` sẽ được thưởng bằng Ether. Mặt khác, nếu khối được phát hiện là không chính xác, `validator` đứng lên đề xuất khối có thể bị đưa vào danh sách đen và mất khoản tiền gửi của họ. Ngoài ra, những `validator` thứ cấp được khuyến khích trung thực: nếu phiếu bầu của họ chênh lệch quá nhiều so với phiếu bầu của các đồng nghiệp, họ có thể bị phạt và mất một số tiền đặt cọc.

<aside>
💡 `Validator đề xuất thêm khối` sẽ nhận được số lượng ether mà các giao dịch phải bỏ ra để thực hiện, còn các `validator thứ cấp` sẽ nhận được số lượng tiền thưởng tỷ lệ thuận với số lượng tiền đặt cọc của họ.

</aside>

### Bảng so sánh tên gọi giữa thuật toán PoW và PoS

| Thuật toán | Tên gọi   | Hành động       |
| ---------- | --------- | --------------- |
| PoW        | Miner     | Mining          |
| PoS        | Validator | Forge hoặc Mint |

![Quy trình tạo một khối hợp lệ trong thuật toán đồng thuận PoS](/static/images/thuat-toan-dong-thuan-pos/Untitled%201.png)

Quy trình tạo một khối hợp lệ trong thuật toán đồng thuận PoS

![Trường hợp phát hiện tác nhân gây hại trong thuật toán đồng thuận PoS](/static/images/thuat-toan-dong-thuan-pos/Untitled%202.png)

Trường hợp phát hiện tác nhân gây hại trong thuật toán đồng thuận PoS
