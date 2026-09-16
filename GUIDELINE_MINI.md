# Mini guideline - nhóm: cá nhân  |  người gán: Nguyễn Hải Nam  |  ngày: 16/9/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Gióng từ thắt lưng xuống dưới khoảng 10-15cm, ở điểm phình to nhất của hông hai bên. Đặt cờ v=1 (occluded) nếu bị áo khoác che khuất hoàn toàn. | Hông hiếm khi lộ rõ dưới lớp áo dày, cần gióng theo cấu trúc cơ thể để điểm rơi nhất quán. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đánh dấu tại vị trí ước lượng của lỗ tai dựa trên góc mặt và gò má. Đặt cờ v=1. | Tóc/mũ thường che kín tai, nếu không ước lượng sẽ mất điểm tham chiếu cho đầu. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp gối, mắt cá nằm ngoài rìa ảnh bị gán v=0 (không vẽ điểm). Bounding box kéo sát chạm mép ảnh dưới. | Đảm bảo tính nhất quán với giới hạn không gian của thuật toán và không vượt khung hình. |
| Cổ tay nằm sau tay lái / sau thân mình | Gióng theo chiều hướng của cẳng tay và điểm gập của khuỷu tay. Đặt v=1 nếu bị che khuất. | Dù bị che, cổ tay vẫn có thể đoán được vị trí khá chính xác qua đường thẳng của cẳng tay. |
| Hai người chồng lên nhau | Gán khớp cho từng người dựa theo trục cơ thể và hướng quần áo. Khớp bị đè thì đặt v=1. | Tránh hiện tượng model gán nhầm tay người này cho người kia. |
| Người nhỏ đến mức nào thì không gán nữa | Người ở quá xa nền, có diện tích bounding box < 32x32 pixels hoặc không thể phân biệt mắt/mũi thì không gán. | Hình quá nhỏ không đủ đặc trưng để mạng nơ-ron học (bị nhiễu hạt quá nhiều). |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung. *(Bạn tự chèn thêm link ảnh vào đây nhé)*

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `2`, khớp `left_ankle`

- Mơ hồ ở chỗ nào: Bàn chân và mắt cá trái bị cỏ/chướng ngại vật che lấp hoàn toàn, không thấy rõ khớp.
- Bạn quyết thế nào: Ước lượng chiều dài cẳng chân từ đầu gối xuống, đặt điểm ở cuối cẳng chân với cờ `v=1`.
- Vì sao: Để bảo toàn cấu trúc skeleton (khung xương) của người. Dù bị che nhưng chân vẫn nằm trong ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đánh `v=0` (bỏ khớp), model sẽ học cách "xóa" chân ngay khi gặp vật cản nhỏ thay vì học khả năng đoán xuyên thấu (occlusion handling).

### Ca 2 - ảnh `train_13`, người thứ `1`, khớp `right_hip`

- Mơ hồ ở chỗ nào: Người mặc áo phông rộng trùm qua mông, không thể nhìn thấy đường nếp gấp hông hoặc thắt lưng.
- Bạn quyết thế nào: Ước lượng dọc theo trục đùi đi lên và cột sống đi xuống để tìm giao điểm, đánh cờ `v=1`.
- Vì sao: Hông là điểm kết nối sống lưng và chân. Dù áo rộng vẫn có nếp gấp vải hở ra ở đoạn gập đùi.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đặt điểm ngẫu nhiên quá cao (ngang bụng) hoặc quá thấp, model sẽ học sai tỷ lệ cơ thể phần thân người.

### Ca 3 - ảnh `train_15`, người thứ `1`, khớp `left_shoulder`

- Mơ hồ ở chỗ nào: Bức ảnh chụp nghiêng, người xoay hông và góc máy làm cho vai trái khuất một phần sau cổ/đầu.
- Bạn quyết thế nào: Vẫn đánh dấu vào điểm tương xứng ngang qua trục cổ so với vai phải, với cờ `v=1`.
- Vì sao: Dựa trên đối xứng cơ thể 2 bên, vai trái chắc chắn ở vị trí đó dù có bị mặt đè lên.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu kéo vai trái nhích ra ngoài cho "thấy rõ" điểm, model sẽ học sai phối cảnh 3D và làm lệch độ dốc của đôi vai.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_hip` (bạn `40%` / họ `20%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline chưa thống nhất rõ ràng việc hông bị áo phông phủ qua có được tính là bị che khuất (`v=1`) hay không, dẫn đến mỗi người làm một kiểu.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: "Hông bị áo che khuất hoàn toàn bề mặt phải được đánh `v=1` (occluded)".
