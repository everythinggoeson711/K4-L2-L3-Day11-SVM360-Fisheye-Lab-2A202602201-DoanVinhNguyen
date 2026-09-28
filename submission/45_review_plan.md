# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Vật ở ngưỡng phạm vi R01 (cao 40–50 px hoặc chỉ lộ một mảnh), zone center–mid: `adasind_001320.jpg` L4, `adasind_014670.jpg` L1, `adasind_034080.jpg` L1, L7, L8 | 5 ca: 4 SPURIOUS của L (L_only) + 1 ca QA ngưỡng; 1 ca nghi reference thiếu (E0), 1 escalate (E2), 3 E5 | Chiếm 4/5 spurious của L trong slice, thay đổi 1–2 px hoặc cách hiểu R01 là đổi kết quả; nếu không thống nhất thì số local quality giữa các slice không so được | Ảnh crop từng vật với thước đo pixel, số đo chiều cao phần nhìn thấy, quyết định của người soát thứ hai, phiên bản luật (v1.0.0 → v1.1.0) |
| Vật bị khung/vòng kính cắt và ThreeWheeler/rider, zone mid: `adasind_014670.jpg` L5/R5, `adasind_001320.jpg` L1, L6, L7, `adasind_034080.jpg` L3, L4, L5 | 1 WRONG_CLASS + 1 MISSING + 1 SPURIOUS (E1, đã rework) và ~13 dòng M sai class/tách rider (E4) | Mid có n_ref=9 và nhiều lỗi nhất cho cả L (1 missing/3 spurious) và M (6/7); đây là class đặc thù giao thông Ấn Độ mà model và người mới dễ đọc nhầm | Ảnh trước/sau rework, dòng findings L5+R5, bảng confusion (Truck→ThreeWheeler), iou-sweep 0.3/0.5/0.7 |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame liền block B1, 20 vật reference và một camera trước gắn trên xe hai bánh; edge chỉ có 4 vật nên không nói được "rìa dễ sai hơn". Teaching reference cũng có thể sai (ví dụ 014670 L1). Các tỷ lệ ở `local_quality.md` là mô tả trên vài frame, không phải ước lượng tỷ lệ lỗi của dataset hay của hệ SVM.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: (1) liệt kê trước bảng phủ `camera × điều kiện` (ngày/đêm/mưa, đô thị/cao tốc/bãi đỗ, chạy/lùi) và `camera × class` (đặc biệt ThreeWheeler, rider, người đi bộ sát xe) rồi kiểm mỗi ô có ≥1–2 frame; (2) lấy mẫu theo **cảnh/chuyến** trước rồi mới chọn frame, tối đa 1 frame mỗi 5 giây của cùng cảnh và không quá ~10% số frame của một camera từ một chuyến, để frame liền nhau không bị đếm như nhiều ca độc lập; (3) với ca seam, chọn theo **thời điểm** để có frame đồng bộ của hai camera kề nhau; (4) giữ log nguồn (chuyến, timestamp, camera_id) để kiểm trùng. Kế hoạch này có chủ đích dồn vào ca khó (122/200 frame hard) nên chỉ giúp **tìm ca cần soi và kiểm luật**; nó không phải mẫu ngẫu nhiên đại diện nên không dùng để ước lượng tỷ lệ lỗi của 50.000 frame — muốn đo tỷ lệ cần thêm một mẫu ngẫu nhiên phân tầng riêng.
