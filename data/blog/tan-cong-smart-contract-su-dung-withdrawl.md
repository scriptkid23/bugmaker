---
title: Withdrawal Pattern
date: '2023-02-27'
tags: ['Blockchain', 'smart contract', 'design pattern']
draft: false
images: ['/static/images/ky-thuat-tan-cong-smart-contract/thumbnail.png']
summary: Mặc dù các phương pháp gửi ether đã có sẵn trong solidity ví dụ ta có thể sử dụng câu lệnh transfer để gửi ether nhưng nó không được khuyến khích sử dụng vì nó tiềm ẩn rủi ro về bảo mật. Do đó Withdrawal Pattern ra đời.
---

Mặc dù các phương pháp gửi ether đã có sẵn trong solidity ví dụ ta có thể sử dụng câu lệnh `transfer` để gửi ether nhưng nó không được khuyến khích sử dụng vì nó tiềm ẩn rủi ro về bảo mật. Do đó `Withdrawal Pattern` ra đời.

<aside>
❓ Sau đây là một ví dụ về mô hình rút tiền trong thực tế của một hợp đồng mà mục tiêu là gửi nhiều tiền nhất vào hợp đồng để trở thành "người giàu nhất" được lấy cảm hứng từ King of the Ether.
Trong hợp đồng sau đây, nếu bạn không còn là người giàu nhất, bạn sẽ được hoàn lại một khoản tiền ether.

</aside>

Trước tiên chúng ta phải tìm hiểu khi chuyển ether vào hợp đồng thì điều gì sẽ xảy ra.

Do hiện tại cả `contract` và `external accounts` đều không thể ngăn việc ai đó gửi Ether cho chúng. Các hợp đồng có thể chấp nhận hoặc từ chối chuyển khoản thông thường, nhưng có nhiều cách để di chuyển Ether mà không cần tạo `message call`. Một cách đơn giản là sử dụng "mine to" địa chỉ hợp đồng và cách thứ hai là sử dụng `selfdestruct (x)`.

Nếu một hợp động nhận được Ether (không có hàm nào được gọi), thì lúc này hàm `receive ether function` hoặc `fallback function` sẽ được thực thi. Nếu hợp đồng không có các hàm đó, Ether sẽ bị từ chối (bằng cách ném ra một ngoại lệ). Trong quá trình thực hiện một trong những chức năng này, hợp đồng chỉ có thể dựa vào “chi phí gas” mà nó được chuyển (2300 gas) có sẵn cho nó tại thời điểm đó. Khoản chi tiêu này không đủ để sửa đổi dung lượng lưu trữ (mặc dù vậy, đừng coi chi ra 2300 gas là đương nhiên, số tiền chi tiêu có thể thay đổi theo các hard fork trong tương lai). Để chắc chắn rằng hợp đồng của bạn có thể nhận Ether theo cách đó, hãy kiểm tra các yêu cầu về gas của các `receive ehter function` hoặc `fallback function`

Có một cách để chuyển tiếp nhiều gas hơn đến hợp đồng nhận bằng cách sử dụng `address.call {value: x} ("")`. Điều này về cơ bản giống với `addr.transfer (x)`, chỉ khác là nó chuyển tiếp tất cả gas còn lại và mở ra khả năng cho người nhận thực hiện các hành động tốn kém hơn (và nó trả về mã lỗi thay vì tự động truyền lỗi). Điều này có thể bao gồm việc gọi lại hợp đồng gửi hoặc các thay đổi trạng thái khác mà bạn có thể không nghĩ đến. Vì vậy, nó cho phép sự linh hoạt tuyệt vời cho người dùng trung thực nhưng cũng cho các tác nhân xấu.

Nếu bạn muốn gửi Ether bằng `address.transfer`, có một số chi tiết nhất định cần lưu ý:

1.Nếu người nhận là một hợp đồng, nó làm cho `receive function` hoặc `fallback function` của nó được thực thi, từ đó có thể gọi lại `sending contract`.

1. Gửi Ether cũng có thể không thành công vì việc thực hiện hợp đồng người nhận yêu cầu nhiều hơn lượng gas được phân bổ(khi sử dụng require, assert, revert hoặc vì một hoạt động nào đó quá tốn kém nó "hết xăng" (OOG- runs out of gas) ). Nếu bạn sử dụng transfer hoặc send kèm theo trả về một value check điều này có thể cung cấp một phương pháp để người nhận chặn tiến trình trong sending contract.Một lần nữa, phương pháp hay nhất ở đây là sử dụng Withdrawal pattern

```jsx
//SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract SendContract {
    address payable public richest;
    uint public mostSent;

	/// The amount of Ether sent was not higher than
	/// the currently highest amount.
		error NotEnoughEther();

		constructor() payable {
        richest = payable(msg.sender);
        mostSent = msg.value;
    }

		function becomeRichest() public payable {
			if (msg.value <= mostSent) revert NotEnoughEther();
			// This line can cause problems (explained below).
			richest.transfer(msg.value);
      richest  = payable(msg.sender);
      mostSent = msg.value;
    }
}
contract User {
    event ValueReceived(address user, uint amount);

    receive() external payable{
        emit ValueReceived(msg.sender, msg.value);
    }
    function sendEther(SendContract kingOfEther) public payable{
        kingOfEther.becomeRichest{value:msg.value}();
    }
}
contract Attacker {
    function attack(SendContract kingOfEther) public payable{
        kingOfEther.becomeRichest{value:msg.value}();
    }
}
```

_note_

- Khai báo Functions và addresses payable để có thể nhận ether vào hợp đồng.
- msg.value là số lượng ether mà người dùng muốn gửi cùng khi thực hiện một function trong hợp đồng, số lượng này có thể tùy chỉnh.

<aside>
ℹ️ `payable(msg.sender)`: đưa về dạng `address payable` vì thực hiện `transfer` cần phải được khai báo `payable`
Ta có thể thực hiện hàm `becomeRichest()` trực tiếp từ bên trong hợp đồng `SendContract` nhưng khác hợp đồng `User` ở chỗ khi thực hiện hàm `becomeRichest()` trong một hợp đồng khác (hợp đồng `User`) thì ta phải thêm hàm `receive()` để hợp đồng `SendContract` biết được nơi mà nó sẽ gửi ether nhưng khi thực hiện hàm `becomeRichest()` ngay trong hợp đồng `SendContract` thì không cần hàm `receive()` vì nó đã biết được nơi nhận là nằm trong hợp đồng `SendContract`.

</aside>

Trong đoạn mã trên, hợp đồng `SendContract` có chức năng nhận ether từ các account khác nhau, account nào gửi số lượng ether nhiều nhất sẽ trở thành King of Ether. Khi có một account khác soán ngôi King of Ether của account cũ thì account sẽ được nhận lại một số lượng ether.

Hợp đồng `Attacker` được tạo ra từ attacker trong mạng với mục đích xấu biến hợp đồng `SendContract` rơi vào trạng thái "không sử dụng được" .

Cơ chế: attacker tạo một hợp đồng để thực hiện gửi ether vào hợp đồng `SendContract` nhưng vì hợp đồng `SendContract` có cơ chế trả lại ether cho `King of Ether cũ` khi bị soán ngôi, lợi dụng điều đó attacker đã không tạo hàm `receive()` để nhận ether trả về từ hợp đồng `SendContract` do đó khi có một account khác trở thành `King of Ether` thì `richest.transfer(msg.value);` sẽ bị lỗi vì chuyển ether đi nhưng attacker không nhận dẫn đến `becomeRichest()` sẽ bị lỗi và hợp đồng SendContract rơi vào trạng thái "không sử dụng được" .

Sử dụng `Withdrawl pattern`

```jsx
//SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract SendContract {
    address public richest;
    uint public mostSent;

		mapping (address => uint) pendingWithdrawals;

/// The amount of Ether sent was not higher than
/// the currently highest amount.
		error NotEnoughEther();

		constructor() payable {
        richest = msg.sender;
        mostSent = msg.value;
    }

		function becomeRichest() public payable {
			if (msg.value <= mostSent) revert NotEnoughEther();
        pendingWithdrawals[richest] += msg.value;
        richest = msg.sender;
        mostSent = msg.value;
    }

		function withdraw() public {
        uint amount = pendingWithdrawals[msg.sender];
				// Remember to zero the pending refund before
				// sending to prevent re-entrancy attacks
				pendingWithdrawals[msg.sender] = 0;
				payable(msg.sender).transfer(amount);
    }
}
contract Attacker {
    function attack(SendContract kingOfEther) public payable{
        kingOfEther.becomeRichest{value:msg.value}();
    }
}
```

Trong mô hình `withdrawal pattern` đã tách biệt việc trở thành người giàu nhất trong hợp đồng và rút tiền khi không còn trở thành người giàu nhất nữa. Khi attacker muốn tấn công cũng không làm ảnh hưởng tới hàm `becomeRichest()` hơn nữa điểm quan trọng ở đây đó là

`mapping (address => uint) pendingWithdrawals;` sẽ lưu trữ thông tin của những `King of Ether cũ`

do đó nếu attacker thực hiện tấn công vào hàm `withdraw()` sẽ chỉ bị ảnh hưởng bởi attacker vì

`payable(msg.sender).transfer(amount);` là thực hiện rút tiền thông qua account gọi hàm chứ không phụ thuộc vào `King of Ether cũ`(khác với mô hình rút tiền khi không sử dụng withdrawal pattern).
