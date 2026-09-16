# Báo cáo Ngày 4 - Keypoint & Pose
Họ tên: Nguyễn Thị Hương Ly | Nhóm: Lab Day 4 | Ngày: 16/09/2026

Cách dùng: copy file này thành reports/REPORT.md. Điền bằng số liệu do công cụ sinh ra; không tự ước lượng hoặc sửa số trong file JSON.

---

### 1. Nhãn của tôi

| Chỉ số | Giá trị |
| :--- | :--- |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 325 / 145 / 23 |
| Thời gian trung bình mỗi ảnh | ~4.5 phút / ảnh |

**Ba khớp có %v=1 cao nhất (chép từ reports/visibility_report.md):**
1. `left_ear` (62%)
2. `right_ear` (55%)
3. `left_hip` / `right_hip` (38%)

**Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:**
Đúng với thực tế khi gán nhãn. Hai tai là khớp có tỉ lệ bị che cao nhất do người trong ảnh phần lớn đội mũ bảo hiểm hoặc có tóc che phủ. Khớp hông lại mang một dạng khó khác: người mặc trang phục dài/rộng khiến mốc xương chậu bị che khuất hoàn toàn, buộc phải ước lượng vị trí giải phẫu dựa trên trục cơ thể từ vai hạ xuống và đáy thắt lưng thay vì nhìn thấy trực tiếp.

---

### 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| :--- | :--- | :--- |
| OKS trung bình | 0.935 | 0.939 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 0.966 | 1.000 |
| Lỗi dao_trai_phai | 0 | 0 |
| Lỗi nham_nguoi | 0 | 0 |
| Lỗi xoa_khop_bi_che | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):**
* Ảnh `train_13.jpg`, người thứ 1: Căn chỉnh lại khớp khuỷu tay và khớp cổ chân từ việc bám theo mép ngoài nếp gấp quần áo về đúng tâm giải phẫu trục xương. Lần chấm trước đạt OKS 0.741 (cảnh báo lệch nhẹ duy nhất của toàn bài), sau khi sửa đã đưa OKS@0.75 đạt mức tuyệt đối 1.000 (0 skeleton cần rework).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào? Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?**
Trong bảng chấm với Gold, số lỗi đảo trái/phải thực tế là 0. Tuy nhiên công cụ `check_pose_labels.py` có đưa ra 4 cảnh báo đảo bên ở `train_02` và `train_16`. Đây là các ảnh khó do người chụp ở tư thế quay nghiêng người và vặn mình, làm vector nối hai mắt và vector nối hai vai bị ngược hướng hình học trên ảnh 2D. Kết quả kiểm tra trực quan trên ảnh skeleton xác nhận khung xương không bị cắt chéo và đối chiếu Gold đạt OKS 0.939 xác nhận nhãn hoàn toàn đúng giải phẫu cơ thể.

---

### 3. Kiểm chéo
Bạn cùng nhóm: Lê Minh Trí (https://github.com/TriLe1016/K4-L2-DAY04-LeMinhTri-2A202602084-KeypointPose)

**Khớp lệch %v=1 nhiều nhất giữa hai bảng đếm:**

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| :--- | :---: | :---: | :---: | :--- |
| `right_eye` | 31% | 10% | 21% | Khác biệt nhận định khi mặt chụp nghiêng: bên tôi gán v=1 khi mắt bị sống mũi/góc mặt che một phần, bên Trí coi như nhìn thấy (v=2). |
| `nose` | 28% | 10% | 17% | Khác biệt đánh giá mức độ che khuất của bóng râm và góc nghiêng đầu. |
| `left_eye` | 34% | 21% | 14% | Khác biệt tiêu chí đánh giá mắt ở góc chụp bán diện. |

**Luật mới đã bổ sung vào GUIDELINE_MINI.md sau khi thống nhất:**
Ngũ quan vùng mặt (mắt, mũi) ở các góc chụp nghiêng nếu vẫn quan sát được một phần con ngươi hoặc sống mũi thì thống nhất giữ v=2; chỉ chuyển sang cờ v=1 khi bị vật cản ngoại cảnh (kính râm, tóc, khẩu trang, mũ) che khuất hoàn toàn.

---

### 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| :--- | :--- | :--- | :--- |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

#### Trả lời năm câu hỏi ở cuối notebook

**1. pose_mAP50-95 thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
`pose_mAP50-95` tăng nhẹ **+0.0055** (từ 0.6853 lên 0.6908), đồng thời `pose_precision` tăng **+0.0058**. Mức tăng này cho thấy 20 ảnh bổ sung không làm hỏng trọng số chung của COCO mà tinh chỉnh nhẹ khả năng bắt khớp ở tư thế sinh hoạt/làm việc có độ che khuất đặc thù; tuy nhiên do tập dữ liệu chỉ có 20 ảnh nên mức thay đổi không quá lớn.

**2. box_mAP và pose_mAP chênh nhau bao nhiêu? Model tìm người dễ hơn hay tìm khớp dễ hơn? Vì sao?**
Ở bản gốc, `box_mAP50-95` đạt 0.8119 trong khi `pose_mAP50-95` chỉ đạt 0.6853 (chênh ~0.127, box cao hơn hẳn). Model tìm người (box) **dễ hơn** tìm khớp (pose) rất nhiều, vì bounding box chỉ cần bao quanh diện mạo tổng quát của cơ thể, còn tìm khớp đòi hỏi định vị tọa độ pixel chính xác của từng điểm giải phẫu nhỏ và rất dễ sai sót khi chi bị gấp khúc, che khuất hoặc xoay chiều.

**3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**
Tại `train_10` (model đoán 2 / bạn 1) và `train_03` (model đoán 4 / bạn 2), model gặp lỗi ở tầng phát hiện khi nhận nhầm bóng người mờ hoặc vật thể nền thành người thật. Ở cấp độ khớp, xuất hiện lỗi **lệch nhẹ** tại các khớp cổ chân/cổ tay ở người ở xa góc máy và lỗi **trượt hẳn** (miss detection) ở các khớp bị đồ vật che khuất hoàn toàn.

**4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
Ảnh có OKS thấp nhất là `train_13` (OKS = 0.629). **Nhãn của tôi đúng**. Căn cứ vào việc khi đối chiếu với bộ nhãn chuẩn (`gold`), toàn bộ skeleton trong `train_13` sau rework đều đạt OKS > 0.90 và OKS trung bình toàn bộ bài đạt 0.939. Model đoán kém vì ảnh này có người ở xa, độ phân giải thấp và tư thế xoay nghiêng phức tạp.

**5. Ảnh bạn gán tệ nhất có cũng là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
Đúng, trùng nhau hoàn toàn. Trước khi rework, `train_13` là ảnh tôi có OKS thấp nhất so với Gold (0.741) và cũng chính là ảnh model có OKS thấp nhất (0.629). Sự trùng hợp từ hai phép so sánh độc lập khẳng định `train_13` là một **hard case khách quan** của tập dữ liệu (ảnh mờ, góc chụp xiên, chi bị co ngắn) gây khó khăn cho cả người gán lẫn mô hình học máy.

---

### 5. Một rule evidence bạn đã dùng

Tại ảnh `train_04.jpg`, người thứ 1: Bốn khớp chi dưới (`left_knee`, `right_knee`, `left_ankle`, `right_ankle`) bị vật cản phía trước che khuất hoàn toàn. Bằng chứng thị giác nhìn thấy là thân trên và đầu của người này nằm trọn vẹn ở trung tâm khung hình, các khớp không chạm hay vượt ra ngoài mép ảnh. Do đó, theo quy tắc biên ảnh, tôi quyết định chọn trạng thái **v=1 (Occluded)** và đặt chấm tại vị trí ước lượng dưới vật cản, thay vì chọn v=0 (Outside). Việc giữ v=1 giúp mô hình học được mối quan hệ không gian đầy đủ của khung xương ngay cả khi có vật cản che chắn.