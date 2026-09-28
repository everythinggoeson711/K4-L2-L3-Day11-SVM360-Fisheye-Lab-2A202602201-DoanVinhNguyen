# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `1d03cfbc644c3594f69b811ad283093c9988d331cabd98d3008e6b05c23a208e`; slice `B1-edge`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_001320.jpg, adasind_014670.jpg, adasind_034080.jpg. Frame thiếu trong export: không.
TP=19; FP=5; FN=1; số lần đối chiếu=24; mean IoU của TP=0.847.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.792 | 0.950 | 0.833 |
| precision | 0.792 | 0.880 | 0.600 |
| recall | 0.950 | 0.900 | 0.500 |
| jaccard | 0.760 | 0.780 | 0.500 |
| dice | 0.864 | 0.861 | 0.667 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Car | 4 | 1 | 0 | 0.958 | 0.800 | 1.000 | 0.800 | 0.889 |
| Pedestrian | 5 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 6 | 4 | 0 | 0.833 | 0.600 | 1.000 | 0.600 | 0.750 |
| Truck | 1 | 0 | 1 | 0.958 | 1.000 | 0.500 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_001320.jpg | 6 | 1 | 0 | 0.857 | 0.857 | 1.000 |
| adasind_014670.jpg | 4 | 2 | 1 | 0.667 | 0.667 | 0.800 |
| adasind_034080.jpg | 9 | 2 | 0 | 0.818 | 0.818 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 4 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 5 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 6 | 0 | 0 |
| Truck | 0 | 0 | 0 | 1 | 1 | 0 |
| <extra> | 0 | 1 | 0 | 3 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
