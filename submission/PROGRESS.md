# Tiến độ thực chạy — 2026-10-08

## Đã hoàn thành

- NB0: cài `my_dpo_loss` bằng `logsigmoid`, chạy hết notebook trên CPU và giữ output. Kiểm tra trả về loss ban đầu 0,6931 = ln 2; `scripts/test_lab22.py` và toàn bộ 54 test CPU đều qua.
- NB2: tải đúng `sailor2/sea-ultrafeedback-onpolicy`, lọc tiếng Việt, dùng tokenizer Qwen3-4B mặc định và `MAX_LEN=768`. Tách được 800 cặp train / 100 cặp held-out, đã xác nhận không trùng prompt. Median độ dài: chosen 94 token, rejected 86 token; chosen dài hơn trong 65,875% cặp. Lưu split, `stats.json`, notebook có output và `02b-pref-length.png`.
- Ba cặp đầu được đọc thủ công: cặp 1 có hai câu khá giống nhau và chosen dài hơn một chút; cặp 2 chỉ khác nhãn ngắn nên khó xác nhận sở thích nếu thiếu ngữ cảnh tiếng Tây Ban Nha; cặp 3 rejected dài hơn và đưa liên kết cụ thể, vì thế không thể kết luận chosen luôn tốt hơn chỉ từ độ dài. Các nhãn có nhiễu; 65,875% là cảnh báo cần kiểm tra thêm khi đánh giá.

## Chưa có bằng chứng thực nghiệm

NB1, NB3 và NB4 chưa chạy. Máy hiện tại có RTX 4060 8 GB, trong khi `HARDWARE-GUIDE.md` ước tính DPO Qwen3-4B cần khoảng 9–12 GB VRAM và môi trường PyTorch hiện là bản CPU. Vì vậy chưa có SFT adapter, mô hình SFT gộp, DPO adapter, reward curves, câu trả lời so sánh, kết quả judge hoặc số liệu để điền `REFLECTION.md`. Không tạo số liệu thay thế.

## Cách hoàn tất trên Colab T4

1. Mở `colab/Lab22_DPO_T4.ipynb`, chọn T4 GPU, chạy phần setup rồi NB0–NB4 theo thứ tự. Bản Colab đã có lời giải NB0.
2. Trước khi phiên hết hạn, tải `submission/screenshots/`, `data/eval/`, các file `.json` trong `adapters/dpo/`, `data/pref/*.parquet` và notebook có output. Khi đưa kết quả Colab vào repo, thay cả hai file Parquet cùng lúc để chúng khớp dấu vân tay mà NB3 ghi vào `split.json`.
3. Điền `submission/REFLECTION.md` từ các file số liệu thật. Chạy `python scripts/verify.py` (hoặc `make verify` trên Linux/Colab) và chỉ nộp khi lệnh kết thúc thành công.

`python scripts/verify.py` hiện báo thiếu đúng các bằng chứng NB1, NB3, NB4 và các mục tương ứng trong bài phản tư.
