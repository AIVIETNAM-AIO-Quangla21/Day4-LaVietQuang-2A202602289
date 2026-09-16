# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Lã Việt Quang   Người kiểm: Lã Việt Quang   Ngày: 16/09/2026

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels <bài của họ>
python3 tools/visualize_pose.py --images dataset/images/train --labels <bài của họ> --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare <bài của họ>
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | 20 ảnh, 28 skeleton; trung bình 17.0 khớp/người. |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☐ | Cần rà soát `train_02`, `train_06`, `train_16`; checker cảnh báo trái/phải ở vai/hông. |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Đã xem các ảnh visualization; chưa thấy skeleton nối sang người khác. |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | Báo cáo có 155 khớp `v=1`; mọi dòng vẫn đủ 17 keypoint. |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Có 0 khớp `v=0`; không phát hiện dùng Outside cho khớp bị che. |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Không có cờ `v` ngoài 0/1/2; cần giữ quy tắc không dùng `Hidden` khi chỉnh sửa. |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | 28 annotation, mỗi mảng `keypoints` dài 51 số. |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | 20 file nhãn, mọi dòng 56 số; `data.yaml` dùng `[17, 3]`. |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☐ | Đã có `reports/visibility_report.md` và JSON; chưa có bảng của bạn cùng nhóm để compare. |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Đã ghi 3 ca: trái/phải ở `train_02`, chồng lấn ở `train_03`, tai bị mũ che ở `train_20`. |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | Đạt định dạng, 0 lỗi chặn nộp; còn 6 cảnh báo cần rà soát trái/phải. |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_02` | 1 | `left_shoulder/right_shoulder` | Checker cảnh báo hai vai có thể đảo trái/phải so với mắt | Mở visualization, xác định trái/phải theo cơ thể rồi kéo/đổi lại hai điểm nếu cần. |
| `train_02` | 1 | `left_hip/right_hip` | Checker cảnh báo hai hông có thể đảo trái/phải | Kiểm tra theo trục cơ thể và sửa cặp hông nếu xác nhận bị đảo. |
| `train_06` | 1 | `left_shoulder/right_shoulder` | Checker cảnh báo hai vai có thể đảo trái/phải so với mắt | Đối chiếu vai với hướng mặt và sửa cặp điểm nếu cần. |
| `train_06` | 1 | `left_hip/right_hip` | Checker cảnh báo hai hông có thể đảo trái/phải | Đối chiếu hông với cùng người, không theo phía ảnh, rồi sửa nếu cần. |
| `train_16` | 2 | `left_shoulder/right_shoulder` | Checker cảnh báo hai vai có thể đảo trái/phải so với mắt | Mở đúng skeleton người thứ 2, xác định trái/phải theo cơ thể rồi sửa. |
| `train_16` | 2 | `left_hip/right_hip` | Checker cảnh báo hai hông có thể đảo trái/phải | Kiểm tra lại cặp hông của người thứ 2 và sửa nếu xác nhận đảo. |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Cảnh báo đảo trái/phải ở các cặp vai và hông của `train_02`, `train_06`, `train_16`.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Chủ yếu là lỗi thao tác/đối chiếu trái-phải; guideline cần nhắc lại cách xác định theo cơ thể để ngăn lặp lại.
