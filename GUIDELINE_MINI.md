# Mini guideline - nhóm: 1 | người gán: Lã Việt Quang | ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Đặt hông theo vị trí giải phẫu ước lượng ở hai bên thân, ngay cả khi quần áo dài hoặc vật thể che; còn trong ảnh thì đặt chấm và dùng `v = 1` nếu bị che. [Ảnh mẫu](outputs/vis_train/train_03.jpg) | Hông là khớp bắt buộc, không được xoá chỉ vì không thấy bề mặt. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Ước lượng vị trí tai dưới phần che, vẫn đặt chấm và dùng `v = 1`; chỉ dùng `v = 0` khi tai nằm ngoài mép ảnh. [Ảnh mẫu](outputs/vis_train/train_20.jpg) | Phân biệt che khuất với ngoài khung để không làm mất keypoint. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Gán người nếu phần thân còn đủ để nhận diện; các khớp nằm ngoài mép ảnh không đặt chấm và dùng `v = 0`, các khớp còn trong ảnh vẫn phải đủ 17 điểm. [Ảnh mẫu](outputs/vis_train/train_01.jpg) | Giữ đủ skeleton và chỉ loại các khớp thật sự ngoài khung. |
| Cổ tay nằm sau tay lái / sau thân mình | Đặt cổ tay theo hướng cẳng tay và vị trí giải phẫu ước lượng, đánh `v = 1` nếu còn trong ảnh. [Ảnh mẫu](outputs/vis_train/train_02.jpg) | Tay lái che cổ tay nhưng không làm cổ tay biến mất khỏi ảnh. |
| Hai người chồng lên nhau | Hoàn thành toàn bộ 17 điểm của người thứ nhất rồi mới sang người thứ hai; theo dõi từng skeleton và trái/phải theo cơ thể, không theo phía ảnh. [Ảnh mẫu](outputs/vis_train/train_03.jpg) | Tránh nối điểm sang người bên cạnh và tránh nhầm người. |
| Người nhỏ đến mức nào thì không gán nữa | Không đặt ngưỡng bỏ người trong bộ dữ liệu này: mọi người nhìn thấy và nhận diện được đều phải gán đủ 17 điểm. Nếu người quá nhỏ hoặc mơ hồ, ghi ca đó vào guideline để thống nhất, không tự bỏ. [Ảnh mẫu](outputs/vis_train/train_01.jpg) | Bộ dữ liệu đã được chọn để gán toàn bộ người; xoá skeleton làm mất độ bao phủ. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02`, người thứ `1`, khớp `left/right_shoulder`, `left/right_hip`

- Mơ hồ ở chỗ nào: Người đi xe đạp quay lệch, tay lái và tư thế làm vị trí hai vai/hông dễ bị đọc theo phía bức ảnh.
- Bạn quyết thế nào: Gán trái/phải theo cơ thể người; kiểm tra lại trục mắt, vai và hông trước khi lưu. Cảnh báo checker hiện tại yêu cầu mở ảnh để rà soát lại.
- Vì sao: COCO quy ước trái/phải theo người được gán, không theo trái/phải trên ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học hoán đổi trái/phải ở vai và hông, sau đó lỗi bị lặp lại khi augmentation lật ảnh.

### Ca 2 - ảnh `train_03`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Hai người và xe đạp chồng lên nhau; cổ tay bị tay lái hoặc thân người che.
- Bạn quyết thế nào: Giữ cổ tay trong skeleton của đúng người, đặt tại vị trí ước lượng theo cẳng tay và dùng `v = 1`.
- Vì sao: Khớp vẫn nằm trong khung ảnh nhưng không nhìn rõ, nên là Occluded chứ không phải Outside.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học cổ tay ở sai người hoặc học rằng cổ tay bị che là không tồn tại.

### Ca 3 - ảnh `train_20`, người thứ `1`, khớp `left_ear/right_ear`

- Mơ hồ ở chỗ nào: Mũ bảo hiểm che một phần tai và khuôn mặt bị khuất bởi kính chắn gió.
- Bạn quyết thế nào: Ước lượng hai tai ở vị trí giải phẫu dưới mũ, vẫn đặt chấm và dùng `v = 1` cho tai bị che.
- Vì sao: Tai còn trong vùng ảnh; luật lớp yêu cầu không xoá keypoint bị che.
- Nếu người khác quyết ngược lại thì model học vị trí tai lệch theo mũ hoặc học tai không có khi người đội mũ bảo hiểm.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: Chưa xác định vì chưa có thư mục nhãn của bạn cùng nhóm; báo cáo hiện tại của nhóm mình có `left_knee`/`left_ankle` 50% và `right_knee` 61% nhưng chưa có bảng đối chiếu.
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Chưa thể kết luận khi chưa chạy `--compare`; riêng checker của bài này đang có cảnh báo cần rà soát trái/phải ở `train_02`, `train_06`, `train_16`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Chưa bổ sung; sẽ so sánh hai bảng trước, sau đó cập nhật luật về hông, cổ tay và tai nếu có chênh lệch.
