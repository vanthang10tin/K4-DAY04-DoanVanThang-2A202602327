# Mini guideline - nhóm: T037  |  người gán: Đoàn Văn Thắng (2A202602327)  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Ước lượng vị trí mấu chuyển lớn xương đùi (greater trochanter) / mào chậu dựa theo nếp gập đùi và đường dóng thẳng đứng từ nách/thắt lưng xuống. Nếu quần áo thụng che khuất hoàn toàn bề mặt giải phẫu, chọn `v = 1` và vẫn đặt chấm ước lượng. | Hông không có mốc bề mặt nhìn thấy trực tiếp khi mặc quần áo; đây là điểm chốt giải phẫu nối thân trên và chân. Gán `v = 1` để model học cách ước lượng điểm khớp dưới lớp vải, không chọn `v = 0` vì người vẫn nằm trong khung hình. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu vành tai hoặc dái tai còn lộ một phần -> chọn `v = 1`, đặt chấm tại chân tai (vị trí nắp tai tragus). Nếu mũ bảo hiểm/tóc che kín hoàn toàn nhưng đầu còn trong ảnh -> vẫn chọn `v = 1` và ước lượng vị trí đối xứng qua trục mặt (mắt-mũi). Chỉ dùng `v = 0` khi toàn bộ đầu bị cắt ra ngoài mép ảnh. | Tai là mốc xác định góc quay (yaw/pitch) của đầu. Che khuất là trạng thái `v = 1`, cần giữ đủ cấu trúc 5 điểm vùng mặt để bảo toàn skeleton giải phẫu. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm hẳn ngoài khung hình (đầu gối, cổ chân) -> chọn `v = 0` (Outside), toạ độ `(0.0, 0.0)`. Riêng khớp sát mép (hông): nếu tâm khớp còn trong ảnh (`0 <= x, y <= 1.0`) -> gán `v = 1` (hoặc `v = 2` nếu nhìn rõ); nếu tâm khớp đã trôi ra ngoài biên ảnh (`y > 1.0` hoặc `x < 0`) -> bắt buộc gán `v = 0`. | Tránh lỗi toạ độ vượt quá biên `[0, 1]` khi chuẩn hoá khiến công cụ `check_pose_labels.py` báo KHÔNG ĐẠT và gây lỗi gradient/loss khi huấn luyện model. |
| Cổ tay nằm sau tay lái / sau thân mình | Cổ tay bị che khuất bởi ghi-đông, vô lăng hoặc thân mình nhưng cánh tay còn trong ảnh -> đặt chấm ước lượng tại vị trí khớp cổ tay theo trục cẳng tay kéo dài và gắn cờ `v = 1` (Occluded). | Đây là trường hợp bị vật thể che khuất (Occluded), không phải ra ngoài khung. Nếu tick Outside (`v = 0`) sẽ phạm lỗi xoá khớp bị che và mất điểm OKS. |
| Hai người chồng lên nhau | Gán xong trọn vẹn 17 điểm của người đứng trước rồi mới chuyển sang người đứng sau. Với người phía sau: khớp bị thân người trước che -> gắn cờ `v = 1` và ước lượng đúng theo trục cơ thể người sau, tuyệt đối không chấm nhầm sang thân người trước. | Tránh lỗi "nhầm người" (lỗi 2 của slide 46) - một lỗi giải phẫu nghiêm trọng bị trừ điểm nặng trong OKS và khiến đường nối skeleton bị giật chéo sang người bên cạnh. |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả mọi người có thể phân biệt được tư thế người (chiều cao bounding box >= 30px). Với 20 ảnh core của bài lab, tất cả các nhân vật xuất hiện rõ ràng đều phải được gán đủ skeleton. | Đảm bảo độ bao phủ (coverage) 100% so với tập Gold để không bị trừ điểm thiếu người. |

*Lưu ý: Với mỗi luật trên, cần chèn screenshot minh họa từ CVAT (hoặc ảnh crop từ `outputs/vis_train/`) vào bài báo cáo.*

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04.jpg`, người thứ `1` (id 6), khớp `right_hip`

- Mơ hồ ở chỗ nào: Người này bị mép dưới của ảnh cắt ngang hông. Khớp `left_hip` nằm ở y=450.75 (chiều cao ảnh 457, tỉ lệ 0.986), nhưng khớp `right_hip` được chấm ở y=460.32 (tỉ lệ 1.007 > 1.0) và để cờ `v = 2`.
- Bạn quyết thế nào: Khi khớp đã vượt qua mép dưới ảnh, bắt buộc phải đổi sang `v = 0` (Outside, toạ độ 0, 0), hoặc nếu ước lượng hông còn chạm mép dưới thì phải ép toạ độ y <= 457 (y <= 1.0) và gắn cờ `v = 1` vì mép ảnh đã cắt mất bề mặt nhìn thấy.
- Vì sao: Quy tắc bắt buộc: ra ngoài mép ảnh -> `v = 0`, không đặt chấm. Để `v = 2` với toạ độ ngoài ảnh làm `tools/check_pose_labels.py` báo lỗi KHÔNG ĐẠT định dạng.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học toạ độ ngoài biên [0, 1], gây lỗi tính loss hoặc dự đoán box/keypoint tràn viền màn hình.

### Ca 2 - ảnh `train_10.jpg`, người thứ `1`, khớp `left_knee, right_knee, left_ankle, right_ankle`

- Mơ hồ ở chỗ nào: Người ngồi ngay giữa ảnh nhưng phần chân từ gối trở xuống bị che hoàn toàn bởi bàn/quầy bar phía trước. Trong bản gán ban đầu, 4 khớp chân này bị tick `v = 0` (Outside).
- Bạn quyết thế nào: Phải chuyển cả 4 khớp này thành `v = 1` (Occluded) và ước lượng vị trí cẳng chân/bàn chân phía dưới mặt bàn.
- Vì sao: Người hoàn toàn nằm trong khung ảnh, đây là trường hợp bị vật thể che khuất (Occluded) chứ không phải ra ngoài mép ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Dính lỗi "Xoá khớp bị che" (lỗi 3 slide 46). Khớp bị tính 0 điểm OKS so với Gold, và model không học được cách định vị tư thế cơ thể khi bị vật thể che lấp.

### Ca 3 - ảnh `train_16.jpg`, người thứ `1`, khớp `left_shoulder, right_shoulder, left_hip, right_hip`

- Mơ hồ ở chỗ nào: Người mẫu quay lưng/nghiêng người nhìn ngoái lại qua vai. Trên ảnh 2D, chiều từ mắt trái sang mắt phải ngược với chiều từ vai trái sang vai phải, dễ gây cảm giác nhầm lẫn giữa bên trái ảnh và bên trái người.
- Bạn quyết thế nào: Tuân thủ quy tắc số 1: Trái/phải tính theo cơ thể người, không theo bức ảnh (tự đặt mình vào vị trí người mẫu để xác định vai trái, hông trái).
- Vì sao: Tránh lỗi đảo trái/phải (`dao_trai_phai`) - lỗi nguy hiểm nhất theo rubric (25 điểm).
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model bị dạy sai giải phẫu trái/phải; khi thực hiện augmentation lật ngang (`fliplr=0.5`), sai lầm bị nhân đôi và model học sai vĩnh viễn.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:

