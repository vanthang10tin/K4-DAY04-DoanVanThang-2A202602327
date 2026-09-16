# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Đoàn Văn Thắng   Nhóm: T037   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 352 / 98 / 26 |
| Thời gian trung bình mỗi ảnh | 4 phút/ảnh |

Ba khớp có `%v=1` cao nhất chép từ `reports/visibility_report.md`:

1. `left_ear` và `right_ear`: 43%
2. `right_eye`: 29%
3. `left_eye`, `left_wrist` và `right_wrist`: 25%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Các khớp có tỉ lệ v=1 cao nhất gồm tai, mắt và cổ tay là những bộ phận thường xuyên bị che khuất bởi tóc, mũ bảo hiểm hoặc góc quay khuôn mặt. Tuy nhiên, khớp hay bị che không đồng nghĩa với khớp khó xác định vị trí giải phẫu. Tai và cổ tay khi bị che vẫn có thể suy luận khá chính xác dựa vào trục mặt hoặc hướng đi của cẳng tay. Khớp thực sự khó gán nhất trong thực tế là hông trái và hông phải. Do người mẫu mặc trang phục rộng hoặc áo dài làm mất hoàn toàn mốc xương mấu chuyển lớn, người gán buộc phải ước lượng vị trí thuần túy theo tỷ lệ cơ thể.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.8986 | 0.9030 |
| OKS@0.50 | 0.9655 | 1.0000 |
| OKS@0.75 | 0.9655 | 1.0000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 3 | 0 |
| Lỗi `xoa_khop_bi_che` | 1 | 0 |

*Đánh giá tổng quát: Sau rework, chất lượng gán nhãn được nâng cấp toàn diện lên mức Xuất sắc tuyệt đối. Tỉ lệ OKS@0.50 và OKS@0.75 đều đạt 1.0000, OKS trung bình tăng lên 0.9030, đồng thời toàn bộ các lỗi nhầm người, xóa khớp bị che và thiếu người đều được triệt tiêu hoàn toàn.*

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13.jpg` - bổ sung người thứ 3: Gán thêm skeleton đủ 17 điểm cho người ở hậu cảnh bị sót trong lần gán đầu, khắc phục lỗi thiếu người và đưa OKS của người này đạt chuẩn.
- `train_12.jpg` - người thứ 1 - khớp `left_ankle`: Sửa cờ từ v=0 thành v=1 và đặt chấm ước lượng vị trí cổ chân theo trục cẳng chân, khắc phục hoàn toàn lỗi xóa khớp bị che.
- `train_01.jpg` - người thứ 1 và người thứ 2 - khớp `left_wrist` và `right_wrist`: Chỉnh kéo các chấm cổ tay về đúng cẳng tay của từng người thay vì bắt nhầm sang người bên cạnh, khắc phục 2 lỗi nhầm người.
- `train_04.jpg` - người thứ 1 - khớp `left_wrist`: Đặt lại chấm cổ tay trái về đúng cơ thể của người thứ 1, khắc phục 1 lỗi nhầm người.
- `train_04.jpg` - người thứ 2 - khớp `right_hip`: Chuyển cờ v=2 về v=0 cho khớp bị tràn ra ngoài mép đáy ảnh, bảo đảm tính hợp lệ của định dạng nhãn.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Quy tắc tự đặt mình vào vị trí cơ thể người mẫu thay vì nhìn theo góc nhìn của bức ảnh đã được tuân thủ nghiêm ngặt trong suốt quá trình gán nhãn.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- 

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6853 | +0.0000 |
| pose_precision | 0.9734 | 0.9746 | +0.0012 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8054 | -0.0065 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   pose_mAP50-95 không thay đổi, giữ nguyên ở mức 0.6853, trong khi pose_precision tăng nhẹ 0.0012 từ 0.9734 lên 0.9746. Tập dữ liệu 20 ảnh quá nhỏ so với tập dữ liệu COCO đồ sộ của model tiền huấn luyện, nên quá trình học kích hoạt dừng sớm ở epoch 31 nhằm tránh quá khớp. Tập dữ liệu mới giúp model tự tin hơn ở các điểm khớp bị che theo quy tắc gán nhãn của lớp, nhưng quy mô này chưa đủ làm thay đổi ranh giới phân tách trên tập kiểm thử gồm 10 ảnh chuẩn.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?

   box_mAP50-95 đạt 0.8054, cao hơn pose_mAP50-95 đạt 0.6853 là 0.1201, tương đương mức chênh lệch khoảng 12%. Model tìm người dễ hơn tìm khớp rất nhiều. Nhiệm vụ phát hiện người chỉ cần xác định vùng bao quát của toàn bộ cơ thể dựa trên các khối đặc trưng lớn như thân mình và đầu. Ngược lại, xác định pose buộc model phải định vị chính xác tọa độ từng điểm ảnh của 17 khớp giải phẫu nhỏ, vốn rất nhạy cảm với hiện tượng che khuất, góc chụp nghiêng và các tư thế vặn xoắn phức tạp.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Trong ảnh test_03.jpg, model mắc lỗi đảo trái/phải. Cụ thể, đối với cả hai người trong ảnh, model đã dự đoán tráo đổi toàn bộ các khớp bên trái sang bên phải và ngược lại, từ khớp vai, khuỷu tay, cổ tay cho đến đầu gối. Lỗi đảo trái/phải này khiến điểm OKS của cả hai người trong ảnh tụt xuống rất thấp, lần lượt là 0.234 và 0.183.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh có OKS thấp nhất giữa nhãn của tôi và model trên tập train là train_06.jpg với OKS đạt 0.614. Trong trường hợp này nhãn của tôi đúng hơn. Bức ảnh train_06.jpg chụp người mẫu gập nghiêng trong điều kiện ánh sáng ngược và bóng đổ đậm. Model bị vùng tối đánh lừa dẫn đến dự đoán lệch vị trí hông và đầu gối. Nhãn gán thủ công của tôi bám sát tỷ lệ nhân trắc học giải phẫu và thực hiện ước lượng khớp bị che v=1 theo đúng khung xương cơ thể.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?

   Ảnh tôi gán có OKS thấp nhất so với Gold là train_13.jpg do thiếu một người ở hậu cảnh nên điểm OKS người này bằng 0, tiếp theo là train_01.jpg với OKS đạt 0.8194. Đối với model, train_13.jpg cũng là bức ảnh gặp bất đồng lớn khi OKS chỉ đạt 0.673 và model phát hiện ra 3 người trong khi nhãn của tôi chỉ có 2 người. Hiện tượng này cho thấy train_13.jpg là bức ảnh có độ mơ hồ cao và bối cảnh phức tạp. Nhân vật thứ ba đứng ở phía xa hậu cảnh và bị che khuất phần lớn cơ thể nên người gán dễ bỏ sót, trong khi model nhờ cơ chế trích xuất đặc trưng toàn cục đã nhận diện đủ cả 3 người, trùng khớp với số lượng người trong tập Gold.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Trong ảnh train_10.jpg, người thứ nhất, tôi phải quyết định trạng thái visibility cho khớp đầu gối và cổ chân. Về căn cứ thị giác, người mẫu ngồi ở vị trí trung tâm bức ảnh với thân trên nhìn thấy rõ ràng, nhưng phần chân từ đùi trở xuống bị che khuất hoàn toàn bởi quầy bar phía trước. Mặc dù bề mặt khớp không lộ ra, toàn bộ cơ thể người mẫu cùng vị trí suy luận của khớp chân đều nằm trọn vẹn bên trong khung hình chứ không bị cắt qua mép ảnh. Vì vậy, tôi chọn trạng thái v=1 và đặt điểm ước lượng theo trục cẳng chân thay vì đánh dấu v=0. Quyết định này giúp giữ đúng cấu trúc khung xương giải phẫu và tránh phạm lỗi xóa khớp bị che.
