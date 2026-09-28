# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_001320.jpg
- L4 mid SPURIOUS
## adasind_014670.jpg
- L1 center SPURIOUS
- L5+R5 mid WRONG_CLASS
## adasind_034080.jpg
- L1 mid SPURIOUS
- L7 center SPURIOUS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 7 | 7 | 0 | 2 |
| mid | 9 | 8 | 1 | 3 |
| edge | 4 | 4 | 0 | 0 |
