---
title: GAS là gì?
date: '2022-03-28'
tags: ['Blockchain', 'GAS', 'ethereum']
draft: false
summary: Giải thích chi tiết về GAS và lý do tại sao lại cần GAS
---

# Lời mở đầu

GAS là đơn vị đo lường công việc tính toán cần thiết để thực hiện các hoạt động cụ thể trên Ethereum. Nói một cách đơn giản, nếu giao dịch của chúng ta phức tạp, có nghĩa là nó cần nhiều tài nguyên tính toán hơn (chẳng hạn như thời gian CPU và bộ nhớ) do vậy ta sẽ cần phải trả nhiều GAS hơn. Tất cả các mã opcode trên EVM sẽ có giá GAS và không thay đổi trong tương lai. Nếu phí GAS này quá thấp, giao dịch có thể không bao giờ được thực hiện; phí GAS càng nhiều, cơ hội các giao dịch sẽ được các thợ đào chọn để đưa vào khối càng cao. Ngược lại, nếu giao dịch có một khoản phí GAS thích hợp được các thợ đào lựa chọn nhưng có quá nhiều hoạt động phức tạp trong giao dịch đó, nó có thể dẫn đến hết GAS. Trong trường hợp này, giao dịch sẽ không thành công nhưng vẫn được thực hiện và trở thành một phần của khối, người khởi tạo giao dịch sẽ không nhận được bất kỳ khoản hoàn trả nào và tất cả các thay đổi trạng thái được thực hiện bởi giao dịch sẽ được hoàn nguyên.

GAS được chia thành 3 phần: **GAS cost**, **GAS price** và **GAS limit**. Trong Ethereum chi phí giao dịch được tính với công thức

$ transactionFee(txFee) = actualGasCost \* gasPrice $

- **GAS Cost** chỉ ra rằng mỗi opcode cần bao nhiêu GAS, được xác định trước trong Ethereum yellow paper[thêm link tham khảo]. Ví dụ: một opcode “Addition” cần 3 GAS, bất kể sự biến động của giá ether thì GAS cho mỗi opcode sẽ không bị thay đổi. Đó là lý do tại sao GAS được sử dụng để ước tính chi phí của một giao dịch thay vì ether. Nếu ether được sử dụng cho chi phí GAS, giá có thể thay đổi mạnh.
- **GAS Price** cho biết một đơn vị GAS tương đương với bao nhiêu ether. Có một số trang web cung cấp giá trung bình của GAS Price ví dụ như: ethgasstation.info, etherscan.io/chart/gasprice. Đôi khi, người dùng có thể sẵn sàng trả giá cao hơn để làm cho giao dịch của họ được ưu tiên trong việc được các thợ đào chọn và thực hiện.
- **GAS Limit** là giới hạn sử dụng GAS cho một giao dịch cụ thể, đó là số GAS tối đa cần thiết để thực hiện giao dich do vậy số lượng GAS sẽ nhiều hơn so với thực tế. Vì độ phức tạp của giao dịch khác nhau, nên số lượng GAS đã sử dụng chỉ được biết đến sau khi giao dịch đã hoàn thành. Vì vậy, trước khi gửi giao dịch, ta cần đặt GAS Limit Nếu GAS limit được đặt quá thấp, thợ đào sẽ cố gắng hoàn thành giao dịch cho đến khi hết GAS. Thợ đào sẽ nhận được phần thưởng khi hết GAS vì họ đã dành thời gian và sức lực cho việc xác minh giao dịch của người dùng.

# Tại sao phải là GAS

Có 3 lý do chính để chúng ta sử dụng GAS đó là:

- Tài chính
- Lý thuyết
- Lý thuyết tính toán

Từ quan điểm tài chính, mục đích là khuyến khích các thợ đào thực hiện giao dịch và hợp đồng thông minh bằng cách sử dụng thời gian và tài nguyên của chính họ. Nhiều hoạt động phức tạp cần nhiều tài nguyên tính toán hơn; nghĩa là họ cần phải trả nhiều GAS hơn. Nếu người dùng muốn giao dịch của họ được ưu tiên, anh ta có thể gửi giao dịch với GAS Price cao hơn. Bằng cách này, giao dịch có thể được xử lý sớm hơn bởi thợ đào. Khi bù đắp cho tài nguyên tính toán mà người khai thác đầu tư vào, GAS trở nên quan trọng hơn sau khi sự đồng thuận chuyển sang Proof of Stake (POS). Trong kỷ nguyên POS, người khai thác không còn được thưởng bằng các khối khai thác và đóng gói giao dịch, điều quan trọng hơn là người khai thác phải xử lý giao dịch và được trả tiền cho việc sử dụng tài nguyên trên blockchain.

Mục đích lý thuyết là để điều chỉnh sự thúc đẩy của những người tham gia trên mạng. Phần lớn lý thuyết blockchain thảo luận về cách giảm thiểu các tác nhân gây hại hoặc độc hại trong một môi trường không đáng tin cậy. GAS giải quyết một phần vấn đề này: Các thợ đào được khuyến khích làm việc trên mạng và không khuyến khích các hành động xấu hoặc viết mã độc hại vì họ đang đặt ether của chính họ vào nguy hiểm.

Từ quan điểm lý thuyết tính toán, lý do tính toán đằng sau GAS dựa trên nền tảng của lý thuyết tính toán - "Bài toán dừng" (Halting Problem). "Bài toán dừng" đưa ra để xác định xem một chương trình tùy ý sẽ ngừng chạy hay nó sẽ chạy mãi mãi chỉ bằng cách nhìn vào mô tả và các giá trị đầu vào. Năm 1936, Alan Turing xác định rằng không thể có máy móc nào giải quyết được vấn đề tạm dừng. Trong EVM, điều này có nghĩa là người khai thác không bao giờ có thể bắt đầu xử lý một giao dịch và biết 100% rằng giao dịch sẽ không diễn ra mãi mãi. Với GAS - cụ thể GAS limit - một lượng khí hữu hạn luôn được gắn vào một giao dịch. Ngay cả khi thợ đào bắt đầu xử lý một giao dịch được mã hóa với vòng lặp vô thời hạn do lỗi hoặc bị tấn công trên mạng nhưng cuối cùng GAS vẫn sẽ cạn kiệt, giao dịch sẽ kết thúc và thợ đào vẫn sẽ được bồi thường.
