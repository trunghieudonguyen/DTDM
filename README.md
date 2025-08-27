# Hướng dẫn biên dịch tài liệu (PDF)
**Tác giả:** Đỗ Nguyễn Trung Hiếu

Dự án sử dụng các gói LaTeX phổ biến (`graphicx`, `geometry`, `titlesec`, `tocloft`, `fontawesome`, `tikz`, `listings`, `hyperref`, `xurl`, v.v.) và **trích dẫn bằng BibTeX** với style `IEEEtran`. Các hình ảnh nằm trong thư mục `Demo/`. Tệp chính là **`main.tex`**.

---

## 1) Chuẩn bị môi trường

### A. Ubuntu / Debian (khuyến nghị)

**Cách dễ nhất** (đầy đủ, ít lỗi vặt):
```bash
sudo apt update
sudo apt install -y texlive-full latexmk
```
> `texlive-full` dung lượng lớn nhưng đảm bảo có đủ tất cả gói cần thiết (`IEEEtran`, `fontawesome`, `tocloft`, `titlesec`, `tikz`, `xurl`, `listings`, `hyphenat`, `ragged2e`, `scrextend`, `etoolbox`, v.v.).

**Cách tối gọn** (tiết kiệm dung lượng, nhưng có thể phải cài thêm nếu thiếu):
```bash
sudo apt update
xargs -a requirements.txt sudo apt install -y
```
Trong đó **`requirements.txt`** đã liệt kê sẵn các gói cần thiết:
```
texlive-latex-base
texlive-latex-recommended
texlive-latex-extra
texlive-fonts-recommended
texlive-fonts-extra
texlive-publishers
texlive-binaries
texlive-lang-vietnamese
latexmk
```
- `texlive-publishers`: chứa `IEEEtran.bst` (style thư mục tài liệu).  
- `texlive-fonts-extra`: chứa `fontawesome`.  
- `texlive-lang-vietnamese`: hỗ trợ tiếng Việt (T5 encoding).

### B. macOS
Cài MacTeX (bản không GUI) qua Homebrew:
```bash
# Cài Homebrew nếu chưa có: https://brew.sh/
brew install --cask mactex-no-gui
sudo tlmgr update --self
sudo tlmgr install latexmk
```
> Nếu thiếu gói nào, có thể cài thêm bằng `tlmgr install <tên-gói>`.

### C. Windows
Cài **MiKTeX** (bật auto-install package khi biên dịch):
- Tải: https://miktex.org/download  
- Sau khi cài, mở **MiKTeX Console** → **Settings** → bật **Install missing packages on-the-fly**.

Cài `latexmk` nếu chưa có (đa số bản MiKTeX đã bao gồm).

---

## 2) Lấy mã nguồn dự án

Clone từ Git:
```bash
git clone https://github.com/trunghieudonguyen/DTDM
cd DTDM
```

---

## 3) (Tuỳ chọn) Cài Python packages
Dự án không yêu cầu Python packages để biên dịch PDF. Tuy nhiên file `requirements.txt` (Python) được cung cấp cho mục đích chuẩn hoá thao tác.

```bash
pip install -r requirements.txt
```

---

## 4) Biên dịch tài liệu

### Cách 1 — Dùng `latexmk` (khuyến nghị, tự chạy BibTeX)
```bash
latexmk -pdf -interaction=nonstopmode -file-line-error main.tex
```
Kết quả: `./main.pdf`

Một số lệnh tiện dụng:
```bash
latexmk -pdf -quiet main.tex   # Biên dịch yên lặng
latexmk -c                     # Xoá file trung gian, giữ PDF
latexmk -C                     # Xoá tất cả, kể cả PDF
```

### Cách 2 — Thủ công (để hiểu quy trình BibTeX)
```bash
pdflatex -interaction=nonstopmode -file-line-error main.tex
bibtex main
pdflatex -interaction=nonstopmode -file-line-error main.tex
pdflatex -interaction=nonstopmode -file-line-error main.tex
```
Kết quả: `main.pdf`

---

## 5) Cấu trúc dự án (rút gọn)
- `main.tex` — tệp LaTeX gốc, ghép các chương mục con.
- `references.bib` — cơ sở dữ liệu tham khảo (`IEEEtran`).
- Các tệp con: `Introduction.tex`, `Oracle_cloud/Noidung.tex`, `Demo/Noidung.tex`, `Conclusion.tex`, `Tai_lieu_tham_khao.tex`, `TableOfContents.tex`, v.v.
- Thư mục `Demo/` — chứa hình minh hoạ `.png`.

---

## 6) Lệnh nhanh (mọi hệ điều hành)
```bash
# B1) Cài TeX (Ubuntu ví dụ)
sudo apt update && xargs -a requirements.txt sudo apt install -y

# B2) Giải nén/clone dự án và cd vào thư mục gốc DNTH_DTDM

# B3) (Tuỳ chọn) Cài Python packages
pip install -r requirements.txt

# B4) Biên dịch
latexmk -pdf -interaction=nonstopmode -file-line-error main.tex
# => ./main.pdf
```

---

## 7) Khắc phục sự cố thường gặp
- **Thiếu `IEEEtran.bst` hoặc lỗi thư mục tài liệu**: cài `texlive-publishers`.  
- **Lỗi tiếng Việt / ký tự hiển thị sai**: đảm bảo có `texlive-lang-vietnamese` và trong preamble có:
  ```tex
  \usepackage[utf8]{inputenc}
  \usepackage[T5]{fontenc}
  ```
- **Thiếu `fontawesome`, `tocloft`, `titlesec`, `xurl`, `hyphenat`, v.v.**: cài thêm `texlive-latex-extra texlive-fonts-extra`.  
- **Muốn biên dịch lại từ đầu sạch**: `latexmk -C`

---

## 8) Tác giả
- **Đỗ Nguyễn Trung Hiếu**

