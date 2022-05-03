## Hướng dẫn xây dựng mô hình nhận dạng tiếng nói tự động sử dụng bộ công cụ của Kaldi
- Trang chủ chính của Kaldi: http://kaldi-asr.org/
- Trang hướng dẫn cấu trúc thư mục của Kaldi: https://www.eleanorchodroff.com/tutorial/kaldi/introduction.html
- Trang giải thích các mô hình trong Kaldi: https://medium0.com/@jonathan_hui/speech-recognition-kaldi-35fec0320496
- Trang hướng dẫn cách chạy Kaldi: http://jrmeyer.github.io/asr/2016/01/26/Installing-Kaldi.html

Kaldi là một bộ công cụ nhận dạng giọng nói tự động (ASR) hiện đại, chứa hầu hết mọi thuật toán hiện đang được sử dụng
trong các hệ thống ASR. Nó cũng chứa các công thức để đào tạo các mô hình âm thanh của riêng bạn trên kho ngữ liệu thường
được sử dụng như Wall Street Journal Corpus, TIMIT, v.v. Các công thức này cũng có thể dùng làm khuôn mẫu để đào tạo các
mô hình âm thanh trên dữ liệu giọng nói của riêng bạn.

### 1. Cấu trúc thư mục của Kaldi


