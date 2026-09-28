# Guideline patch

- **Rule mới đề xuất:** 
  - Quy tắc xử lý xe bị che khuất ở mép ảnh (Occluded Edge Vehicle) & Vùng chồng lấp (Seam Zone):
    1. Vật thể bị che khuất > 85% hoặc diện tích hiển thị còn lại nhỏ hơn 15x15 pixel ở biên ảnh không gắn nhãn 2D box mà đánh dấu vùng `ignore_region`.
    2. Đối với vùng chồng lấp (Seam Zone) giữa 2 camera, không tự ý ghép (merge) 2 box thành 1 track chung trên không gian 2D riêng lẻ. Nhãn 2D giữ nguyên `camera_id` cục bộ và thêm thuộc tính `is_seam_overlap=true`.
- **Áp dụng cho:** Class `vehicle` (`car`, `truck`, `bus`), attribute `occluded`, `is_seam_overlap`, zone `seam_zone` và `ignore_region`.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện tại v1.0.0 chưa quy định ngưỡng che khuất tối thiểu ở mép ảnh gây ra sự bất đồng nhất giữa các annotator (người vẽ box mép quá nhỏ, người bỏ qua). Đồng thời v1.0.0 chưa có hướng dẫn rõ ràng về cách gán nhãn vật thể xuất hiện đồng thời trên 2 camera trong hệ thống SVM 360, dẫn đến việc duplicate tracking ID vô căn cứ.
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Round 2 (áp dụng ngay cho toàn bộ các lô gán nhãn từ ngày 29/09/2026).