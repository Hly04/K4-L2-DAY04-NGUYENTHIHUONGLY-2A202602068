# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.21 khớp có v > 0 mỗi người
- Tổng: v=2 325 | v=1 145 | v=0 23

So sánh với `tri_lab\dataset\labels\train` (29 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 2 | right_eye | 31% | 10% | 21 |
| 0 | nose | 28% | 10% | 17 |
| 1 | left_eye | 34% | 21% | 14 |
| 4 | right_ear | 55% | 45% | 10 |
| 11 | left_hip | 38% | 45% | 7 |
| 5 | left_shoulder | 17% | 10% | 7 |
| 13 | left_knee | 31% | 24% | 7 |
| 15 | left_ankle | 14% | 17% | 3 |
| 6 | right_shoulder | 3% | 7% | 3 |
| 16 | right_ankle | 21% | 24% | 3 |
| 3 | left_ear | 62% | 62% | 0 |
| 7 | left_elbow | 21% | 21% | 0 |
| 8 | right_elbow | 14% | 14% | 0 |
| 9 | left_wrist | 34% | 34% | 0 |
| 10 | right_wrist | 31% | 31% | 0 |
| 12 | right_hip | 38% | 38% | 0 |
| 14 | right_knee | 28% | 28% | 0 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
