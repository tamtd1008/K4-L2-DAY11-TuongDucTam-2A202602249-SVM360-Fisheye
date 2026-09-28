# Sensor context

- **Rig:** Camera front-facing (camera nhìn phía trước) đặt ở vị trí cao tầm kính lái (windshield / dashcam mount) hoặc nóc xe tự hành, hướng góc nhìn rộng ra toàn cảnh bãi đỗ xe phía trước.
- **`ego_body`:** Không xuất hiện phần thân xe chủ (`ego_body` như nắp capo, gương chiếu hậu hay tay lái) trong khung hình. Khung hình hoàn toàn sạch, tập trung toàn bộ vào đường xá và không gian bãi đỗ.
- **Vòng kính (lens circle):** 
  - Khung hình dạng hình chữ nhật tiêu chuẩn (rectilinear view), không thấy viền tròn đen méo fisheye (lens circle boundary/vignetting) ở 4 góc.
  - Vùng hiệu dụng (active pixel area) chiếm trọn **100%** diện tích khung hình.