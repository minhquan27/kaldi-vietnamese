## Hướng dẫn xây dựng mô hình nhận dạng tiếng nói tự động sử dụng bộ công cụ của Kaldi
- Trang chủ chính của Kaldi: http://kaldi-asr.org/
- Trang hướng dẫn cấu trúc thư mục của Kaldi: https://www.eleanorchodroff.com/tutorial/kaldi/introduction.html
- Trang giải thích các mô hình trong Kaldi: https://medium0.com/@jonathan_hui/speech-recognition-kaldi-35fec0320496
- Trang hướng dẫn cách chạy Kaldi: http://jrmeyer.github.io/asr/2016/01/26/Installing-Kaldi.html

Kaldi là một bộ công cụ nhận dạng giọng nói tự động (ASR) hiện đại, chứa hầu hết mọi thuật toán hiện đang được sử dụng
trong các hệ thống ASR. Nó cũng chứa các công thức để đào tạo các mô hình âm thanh của riêng bạn trên kho ngữ liệu thường
được sử dụng như Wall Street Journal Corpus, TIMIT, v.v. Các công thức này cũng có thể dùng làm khuôn mẫu để đào tạo các
mô hình âm thanh trên dữ liệu giọng nói của riêng bạn.


### 1. Cài đặt Kadli
git clone https://github.com/kaldi-asr/kaldi.git kaldi --origin upstream

cd kaldi/tools
extras/check_dependencies.sh
make

cd kaldi/src
./configure
make depend
make

### 2. Cấu trúc thư mục của Kaldi
- Thư mục cấp cao nhất của Kaldi là `egs` chứa các công thức đào tạo các mô hình trên các bộ dữ liệu  Wall Street
Journal Corpus (wjs), TIMIT (timit), Resource Management (rm),... Dưới các thư mục này sẽ chứa các phiên bản khác nhau
như s3, s4, s5. Thông thường con số cao nhất biểu thị cách huấn luyện mô hình mới nhất.
- Đối với mỗi thư mục chứa các công thức đào tạo mô hình có một cấu trúc tiêu chuẩn:
     + Các file tập lệnh `bash shell`: run.sh, cmd.sh, path.sh
     + Thư mục `data`: chứa các thông tin dữ liệu được sử dụng như transcripts, dictionaries,...
     + Thư mục `exp`: chứa đầu ra của các tập lệnh đào tạo và căn chỉnh; hoặc các mô hình âm thanh.
     + Ngoài ra còn các thư mục khác: `local`, `utils`, `steps`. Cấu trúc thư mục Kaldi được mô tả dưới hình.


### 3. Tổng quan quy trình đào tạo của Kaldi
- 1. Định dạng transcript cho Kaldi (Format transcripts for Kald): Kaldi yêu cầu định dạng dữ liệu/bản ghi để đào tạo
mô hình: id người nói trong mỗi câu nói, danh sách từ và âm vị trong văn bản,...
- 2. Trích xuất đặc trưng từ âm thanh (Extract acoustic features from the audio): trích xuất Mel Frequency Cepstral
Coefficients (MFCC) được sử dụng nhiều nhất, sau đó Perceptual Linear Prediction (PLP) và các trích xuất khác,...
các đặc trưng này là cơ sở để xây dựng mô hình âm học.
- 3. Đào tạo mô hình monophone (Train monophone models): Mô hình monophone là mô hình âm học không bao gồm bất kì thông tin ngữ
cảnh nào trước và sau của các âm vị (phone).
- 4. Căn chỉnh audio bằng mô hình âm học (Align audio with the acoustic models)
- 5. Đào tạo mô hình triphone (Train triphone models): Mô hình triphone biểu diễn âm vị trong ngữ cảnh bao gồm âm vị trái
và âm vị phải.
- 6. Căn chỉnh audio sử dụng mô hình âm học và mô hình triphone (Re-align audio with the acoustic models & re-train triphone models):
Lặp lại các bước 4 và bước 5

### 4. Chuẩn bị dữ liệu cho Kaldi
- 1. Chỉnh sửa đường dẫn cho Kaldi trong file path.sh
     + vim path.sh
     + export KALDI_ROOT = path_kaldi
- 2. Tạo các file trong thư mục data/train:
     + text: <uterranceID> <text_transcription>: bản ghi chứa id audio và câu nói tương ứng.
     + wav.scp: <uterranceID> <full_path_to_audio_file>: id audio với đường dẫn tuyệt đối của audio tương ứng.
     + spk2gender: <speakerID> <duality>: id người nói và giới tính tương ứng.
     + utt2spk: <uterranceID> <speakerID>: id audio với id người nói cụ thể (1 audio nhiều người nói).
     + spk2utt: <speakerID> <uterranceID>: id người nói với id audio tương ứng (1 người nói ứng với nhiều audio).
     + segments: <uterranceID> <file_id> <start_time> <end_time>: thời gian bắt đầu và kết thúc mỗi câu nói.
- 3. Tạo các file language data trong thư mục data/local/lang:
     + lexicon.txt: <word> <phone 1> <phone 2>...: chứa các từ và phiên âm tương ứng.
     + nonsilence_phones.txt: <phone>: chứa danh sách các âm không im lặng.
     + silence_phones.txt: <phone>: chứa các âm im lặng và ngoài từ vựng.

### 5. Xây dựng quy trình huấn luyện cơ bản cho Kaldi
- 1. 


