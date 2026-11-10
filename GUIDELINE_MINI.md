# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con dùng để chở người, gồm sedan, hatchback, SUV, taxi và xe bán tải. | Xe bán tải hoặc xe cabin nhỏ vẫn thuộc `car`; không gán xe buýt, xe tải hay mô tô vào lớp này. |
| `truck` | Xe tải chuyên chở hàng hóa hoặc xe đầu kéo. | Xe ben, xe tải thùng và xe đầu kéo; phân biệt với `car` vì xe bán tải/cabin nhỏ không thuộc `truck`. |
| `bus` | Xe buýt hoặc minibus dùng để chở nhiều hành khách. | Xe buýt đô thị, xe khách, xe buýt trường học; phân biệt với `van` nhờ công năng chở khách, thân lớn và nhiều hàng ghế. |
| `van` | Xe van hoặc minivan dùng để chở hàng hay chở người. | Xe giao hàng thân kín, minivan; không gán xe buýt lớn hoặc xe con thông thường vào lớp này. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật nhỏ nhất bao trọn phần nhìn thấy của xe, không cắt vào vật thể và hạn chế tối đa nền thừa; không tính bóng đổ vào box.
- Vật thể bị che khuất: vẫn gán nhãn nếu còn đủ đặc điểm để xác định chắc chắn lớp; box bao phần nhìn thấy và chỉ ước lượng phần bị che khi có thể suy ra rõ ràng ranh giới của toàn bộ xe.
- Vật thể bị cắt bởi rìa ảnh: vẫn gán nhãn nếu nhận diện được lớp; box bám phần nhìn thấy và kéo đến đúng rìa ảnh, không ước lượng ra ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: chỉ gán nhãn khi vẫn phân biệt chắc chắn được loại xe và có thể vẽ box hợp lý; bỏ qua nếu chỉ thấy vài pixel, quá nhòe hoặc không thể phân biệt giữa các lớp.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002.jpg` | Vật thể có kích thước nhỏ, hình ảnh mờ khiến việc xác định phương tiện và phân loại vào 4 class gặp khó khăn. | Tạm thời chưa gán nhãn, cần kiểm tra ảnh gốc ở độ phóng đại cao hơn. Nếu vẫn không xác định được, escalation lên Lab Coach. | Schema yêu cầu gán đúng một trong bốn lớp car, truck, bus, van. Khi thiếu đặc điểm nhận diện đáng tin cậy, không được tự suy đoán class vì có thể tạo nhãn sai và ảnh hưởng tính nhất quán của Ground Truth. |
| `drive_003.jpg` |Vật thể bị khuất viền, khiến khó phân loại vào 4 class |Tạm thời chưa gán nhãn, Nếu vẫn không xác định được, escalation lên Lab Coach.  |Schema yêu cầu gán đúng một trong bốn lớp car, truck, bus, van. Khi thiếu đặc điểm nhận diện đáng tin cậy, không được tự suy đoán class vì có thể tạo nhãn sai và ảnh hưởng tính nhất quán của Ground Truth. |
| `drive_039.jpg` | | | | |Vật thể bị khuất viền, khiến khó phân loại vào 4 class |Tạm thời chưa gán nhãn, Nếu vẫn không xác định được, escalation lên Lab Coach.  |Schema yêu cầu gán đúng một trong bốn lớp car, truck, bus, van. Khi thiếu đặc điểm nhận diện đáng tin cậy, không được tự suy đoán class vì có thể tạo nhãn sai và ảnh hưởng tính nhất quán của Ground Truth. |

