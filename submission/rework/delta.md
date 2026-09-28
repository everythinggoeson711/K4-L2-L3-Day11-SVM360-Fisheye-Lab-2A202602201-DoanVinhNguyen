# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 7 | 7 | 0 | 0 | 2 | 2 |
| mid | 8 | 9 | 1 | 0 | 3 | 2 |
| edge | 4 | 4 | 0 | 0 | 0 | 0 |

## Findings action=rework
- adasind_014670.jpg L5+R5 WRONG_CLASS: đã sửa
- adasind_014670.jpg L5 SPURIOUS: đã sửa
- adasind_014670.jpg R5 MISSING: đã sửa
- adasind_034080.jpg L3+R5 BOX_GEOMETRY: không áp dụng
- adasind_034080.jpg L6+R2 BOX_GEOMETRY: đã sửa

## Nhận xét của tôi (đối chiếu lại trên ảnh)

- Ba ca được sửa đều là P1 có `action=rework` trong `findings.csv`, chỉ trên slice B1-edge; bản khóa ban đầu `1D03-CFBC` giữ nguyên, bản sau sửa khóa `8FE8-1B9A`.
- **014670 L5 → Truck (0,845,70,1100):** xe cắt biên trái thực ra là xe tải (thành thùng nan xám ở y≈845–935 phía trên thân vàng). Đổi class và kéo box lên. Đây là thay đổi duy nhất làm bảng đổi: zone mid matched 8 → 9, missing 1 → 0, spurious 3 → 2 (IoU với R5 từ 0.667 sai class thành 1.000 đúng class).
- **034080 L3 Bike, cạnh dưới 1246 → 1300:** lốp chạm đất nằm ngay dưới vùng làm mờ. IoU với R5 0.767 → 0.934. Đã matched từ trước nên bảng không đổi; công cụ báo "không áp dụng" vì dòng có cell `LR_noM` — tôi kiểm lại bằng IoU ở trên và bằng overlay.
- **034080 L6 Car (505,1070,571,1150) → (508,1088,586,1153):** bỏ phần mái auto phía trên, thêm phần đầu xe bên phải. IoU với R2 0.568 → 0.931; trước đây sát ngưỡng 0.5, nếu dùng ngưỡng 0.7 (iou-sweep) ca này đã bị tính missing + spurious.
- Spurious còn lại (center 2, mid 2) là các ca tôi **giữ có lý do** hoặc **escalate**: 014670 L1 và 034080 L7 (auto cao 47 và 41 px — R thiếu hoặc ngưỡng), 034080 L1 (vật tối bị che, E5), 001320 L4 (mảnh xe trắng, chờ luật mới). Tôi không xóa chúng chỉ để số khớp reference.
