# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu.

- Mã học viên theo lớp: 2A202604040
- Ngày / CVAT local: 11/11/2026 - 12/11/2026 / CVAT local 2.x
- Công cụ đã dùng: CVAT local (Brush, Polygon, Eraser) kết hợp AI assistance (YOLOv8-seg)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh LMS đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, chiếc xe taxi màu vàng nằm ở làn giữa ngay sau người đi bộ.
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Dùng công cụ Brush bo viền bám sát theo mép thân xe, gương chiếu hậu và lốp xe tiếp xúc mặt đường; loại bỏ phần bóng đen đổ trên mặt đường bên dưới gầm xe.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Ở các đối tượng sau khi dùng AI hỗ trợ, gợi ý thường bị lỗi ăn lấn bóng đen gầm xe xuống mặt đường và gộp dính 2 xe sát nhau thành 1. Tôi đã dùng công cụ Eraser/Brush để gọt lại mép thân xe và tách rời từng instance độc lập.
- Nếu không dùng gợi ý: không áp dụng.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `medium_instance`, ảnh `000000181542.jpg`, người đi bộ cắt ngang trước đầu xe taxi vàng.
- Lỗi thuộc loại: gộp-tách / lỗi che khuất (Occlusion).
- Bằng chứng tôi nhìn thấy: Khi người đi bộ cắt ngang qua thân xe taxi vàng, chiếc xe bị chia cắt thành 2 mảng nhìn thấy tách rời nhau (nửa trái và nửa phải). Ban đầu hệ thống tạo thành 2 object `car` riêng biệt.
- Quy tắc và hành động sửa: Theo quy tắc Occlusion (che khuất) của Instance Segmentation, cùng một vật thể dù bị che khuất chia làm nhiều phần nhìn thấy rời nhau vẫn là 1 instance duy nhất, chỉ vẽ phần nhìn thấy. Đã xóa 1 object và dùng Brush vẽ cả 2 nửa thân xe taxi vàng vào chung 1 Object `car`.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại thành công file `medium_instance.zip` chuẩn định dạng `COCO 1.0`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Chạy `python scripts/inspect_submissions.py --dir submissions` báo `[OK]` cho toàn bộ 9 task, ghi nhận 53 annotations cho `medium_instance`, 17 annotations cho `hard_panoptic` và đầy đủ các checkpoints.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. Ảnh `000000460147.jpg` (Hard Panoptic): Các trụ cột đèn / lan can hai bên thành cầu | Gán vào `traffic light` / `building` hay để trống? | Schema của `hard_panoptic` chỉ có 12 class và không có nhãn `pole`. Quy tắc đề bài: "Không tự bịa nhãn, pixel ngoài schema để trống". | Quyết định để trống (thuộc unlabelled background), không gán nhầm sang `traffic light`. |
| 2. Ảnh `000000350023.jpg` (Hard Panoptic): Xe lồng chở nhiều ô tô con ở làn giữa | Gán toàn bộ là 1 `truck` hay bóc tách từng xe con trên giá chở là `car`? | Xe chuyên dụng vận chuyển hàng hóa, các xe trên thùng được xếp vào nhóm hàng hóa chuyên chở. | Quyết định vẽ bao trùm toàn bộ thành 1 Object `truck` để đảm bảo tính toàn vẹn của phương tiện vận tải. |
| 3. Ảnh `000000350023.jpg` (Hard Panoptic): Dòng xe tắc đường ở tít xa chân trời | Có nên cố tách từng đốm xe siêu nhỏ hay dừng lại? | Quy tắc ngưỡng kích thước và độ rõ nét trong segmentation: các xe ở quá xa bị nhòe và dính chùm không đủ độ phân giải để tách instance chính xác. | Quyết định không tạo instance cho cụm xe quá xa để tránh gán nhãn suy đoán sai lệch, giữ nền `road`. |

