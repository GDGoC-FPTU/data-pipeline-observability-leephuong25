
---

# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-XXXX
**Name:** Lê Thị Phương
**Date:** 15/04/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is Laptop at $1200." | 9 | Gợi ý chính xác và thực tế. Agent dựa trên tập dữ liệu đã qua ETL (được làm sạch, chuẩn hóa các trường thông tin và tính toán sẵn `discounted_price`). |
| Garbage Data (`garbage_data.csv`) | "Based on my data, the best choice is Nuclear Reactor at $999999." | 1 | Gợi ý hoàn toàn vô nghĩa và phi thực tế. Agent bị đánh lừa bởi dữ liệu lỗi, không được lọc bỏ trước khi đưa vào mô hình. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi phân tích trực tiếp file `garbage_data.csv`, có thể thấy AI Agent đã bị "đầu độc" thông tin (Data Poisoning) dẫn đến việc đưa ra câu trả lời sai lệch hoàn toàn. Cụ thể, bộ dữ liệu rác này chứa hàng loạt các vấn đề nghiêm trọng vi phạm tính toàn vẹn của dữ liệu:

1.  **Outliers (Giá trị ngoại lai/Dữ liệu bị đầu độc):** Bản ghi số 3 (`Nuclear Reactor` với giá `999999`) là một điểm dị thường cực đoan. Vì AI tính toán dựa trên các con số thay vì "nhận thức thực tế", một sản phẩm có giá trị cao bất thường có thể làm thay đổi hoàn toàn trọng số phân tích của Agent, khiến nó ưu tiên gợi ý món đồ vô lý này.
2.  **Wrong Data Types (Sai kiểu dữ liệu):** Ở bản ghi số 2 (`Broken Chair`), giá trị cột price lại là chuỗi text (`"ten dollars"`) thay vì kiểu số (integer/float). Điều này phá hỏng các hàm tính toán toán học (như tính toán chiết khấu, so sánh giá) của Agent.
3.  **Duplicate IDs (Trùng lặp định danh):** Mã ID `1` được dùng chung cho cả `Laptop` và `Banana`. Việc trùng lặp khóa chính (Primary Key) làm Agent mất khả năng phân biệt các thực thể độc lập, gây nhầm lẫn trong quá trình truy xuất ngữ cảnh.
4.  **Null Values (Thiếu hụt dữ liệu):** Bản ghi cuối cùng (`Ghost Item`) bị trống hoàn toàn ở cột ID và Category. Khi các trường thuộc tính quan trọng bị rỗng, Agent sẽ thiếu cơ sở để phân loại và lọc sản phẩm, sinh ra các lỗi ẩn (silent errors) trong quá trình xử lý logic.

Tất cả những lỗi này hội tụ lại tạo ra nguyên lý **GIGO (Garbage In, Garbage Out)** – Dữ liệu đầu vào là rác thì kết quả trả về chắc chắn là rác, bất kể AI có thông minh đến đâu.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Hoàn toàn đồng ý.

Thí nghiệm này chứng minh rõ ràng rằng Dữ liệu chất lượng cao (Clean Data) quan trọng hơn một câu Lời nhắc (Prompt) hay. Khi chạy trên bộ `processed_data.csv` đã qua xử lý (lọc lỗi, chuẩn hóa kiểu dữ liệu, tính toán sẵn `discounted_price`), Agent đưa ra tư vấn chính xác. Ngược lại, chỉ cần một vài bản ghi lỗi trong `garbage_data.csv` (như sai kiểu, trùng ID, hoặc điểm ngoại lai Nuclear Reactor), toàn bộ hệ thống logic của Agent lập tức sụp đổ. 

Prompt có thể định hướng cách AI trình bày, nhưng **Dữ liệu mới là nguồn sự thật (Ground Truth) định hình tư duy của AI**. Việc xây dựng Data Pipeline để đảm bảo Data Quality là bước sống còn trước khi triển khai bất kỳ hệ thống AI/Agent nào.