# Spread and rest
## Spread
Toán tử `Spread` đã được giới thiệu trong JavaScript với ES 6 , bổ sung thêm một số tính năng tuyệt vời giúp làm việc với mảng và tham số hàm dễ dàng hơn. Một tính năng như vậy được giới thiệu với ES 6 ( ECMAScript là một tiêu chuẩn JavaScript nhằm đảm bảo khả năng tương tác của các trang web trên các trình duyệt web khác nhau ) là việc bổ sung toán tử `Spread`. 
```js
const a = [1,2,3];
const b = [3,4,5];
const c = [...a , ...b];
console.log(c); // [1,2,3,4,5,6]
```
## Rest 
Toán tử `Rest` trong Javascript cũng được giới thiệu trong JavaScript với ES 6 đều sử dụng ba dấu chấm (…). Chúng ta hãy xem một ví dụ để thấy vấn đề mà toán tử còn lại giúp đơn giản hóa.
```js
function multipleArg() {
	let args = Array.from(arguments);
	let finalArray = args.map(ele => ele * 2);
	console.log(finalArray);
}
multipleArg(1, 2);
multipleArg(4, 2, 4);
```
Đây là cách chúng ta viết hàm trước khi giới thiệu ES 6 vì đối tượng đối số không phải là một mảng và trước tiên chúng ta phải chuyển đổi nó thành một mảng bằng phương thức Array.from rồi sử dụng nó. Mặc dù phương pháp này đúng và hiệu quả nhưng phức tạp hơn rất nhiều. Toán tử còn lại trong JavaScript cung cấp một cách để đạt được kết quả tương tự theo cách rõ ràng hơn và dễ dàng hơn nhiều khi làm việc với các tham số không xác định. Hãy xem làm thế nào có thể đạt được kết quả tương tự với các tham số còn lại.
```js
function multipleArg(... args) {
	let finalArray = args.map(ele => ele * 2);
	console.log(finalArray);
}
multipleArg(1, 2);//[2, 4]
multipleArg(4, 2, 4);//[8, 4, 8]
```
Ở đây, với sự trợ giúp của các tham số còn lại, giống như toán tử `Rest`, sử dụng ba dấu chấm (…) , chúng ta có thể truyền các tham số không xác định cho một hàm. Ví dụ: tất cả các tham số trong đoạn mã trên đều có sẵn bên trong hàm trong args dưới dạng một mảng. Phương pháp sử dụng toán tử còn lại này giúp mã của chúng ta dễ đọc hơn và loại bỏ các dòng không cần thiết trong mã.
## Sự khác biệt giữa các toán tử spread and rest
Mặc dù cả hai toán tử trong JavaScript đều có cú pháp giống hệt nhau, tức là ba dấu chấm (…) , nhưng các toán tử này không giống nhau và thực hiện các tác vụ khác nhau.  
- `Spread` dùng để trải hết các phần tử trong array hoặc object ra 
- `Rest` dùng để gộp hết các phần tử lại thành một mảng 