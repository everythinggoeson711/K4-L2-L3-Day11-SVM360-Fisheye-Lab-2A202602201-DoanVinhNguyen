# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 7 | 0 | 2 | 2 | 4 | SPURIOUS (2) |
| mid | 9 | 1 | 3 | 6 | 7 | SPURIOUS (2) |
| edge | 4 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: **mid** gãy nhiều nhất cho cả hai. Với 9 vật reference ở mid, L có 1 missing + 3 spurious (1 missing và 1 spurious là cùng một vật: xe tải cắt biên trái 014670 bị tôi gọi ThreeWheeler), còn M có 6 missing + 7 thừa. Center: L chỉ có 2 spurious (auto 47 px ở 014670 mà R thiếu, auto 41 px ở 034080), M 2 missing + 4 thừa. Edge (4 vật) gần như khớp: L 0/0, M 1/1.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: phần lớn "gãy" của M ở mid/center **không** do méo rìa mà do lệch quy ước nhãn — model gọi auto/e-rickshaw là Truck/Car/Bus (≥8 ca kể cả C0) và tách rider thành Pedestrian + Bike (R03), nên ghép cùng class thất bại dù box đúng chỗ (E4_model_domain, cần kiểm trên nhiều frame hơn). Lỗi của tôi ở mid là vật bị **khung ảnh** cắt nên thiếu ngữ cảnh class, và box sát ngưỡng IoU ở vật bị che (034080 L6 IoU 0.568). Model không box `ego_body` như người đi bộ ở slice này chỉ vì reference ignore đã loại các box đó (M7/M8 ở 001320). Giới hạn: chỉ 3 frame, 20 vật reference, một camera; zone center/mid/edge chỉ là khoảng cách tới tâm vòng kính, không nói vật gần/xa xe, và edge chỉ có 4 vật nên không kết luận được "rìa dễ sai hơn".
