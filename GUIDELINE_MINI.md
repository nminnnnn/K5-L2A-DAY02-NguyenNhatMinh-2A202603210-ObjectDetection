# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |xe con, sedan, SUV, taxi, xe bán tải | Sedan, hatchback, coupe, SUV; phân biệt với `van` ở thân xe thấp/gọn hơn và không có khoang chở hàng lớn dạng hộp. |
| `truck` |xe tải chở hàng, xe đầu kéo | Xe ben, xe đầu kéo, xe tải thùng, xe bán tải; thường có cabin và khoang/thùng hàng tách biệt. |
| `bus` | xe buýt, minibus chở khách | Xe buýt đô thị, xe khách, xe buýt trường học; thân dài, nhiều hàng ghế và thường có nhiều cửa hoặc cửa lớn. |
| `van` | xe van, minivan chở hàng hoặc chở người | Minivan, xe giao hàng thân hộp; khoang cabin và khoang sau liền thân, nhỏ hơn `bus` và không có thùng hàng tách rời như `truck`. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật nhỏ nhất bao trọn phần nhìn thấy của xe, không cắt vào vật thể và hạn chế tối đa nền thừa; không tính bóng đổ vào box.
- Vật thể bị che khuất: vẫn gán nhãn nếu còn đủ đặc điểm để xác định chắc chắn lớp; box bao phần nhìn thấy và chỉ ước lượng phần bị che khi có thể suy ra rõ ràng ranh giới của toàn bộ xe.
- Vật thể bị cắt bởi rìa ảnh: vẫn gán nhãn nếu nhận diện được lớp; box bám phần nhìn thấy và kéo đến đúng rìa ảnh, không ước lượng ra ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: chỉ gán nhãn khi vẫn phân biệt chắc chắn được loại xe và có thể vẽ box hợp lý; bỏ qua nếu chỉ thấy vài pixel, quá nhòe hoặc không thể phân biệt giữa các lớp.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| `drive_002.jpg` | Vật thể có kích thước nhỏ, hình ảnh mờ khiến việc xác định phương tiện và phân loại vào 4 class gặp khó khăn. | Tạm thời chưa gán nhãn, cần kiểm tra ảnh gốc ở độ phóng đại cao hơn. Nếu vẫn không xác định được, escalation lên Lab Coach. | Schema yêu cầu gán đúng một trong bốn lớp car, truck, bus, van. Khi thiếu đặc điểm nhận diện đáng tin cậy, không được tự suy đoán class vì có thể tạo nhãn sai và ảnh hưởng tính nhất quán của Ground Truth. |
| ``drive_003.jpg` |Vật thể bị khuất viền, khiến khó phân loại vào 4 class |Tạm thời chưa gán nhãn, Nếu vẫn không xác định được, escalation lên Lab Coach.  |Schema yêu cầu gán đúng một trong bốn lớp car, truck, bus, van. Khi thiếu đặc điểm nhận diện đáng tin cậy, không được tự suy đoán class vì có thể tạo nhãn sai và ảnh hưởng tính nhất quán của Ground Truth. |
| `drive_...` | | | | 39
| `drive_...` | | | |
