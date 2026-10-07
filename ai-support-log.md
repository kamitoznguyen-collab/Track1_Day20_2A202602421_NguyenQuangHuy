# AI Support Log

Công cụ: **Gemini (Antigravity IDE)**.

## AI đã giúp tôi ở đâu?

- **Gemini:** Đọc hiểu tài liệu đề bài, lập kế hoạch chi tiết `KE_HOACH_LAM_BAI.md` và tạo khung thư mục.
- **Gemini:** Kết nối Codebase Memory để đọc mã nguồn dự án P-143 (cấu trúc DB, bảng `approvals`, `upload_events`, workflow duyệt `PROPOSED → ACTIVE`), đảm bảo rằng các event đề xuất trong mục 06 hoàn toàn tính được từ hệ thống đang có.
- **Gemini:** Soạn bản nháp chi tiết `metrics-pack.md` (mục 00–07) và `README.md` dựa trên PRD và mã nguồn; đóng vai "phản biện" để kiểm tra tính hợp lý của core action và nhịp độ (cadence).

## AI sai, hời hợt hoặc đề xuất metric sai nature ở đâu?

- Trong lúc brainstorm, AI ban đầu có xu hướng dễ gợi ý chọn "tải hóa đơn lên" làm core action và đo lường theo ngày (daily active) vì thấy kế toán "nhận hóa đơn mỗi ngày". Tôi đã chặn hướng này vì hành vi đó bị phụ thuộc vào tần suất của nhà cung cấp thay vì giá trị (đã ghi nhận trong bảng Revision mục 07).
- Các ngưỡng đề xuất (vd: 20 hóa đơn / 14 ngày cho activation, ≥ 50% cho retention, ≤ 2 ngày cho NSM) chỉ là các con số giả định hợp lý, chưa có dữ liệu thật. Không có benchmark chuẩn xác cho tính năng hệ thống này nên bài không đặt mục tiêu số, cần pilot để hiệu chỉnh.
- AI đề xuất tên property `po_matched_by` trong sự kiện `invoice_reconciled` cho đồng bộ, nhưng thực tế trong code hiện tại chỉ có field `matched_by` ở cấp độ từng dòng hóa đơn.

## Tôi đã tự sửa hoặc quyết định lại điều gì?

- Tôi quyết định chọn sử dụng dự án P-143 (dự án thực tế của nhóm) thay vì làm 3 dự án giả định mà đề bài đưa ra.
- Các quyết định lõi (chốt core action cuối cùng, diễn giải lý do chọn cadence, công thức NSM, metric hypothesis) là do tôi chốt lại dựa trên logic sản phẩm để có thể bảo vệ được trước coach.
