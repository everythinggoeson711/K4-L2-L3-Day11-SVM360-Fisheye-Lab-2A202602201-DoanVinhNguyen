# Escalation ticket

## Ticket 1

- **Frame:** `adasind_001320.jpg`, object L4 Car (221,838,240,884) — slice B1-edge. Cùng loại: `adasind_034080.jpg` L7 (41 px), L1 (vật tối bị che), `adasind_014670.jpg` L1 (47 px, R thiếu).
- **Ảnh chụp:** `submission/screenshots/ticket1_001320_L4_car_sliver.png` (L cyan, R vàng, M hồng).
- **Expected impact:** R01 không nói đo phần nhìn thấy hay tối thiểu bao nhiêu phần vật mới cần box. Ở slice ba frame này, 4/5 box L bị tính SPURIOUS thuộc nhóm ranh giới đó; precision ThreeWheeler giảm còn 0.600 và local quality micro 0.792 phản ánh bất đồng về luật hơn là chất lượng vẽ. Nếu không sửa, các annotator khác nhau sẽ xử lý khác nhau, số MISSING/SPURIOUS giữa slice không so sánh được và reference có thể bị "sửa theo" người gán.
- **Owner:** `guideline`
- **Recommendation:** duyệt patch R01a trong `20_guideline_patch.md` (v1.1.0): đo H trên phần nhìn thấy, yêu cầu rộng ≥12 px và một chi tiết nhận dạng class; mảnh không đọc được → `ignore_region` reason `unreadable`. Sau khi duyệt, người soát thứ hai đo lại các ca 40–47 px ở B1-edge và bổ sung box auto 014670 L1 vào reference nếu xác nhận (E0).

## Ticket 2

- **Frame:** `adasind_034080.jpg` (Bike L3/R5 vs M8, M12 Pedestrian + M9 Bike; L4/R8 vs M7 Pedestrian) và `adasind_001320.jpg` (Bike L7/R2 vs M3 Pedestrian + M4 Bike); cùng mẫu ở C0 `adasind_019560.jpg` (M2, M5).
- **Ảnh chụp:** `submission/screenshots/ticket2_034080_model_rider_split.png`.
- **Expected impact:** model YOLO26m đóng băng tách người lái xe hai bánh thành Pedestrian + Bike (trái R03) và gọi auto/e-rickshaw là Truck/Car/Bus (≥8 ca, trái R04). Nếu dùng model làm pre-label hoặc làm "M" trong đối chiếu, mỗi rider sinh 1–2 box thừa và mỗi ThreeWheeler thành một cặp MISSING + SPURIOUS; ở slice này 15/25 dòng SPURIOUS trong findings đến từ hai mẫu này, che mất lỗi thật của người gán.
- **Owner:** `ai_team`
- **Recommendation:** trước khi dùng model làm pre-label: (1) thêm bước hậu xử lý gộp Pedestrian chồng ≥50% lên Bike thành một Bike theo R03; (2) ánh xạ/fine-tune lớp ThreeWheeler trên ảnh fisheye có auto-rickshaw; (3) đo lại bằng `iou-sweep` trên ≥20 frame nhiều block thay vì 3 frame, vì đây là giả thuyết E4 cần thêm ca mới kết luận.
