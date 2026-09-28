# Escalation ticket

## Ticket 1

- **Frame:** frame_000142.jpg (Cam Front-Left Intersection)
- **Ảnh chụp:** `submission/screenshots/escalation_frame_142.png`
- **Expected impact:** Ảnh hưởng trực tiếp đến thuật toán ghép hình BEV (Surround View Monitor) và hệ thống Multi-Camera Tracking. Nếu duplicate box ở vùng seam không được xử lý theo quy tắc chuẩn, mô hình perception sẽ phát hiện 2 vật thể ảo (ghost objects), gây phanh gấp hoặc đánh lái sai nguy hiểm trong bài toán tự hành.
- **Owner:** `guideline` (phối hợp với `ai_team`)
- **Recommendation:** Ban hành khẩn cấp quy tắc xử lý vùng Seam ở Patch v1.1.0; bổ sung quy chuẩn chuyển đổi tọa độ camera sang tọa độ Bird's Eye View (BEV) dựa trên Calibration Extrinsic trước khi thực hiện Fusing Track ID.