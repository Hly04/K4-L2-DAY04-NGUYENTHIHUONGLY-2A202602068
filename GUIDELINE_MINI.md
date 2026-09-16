# Mini guideline - nhóm: Lab Day 4 | người gán: Nguyễn Thị Hương Ly | ngày: 16/09/2026

Điền file này trong lúc gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

---

### 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

* Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file .SVG chung.
* Mọi người trong ảnh đều có đủ 17 điểm. Điểm không dùng được thì gắn cờ, không xoá.
* Trái/phải tính theo cơ thể người, không theo bức ảnh.
* Bị che, còn trong khung -> v = 1, vẫn đặt chấm ở vị trí ước lượng.
* Ra ngoài mép ảnh -> v = 0, không đặt chấm.
* Không dùng Hidden (h) - nó không được lưu vào file.

---

### 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| :--- | :--- | :--- |
| **Hông của người mặc quần áo dài** | Ước lượng giải phẫu tại mào chậu/khớp háng, nằm ngang đáy thắt lưng và thẳng trục đứng từ vai xuống; gán cờ v = 1. | Quần áo rộng che khuất hoàn toàn bề mặt giải phẫu; cần mốc cố định để tránh lệch OKS khi so khớp. |
| **Tai bị tóc hoặc mũ bảo hiểm che một phần** | Bật cờ v = 1 (Occluded, phím `q`) và đặt chấm tại vị trí ước lượng đối xứng qua sống mũi/mắt. | Khớp vẫn nằm trong khung hình và thuộc phần đầu, bắt buộc giữ chấm và không được dùng v = 0. |
| **Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên)** | Các điểm từ hông trở xuống nằm ngoài biên ảnh đặt cờ v = 0 (Outside, phím `o`), tuyệt đối không đặt chấm. | Khớp đã hoàn toàn rơi ra ngoài không gian pixel của ảnh, tuân thủ đúng định nghĩa cờ Outside. |
| **Cổ tay nằm sau tay lái / sau thân mình** | Đặt chấm tại vị trí giải phẫu ước lượng của cổ tay và gắn cờ v = 1 (Occluded). | Khớp vẫn ở trong khung cảnh nhưng bị vật cản che khuất; tránh lỗi gán nhầm sang v = 0 khi người ở giữa ảnh. |
| **Hai người chồng lên nhau** | Gán dứt điểm toàn bộ 17 điểm cho người phía trước trước, sau đó mới gán người phía sau (các khớp bị che gán v = 1). | Tránh nối nhầm khớp của người này sang cơ thể người bên cạnh làm gãy cấu trúc skeleton. |
| **Người nhỏ đến mức nào thì không gán nữa** | Chỉ gán người nhìn rõ tối thiểu phần đầu và thân trên; bỏ qua các vệt mờ hậu cảnh hoặc bóng đồ vật bị model bắt nhầm. | Tránh tạo thêm người ảo ở background dẫn đến lệch số lượng cá thể so với ground truth (như trường hợp train_03, train_10). |

*(Chèn ảnh chụp màn hình CVAT minh họa cho từng trường hợp theo yêu cầu slide 12).*

---

### 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

**Ca 1 - ảnh `train_02` và `train_16`, người thứ 1, khớp vai (`left/right_shoulder`) và hông (`left/right_hip`)**
* **Mơ hồ ở chỗ nào:** Script `check_pose_labels.py` đưa ra 4 cảnh báo đảo trái/phải do trục hoành x của hai vai/hông ngược chiều hình học với trục x của hai mắt.
* **Bạn quyết thế nào:** Giữ nguyên nhãn đã gán theo đúng cơ thể người; không đổi chéo điểm chỉ để dập cảnh báo tự động của tool.
* **Vì sao:** Người trong ảnh ở tư thế quay nghiêng người hoặc vặn mình, khiến hình chiếu 2D của vector mắt và vector vai bị ngược hướng. Kết quả trực quan qua `visualize_pose.py` xác nhận xương không bị chéo chữ X; khi đối chiếu với tập `gold`, OKS đạt 0.939 và ghi nhận 0 lỗi đảo bên.
* **Nếu người khác quyết ngược lại thì model học sai cái gì:** Nếu ép đổi điểm trái/phải theo tool heuristic, nhãn sẽ bị sai giải phẫu thật; khi huấn luyện áp dụng data augmentation lật ảnh ngang (fliplr), model sẽ bị học sai quy luật nhận diện tư thế vặn người.

**Ca 2 - ảnh `train_04`, người thứ 1, 4 khớp chi dưới bị che**
* **Mơ hồ ở chỗ nào:** 4 khớp chân bị vật cản phía trước che khuất hoàn toàn, phân vân giữa việc xóa điểm/gán v = 0 hay chấm điểm ước lượng với v = 1.
* **Bạn quyết thế nào:** Đặt chấm vào vị trí giải phẫu ước lượng sau vật cản và tick cờ `Occluded` (v = 1, phím `q`), tuyệt đối không để v = 0.
* **Vì sao:** Người đứng trọn vẹn ở trung tâm khung ảnh, các khớp không rơi ra ngoài mép ảnh. Tool `check_pose_labels.py` quy định người ở giữa ảnh thì khớp bị che bắt buộc phải dùng v = 1.
* **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model hiểu nhầm rằng các khớp bị che đã đi ra ngoài khung hình hoặc biến mất, làm mất khả năng dự đoán khớp khuất (occlusion handling).

**Ca 3 - ảnh `train_13`, người thứ 1, khớp khuỷu tay và cổ chân**
* **Mơ hồ ở chỗ nào:** Góc chụp xiên và tư thế phức tạp khiến tâm xoay thực tế của khớp khó xác định chính xác theo pixel, dễ bị kéo lệch theo mép nếp gấp trang phục.
* **Bạn quyết thế nào:** Rework lại trên CVAT: căn chỉnh lại tâm chấm dựa theo trục xương cẳng tay/cẳng chân thay vì đặt theo mép ngoài quần áo.
* **Vì sao:** Lần đối chiếu đầu với tập `gold` bị cảnh báo OKS 0.741 (lệch nhẹ) và khi so với model chỉ đạt 0.629. Sau khi rework và export lại, OKS với Gold đã tăng lên, đưa OKS@0.75 toàn bài đạt mức tuyệt đối 1.000 (0 skeleton cần sửa).
* **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model học tọa độ khớp bị trôi (keypoint drift) lệch khỏi tâm giải phẫu, làm giảm chỉ số `pose_mAP50-95` khi đánh giá trên tập test độc lập.

---

### 4. Sau khi so visibility report với bạn cùng nhóm

* **Khớp lệch %v=1 nhiều nhất:** `right_eye` (bạn 31% / Trí 10% - lệch 21%), `nose` (bạn 28% / Trí 10% - lệch 17%), và `left_eye` (bạn 34% / Trí 21% - lệch 14%).
* **Nguyên nhân là guideline chưa rõ hay một trong hai bên gán sai:** Guideline ban đầu chưa định nghĩa rõ tiêu chí che khuất đối với khuôn mặt khi chụp góc nghiêng (bán diện). Một bên gán v = 1 khi mắt/mũi bị sống mũi hoặc bóng râm góc nghiêng che khuất một phần; bên còn lại coi như nhìn thấy (v = 2).
* **Luật mới bổ sung vào mục 2 sau khi thống nhất:** Ngũ quan vùng mặt (mắt, mũi) ở góc chụp nghiêng nếu vẫn quan sát được một phần con ngươi hoặc sống mũi thì thống nhất giữ v = 2; chỉ chuyển sang cờ v = 1 khi bị vật cản ngoại cảnh (kính râm, tóc xõa, khẩu trang, mũ bảo hiểm) che khuất hoàn toàn.