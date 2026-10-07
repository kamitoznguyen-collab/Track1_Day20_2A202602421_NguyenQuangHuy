# Track1 · Day 20 — Product Metrics Pack

| | |
|---|---|
| **Họ tên** | Nguyễn Quang Huy |
| **MHV** | 2A202602421 |
| **Dự án chọn làm** | P-143 — Đối soát hóa đơn 3 chiều (hóa đơn mua vào ↔ PO ↔ phiếu nhập kho), dự án nhóm Fantastic4 |
| **Use case** | Kế toán viên công nợ phải trả xử lý hóa đơn nhà cung cấp tới khi được duyệt thanh toán |
| **Metrics Pack** | [metrics-pack.md](metrics-pack.md) (mục 00–07, sơ đồ loop vẽ bằng Mermaid, GitHub hiển thị trực tiếp) |
| **AI Support Log** | [ai-support-log.md](ai-support-log.md) |

## Tóm tắt chuỗi quyết định

| Bước | Kết quả |
|---|---|
| Core action | KTV **ra quyết định cấp 1** (duyệt / trả lại / từ chối) cho một hóa đơn đã đối chiếu — không phải tải lên, không phải xếp loại của hệ thống |
| Cadence | Phản ứng theo sự kiện (hóa đơn về) → đo theo **tuần làm việc có hóa đơn về**, ở **cấp tổ chức** |
| NSM | Số hóa đơn được quyết định cấp 1 trong **≤ 2 ngày làm việc** và **không bị đảo**, mỗi tuần, mỗi tổ chức |
| Retention | Tổ chức · cohort tuần activation · return = `invoice_l1_decided` · tuần làm việc (bỏ tuần không có hóa đơn) · ≥ 50% hóa đơn của tuần · doanh nghiệp mua theo PO |
| Loop | Workflow loop: sửa dòng khớp sai → quy tắc theo nhà cung cấp → lô sau tự khớp nhiều hơn, xong nhanh hơn |
| Tracking | 8 event, đều suy được từ dữ liệu P-143 đang có; 3 tiêu chí nghiệm thu (không trùng, không đếm file trùng, loại trừ + múi giờ) |

## Điều tôi mang về áp dụng cho dự án thật

1. **Đổi dashboard của P-143 từ "đếm" sang NSM.** Màn Tổng quan đang đếm số hóa đơn theo trạng thái. Thêm một ô NSM
   (quyết định trong ≤ 2 ngày làm việc, không bị đảo) và counter-metric "tỷ lệ duyệt nhầm" đặt ngay cạnh, để không ai
   tối ưu tốc độ bằng cách duyệt mù.
2. **Tracking không cần SDK mới.** Audit log `STATUS_CHANGED`, bảng `approvals`, `upload_events`, `line_match_overrides`
   và trạng thái quy tắc nhà cung cấp đã đủ để tính cả 8 event. Việc cần làm là một view SQL theo tuần làm việc giờ Việt
   Nam, có loại trừ tổ chức demo.
3. **Đầu tư vào phần "loop" thay vì thông báo.** Giả thuyết đáng kiểm nhất là quy tắc theo nhà cung cấp làm lô sau nhanh
   hơn. Pilot cần đo tỷ lệ dòng tự khớp theo lô thứ 1→4 của cùng nhà cung cấp trước khi làm thêm tính năng nhắc việc.
4. **Activation phải gắn với lô hóa đơn thứ hai,** không phải lần đăng nhập đầu: onboarding pilot nên giúp khách nạp đủ
   PO + phiếu nhập trước khi hóa đơn về, vì thiếu chứng từ là lý do lớn nhất khiến core action không xảy ra.
