# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.19 khớp có v > 0 mỗi người
- Tổng: v=2 309 | v=1 128 | v=0 22

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 6 | 0 | 22% |
| 1 | left_eye | 19 | 8 | 0 | 30% |
| 2 | right_eye | 20 | 7 | 0 | 26% |
| 3 | left_ear | 11 | 16 | 0 | 59% |
| 4 | right_ear | 12 | 15 | 0 | 56% |
| 5 | left_shoulder | 24 | 3 | 0 | 11% |
| 6 | right_shoulder | 26 | 1 | 0 | 4% |
| 7 | left_elbow | 23 | 4 | 0 | 15% |
| 8 | right_elbow | 23 | 4 | 0 | 15% |
| 9 | left_wrist | 18 | 9 | 0 | 33% |
| 10 | right_wrist | 17 | 9 | 1 | 33% |
| 11 | left_hip | 17 | 10 | 0 | 37% |
| 12 | right_hip | 16 | 11 | 0 | 41% |
| 13 | left_knee | 16 | 8 | 3 | 30% |
| 14 | right_knee | 18 | 7 | 2 | 26% |
| 15 | left_ankle | 15 | 4 | 8 | 15% |
| 16 | right_ankle | 13 | 6 | 8 | 22% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
