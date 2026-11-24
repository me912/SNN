# Project rules — SNN thesis workspace

## 1. Vẽ hình phần cứng → dùng skill `micro-arch-drawing`

Mọi hình vi kiến trúc / sơ đồ khối module đều vẽ bằng draw.io theo **skill global
`micro-arch-drawing`** (đã cài ở `~/.config/opencode/skills/micro-arch-drawing/`).
Skill đó chứa đầy đủ: quy tắc few-shot, palette, font, kiểu dây, bố cục, mẫu XML,
script kiểm tra. Đọc skill trước khi bắt tay vẽ.

Phần cứng nhất của rule:

- **Hỏi người dùng chọn ảnh mẫu trước khi vẽ** — không tự lấy một file `.drawio` bất kỳ.
- Từ ảnh mẫu đó tra ra `.drawio` tương ứng (`find_pair.py`) và đọc file đó để lấy style;
  ảnh để lấy bố cục, `.drawio` để lấy `style=`.
- Trong câu trả lời phải ghi rõ: ảnh mẫu nào, file `.drawio` nào, trang nào.
- Không tự phát minh style; không vẽ lại waveform/timing (đó là ảnh chụp, không phải sơ đồ).

## 2. Corpus tham chiếu của project

| Thư mục | Nội dung | Trạng thái |
|---|---|---|
| `fewshot/drawio/` | 3 file `.drawio` gốc + 88 file phục hồi từ SVG (`from-svg/`) | chỉ đọc |
| `fewshot/drawio/images/` | ảnh PNG export của từng file `.drawio` (cùng tên, scale 2) — xem nhanh không cần mở draw.io | chỉ đọc |
| `fewshot/images/` | 114 hình vi kiến trúc trích từ XiangShan design doc (`manifest.json` kèm 51 hình bị loại + lý do) | chỉ đọc |
| `tools/staging/` | 166 hình gốc chưa phân loại, giữ để truy vết | chỉ đọc |

Không sửa, không đổi tên, không thêm file vào `fewshot/`.

## 3. Vị trí và kiểm tra file vẽ mới

- Hình mới: `figures/<module>-<nội-dung>.drawio` (vd `figures/LSQ-Freelist-Recycle.drawio`).
- Kiểm tra trước khi báo xong:
  ```powershell
  python "$HOME\.config\opencode\skills\micro-arch-drawing\scripts\check_drawio.py" "figures\*.drawio"
  ```
- Đo lại style của corpus khi cần:
  ```powershell
  python "$HOME\.config\opencode\skills\micro-arch-drawing\scripts\style_stats.py" fewshot\drawio
  ```

## 4. Trích ảnh từ PDF → skill `pdf-image-extract`

Kiểm kê bằng `pdfimages -list` / `pdfcpu images list`, extract raster gốc hoặc render
hình vector, luôn verify bằng mắt. Script của project: `tools/extract_all.py`
(extract), `tools/discover.py` (kiểm kê + caption), `tools/finalize.py` (publish).

## 5. Ghi nguồn

Palette/font trong corpus đo từ *XiangShan Design Doc* (CC BY 4.0) — hình xuất bản dùng
lại phong cách này cần ghi attribution cho tài liệu gốc.
