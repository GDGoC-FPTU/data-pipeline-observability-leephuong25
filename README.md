# Day 10 — Data Pipeline & Observability (Lab)

**Student ID:** AI20K-2A202600107
**Name:** Lê Thị Phương

---

## Mô tả ngắn
Triển khai một pipeline ETL đơn giản đọc dữ liệu từ `raw_data.json`, thực hiện kiểm tra chất lượng (validation), biến đổi dữ liệu (transform) và lưu kết quả ra `processed_data.csv`. Bài tập đồng thời đánh giá yếu tố observability: in logging (số record processed/dropped) và thêm timestamp `processed_at` vào kết quả.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
```bash
python agent_simulation.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline
├── experiment_report.md     # Bao cao thi nghiem
├── agent_simulation.py      # Script de so sanh Clean va Garbage data
├── garbage_data.csv         # Du lieu nhiem nhieu de test
└── README.md                # File nay
```

---

## Ket qua

Pipeline đã xử lý 5 bản ghi từ `raw_data.json`, giữ lại 3 bản ghi hợp lệ và loại 2 bản ghi không hợp lệ.
- 3 bản ghi hợp lệ được ghi vào `processed_data.csv`
- 2 bản ghi bị loại vì: `price <= 0` và `missing category`
