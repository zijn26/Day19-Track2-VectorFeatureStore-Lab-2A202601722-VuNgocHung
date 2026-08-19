# Reflection — Lab 19

**Path đã chạy:** lite

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

- **`exact` queries** (96.7%): BM25 và Hybrid ngang nhau vì query chứa đúng thuật ngữ verbatim trong văn bản.
- **`paraphrase` queries** (33.3% BM25 vs 24.0% Semantic vs 32.0% Hybrid): Với model `bge-small-en` (tiếng Anh baseline), semantic recall trên tiếng Việt paraphrase bị giảm điểm (chuyển sang `bge-m3` ở Docker path sẽ giúp semantic vượt trội).
- **`mixed` queries** (100.0% Hybrid vs 97.0% BM25 vs 98.5% Semantic): **Hybrid (RRF k=60) thắng tuyệt đối 100.0%** nhờ kết hợp tín hiệu khớp từ khóa chính xác và tương đồng ngữ nghĩa.
- **Khi nào KHÔNG dùng Hybrid**: 
  1. Khi cần latency siêu thấp (< 5ms): Pure BM25 nhanh gấp 7 lần (4ms vs 29ms).
  2. Khi tra cứu mã ID/SKU/tên riêng chính xác: Pure BM25 là đủ và tránh được nhiễu vector.

---

## Điều ngạc nhiên nhất khi làm lab này

Công thức RRF ($1 / (k + rank)$) tuy rất đơn giản nhưng mang lại khả năng tổng quát hóa cực tốt trên mọi nhóm câu hỏi thực tế.
