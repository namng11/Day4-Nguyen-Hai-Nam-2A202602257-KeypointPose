# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 17.0 khớp có v > 0 mỗi người
- Tổng: v=2 356 | v=1 120 | v=0 0

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 7 | 0 | 25% |
| 1 | left_eye | 18 | 10 | 0 | 36% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 15 | 13 | 0 | 46% |
| 4 | right_ear | 17 | 11 | 0 | 39% |
| 5 | left_shoulder | 24 | 4 | 0 | 14% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 24 | 4 | 0 | 14% |
| 8 | right_elbow | 24 | 4 | 0 | 14% |
| 9 | left_wrist | 17 | 11 | 0 | 39% |
| 10 | right_wrist | 20 | 8 | 0 | 29% |
| 11 | left_hip | 24 | 4 | 0 | 14% |
| 12 | right_hip | 23 | 5 | 0 | 18% |
| 13 | left_knee | 20 | 8 | 0 | 29% |
| 14 | right_knee | 21 | 7 | 0 | 25% |
| 15 | left_ankle | 21 | 7 | 0 | 25% |
| 16 | right_ankle | 20 | 8 | 0 | 29% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
