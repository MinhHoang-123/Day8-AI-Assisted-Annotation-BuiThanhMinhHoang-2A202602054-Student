# Vì sao chọn lô này?

Trong 50 dòng đứng đầu `outputs/selection_round1.csv`, chọn năm frame bạn sẽ ưu tiên nếu chỉ có
ngân sách rà năm ảnh. Ghi tên, điểm, thời điểm, thứ tự và lý do; tối thiểu một quyết định phải xét
ảnh gần trùng hoặc trường hợp model không dự đoán được box: Nếu chỉ được sửa 5 ảnh, tôi chọn frame_0291.jpg (hạng 1, điểm 0.7638), frame_0069.jpg (hạng 2, điểm 0.7143), frame_0008.jpg (hạng 3, điểm 0.6887), frame_0021.jpg (hạng 4, điểm 0.6757) và frame_0176.jpg (hạng 5, điểm 0.6742).Năm ảnh này đứng đầu danh sách. frame_0291.jpg và frame_0292.jpg cách nhau 1 giây, cảnh gần giống nhau, nên tôi không lấy cả hai.

Ba frame thuộc lô 12 ảnh model chọn và bằng chứng trong CSV/ảnh contact sheet: frame_0367.jpg, frame_0000.jpg và frame_0079.jpg. Cả ba đều có điểm dưới0,68 và AI khoanh nhiều xe nhưng còn nhiều khung chưa chắc.

Một frame có điểm cao nhưng không chọn hoặc một frame có điểm thấp vẫn nên xem, và lý do: frame_0247.jpg có điểm 0.6473 nhưng có nhiều xe bị che khuất, nên tôi đã sửa và bổ sung 4 khung.

Điều phép chọn này chưa chứng minh về chất lượng mô hình: Tôi chọn những frame có điểm cao nhất trong lô 12 ảnh, điều này chưa chứng minh được về chất lượng mô hình.
