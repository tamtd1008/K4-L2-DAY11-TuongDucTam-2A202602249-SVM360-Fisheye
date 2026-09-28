# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 4 | 0 | 0 | 1 | 4 | — |
| mid | 13 | 4 | 1 | 3 | 7 | MISSING (4) |
| edge | 0 | 0 | 0 | 0 | 1 | — |

## Nhận xét

- **Zone có nhiều vấn đề nhất:** 
  * Vùng **mid** là nơi ghi nhận tỷ lệ lỗi và sai lệch cao nhất ở cả nhãn người (**L**) lẫn mô hình (**M**). 
  * Chi tiết dữ liệu từ bảng:
    - **Nhãn người (L):** Bỏ sót tới **4** đối tượng (`L missing` = 4, dạng lỗi chính là `MISSING`) và gắn dư **1** đối tượng (`L spurious` = 1) trên tổng số 13 nhãn tham chiếu (`n_ref` = 13).
    - **Mô hình (M):** Bỏ sót **3** đối tượng (`LR_noM` + `R_only`) và dự đoán thừa tới **7** đối tượng (`LM_noR` + `M_only`).
  * Ở vùng **center** (4 nhãn tham chiếu), nhãn người L đạt độ chính xác tốt (0 missing, 0 spurious), nhưng mô hình M vẫn bị nhiễu nặng với **4** box thừa và bỏ sót **1** đối tượng.
  * Vùng **edge** không có nhãn tham chiếu (`n_ref` = 0) nhưng mô hình M vẫn phát hiện giả **1** box thừa.

- **Giả thuyết nguyên nhân & Giới hạn của slice:**
  * **Giả thuyết nguyên nhân:**
    1. **Độ biến dạng Fisheye ở vùng mid/edge:** Khi đi xa tâm ảnh (vùng `mid`), độ méo dãn góc của ống kính fisheye tăng mạnh làm biến dạng hình học vật thể, khiến mô hình M khó nhận diện chính xác hình dạng và gán nhầm nhiễu nền/bóng râm thành đối tượng (`M thừa` = 7).
    2. **Chất lượng nhãn người ở vùng biên/xa:** Người gán nhãn (L) dễ bỏ sót các đối tượng bị che khuất hoặc có kích thước nhỏ ở vùng `mid` (`L missing` = 4), đồng thời thiếu vùng bỏ qua quy chuẩn (`ignore_region`) cho các vật thể không rõ ràng.
    3. **Thiếu vùng mặt định vị xe (`ego_body`) / quy chuẩn box:** Thiếu quy tắc loại trừ phần thân xe ego hoặc biên giới hạn dẫn đến việc mô hình dự đoán tràn ra các vùng không mong muốn.
  * **Giới hạn của slice:**
    1. **Cách chia Zone phân bố theo tâm kính:** Việc phân chia `center / mid / edge` hoàn toàn dựa trên khoảng cách tương đối tới tâm vòng kính fisheye, **không phản ánh khoảng cách thực tế (gần/xa) của vật thể đối với xe** và cũng **không phân biệt được hướng camera** (trước/sau/bên hông).
    2. **Mẫu dữ liệu quá nhỏ:** Slice chỉ gồm **3 frame** ADASIND nên các con số thống kê mang tính cục bộ, chưa đủ tổng quát để kết luận toàn bộ miền dữ liệu (`E4_model_domain`).
