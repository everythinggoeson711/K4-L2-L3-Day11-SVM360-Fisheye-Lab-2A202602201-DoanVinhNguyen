# QA review · B1-edge

Mã khóa: 1D03-CFBC

**Hình thức:** cold review cá nhân (làm solo, không có bạn đổi bài). Bản được soát là chính `submission/r1_craft/annotations.xml` đã khóa lúc 21:02:35; review bắt đầu lúc 21:07:50 sau khoảng nghỉ > 5 phút. Đây **không** phải review của người khác. Chỉ dùng ảnh gốc trong `assets/images/`, `qa_overlay.html` và `docs/02-rules-vi.md`; chưa mở reference, model overlay hay worked HTML của slice này. Object_ref là chỉ số box L (cao ≥ 40 px, theo thứ tự XML) trong từng frame, như overlay hiển thị.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_001320.jpg | L4 | R01 | Car (221,838,240,884) cao 46 px nhưng chỉ rộng 19 px: chỉ thấy một mảnh xe trắng (cửa kính) giữa e-rickshaw đỏ và tuk-tuk L3. Chiều cao đạt H nhưng phần nhìn thấy quá ít để chắc là ô tô; cần quyết định giữ (occluded) hay coi là không đọc được. |
| adasind_014670.jpg | L5 | R04 | Xe vàng bị khung trái cắt (0,925,70,1095) được gọi ThreeWheeler. Phần nhìn thấy là thân vàng, bánh nhỏ ở y≈1015–1076 và chữ tím trên hông; có thể là hông một auto nhưng cũng có thể là van/xe buýt nhỏ sơn vàng. Class cần người thứ hai xác nhận; `truncated=true` đúng vì chạm biên x=0. |
| adasind_034080.jpg | L1 | R04 | Vật tối (98,1036,141,1115) phía sau xe trắng L2 đã được đổi Car → ThreeWheeler lúc tự soát vì có một đèn tròn ở giữa đầu xe. Bị che phần lớn bởi xe máy L3, thấy ~43×79 px; class vẫn là suy đoán từ một chi tiết. |
| adasind_034080.jpg | L4 | R03 | Người áo trắng (132,1015,182,1112) được box là Bike occluded (coi là người ngồi trên xe phía sau). Trên ảnh không thấy bánh xe hay yên của xe này — chỉ thấy thân người ở độ cao ngồi. Nếu người này đang đứng/đi bộ thì phải là Pedestrian. R03 không có hướng dẫn khi xe của rider bị che hoàn toàn. |
| adasind_034080.jpg | L3 | R02 | Bike hai người (117,996,275,1246): cạnh dưới đặt ở 1246, nhưng bánh sau nằm trong vùng làm mờ (≈1150–1258) nên không xác định chắc điểm chạm đất; box có thể ngắn hơn vật thật 10–15 px. Không đổi quyết định nhãn chính. |
| adasind_034080.jpg | L8 | R01 | ThreeWheeler occluded (259,1055,299,1097) cao 42 px, sát ngưỡng H. Nhìn thấy một thân xe tối có người phía sau; có thể là auto hoặc xe máy chở người. Nằm sát ngưỡng phạm vi và class không chắc. |
| adasind_034080.jpg | L7 | R01 | ThreeWheeler (333,1065,370,1106) cao 41 px — đúng ngưỡng; hình dáng đuôi auto (mái cong, hai đèn hậu) đọc được nên giữ hợp lý. Ghi để nhắc rằng thay đổi 2 px sẽ đổi phạm vi. |

Không thấy lỗi `lens_border` (6 polygon import khớp vòng kính) hay `ego_body` (3 polygon, đều có reason). Không có box nằm trong ignore, không có box trùng.

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
