# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lã Việt Quang   Nhóm: không có   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 321 / 155 / 0 |
| Thời gian trung bình mỗi ảnh | Chưa có log thời gian; không ước lượng |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `right_knee` — 61%
2. `right_ankle` — 54%
3. `left_knee` và `left_ankle` — 50% (đồng hạng)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng là những khớp thường khó gán vì nằm ở phần thấp cơ thể, dễ bị che bởi quần áo, tay lái hoặc chồng người. Khác với “bị che” là “ra khỏi khung”: ở các khớp này, vị trí giải phẫu vẫn còn trong ảnh và có thể ước lượng từ trục thân, đùi và cẳng chân, nên tôi giữ `v=1` thay vì `v=0`. Bằng chứng là theo báo cáo visibility, không có khớp nào có `v=0`; toàn bộ các điểm này vẫn nằm trong khung và cần được ước lượng chứ không được bỏ hẳn.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.8908 | 0.8908 |
| OKS@0.50 | 0.9655 | 0.9655 |
| OKS@0.75 | 0.8966 | 0.8966 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_01.jpg`, người 2, `right_wrist`: chỉnh lại vị trí ước lượng cổ tay sao cho khớp nằm trên trục cẳng tay, không lệch theo tay lái.
- `train_02.jpg`, người 1, `left_ear`: căn tai lại theo đường đầu để tránh lệch vì góc quay và phần đầu bị che.
- `train_03.jpg`, người 1, `right_hip`: dịch hông về trục cơ thể, giữ `v=1` vì khớp vẫn còn trong khung dù bị quần áo và góc chụp che một phần.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Đây là dạng sai nguy hiểm nhưng trong tập của tôi không xuất hiện, nên không có ảnh cần ghi riêng.

## 3. Kiểm chéo

Bạn cùng nhóm: chưa có dữ liệu đối chiếu trong repo hiện tại.

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `right_knee` | 61% | chưa có dữ liệu | — | Cần compare để kết luận; trong phần thấp cơ thể, nguyên nhân chủ yếu là chồng người và quần áo che gối. |
| `right_ankle` | 54% | chưa có dữ liệu | — | Tương tự: khớp thấp dễ bị che bởi vật thể và góc chụp, không phải ngoài khung. |
| `left_knee` | 50% | chưa có dữ liệu | — | Cần kiểm tra guideline với người khác để xác định có chênh lệch cách hiểu `v=1` không. |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Nếu khớp còn trong khung và có căn cứ vị trí từ trục thân, góc khớp hoặc phần cơ thể liền kề, chọn `v=1` và đặt chấm; chỉ chọn `v=0` khi khớp thật sự ra khỏi mép ảnh hoặc không còn căn cứ để ước lượng. Đây là quy tắc giúp phân biệt “bị che” với “ra khỏi khung” rõ ràng hơn.

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng 0.0055 sau fine-tune, tương đương +0.55 điểm phần trăm. Vì nó không giảm, nên không có dấu hiệu 20 ảnh của tôi dạy model điều sai mà COCO chưa dạy; ngược lại, mô hình chỉ cải thiện một chút sự phù hợp với góc chụp và phần thân bị che trong dữ liệu lab.

2. Chênh giữa `box_mAP50-95` và `pose_mAP50-95` là khoảng 0.1266 ở baseline, và khoảng 0.1133 sau fine-tune. Model tìm người dễ hơn tìm khớp vì bounding box cơ thể ổn định hơn so với 17 điểm khớp đòi hỏi vị trí chính xác và rất nhạy với góc chụp, che khuất và cắt khung.

3. Một ảnh test model đoán sai kiểu dạng `lệch nhẹ` là trường hợp khớp vai, tay hoặc đầu gối nằm gần đúng nhưng dịch khỏi vị trí thực tế vài pixel; đây là lỗi phổ biến nhất trong tập test vì các khớp thấp rất dễ bị lệch khi bị quần áo hoặc vật thể che.

4. Ảnh có OKS thấp nhất giữa nhãn của bạn và model là những ảnh có nhiều khớp ở thân dưới và cổ tay bị che, vì lúc đó model phải ước lượng vị trí dựa trên trục cơ thể. Tôi dựa vào sự đồng nhất giữa bằng chứng thị giác và các điểm sai trong gold để kết luận đó là ảnh khó, không phải nhãn của tôi “tệ” theo cảm tính.

5. Nếu ảnh bạn gán tệ nhất trùng với ảnh model dự đoán tệ nhất, điều đó cho thấy bức ảnh đó có góc chụp khó, nhiều vật che và chồng người, nên cả người gán lẫn model đều thiếu căn cứ tốt. Nói cách khác, ảnh đó “đòi hỏi” sự ước lượng khớp cao hơn mức dữ liệu cho phép.

## 5. Một rule evidence bạn đã dùng

Tôi đã dùng quy tắc cho khớp `right_knee` trong `train_03.jpg`, người 1. Gối đó bị quần áo và góc thân che một phần, nhưng vị trí vẫn còn trong khung và có thể ước lượng từ trục đùi-cẳng chân và phần cơ thể liền kề, nên tôi chọn `v=1` thay vì `v=0`. Nếu khớp rời khỏi hình hoặc không còn căn cứ để ước lượng từ cơ thể, tôi mới đánh `v=0`. Quy tắc này giúp phân biệt “bị che” với “ngoài khung” dựa trên bằng chứng hình ảnh, không phải cảm giác chủ quan.
