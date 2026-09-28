# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | BOX_GEOMETRY | 1 |
| center | B1 | MISSING | 2 |
| center | B1 | SPURIOUS | 9 |
| center | C0 | BOX_GEOMETRY | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B1 | MISSING | 1 |
| edge | B1 | SPURIOUS | 1 |
| mid | B1 | ATTRIBUTE | 1 |
| mid | B1 | BOX_GEOMETRY | 2 |
| mid | B1 | MISSING | 6 |
| mid | B1 | SPURIOUS | 13 |
| mid | B1 | WRONG_CLASS | 4 |
| mid | C0 | SPURIOUS | 1 |

## Top defects
- SPURIOUS: 25 (ví dụ frame adasind_019560.jpg)
- MISSING: 9 (ví dụ frame adasind_001320.jpg)
- BOX_GEOMETRY: 4 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: SPURIOUS (25) là lỗi nhiều nhất nhưng **phần lớn không phải lỗi người gán**: 15/25 là `M_only` do model gọi auto/e-rickshaw là Truck/Car/Bus và tách rider thành Pedestrian + Bike, nên box đúng chỗ nhưng không ghép được cùng class (E4_model_domain; ≥8 ca class ThreeWheeler, 5 ca rider ở C0/001320/034080). Phần của tôi: 4 spurious còn lại của L là vật ở ngưỡng phạm vi — auto 47 px mà reference thiếu (014670 L1, E0), auto 41 px (034080 L7), vật tối bị che (034080 L1), mảnh xe 19×46 px (001320 L4) — tức khoảng trống luật R01 hơn là vẽ nhầm (E2/E5). Lỗi thật của tôi (E1) là **class của vật bị khung cắt**: 014670 L5 chỉ thấy thân vàng nên gọi ThreeWheeler, bỏ qua thành thùng nan phía trên (xe tải) → 1 WRONG_CLASS + 1 MISSING + 1 SPURIOUS từ cùng một vật; và 2 box hình học ở vật bị che/làm mờ (034080 L3, L6).
- Cách sửa và ai nhận việc (`owner`): `annotator` (tôi) đã rework 3 ca P1: 014670 L5 → Truck (0,845,70,1100); 034080 L3 cạnh dưới 1246 → 1300; 034080 L6 → (508,1088,586,1153) — delta mid matched 8→9, missing 1→0. `guideline`: thêm luật phần nhìn thấy tối thiểu và cách đọc class cho vật bị khung cắt (`20_guideline_patch.md`, v1.1.0, ticket 1). `ai_team`: ánh xạ class ThreeWheeler và quy ước rider trước khi dùng model làm pre-label (ticket 2). `qa`: soát lại 014670 L1 (có thể là lỗi reference) và các ca E5.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/rework_014670_L5_before.png` và `_after.png` (R04, dòng r1_craft/r3_diag L5+R5, L5, R5); `screenshots/ticket1_001320_L4_car_sliver.png` (R01, r3_diag 001320 L4, action=escalate); `screenshots/ticket2_034080_model_rider_split.png` (R03, r3_diag 001320 M3 và 034080 M7–M9, M12); `r3_diag/local_quality.md` (TP=19, FP=5, FN=1; class yếu nhất ThreeWheeler precision 0.600 và Truck recall 0.500 — cả hai đến từ cùng xe tải 014670 và các auto ở ngưỡng); `rework/delta.md`.
