# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Lê Minh Trí | Người kiểm: Nguyễn Thị Hương Ly | Ngày: 16/09/2026

Chạy trước khi soi bằng mắt:
```bash
python tools/check_pose_labels.py --images dataset/images/train --labels tri_lab/dataset/labels/train
python tools/visualize_pose.py --images dataset/images/train --labels tri_lab/dataset/labels/train --out outputs/vis_review_tri
python tools/visibility_report.py --labels dataset/labels/train --compare tri_lab/dataset/labels/train
```

---

### Bảng kiểm tra chất lượng

| Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| :--- | :---: | :--- |
| 1. Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đã bổ sung 2 người ở train_13 (người #1 và #2); đủ 20 ảnh, 29 skeleton |
| 2. Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Đã sửa đảo trái/phải ở train_02 và train_06; còn nghi vấn nhẹ ở train_13 người #2 |
| 3. Không có xương nào kéo dài sang một cơ thể khác | ☑ | Đã sửa ca nhầm người ở train_03 người #1 (khớp left_hip bị dính sang người bên cạnh) |
| 4. Khớp bị che dùng v = 1 và có chấm, không phải v = 0 | ☑ | Đạt chuẩn: tổng 128 điểm v=1; các khớp khuất bởi mũ bảo hiểm/xe đều đặt chấm ước lượng |
| 5. v = 0 chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Đạt chuẩn: tổng 21 điểm v=0, chỉ xuất hiện ở các vị trí ra ngoài viền ảnh |
| 6. Không có dấu hiệu dùng Hidden (điểm v = 2 nằm ở chỗ vô lý) | ☑ | Tổng v=2 là 344 điểm; không dùng nhầm phím h |
| 7. Export đúng COCO Keypoints 1.0: mảng keypoints có 51 số mỗi người | ☑ | Đạt chuẩn định dạng COCO Keypoints 1.0 |
| 8. Bản YOLO Pose: mỗi dòng 56 số, kpt_shape: [17, 3] | ☑ | Đạt chuẩn định dạng YOLO Pose |
| 9. Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | Đã đối chiếu; 7 khớp lệch bằng 0%, các khớp chi đồng thuận cao, lệch tập trung ở vùng mặt |
| 10. Mọi ca không rõ đều được ghi trong GUIDELINE_MINI.md | ☑ | Đã ghi nhận rõ quy ước tai bị che, hông người mặc quần áo dài |
| 11. check_pose_labels.py chạy 0 lỗi | ☑ | Đạt chuẩn định dạng; OKS@0.75 sau rework đạt 1.000 |

---

### Lỗi tìm được

Chép sang reports/review_partner.md. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| :--- | :--- | :--- | :--- | :--- |
| train_13.jpg | 2 | left/right_shoulder, left/right_hip | Nghi vấn đảo trái/phải do tool cảnh báo vai/hông ngược chiều với hai mắt trên bóng người nhỏ, mờ ở rìa ảnh | Mở visualize_pose.py phóng to góc rìa để xác định lại hướng quay; đảo lại cặp điểm trái/phải nếu đường nối bị chéo |
| train_06.jpg | 1 | left_elbow, left_wrist | Lệch nặng (>130px) do bị che bởi xe và balo, kèm đảo trái/phải toàn bộ skeleton | Đổi lại chiều trái/phải và kéo cổ tay/khuỷu tay về đúng trục giải phẫu (Trí đã khắc phục ở bản sau rework) |
| train_03.jpg | 1 | left_hip | Nhầm người: khớp hông bị gán dính sang thân người bên cạnh | Kéo điểm hông về đúng vị trí xương chậu của người #1 (Trí đã khắc phục ở bản sau rework) |
| train_13.jpg | 1 và 2 | Toàn bộ 17 khớp | Thiếu người: bỏ sót không gán 2 người trong ảnh | Bổ sung đầy đủ 17 điểm cho cả hai người (Trí đã khắc phục ở bản sau rework) |

---

### Hai câu kết luận

* **Lỗi lặp đi lặp lại nhiều nhất của bài này:** Lỗi xác định nhầm chiều trái/phải trên các bức ảnh chụp người ở tư thế nghiêng hoặc bị che khuất thân thể (train_02, train_06, train_13).
* **Nó là lỗi thao tác hay lỗi guideline chưa rõ?** Đây là lỗi thao tác khi làm nhanh trên các góc chụp nghiêng, người gán nhìn theo hệ quy chiếu 2D của ảnh thay vì đặt mình vào hướng giải phẫu của cơ thể người.