# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | MISSING | 1 |
| center | B4 | SPURIOUS | 4 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B4 | SPURIOUS | 1 |
| mid | B4 | MISSING | 8 |
| mid | B4 | SPURIOUS | 9 |
| mid | C0 | MISSING | 1 |
| unknown | B4 | Ch? co nhan L | 14 |

## Top defects
- SPURIOUS: 15 (ví dụ frame adasind_019560.jpg)
- Ch? co nhan L: 14 (ví dụ frame adasind_249480)
- MISSING: 11 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: 
Nguyên nhân khả dĩ (why):

Lỗi chủ quan của Annotator: Người gán nhãn bỏ sót các đối tượng thực tế xuất hiện ở vùng mid zone (rìa đường hoặc bị khuất một phần) do thiếu cẩn thận khi kiểm tra mắt thường.

Không nắm rõ quy chuẩn kích thước/độ rõ: Gán thừa đối tượng ở quá xa/mờ không đủ tiêu chuẩn (L3), nhưng lại bỏ sót các đối tượng rõ ràng hơn ở lề đường (R5, R6).

- Cách sửa và ai nhận việc (`owner`): 
Cách sửa: Thực hiện chỉnh sửa (rework) file nhãn Reference L. Xóa box L3 thừa, bổ sung đầy đủ các box bị sót (R5, R6 trong frame adasind_261480.jpg và R5+M7 trong frame adasind_265065.jpg).

Người nhận việc (owner): annotator (Đội ngũ gán nhãn).

- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): 
Dòng trong findings.csv:

r3_diag,B4-mid,adasind_261480.jpg,L3,L_only,SPURIOUS,E0_reference_defect,P1,annotator,L_only,compare_html,rework,v1

r3_diag,B4-mid,adasind_261480.jpg,R5,R_only,MISSING,E0_reference_defect,P1,annotator,R_only,compare_html,rework,v1

r3_diag,B4-mid,adasind_261480.jpg,R6,R_only,MISSING,E0_reference_defect,P1,annotator,R_only,compare_html,rework,v1

r3_diag,B4-mid,adasind_265065.jpg,R5+M7,RM_noL,MISSING,E0_reference_defect,P1,annotator,RM_noL,compare_html,rework,v1
