# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_261480.jpg` (mid zone) | 3 ca: 1 SPURIOUS (`L3`), 2 MISSING (`R5`, `R6`) | Lỗi bất đồng trực tiếp giữa nhãn Reference L và nhãn Reviewer R ở các đối tượng xe máy/người đi bộ bị sót hoặc gán thừa do độ méo fisheye. | File `local_quality_conflicts.csv` (dòng extra/missing), ảnh so sánh HTML `model_compare.md`, và dòng chẩn đoán trong `findings.csv`. |
| `adasind_265065.jpg` (mid zone) | 2 ca: 1 MISSING (`R5+M7`), 1 E5_unresolved (`R7`) | Có lỗi gán nhãn bỏ sót rõ ràng (`R5+M7`) và 1 ca tranh chấp quy chuẩn ranh giới ở rìa (`R7`) cần làm rõ với QA. | File `findings.csv` (dòng `R5+M7` và `R7`), ticket leo thang `30_escalation_ticket.md`, và ảnh chụp `screenshots/adasind_265065.png`. |

Giới hạn của kết luận từ ba frame ADASIND: Số lượng mẫu quá nhỏ (chỉ 3 frame) và thuộc duy nhất 1 camera ADASIND nên chỉ phản ánh lỗi cục bộ trên lát cắt dữ liệu này, chưa đủ tính đại diện thống kê để đánh giá độ chính xác toàn bộ hệ thống hoặc suy rộng ra phân bố lỗi của 4 camera SVM 360 (`E4_model_domain`).

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi: 
- **Cách soát độ phủ & Tránh trùng lặp cảnh:** Trích xuất 200 frame phân bổ đều cho 4 camera (`front`, `rear`, `left`, `right`) × 2 mức độ (`normal`, `hard`). Để tránh lấy nhiều frame liền nhau trong cùng một cảnh (sequence overlap), các frame phải được lấy mẫu giãn cách theo khoảng thời gian (stride/subsampling ít nhất 30-50 frame) hoặc trích xuất từ các chuỗi hành trình khác nhau.
- **Vì sao kế hoạch chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:** Kế hoạch lấy mẫu ưu tiên cố tình chọn các lát cắt khó (`hard slice` chiếm 40% dung lượng) nhằm chủ động "săn lỗi" và phát hiện sớm các trường hợp biên (edge cases). Do phân bố mẫu không tuân theo xác suất ngẫu nhiên tự nhiên của toàn bộ 50.000 frame, tỷ lệ lỗi tính được trên 200 frame này sẽ bị lệch (biased toward high error rate) và không đại diện cho tỷ lệ lỗi thực tế (true defect rate) của toàn bộ tập dữ liệu.