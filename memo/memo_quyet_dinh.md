# Memo Quyết Định — ChatUni (AI Chatbot Tuyển sinh & FAQ cho Đại học)

**Ngày:** 14/10/2026
**Người soạn:** Nhóm ABU (Day26_Track01_ABU)
**Vấn đề quyết định:** Có nên cho ChatUni pilot với thí sinh thật, sửa cách triển khai, hay dừng?

---

## 1. Vấn đề và nguyên nhân gốc

**Vấn đề:** ChatUni — chatbot FAQ tuyển sinh dùng RAG trên tài liệu chính thống — đã chạy được ở mức prototype nội bộ, nhưng chưa thể đưa vào pilot với thí sinh thật. Bộ test 30 golden cases chỉ đạt ~90% trả lời đúng (chưa tới ngưỡng mục tiêu 98%), Ban Giám hiệu chưa duyệt pilot vì lo chatbot trả lời sai thông tin tuyển sinh gây rủi ro uy tín. Trong khi đó mỗi mùa tuyển sinh, hàng nghìn câu hỏi lặp lại vẫn dồn lên cán bộ tư vấn và thí sinh nhận thông tin chậm ngoài giờ hành chính.

**Phạm vi phân tích:**
- Sản phẩm: ChatUni — AI chatbot trả lời câu hỏi tuyển sinh & FAQ (học phí, ngành học, điểm chuẩn, hồ sơ)
- Người dùng chính: thí sinh/phụ huynh (người hỏi) và cán bộ tư vấn tuyển sinh (người duyệt nội dung, xử lý ca ngoài phạm vi)
- Quy trình liên quan (3): (1) trả lời FAQ tuyển sinh · (2) duyệt log trả lời và cập nhật tài liệu nguồn · (3) chuyển tiếp câu hỏi ngoài phạm vi cho cán bộ tư vấn

**Nguyên nhân gốc 1 — Độ tin cậy chưa đủ ngưỡng + chưa có cơ chế chặn lỗi khi vận hành** *(trục Tin cậy + Mollick)*
90% đúng là chưa an toàn để mở cho thí sinh thật, nhưng quan trọng hơn: chưa có cơ chế nào chặn 10% sai đó khi vận hành. Human-in-the-loop (nhân viên tư vấn duyệt log hằng ngày) mới là thiết kế trên giấy, chưa chạy thật; chưa có điều kiện dừng được thống nhất; thí sinh không có cách phân biệt câu trả lời có nguồn với câu trả lời không chắc chắn. Bot bị định vị "AI tự động" cho toàn bộ FAQ, trong đó có chủ đề rủi ro (học phí, quyền lợi ưu tiên) — vi phạm nguyên tắc phân vùng của Mollick.

**Nguyên nhân gốc 2 — Governance và readiness THIẾU** *(Gartner-Lite)*
Direction ĐẠT — mục tiêu rõ: giảm tải câu hỏi FAQ lặp lại mỗi mùa tuyển sinh. Nhưng Readiness THIẾU: phòng IT chưa duyệt hạ tầng/bảo mật; tài liệu nguồn chưa có owner và lịch cập nhật khi học phí/điểm chuẩn thay đổi (câu trả lời đúng hôm nay có thể sai sau khi trường đổi học phí). Absorption THIẾU: chưa có owner vận hành dài hạn, chưa có vòng phản hồi lỗi (bot sai thì ai phát hiện, ai sửa nguồn, trong bao lâu).

## 2. Framework đã dùng và bằng chứng

| Framework | Dùng để | Kết luận |
|---|---|---|
| Gartner-Lite | Đánh giá sẵn sàng tổ chức | Direction ĐẠT · Readiness THIẾU (hạ tầng IT, governance tài liệu nguồn) · Absorption THIẾU (owner vận hành, vòng phản hồi lỗi) → sửa readiness trước, chưa mở rộng |
| Mollick | Phân chia việc người–AI | Sai vùng: bot bị định vị "AI tự động" cho toàn bộ FAQ, gồm cả chủ đề rủi ro. Đúng ra: AI tự động chỉ với FAQ rủi ro thấp đã kiểm chứng; AI hỗ trợ (nháp + trích nguồn) phần còn lại; cán bộ tư vấn và TP Tuyển sinh giữ quyền duyệt và chịu trách nhiệm |
| ADKAR | Chẩn đoán người dùng | Ban Giám hiệu nghẽn ở Desire (lo rủi ro uy tín — không phải thiếu hiểu biết) → đào tạo không giải quyết, phải xử lý bằng điều kiện dừng và bằng chứng chất lượng; cán bộ tư vấn nghẽn ở Knowledge/Ability (chưa thực hành duyệt log trong quy trình thật); Reinforcement chưa có (chưa có vòng phản hồi lỗi) |

**Bằng chứng:**
1. **Quan sát từ workflow nhóm (Day25):** bộ test 30 golden cases đạt ~90% trả lời đúng có trích nguồn — chưa đạt ngưỡng 98% mà chính nhóm đặt làm điều kiện pilot.
2. **Quan sát stakeholder (Day25):** Ban Giám hiệu (Blocker) chưa duyệt pilot và chưa cấp ngân sách vì lo trả lời sai; Trưởng phòng CNTT chưa ủng hộ vì gánh nặng vận hành/bảo mật — hai điểm nghẽn readiness còn nguyên cho đến hôm nay.
3. **Bài học tham khảo (từ tài liệu lab):** Morgan Stanley — độ tin cậy và tuân thủ phải đi trước mở rộng; DWP/GDS — cách đo quyết định độ tin cậy, số tự khai không đáng tin bằng nhóm so sánh.

## 3. Thay đổi sau phản biện (≥2) — nhóm phản biện: 3CE

| # | Góp ý từ nhóm phản biện | Thay đổi trong v2 |
|---|---|---|
| 1 | Dashboard v1 chỉ đo "số tin nhắn bot xử lý" và "số câu trả lời" — activity metric, không chứng minh được độ tin cậy hay giá trị | Thay bằng **product metric**: "Tỷ lệ câu trả lời được xác minh đúng với tài liệu nguồn" (eval + audit mẫu) và **workflow metrics**: "Tỷ lệ câu hỏi ngoài phạm vi được chuyển đúng cho cán bộ" + "Thời gian từ câu hỏi đến câu trả lời" |
| 2 | v1 để owner chung chung ("IT") và cột "khi chỉ số xấu" trống — không dẫn tới hành động nào | Gán owner theo vai cụ thể (AI Engineer, TP Tuyển sinh, Team Lead) và bổ sung **hành động cụ thể khi chỉ số xấu** cho từng dòng, có điều kiện dừng rõ ràng |
| 3 | Chỉ số "mức độ hài lòng khách" không thu được số liệu đáng tin trong giai đoạn beta | Bỏ, thay bằng chỉ số đo được từ log có sẵn: số câu sai bị phát hiện mỗi tuần và tỷ lệ FAQ xử lý không cần cán bộ can thiệp |

## 4. Quyết định

**SỬA — tiếp tục beta có kiểm soát, dừng mở rộng công khai cho đến khi đủ điều kiện.**

ChatUni chỉ mở pilot công khai cho thí sinh khi: (1) tỷ lệ trả lời đúng nguồn ≥98% duy trì 2 tuần liên tục trên bộ golden cases mở rộng; (2) cán bộ tư vấn duyệt log hằng ngày trong toàn bộ beta; (3) phòng IT duyệt hạ tầng (container cô lập, rollback); (4) điều kiện dừng được chốt trước: tỷ lệ sai >2% trong 1 tuần thì tạm dừng. Không thêm chủ đề mới vào phạm vi bot khi chưa đạt điều kiện trên.

**Lý do (một câu):** 90% đúng với thông tin tuyển sinh là chưa đủ để mở cho thí sinh thật, nhưng khoảng cách nằm ở cơ chế kiểm soát (human-in-the-loop, điều kiện dừng, governance nguồn) chứ không phải ở mô hình — nên sửa cách vận hành trước khi mở rộng.

## 5. Bước tiếp theo và owner

| Giai đoạn | Việc chính | Owner | Dấu hiệu hoàn thành (gate) |
|---|---|---|---|
| 0–30 ngày | Ghi baseline hiện trạng: số câu FAQ lặp/tháng, thời gian trả lời trung bình kênh fanpage/Zalo; nâng bộ golden cases lên 100 câu hỏi thật; chốt hạ tầng pilot với phòng IT; chỉ định owner tài liệu nguồn (TP Tuyển sinh) | Team Lead / PM | Có baseline + owner nguồn được chỉ định + IT duyệt hạ tầng |
| 31–60 ngày | Chạy beta 4 tuần có human-in-the-loop (cán bộ duyệt log hằng ngày); eval tự động mỗi release; theo dõi tỷ lệ đúng nguồn, tỷ lệ handoff; sửa tài liệu nguồn theo log sai | AI Engineer + TP Tuyển sinh | ≥98% đúng nguồn 2 tuần liên tục; 100% câu ngoài phạm vi được handoff |
| 61–90 ngày | Go-live pilot công khai với thí sinh thật; so với target; chốt owner vận hành dài hạn; audit governance (phân quyền, log, lưu trữ); quyết định mở rộng / sửa / dừng | Ban Giám hiệu + TP Tuyển sinh | Quyết định mở rộng có căn cứ trên dữ liệu 60 ngày beta + pilot |

**Điều kiện mở rộng:** chất lượng (≥98% đúng nguồn), hành vi (handoff đúng 100%) và giá trị (thời gian phản hồi, tỷ lệ FAQ tự xử lý) đạt mục tiêu nhóm đã xác nhận — không phải khi hết 90 ngày.
