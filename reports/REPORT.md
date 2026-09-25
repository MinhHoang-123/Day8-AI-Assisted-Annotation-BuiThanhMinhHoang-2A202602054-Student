# Báo cáo Lab Ngày 08: Học chủ động cho bộ phát hiện xe

Họ và tên: Bùi Thanh Minh Hoàng
MSSV: 2A202602054

Công cụ gán nhãn đã dùng:CVAT (AnyLabeling, CVAT, SAM hoặc sửa trực tiếp file nhãn)

Sao chép file này thành `reports/REPORT.md` rồi điền vào các chỗ . Mọi con số phải truy được
từ `reports/rounds_table.md`, `outputs/selection_round1.csv`, `outputs/metrics_round*.json` hoặc
`outputs/round*_diff.md`. Không coi nhãn test do mô hình tạo là chân lý tuyệt đối.

## 1. Dữ liệu và cách chia tập

Tại sao tập chưa gán nhãn (pool) và tập kiểm thử (test set) được chia theo trục thời gian, có vùng
đệm ở giữa, thay vì chia ngẫu nhiên? Nếu chia ngẫu nhiên, số đo trên tập kiểm thử sẽ bị lệch theo
hướng nào, và vì sao?

Trong video, camera đứng yên, xe chạy qua. Nếu chia test ngẫu nhiên, một xe có thể lọt vào cả pool
lẫn test. Khi AI học từ xe đó, test sẽ cho điểm đẹp nhưng không phản ánh thực tế. Chia theo trục
thời gian giữ độc lập cho hai tập.

## 2. Mô hình khởi đầu lạnh (cold start)

Chép dòng vòng 0 từ `rounds_table.md`. Dựa vào `outputs/compare_round0.jpg`, cho biết mô hình khởi
đầu lạnh không khớp nhãn tham chiếu ở những loại xe nào. Độ phủ (recall) theo kích thước xe cho
thấy điều gì? Một trường hợp nào cần người rà lại nhãn tham chiếu trước khi kết luận mô hình sai?

Trong `outputs/compare_round0.jpg`, AI khoanh hụt 3 xe ở xa và sai 1 box. Theo `rounds_table.md`,
AP50 = 0.601. Độ phủ xe nhỏ = 0.182, xe vừa = 0.547, xe lớn = 0.561. Module xe nhỏ tệ nhất.
Cần rà lại nhãn tham chiếu ở xe xa trước khi tin là AI dở, vì nhãn chấm cũng do AI tạo.

## 3. Chiến lược chọn mẫu

Giải thích bằng lời công thức `score = W_U·U + W_A·A + W_D·D` và vai trò của `MIN_GAP_S`.
Dẫn ba frame trong `reports/SELECTION.md` và một frame khác để chứng minh cách bạn cân nhắc
độ bất định, ảnh gần trùng và công gán nhãn. Điểm bất định có chứng minh ảnh đó sẽ cải thiện
mô hình không? Vì sao?

Từ `outputs/selection_round1.csv`, tôi ưu tiên 5 ảnh điểm cao nhất:
frame_0291.jpg (0.7638), frame_0069.jpg (0.7143), frame_0008.jpg (0.6887), frame_0021.jpg (0.6757),
frame_0176.jpg (0.6742).
frame_0291.jpg và frame_0292.jpg gần trùng, tôi chỉ chọn 1.
Trong `reports/SELECTION.md`, 3 frame AI chọn đều dưới 0.68: frame_0367.jpg, frame_0000.jpg,
frame_0079.jpg.
frame_0247.jpg (0.6473) có xe bị che, tôi chọn sửa.
Điểm bất định cao không đảm bảo cải thiện, nhưng hướng vào vùng AI chưa chắc nên có nhiều
hy vọng hơn.


## 4. Các vòng học chủ động (active learning)

Chép bảng từ `rounds_table.md`. Với mỗi vòng, trình bày:

- mức độ bạn đã sửa nhãn gợi ý (số box giữ nguyên, chỉnh sửa, xoá, thêm mới, lấy từ
  `outputs/round*_diff.md`);
- AP50 thay đổi bao nhiêu so với khởi đầu lạnh và so với vòng trước;
- nhóm xe nào tốt lên hoặc xấu đi theo số đo trên cùng tập test.

Dựa vào các ảnh `compare_round*.jpg`, chỉ ra một ca kết quả đổi sau fine-tune (tốt hơn hoặc xấu
đi), cùng lý do có thể kiểm. Dùng `BLIND_SCAN.md`, `REVIEW_LOG.csv` và `round1_diff.md` phân biệt
quan sát độc lập, lỗi pre-label đã sửa và kết quả mô hình sau train. Mô tả một ca khó theo guideline.

AP50: 0.601 → 0.628 (+0.027)
Xe nhỏ: 0.182 → 0.278 (+0.096)
Xe vừa: 0.547 → 0.595 (+0.048)
Xe lớn: 0.561 → 0.573 (+0.012)

frame_0247.jpg: sửa 4 khung, thêm 2, giữ 6. Cải thiện R50 xe nhỏ.

## 5. Kết luận và giới hạn

Kết quả vòng này so với cold start ra sao? Vì sao bạn dừng hoặc tiếp tục? Đề xuất hai ca còn yếu
hoặc bất định cho vòng sau, kèm chi phí rà nhãn và nguy cơ ảnh gần trùng. Tập kiểm thử chỉ 20 ảnh,
có luật bỏ qua xe quá nhỏ và nhãn tham chiếu do mô hình tạo chưa được rà thủ công; các giới hạn đó
ảnh hưởng thế nào đến kết luận? Nếu AP50 giảm, bạn sẽ kiểm tra điều gì trước khi train thêm?

AP50 tăng nhẹ từ 0.601 → 0.628. Xe nhỏ cải thiện nhiều nhất. Tôi dừng vì không có thêm thời
gian. Đề xuất 2 ảnh:
frame_0408.jpg: điểm 0.7607, nhiều xe nhỏ xa
frame_0071.jpg: điểm 0.6957, xe ở xa cạnh rào
Cả hai đều không có xe gần trùng.
