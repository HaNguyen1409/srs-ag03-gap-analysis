# srs-ag03-gap-analysis

**AG-03** · Agent Node · phiên bản **2.2.1** · giấy phép MIT

> Vietnamese-language Claude skill — one independent, reusable service of the SRS-SOP agent pipeline. Machine-readable contract: [`skill.json`](skill.json).

Agent Node AG-03 (Bước B6 SOP viết SRS): phân tích khoảng trống trên REQ đã gán ID, sinh câu hỏi làm rõ có ưu tiên, phương án mặc định và cách kiểm chứng ASK (hỏi trực tiếp) hay DEMO (qua prototype). Chấm điểm nguyên 0..3. Mỗi khoảng trống gắn công việc JTBD (JOB) của khách; dự án đã có phần mềm thêm loại LECH_HIEN_TRANG từ ma trận chức năng B5a. Xuất thêm phiếu câu hỏi làm rõ (XLSX) theo đúng mẫu Q&A của khách.

## Hợp đồng giao tiếp

Skill là một dịch vụ độc lập, ghép lỏng: nhận đầu vào và trả đầu ra dưới dạng tài liệu/JSON mô tả trong [`SKILL.md`](SKILL.md), không gọi trực tiếp skill khác và không gắn với dự án cụ thể. Dùng riêng lẻ, hoặc ghép qua bộ điều phối [`srs-sop-pipeline`](https://github.com/HaNguyen1409/srs-sop-pipeline).

| Bước SOP | Việc | Loại | Nhận từ | Chuyển tới |
|---|---|---|---|---|
| 6 | Phân tích khoảng trống + phiếu câu hỏi làm rõ (XLSX theo mẫu Q&A của khách) | Agent | B5, (B5a) | Khách (phiếu câu hỏi), (B8), B8a, B11, B14, B16 |

**Sản phẩm đầu ra:** Phiếu câu hỏi làm rõ (XLSX theo mẫu Q&A của khách) + gaps[]

**Skill phía trước (cấp đầu vào):** [`srs-agmt-functional-matrix`](https://github.com/HaNguyen1409/srs-agmt-functional-matrix) `^1.1.0`

**Skill phía sau (dùng đầu ra):** [`srs-ag05-feature-matrix`](https://github.com/HaNguyen1409/srs-ag05-feature-matrix) `^2.0.0` · [`srs-agtq-project-overview`](https://github.com/HaNguyen1409/srs-agtq-project-overview) `^1.1.0` · [`srs-ag14-data-design`](https://github.com/HaNguyen1409/srs-ag14-data-design) `^1.0.0`

## Cài đặt

**Claude Code** — clone đúng phiên bản vào thư mục skill:

```bash
git clone --branch v2.2.1 https://github.com/HaNguyen1409/srs-ag03-gap-analysis.git ~/.claude/skills/srs-ag03-gap-analysis
```

Nâng cấp: `git -C ~/.claude/skills/srs-ag03-gap-analysis fetch --tags && git -C ~/.claude/skills/srs-ag03-gap-analysis checkout v<phiên-bản-mới>`.

**claude.ai / Claude Desktop** — tải `srs-ag03-gap-analysis-v2.2.1.zip` ở trang [Releases](https://github.com/HaNguyen1409/srs-ag03-gap-analysis/releases) rồi tải lên ở phần Skills trong cài đặt.

**Cả bộ** — xem [`srs-sop-skills`](https://github.com/HaNguyen1409/srs-sop-skills) (cài toàn bộ skill theo phiên bản đã khoá).

## Quản lý phiên bản

Theo [SemVer](https://semver.org/lang/vi/), áp cho hợp đồng giao tiếp của skill:

| Tăng | Khi |
|---|---|
| MAJOR | Đổi hợp đồng làm vỡ skill phía sau: đổi/bỏ trường JSON, đổi hệ mã ID, đổi bố cục tài liệu đầu ra |
| MINOR | Thêm khả năng tương thích ngược: trường tuỳ chọn, mục mới, chế độ mới |
| PATCH | Sửa câu chữ, sửa lỗi, làm rõ luật mà không đổi đầu ra |

Phát hành: sửa `SKILL.md` → tăng `version` trong frontmatter, `VERSION`, `skill.json` → thêm mục vào [`CHANGELOG.md`](CHANGELOG.md) → `python tools/check_skill.py` → commit → `git tag -a vX.Y.Z -m "vX.Y.Z"` → `git push --follow-tags`. GitHub Actions kiểm lại, chạy test và đính kèm zip vào Release.

## Giấy phép

MIT — xem [`LICENSE`](LICENSE).
