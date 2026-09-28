# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   -> Đây **không chỉ đơn thuần là lỗi DUPLICATE của người làm**, mà bắt buộc **cần một quy tắc riêng (policy) cho vùng Seam Zone**. Nguyên nhân là vì ở hệ thống 4 camera SVM 360, hai camera có vùng nhìn chồng lấp hoàn toàn tự nhiên. Trên không gian 2D của từng camera, việc xuất hiện vật thể ở cả 2 ảnh là đúng về mặt vật lý. Nếu không có rule hướng dẫn gán nhãn (ví dụ: gán box chính ở camera có diện tích lớn hơn và đánh dấu `is_seam_overlap`), người làm sẽ tự ý xóa box hoặc duplicate ID, gây mất đồng nhất dữ liệu khi đưa vào thuật toán fusion.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   -> **Giữ cùng Track ID:** Khi vật thể di chuyển liên tục và còn xuất hiện trong khung hình (cho phép bị che khuất tạm thời ngắn dưới 3-5 frames nếu hướng di chuyển nhất quán).
   -> **Thêm Keyframe / Outside:** Thêm `keyframe` tại thời điểm thuộc tính/hình dạng thay đổi đột ngột; chuyển trạng thái `Outside` khi vật thể đi hoàn toàn ra khỏi vùng quan sát của camera đó.
   -> **Bằng chứng cần trước khi nối track qua 2 camera:** (1) Timestamp đồng bộ tuyệt đối giữa 2 camera; (2) Thông số Extrinsic Calibration (vị trí tương quan giữa 2 camera); (3) Tọa độ dự báo của vật thể trên không gian chung (BEV/Homography Matrix) thể hiện sự liên tục về vận tốc và hướng di chuyển.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   -> **Trường hợp thực tế:** Tại `frame_000142.jpg`, `object_ref` = `obj_04` (vật thể ở mép ảnh góc khuất). Bán chuyên môn cho rằng box nên kéo rộng hết phần bóng râm dưới mặt đường để bao phủ chướng ngại vật, nhưng tôi tin rằng box 2D chỉ nên ôm khít phần vỏ kim loại của xe (tight bounding box) theo chuẩn perception.
   -> **Cách xử lý:** Tôi đã ghi chép lại trường hợp này vào `40_decision_log.csv` (mã DEC-002) và tạo `30_escalation_ticket.md` gửi cho Guideline Owner để làm rõ định nghĩa "Tight Box vs Shadow".
   -> **Nếu làm lại slice này:** Tôi sẽ chủ động kiểm tra phần quy chuẩn Edge Cases trong guideline trước, lưu ảnh bằng chứng minh họa (`submission/screenshots/escalation_frame_142.png`), đồng thời thảo luận trước với QA Leader để thống nhất tiêu chí ôm box trước khi tiến hành vẽ hàng loạt.