# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Nguyễn Nhật Minh
- MSSV: 2A202603210
- Email: nguyennhatminh140@gmail.com
- Lớp: 2A C304
- Link repo bài làm: <https://github.com/nminnnnn/K5-L2A-DAY02-NguyenNhatMinh-2A202603210-ObjectDetection>
- Link Colab đã dùng: <https://colab.research.google.com/github/nminnnnn/K5-L2A-DAY02-NguyenNhatMinh-2A202603210-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb>

## 2. Schema và mini guideline

Mini guideline của em nằm tại [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Bài có bốn lớp:

- `car = 0`: xe con, taxi, SUV và xe bán tải nhỏ.
- `truck = 1`: xe tải chở hàng, xe tải thùng hoặc xe đầu kéo.
- `bus = 2`: xe buýt và xe khách chở nhiều người.
- `van = 3`: xe van hoặc minivan.

Khi gán nhãn, em cố gắng vẽ box sát phần xe nhìn thấy và không lấy bóng đổ. Xe bị cắt ở rìa ảnh vẫn được gán nếu em nhận ra được lớp. Với xe quá nhỏ, quá mờ hoặc không đủ thông tin, em bỏ qua thay vì đoán.

Ba trường hợp em thấy khó:

1. `drive_002.jpg`: một số xe ở xa và mờ. Em phóng to ảnh để kiểm tra; nếu vẫn không nhận ra chắc chắn thì em không gán nhãn.
2. `drive_003.jpg`: có xe bị che hoặc bị cắt ở rìa ảnh. Nếu vẫn nhận ra loại xe, em gán box theo phần nhìn thấy; nếu không chắc lớp thì em bỏ qua.
3. `drive_039.jpg`: em chưa mô tả đầy đủ trường hợp này trong mini guideline và cần rà lại trước khi nộp bài.

Điểm em chưa chắc nhất là cách phân biệt `bus` và `van` trong một số ảnh. Khi đối chiếu kết quả Gold QC, em thấy có xe nhìn giống xe buýt nhưng nhãn đối chiếu lại là `van`. Em không sửa file gold và ghi lại vấn đề này để hỏi Lab Coach.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file, từ `drive_001.txt` đến `drive_050.txt`.
- Kết quả checker: `ĐẠT cấu trúc: 50/50 file nhãn hợp lệ`.
- Các ảnh em ưu tiên rà bằng mắt: `drive_001.jpg`, `drive_004.jpg`, `drive_009.jpg`, `drive_017.jpg`, `drive_018.jpg`, `drive_021.jpg`, `drive_028.jpg`, `drive_036.jpg`, `drive_042.jpg` và `drive_048.jpg`.
- Khi rà ảnh, em chú ý ba loại lỗi: bỏ sót xe, box chứa quá nhiều nền và gán nhầm lớp. Một số box do YOLOv7 tạo ban đầu cần được kiểm tra lại, đặc biệt với xe nhỏ và trường hợp dễ nhầm giữa `bus` với `van`.
- Commit khóa nhãn ban đầu: `9e369a2` (`not done guildline`). Commit này chứa đủ 50 file nhãn trước bước Gold QC.

Lần đầu chạy checker trên PowerShell, chương trình báo `UnicodeEncodeError` khi in tiếng Việt. Em chạy lại với `PYTHONIOENCODING=utf-8` và nhận được kết quả đạt. Đây là lỗi hiển thị của terminal, không phải lỗi định dạng nhãn.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 446 | Chưa rework |
| FP | 434 | Chưa rework |
| FN | 207 | Chưa rework |
| Precision | 0.5068 | Chưa rework |
| Recall | 0.6830 | Chưa rework |
| F1 | 0.5819 | Chưa rework |
| mean_iou_of_matches | 0.8500 | Chưa rework |
| passed | `false` | Chưa rework |

Kết quả đầy đủ nằm tại [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json). Mean IoU đạt `0.8500`, cao hơn yêu cầu `0.70`, nghĩa là những box đã ghép cặp nhìn chung có vị trí khá gần nhau. Tuy nhiên, F1 chỉ đạt `0.5819`, thấp hơn yêu cầu `0.80`, nên kết quả chung vẫn là `false`.

Em chưa hoàn thành một vòng rework đầy đủ. Em ưu tiên xem lại các ảnh có ít box match như `drive_042`, `drive_021`, `drive_018`, `drive_028`, `drive_009`, `drive_004`, `drive_041`, `drive_017`, `drive_036` và `drive_001`. Em hiểu rằng chỉ nhìn FP và FN thì chưa thể kết luận ngay lỗi do box, bỏ sót hay sai lớp; cần mở từng ảnh để kiểm tra.

**Lưu ý về gold dataset:** Qua kiểm tra trực quan, em thấy một số phương tiện nhìn rõ là `bus` nhưng gold lại dùng lớp `van`. Việc không thống nhất lớp này làm giảm số match và F1. Em không thay đổi gold để tăng điểm mà giữ lại ví dụ và báo Lab Coach xác nhận.

## 5. Kết quả YOLO

- Runtime: Google Colab, dùng CUDA GPU (`device=0`).
- Phiên bản Ultralytics, Torch và tên GPU: em không lưu lại output của phiên Colab cũ nên không còn thông tin chính xác.
- Tham số chính: `yolo26n.pt`, `epochs=50`, `imgsz=640`, `batch=-1`, `optimizer=auto`, `pretrained=true`, `seed=20260909`. Em không thay đổi các tham số chính so với notebook.
- Best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4091 |
| Recall | 0.2339 |
| mAP50 | 0.2707 |
| mAP50–95 | 0.1996 |

Các file kết quả gồm [10 ảnh dự đoán eval](assets/eval_predictions/visuals/), [kết quả train](assets/yolo26_runs/vehicle_annotation/) và [biểu đồ đánh giá](assets/yolo26_runs/vehicle_eval/). Em cũng lưu [confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png) và [đồ thị quá trình train](assets/yolo26_runs/vehicle_annotation/results.png).

Trong [`drive_060.jpg`](assets/eval_predictions/visuals/drive_060.jpg), model nhận ra xe buýt màu trắng ở gần phía trên ảnh là `bus` với confidence `0.88`. Trong [`drive_051.jpg`](assets/eval_predictions/visuals/drive_051.jpg), một xe con màu đỏ ở phía dưới ảnh lại bị dự đoán thành `van` với confidence `0.99`. Qua hai ảnh này, em thấy model đã nhận ra được một số xe nhưng vẫn còn nhầm lớp.

### Trả lời câu hỏi cuối notebook

1. **Lớp nào có recall thấp nhất?** Theo [confusion matrix chuẩn hóa](assets/yolo26_runs/vehicle_eval/confusion_matrix_normalized.png), `van` có recall thấp nhất, gần `0.00`. Nhiều đối tượng có nhãn thật là `van` bị model đoán thành `bus`. Trong [`drive_053.jpg`](assets/eval_predictions/visuals/drive_053.jpg), file nhãn eval có hai đối tượng lớp `van`, nhưng các xe lớn nhìn giống xe buýt lại được model đoán là `bus`. Em cho rằng ví dụ này cũng cần được Lab Coach kiểm tra lại cách phân biệt hai lớp.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp thì kiểm tra gì?** Em sẽ kiểm tra box có bám sát xe hay không. Box vẫn có thể đạt IoU `0.50` nhưng không đạt các mức IoU cao hơn nếu bị lệch, quá rộng, quá hẹp hoặc chứa nhiều nền.
3. **Lỗi class nào có thể liên quan guideline?** Lỗi rõ nhất là nhầm giữa `bus` và `van`. Confusion matrix cho thấy nhiều nhãn `van` bị đoán thành `bus`; đồng thời một số ảnh nhìn giống xe buýt nhưng file eval lại dùng lớp `van`. Ngoài ra, `drive_051.jpg` có xe con bị đoán thành `van`, nên cách phân biệt `car` và `van` cũng cần được mô tả rõ hơn. Em chưa thể khẳng định guideline là nguyên nhân duy nhất vì model còn được train từ tập dữ liệu nhỏ.
4. **Vì sao không dùng 10 ảnh eval để train?** Nếu dùng ảnh eval để train, model đã nhìn thấy dữ liệu được dùng để đánh giá nên kết quả sẽ không còn khách quan. Trong notebook, 10 ảnh này được dùng làm validation để tính precision, recall, mAP và chọn checkpoint `best.pt`; chúng không được dùng để cập nhật trọng số như ảnh train.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách em đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Chạy evaluator | Em chạy lệnh tại `D:\Vinuni` nên Python không tìm thấy file trong thư mục `tools`. | Em chuyển vào đúng thư mục repo rồi chạy lại. | Evaluator chạy được và tạo file JSON kết quả. |
| Chạy checker | PowerShell báo lỗi khi in tiếng Việt. | Em đặt `PYTHONIOENCODING=utf-8` rồi chạy lại. | Checker báo đạt 50/50 file. |
| Gold QC | Có trường hợp nhìn giống `bus` nhưng gold dùng lớp `van`; F1 chỉ đạt `0.5819`. | Em mở ảnh để đối chiếu, giữ nguyên gold và ghi lại vấn đề. | Còn cần Lab Coach xác nhận; em chưa hoàn thành rework. |
| Google Colab | Em mở notebook trực tiếp từ GitHub nên thay đổi và output không được giữ lại sau khi đóng. | Em tải ZIP kết quả về máy; về sau em dùng **Save a copy in Drive**. | Đã giữ được metric và ảnh kết quả, nhưng mất thông tin phiên bản thư viện của phiên cũ. |
| Viết báo cáo trong VS Code | VS Code giữ bản cũ và báo file trên ổ đĩa mới hơn. | Em không chọn Overwrite mà dùng **File: Revert File** để tải lại. | Không ghi đè mất nội dung mới. |

## 7. Tự đánh giá theo rubric

Đối chiếu với [RUBRIC.md](../../RUBRIC.md), em tự đánh giá như sau:

- Đã có đủ 50 file nhãn và checker đạt 50/50.
- Đã định nghĩa bốn lớp và viết quy tắc box trong mini guideline.
- Phần ba ca mơ hồ chưa hoàn chỉnh vì trường hợp `drive_039.jpg` còn thiếu quyết định cụ thể.
- Đã có file Gold QC, nhưng F1 `0.5819` chưa đạt yêu cầu `0.80`; em chưa hoàn thành vòng rework.
- Đã chạy YOLO26n trên Colab, giữ đúng 50 ảnh train và 10 ảnh eval, có metric và ảnh dự đoán.
- Đã tải hai thư mục `eval_predictions` và `yolo26_runs` vào báo cáo.

Vì vậy, em không tự nhận đã hoàn thành toàn bộ bài. Hai phần em cần làm rõ thêm là ca `drive_039.jpg` trong guideline và sự không thống nhất `bus/van` trong gold.

## 8. Công cụ AI và tài liệu tham khảo

- Em dùng CVAT và model YOLOv7 để tạo box ban đầu, sau đó kiểm tra lại toàn bộ nhãn, kéo box khít hơn, đồng thời chỉnh sửa lại những chỗ đánh nhãn sai.
- Em dùng Google Colab và Ultralytics YOLO26n để train và đánh giá model.
- Em dùng ChatGPT/Codex để giải thích lệnh PowerShell, lỗi CVAT/Colab, ý nghĩa các metric và hỗ trợ viết lại báo cáo. Em kiểm tra lại các số liệu bằng file JSON, log train và ảnh prediction trong repo.
- Tài liệu chính em sử dụng: [README.md](../../README.md), [GUIDE.md](../../GUIDE.md), [CHECKPOINTS.md](../../CHECKPOINTS.md), [RUBRIC.md](../../RUBRIC.md) và [SUBMISSION.md](../../SUBMISSION.md).
- Em không đưa mật khẩu, token, gold labels hoặc checkpoint `.pt` vào nội dung báo cáo.
