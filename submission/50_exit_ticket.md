# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?

   **Không phải `DUPLICATE`.** `DUPLICATE` là hai box cho **một vật trên cùng một ảnh** của cùng camera. Ở seam, mỗi camera có ảnh, hệ tọa độ và méo riêng, nên một box trên ảnh `front` và một box trên ảnh `right` đều hợp lệ trong annotation space của từng camera (thường một box là edge/cắt vòng kính, box kia là mid). Cần quy tắc riêng: giữ cả hai box ở mức từng camera, và chỉ đánh dấu "cùng một vật" khi có timestamp đồng bộ, calibration để chiếu hai box về cùng hệ tọa độ xe/BEV và kiểm chồng vị trí, và policy output đích (detector theo camera hay object hợp nhất trong BEV). Nếu thiếu bằng chứng, gắn cờ cần review cross-camera, không xóa box nào.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.

   Trong **một** camera: giữ cùng track ID khi vẫn là cùng vật quan sát được liên tục (vị trí/kích thước thay đổi liên tục, cùng class, không bị thay bởi vật khác); bị che ngắn vài frame thì giữ ID, đánh `occluded` thay vì tạo track mới. Thêm **keyframe** khi hình học đổi lớn mà nội suy không bám được — vật vào vùng rìa bị méo cong, bị vòng kính/khung cắt dần, đổi hướng, hoặc thay đổi `truncated`/`occluded`. Đặt **Outside** khi vật rời trường nhìn hẳn (ra khỏi vòng kính/khung) hoặc bị che hoàn toàn quá lâu để chắc là cùng vật; khi vật quay lại mà không chắc danh tính thì mở track mới. Trước khi nối track **qua hai camera** cần: timestamp đồng bộ (và độ lệch cho phép) giữa hai camera; calibration nội/ngoại để chiếu vị trí vật từ hai ảnh về cùng hệ tọa độ xe và kiểm chúng trùng nhau trong vùng seam; nhất quán class/kích thước/hướng chuyển động; và policy output nói rõ track được hợp nhất ở tầng nào. Thiếu một trong các bằng chứng đó thì giữ hai track riêng theo camera.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?

   `adasind_014670.jpg` L1 — auto đen–vàng ở xa giữa đường, box (359,952,397,999) cao 47 px. Tôi tin nhãn đúng vì vật ≥ H=40 và đọc rõ là đuôi auto; teaching reference không có box, còn model có box M7 cùng vị trí (gọi Truck). Tôi **không** xóa để khớp reference: ghi `E0_reference_defect`, `action=keep_with_reason`, owner `qa`, và đưa vào ticket 1 để người soát thứ hai đo lại; decision log ghi quyết định giữ. Ngược lại ở `014670` L5 người soát (cold review của tôi) đã nghi class và reference cho thấy tôi sai (xe tải có thành thùng nan) → tôi nhận `E1` và rework. Nếu làm lại, tôi sẽ (1) phóng to toàn bộ chiều cao của mọi vật bị khung cắt trước khi chọn class, (2) đo chiều cao phần nhìn thấy bằng thước pixel cho mọi vật 40–50 px và ghi số đo vào selfqc, và (3) không xem output model của slice trước khi khóa — trong buổi này tôi có lỡ in bảng box model của slice trước khi khóa, dù đã chốt danh sách nhãn trước đó và không đổi theo; lần sau sẽ giữ đúng thứ tự tự làm → khóa → mở R/M.
