# Tự soát


## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ
- [x] lens_border và ego_body
- [x] Class sáu nhãn
- [x] Rider và Bike
- [x] Geometry trên ảnh fisheye gốc
- [x] truncated và occluded
- [x] Vật thiếu hoặc box trùng
- [x] ignore_region có reason
- [x] Tên task raw_fisheye và export CVAT 1.1

## Ghi chú tự soát (bản nháp exports/r1-draft.xml, soát trên ảnh gốc theo thứ tự 9 mục)

1. Phạm vi H=40: các box nhỏ nhất là ThreeWheeler 034080 (333,1065,370,1106) cao 41 px và (259,1055,299,1097) cao 42 px — vẫn ≥ H nên giữ. Không box các auto/xe máy xa cao < 40 px (ví dụ auto xám 014670 ≈(401,954,432,985) cao 31 px, xe máy dựng 014670 ≈(438,973,467,997)). Con bò nằm ở 001320 không thuộc 6 class.
2. `lens_border`: 2 polygon import mỗi frame bám vòng kính trong `frames.csv`, không sửa. `ego_body`: tự vẽ ở cả 3 frame (tay, tay lái, đùi người lái scooter ở góc trái dưới); không frame nào thuộc ngoại lệ 006840/271039.
3. Class: tuk-tuk/e-rickshaw là ThreeWheeler, xe tải trước mặt ở 001320 là Truck. **Sửa sau tự soát:** vật tối sau xe trắng ở 034080 (≈98–141, 1036–1115) có một đèn tròn ở giữa đầu xe — đặc điểm của auto-rickshaw — nên đổi từ Car sang ThreeWheeler (R04) và thu box về phần nhìn thấy.
4. Rider: người lái + scooter (001320) và hai người trên một xe máy (034080) là một box Bike; người áo trắng ngồi trên xe phía sau (034080) là Bike occluded riêng, không box Pedestrian. Người đứng cạnh e-rickshaw đỏ (001320) không ngồi trên xe nào → Pedestrian.
5. Geometry: box bám phần thấy trên ảnh fisheye gốc. **Sửa sau tự soát:** box prefill ThreeWheeler đỏ ở 001320 (82,799,188,944) lấn sang người đi bộ bên trái; thu về (88,803,186,942). Không có box phủ cả dãy xe.
6. Attribute: `truncated` chỉ đặt cho vật chạm biên khung (auto vàng trái 001320 và 014670, xe xanh 014670 chạm x=1080, xe trắng trái 034080); `occluded` cho vật bị vật khác che (xe trắng sau tuk-tuk 001320, người áo trắng/auto tối sau người lái, xe trắng sau auto, xe xanh bị người che ở 034080). Công cụ không báo lệch truncated.
7. Thiếu/trùng: không có hai box cùng class IoU > 0.7. Đã thêm các vật prefill bỏ sót: người đi bộ, tuk-tuk giữa đường, xe tải (001320); toàn bộ vật ở 014670 và 034080 (prefill chỉ có lens_border).
8. `ignore_region`: 3 ego_body + 6 lens_border, mỗi polygon có đúng một reason; không box nào nằm ≥50% trong ignore.
9. Tên task `Day11 · ADASIND · B1-edge · raw_fisheye`; export task-level **CVAT for images 1.1**, không kèm ảnh. Bản cuối được export lại sau hai chỉnh sửa ở mục 3 và 5.

K12: bốn polygon viền thấy được cùng class và cùng group_id với box (auto vàng cắt biên 001320, xe xanh 014670, người đi bộ mép phải 034080, auto trung tâm 034080). Box trục thẳng ở vật cong/người đứng mép ảnh lỏng hơn (fill edge trung bình 0.751 so với 0.823 ở center) — chỉ là số minh họa của lab, n rất nhỏ.

## Fill ratio (K12)
- adasind_001320.jpg box 5 edge: 0.907
- adasind_014670.jpg box 1 edge: 0.788
- adasind_034080.jpg box 9 edge: 0.557
- adasind_034080.jpg box 3 center: 0.823
mean edge: 0.751 (n=3)
mean center: 0.823 (n=1)
