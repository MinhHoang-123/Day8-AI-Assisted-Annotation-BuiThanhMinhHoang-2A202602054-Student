# Quét độc lập trước khi xem pre-label

Frame: frame_0099.jpg tên một ảnh trong `to_label/round1/images/train/`

Số xe nhìn thấy bằng mắt: 26

Hai vị trí dễ bị AI bỏ sót hoặc vẽ sai, kèm mô tả xe: 
1.Ở đầu đường ngược chiều( ở giữa phía bên trái phía trên của ảnh) có 3 xe bị khuất , chỉ thấy được 1 bóng đèn pha ở mỗi xe.
2.Ở hướng phía trên của ảnh , chiếc 2 và 3(tình từ trên xuống theo ảnh, nhìn đèn hậu) chiếc 2 bị chiếc 3 che khuất, chỉ thấy 1 đèn hậu (mỗi xe phải 2 đèn hậu)

Chạy `python3 tools/lock_blind.py` ngay sau khi điền. Sau đó giữ file này nguyên vẹn.
