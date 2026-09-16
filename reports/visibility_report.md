# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 17.0 khớp có v > 0 mỗi người
- Tổng: v=2 321 | v=1 155 | v=0 0

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 7 | 0 | 25% |
| 1 | left_eye | 20 | 8 | 0 | 29% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 15 | 13 | 0 | 46% |
| 4 | right_ear | 21 | 7 | 0 | 25% |
| 5 | left_shoulder | 26 | 2 | 0 | 7% |
| 6 | right_shoulder | 28 | 0 | 0 | 0% |
| 7 | left_elbow | 21 | 7 | 0 | 25% |
| 8 | right_elbow | 23 | 5 | 0 | 18% |
| 9 | left_wrist | 18 | 10 | 0 | 36% |
| 10 | right_wrist | 19 | 9 | 0 | 32% |
| 11 | left_hip | 18 | 10 | 0 | 36% |
| 12 | right_hip | 19 | 9 | 0 | 32% |
| 13 | left_knee | 14 | 14 | 0 | 50% |
| 14 | right_knee | 11 | 17 | 0 | 61% |
| 15 | left_ankle | 14 | 14 | 0 | 50% |
| 16 | right_ankle | 13 | 15 | 0 | 54% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
