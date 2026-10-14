# Day26_Track01_ABU

> Dashboard Hành Động Cho Áp Dụng AI v2 — Case: ChatUni — AI Chatbot Tuyển sinh & FAQ cho Đại học

## 1. Thành viên nhóm

| Họ tên | MSSV | Phần phụ trách | Góp ý đã đưa cho nhóm bạn |
|---|---|---|---|
| Nguyễn Hà Khuê | 2A202602938 | Gartner-Lite + phần readiness | Nhóm 3CE: phần readiness mới dán nhãn ĐẠT/THIẾU chưa có căn cứ — đề xuất bổ sung bằng chứng cụ thể (quan sát, log, số liệu) cho từng hạng mục |
| Nguyễn Thị Phương Duyên | 2A202603001 | Mollick + phân chia việc người–AI | Nhóm 3CE: phân chia việc người–AI còn chung chung, chưa chỉ rõ ai giữ quyền duyệt cuối và cơ chế bàn giao khi AI không chắc chắn |
| Đỗ Trọng Bình | 2A202602855 | ADKAR + dashboard chỉ số | Nhóm 3CE: chỉ số "số lượt dùng" là activity metric — đề xuất đổi sang chỉ số chất lượng/giá trị có baseline đo được |
| Nguyễn Văn Thăng | 2A202602835 | Lộ trình 30–60–90 + memo | Nhóm 3CE: lộ trình 30–60–90 còn là danh sách việc — đề xuất bổ sung tiêu chí chuyển giai đoạn (gate) và owner cho từng mốc |


## 2. Phạm vi

- **1 sản phẩm:** ChatUni — AI chatbot trả lời câu hỏi tuyển sinh & FAQ (học phí, ngành học, điểm chuẩn, hồ sơ) cho Đại học, dùng RAG trên tài liệu tuyển sinh chính thống
- **1 nhóm người dùng chính:** Thí sinh/phụ huynh (người hỏi) và cán bộ tư vấn tuyển sinh (người duyệt nội dung, xử lý ca ngoài phạm vi)
- **3 quy trình:** (1) trả lời FAQ tuyển sinh · (2) duyệt log trả lời và cập nhật tài liệu nguồn · (3) chuyển tiếp câu hỏi ngoài phạm vi cho cán bộ tư vấn
- **Vấn đề quan sát được:** Prototype RAG chạy được nội bộ nhưng chưa thể pilot với thí sinh thật — mùa tuyển sinh câu hỏi lặp lại vẫn dồn lên cán bộ tư vấn, thí sinh nhận thông tin chậm ngoài giờ; bộ test 30 golden cases chỉ đạt ~90% đúng, Ban Giám hiệu chưa duyệt pilot vì lo chatbot trả lời sai.

## 3. Nguyên nhân gốc

**1. Độ tin cậy chưa đủ ngưỡng + chưa có cơ chế chặn lỗi khi vận hành** *(trục Tin cậy + Mollick)*
90% đúng là chưa an toàn để mở cho thí sinh thật, nhưng quan trọng hơn: chưa có cơ chế nào chặn 10% sai đó khi vận hành — human-in-the-loop mới thiết kế trên giấy, chưa có điều kiện dừng rõ ràng, thí sinh không có cách phân biệt câu trả lời có nguồn với câu trả lời không chắc chắn. Bot bị định vị "AI tự động" cho toàn bộ FAQ, trong đó có chủ đề rủi ro (học phí, quyền lợi ưu tiên).
*Bằng chứng:* quan sát từ workflow nhóm — bộ test 30 golden cases đạt ~90% (chưa tới ngưỡng mục tiêu 98%); mối lo chính của Ban Giám hiệu là "trả lời sai thông tin tuyển sinh thì ai chịu trách nhiệm".

**2. Governance và readiness THIẾU** *(Gartner-Lite)*
Direction ĐẠT — mục tiêu rõ: giảm tải câu hỏi FAQ lặp lại mỗi mùa tuyển sinh. Nhưng Readiness THIẾU: phòng IT chưa duyệt hạ tầng/bảo mật, tài liệu nguồn chưa có owner và lịch cập nhật khi học phí/điểm chuẩn thay đổi. Absorption THIẾU: chưa có owner vận hành dài hạn, chưa có vòng phản hồi lỗi (sai thì ai sửa, sửa cái gì).
*Bằng chứng:* quan sát stakeholder — Trưởng phòng CNTT chưa ủng hộ (lo gánh nặng vận hành, bảo mật), Ban Giám hiệu (Blocker) chưa duyệt pilot và chưa cấp ngân sách.

*ADKAR:* Ban Giám hiệu nghẽn ở **Desire** (không dám duyệt vì lo rủi ro uy tín — không phải không hiểu vấn đề) → đào tạo không giải quyết, phải xử lý bằng điều kiện dừng rõ ràng và bằng chứng chất lượng; cán bộ tư vấn nghẽn ở **Knowledge/Ability** (chưa thực hành duyệt log trong quy trình thật); **Reinforcement** chưa có (chưa có vòng phản hồi lỗi và theo dõi hành vi mới).

## 4. Cách làm mới (AS-IS / TO-BE)

| AS-IS (trước) | TO-BE (sau) |
|---|---|
| Thí sinh hỏi → chờ cán bộ tư vấn trả lời (hàng nghìn câu lặp mỗi mùa, chậm ngoài giờ) | Thí sinh hỏi chatbot → câu trả lời kèm trích nguồn tài liệu chính thống, tức thì 24/7 |
| Tự tìm trên web — khó biết bản học phí/điểm chuẩn mới nhất | Câu trả lời luôn truy vết về tài liệu nguồn có owner và version |
| Câu hỏi ngoài phạm vi → cán bộ phải xử lý lẫn vào FAQ | Câu hỏi ngoài phạm vi/không chắc chắn → tự động chuyển cán bộ tư vấn |
| Sai không ai biết, không ai sửa | Cán bộ duyệt log hằng ngày trong beta → log sai → sửa tài liệu nguồn |

**Ba thay đổi bắt buộc:**
1. **Nguồn kiểm chứng:** mọi câu trả lời phải trích đoạn tài liệu gốc — không có nguồn thì không trả lời; tài liệu nguồn có owner (TP Tuyển sinh) và lịch cập nhật
2. **Người chịu trách nhiệm:** cán bộ tư vấn duyệt log trả lời hằng ngày trong beta (Mollick: AI hỗ trợ nháp + trích nguồn, người kiểm); phạm vi nội dung do TP Tuyển sinh duyệt
3. **Cách xử lý khi AI không chắc chắn:** ngưỡng tin cậy thấp hoặc ngoài phạm vi → chuyển cán bộ; điều kiện dừng pilot: tỷ lệ sai >2% trong 1 tuần

**Phân chia việc (Mollick):** AI tự động chỉ với FAQ rủi ro thấp đã có nguồn kiểm chứng; AI hỗ trợ (nháp + trích nguồn) cho phần còn lại; người giữ quyền: cán bộ tư vấn và TP Tuyển sinh duyệt nội dung, chịu trách nhiệm câu trả lời cuối.

## 5. Chỉ số

Chi tiết đủ 5 cột xem `dashboard/dashboard_hanh_dong_v2.xlsx` (bản v1 để đối chiếu tại `v1/`).

| Loại | Chỉ số | Mốc đầu | Mục tiêu | Nguồn dữ liệu | Phụ trách | Khi chỉ số xấu |
|---|---|---|---|---|---|---|
| Product | Tỷ lệ câu trả lời được xác minh đúng với tài liệu nguồn | 90% (bộ test 30 câu hiện tại) | ≥98% | Eval golden cases + audit mẫu log | AI Engineer (đo) / TP Tuyển sinh (duyệt) | Thu hẹp phạm vi chủ đề + sửa tài liệu nguồn |
| Workflow (hành vi) | Tỷ lệ câu hỏi ngoài phạm vi được chuyển đúng cho cán bộ tư vấn | Audit tuần beta đầu | 100% | Log bot | Team Lead | Rà lại ngưỡng handoff và bộ phân loại chủ đề |
| Workflow (năng suất) | Thời gian từ thí sinh gửi câu hỏi đến có câu trả lời (câu trong phạm vi) | Đo hiện trạng kênh fanpage/Zalo tuần 1 | ≤1 phút | Log kênh + log bot | TP Tuyển sinh | Xem lại quy trình và thời gian phản hồi handoff |
| Chất lượng | Số câu trả lời sai bị phát hiện mỗi tuần (duyệt log + phản hồi người dùng) | Ghi tuần đầu beta | 0 sau 2 tuần liên tục | Log duyệt + phản hồi | Team Lead | Kích hoạt điều kiện dừng, điều tra nguồn trong 48h |
| Giá trị | Tỷ lệ câu FAQ được chatbot xử lý không cần cán bộ can thiệp | Đo trong beta | ≥70% | Log bot + log ticket | TP Tuyển sinh (chủ nghiệp vụ) | Điều chỉnh phạm vi chủ đề hoặc dừng |

## 6. Quyết định

**SỬA — tiếp tục beta có kiểm soát, dừng mở rộng công khai cho đến khi đủ điều kiện.**

- **Lý do một câu:** 90% đúng với thông tin tuyển sinh là chưa đủ để mở cho thí sinh thật, nhưng khoảng cách nằm ở cơ chế kiểm soát (human-in-the-loop, điều kiện dừng, governance nguồn) chứ không phải ở mô hình — nên sửa cách vận hành trước khi mở rộng.
- **2 thay đổi so với v1:** (1) thay activity metric (số tin nhắn bot xử lý, số câu trả lời) bằng product/workflow metric ra quyết định được (tỷ lệ đúng nguồn, tỷ lệ handoff đúng, thời gian phản hồi); (2) gán owner theo vai cụ thể và bổ sung hành động khi chỉ số xấu cho từng dòng — bản v1 để owner chung chung "IT" và cột hành động trống.
- **Bước tiếp theo:** nâng bộ golden cases lên 100 câu hỏi thật và chốt hạ tầng pilot với phòng IT trong 30 ngày đầu — chi tiết trong `memo/memo_quyet_dinh.md`.

## Cấu trúc repo

```
Day26_Track01_ABU/
├── README.md
├── dashboard/
│   └── dashboard_hanh_dong_v2.xlsx   ← bản v2 sau kiểm tra chéo
├── memo/
│   └── memo_quyet_dinh.md
└── v1/
    └── dashboard_hanh_dong_v1.xlsx   ← bản trước phản biện, để đối chiếu
```
