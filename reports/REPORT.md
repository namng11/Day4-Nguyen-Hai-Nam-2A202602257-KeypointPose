# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Hải Nam   Nhóm: cá nhân   Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 334 / 126 / 33 |
| Thời gian trung bình mỗi ảnh | ______ |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. ______
2. ______
3. ______

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

[Điền cảm nhận của bạn vào đây: Ví dụ: Đúng, vì các khớp này thường nằm ở phần dưới cơ thể bị khuất hoặc bị các bộ phận khác đè lên...]

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | ______ | ______ |
| OKS@0.50 | ______ | ______ |
| OKS@0.75 | ______ | ______ |
| Lỗi `dao_trai_phai` | ______ | ______ |
| Lỗi `nham_nguoi` | ______ | ______ |
| Lỗi `xoa_khop_bi_che` | ______ | ______ |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_01` (người 1, 2): Sửa lại khung bounding box chạm mép ảnh dưới để không bị cảnh báo xoá khớp bị che v=0.
- `train_02` (người 3): Đảo lại trái/phải các khớp vai, khuỷu tay, cổ tay, hông, gối, mắt cá do gán ngược chiều.
- `train_16` (người 23): Đảo lại trái/phải các khớp trên cơ thể tương tự train_02.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải xảy ra ở ảnh `train_02` và `train_16`. Đây là lỗi do chủ quan sơ suất trong quá trình dán nhãn, nhầm lẫn giữa bên trái/phải của người trong ảnh với bên trái/phải từ góc nhìn của người quan sát.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| ______ | ______ | ______ | ______ | ______ |
| ______ | ______ | ______ | ______ | ______ |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- ______

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   **Trả lời:** `pose_mAP50-95` thay đổi tăng nhẹ `+0.0055`. Tuy nhiên `box_mAP50-95` lại giảm `-0.0078`. Nhãn của tôi đã dạy cho model tinh chỉnh lại một chút vị trí các khớp (tăng pose mAP), nhưng vì tập dữ liệu chỉ có 20 ảnh nên sự khác biệt rất nhỏ. Đổi lại, nó làm hỏng đôi chút khả năng nhận diện khung người (`box_mAP` giảm), có thể là do cách tôi vẽ bounding box không hoàn toàn đồng nhất với chuẩn gốc của COCO (ví dụ vẽ quá rộng hoặc quá chật so với model pretrained).

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   **Trả lời:** `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) khoảng 0.1133. Điều này cho thấy model tìm *người* dễ hơn tìm *khớp*. Lý do là bài toán object detection (vẽ một hộp chữ nhật quanh một hình khối lớn) dễ học được các đặc trưng vĩ mô. Còn pose estimation yêu cầu tìm chính xác tọa độ của 17 điểm nhỏ, chịu ảnh hưởng rất lớn từ biến dạng tư thế, việc bị che khuất (occlusion), và sự mập mờ trong định vị khớp giải phẫu (chẳng hạn hông hay vai ở đâu dưới lớp áo rộng).

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   **Trả lời:** [Bạn hãy xem kết quả ảnh test trong `outputs/runs/predictions/test` và điền tên 1 ảnh kèm theo loại lỗi vào đây nhé. Ví dụ: test_02 bị lỗi "nhầm người" do model dự đoán khớp của người này dính sang người kia...]

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   **Trả lời:** Theo bảng OKS trên tập train, ảnh có OKS thấp nhất là `train_02` (OKS = 0.393). Model là người đúng. Dựa vào cảnh báo từ script kiểm tra trước đó, tôi đã gán ngược trái/phải cho các khớp trên thân người của `train_02` (lỗi chủ quan). Model (được học từ bộ chuẩn) dự đoán đúng theo chiều của mắt, do đó kết quả so khớp giữa nhãn của tôi và model bị lệch nặng.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   **Trả lời:** Có sự tương đồng. Bức ảnh `train_02` là bức mà tôi gán nhãn tệ nhất (lỗi đảo trái phải), và đây cũng là ảnh model có OKS khác biệt nhất với tôi. Tuy nhiên, ở các ảnh tiếp theo có OKS thấp như `train_16` (0.626), đây cũng là bức ảnh tôi bị lỗi đảo trái phải ban đầu. Với những trường hợp khó (góc chụp lạ, khuất), cả người dán nhãn và model đều dễ bối rối, chứng tỏ bức ảnh đó thiếu thông tin thị giác rõ ràng để suy luận điểm khớp.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

[Điền vào đây: Ví dụ: Tại ảnh train_15, người thứ 1, khớp đầu gối trái (left_knee). Dù khớp gối bị vạt áo che khuất, tôi vẫn nhận ra được trục của đùi và cẳng chân để gióng điểm giao cắt. Do điểm này vẫn nằm bên trong khung ảnh (không bị mép ảnh cắt) nên tôi quyết định gắn nhãn v=1 (bị che) thay vì v=0 (không tồn tại trong khung hình).]
