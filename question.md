# Quiz sau buổi thuyết trình — MarkdownOffice

> 30 câu hỏi trắc nghiệm (4 lựa chọn / câu). Chọn **một** đáp án đúng nhất.
> Nội dung bám theo slide deck và *MarkdownOffice — Master document*.
> Đáp án + giải thích ở cuối file (mục **Đáp án**).

---

## Phần 1 — Vấn đề: Enterprise document

**Câu 1.** MarkdownOffice được định vị là gì?
- A. Một Markdown editor mới thay thế Notion
- B. Một AI-native / LLM-first **Document Operating System** cho enterprise monorepo
- C. Một plugin cho Microsoft Word
- D. Một dịch vụ lưu trữ file trên đám mây

**Câu 2.** Vấn đề cốt lõi của tài liệu doanh nghiệp hiện nay theo deck là gì?
- A. Doanh nghiệp thiếu tài liệu
- B. Tài liệu quá đẹp, khó chỉnh
- C. Tri thức bị **phân mảnh**, không có *canonical source of truth*
- D. Không đủ dung lượng lưu trữ

**Câu 3.** Vì sao AI Agent khó vận hành đáng tin cậy với Office/Docs truyền thống?
- A. Vì model ngôn ngữ quá yếu
- B. Vì tài liệu tối ưu cho **mắt người**, thiếu cấu trúc machine-readable, version, permission
- C. Vì thiếu kết nối internet
- D. Vì Agent không đọc được tiếng Việt

**Câu 4.** Theo slide, "cái giá phải trả" khi con người mất context KHÔNG bao gồm điều nào?
- A. Onboarding chậm
- B. Handover rủi ro cao
- C. Quyết định dựa trên dữ liệu lỗi thời
- D. Chi phí điện toán đám mây tăng gấp đôi

---

## Phần 2 — Core thesis: markdown-native + LLM-first

**Câu 5.** Source of truth (nguồn sự thật) của MarkdownOffice là gì?
- A. File binary `.docx`
- B. **Markdown-native / text-based** có thể diff, parse, validate
- C. File PDF đã khóa
- D. Ảnh chụp màn hình tài liệu

**Câu 6.** "LLM-first" trong MarkdownOffice nghĩa là gì?
- A. LLM thay thế con người hoàn toàn
- B. Chỉ được dùng đúng một model LLM
- C. AI Agent là **first-class actor** trong hệ thống
- D. Không cần con người phê duyệt bất cứ điều gì

**Câu 7.** Đâu KHÔNG phải lợi thế của Markdown-native (theo deck)?
- A. Human-readable & machine-readable
- B. Diff-friendly → version control tự nhiên
- C. **Bị khóa chặt vào một vendor duy nhất**
- D. Extensible bằng frontmatter, block ID, schema

---

## Phần 3 — Hệ sinh thái sản phẩm

**Câu 8.** Trong MarkdownOffice, một tài liệu được xem là gì?
- A. Một file cô lập
- B. Một **node trong document graph** (structured data)
- C. Một email đính kèm
- D. Một dòng lệnh

**Câu 9.** MarkdownDoc tương ứng với sản phẩm truyền thống nào?
- A. Excel / Google Sheets
- B. PowerPoint / Google Slides
- C. **Word / Google Docs**
- D. Outlook / Gmail

**Câu 10.** MVP của MarkdownSheet nên bắt đầu từ đâu (thay vì clone toàn bộ Excel)?
- A. Game engine
- B. **Workflow data**: status tracking, ownership matrix, risk scoring
- C. Công cụ dựng video
- D. Kết xuất đồ họa 3D

**Câu 11.** Điểm mạnh đặc biệt của MarkdownSlide là gì?
- A. Chỉ hỗ trợ hiệu ứng hoạt hình
- B. Được **generate từ report + sheet + document graph**, có source reference & stale detection
- C. Hoạt động hoàn toàn không cần dữ liệu
- D. Chỉ xuất ra bản in giấy

**Câu 12.** MarkdownJira khác Jira truyền thống ở điểm nào?
- A. Task nằm **cùng một knowledge layer có version**, liên kết document graph
- B. Không có khái niệm task
- C. Chỉ chạy được ở chế độ offline
- D. Không cho phép bình luận

**Câu 13.** MarkdownWorkflow là lớp gì trong hệ sinh thái?
- A. Lớp giao diện người dùng
- B. **Automation layer** cho quy trình lặp lại (handover, onboarding, incident review)
- C. Lớp lưu trữ hình ảnh
- D. Lớp cổng thanh toán

**Câu 14.** MarkdownAgent hoạt động theo mô hình nào?
- A. Như một chatbot bên ngoài tài liệu
- B. Tự commit mọi thay đổi ngay lập tức
- C. **Làm việc theo transaction**: read → generate → validate → propose → review → commit
- D. Chỉ đọc, không bao giờ tạo nội dung

---

## Phần 4 — Cortexpod, Lob & Git-like operations

**Câu 15.** Cortexpod tương ứng với gì trong thế giới hiện tại?
- A. Git
- B. **GitHub** — hub tập trung cho document + Agent
- C. Google Drive
- D. Slack

**Câu 16.** Lob tương ứng với gì?
- A. GitHub
- B. **Git-like version layer** cho structured documents
- C. Một trình duyệt web
- D. Một CSS framework

**Câu 17.** "Agent Proposal" tương ứng với khái niệm nào của Git/GitHub?
- A. Commit
- B. Branch
- C. **Pull Request**
- D. Clone

**Câu 18.** Vì sao Lob không chỉ dùng line-diff như Git?
- A. Vì Git quá nhanh
- B. Vì tài liệu cần **block / AST / semantic diff** và policy-based merge
- C. Vì line-diff tốn phí bản quyền
- D. Vì không ai còn dùng Git

**Câu 19.** Một Lob transaction (giao dịch) ghi lại những gì?
- A. Chỉ đoạn text mới thêm
- B. Chỉ tên file bị đổi
- C. **actor · intent · source · validation · approval · rollback pointer**
- D. Chỉ mốc thời gian

---

## Phần 5 — Demo workflow (Employee Handover)

**Câu 20.** Use case demo xuyên suốt trong deck là gì?
- A. Tính lương nhân viên
- B. **Employee Handover** — chuyển giao khi nhân sự rời đi
- C. Gửi email marketing hàng loạt
- D. Thiết kế logo thương hiệu

**Câu 21.** Ở bước scan tri thức, Agent hoạt động như thế nào?
- A. Đọc tất cả mọi thứ, không giới hạn
- B. **Scan theo permission · scope · policy** (permission-aware)
- C. Chỉ đọc các file công khai
- D. Không đọc gì, chờ người nhập tay

**Câu 22.** Sau khi một proposal được phê duyệt, điều gì xảy ra?
- A. Toàn bộ tài liệu bị xóa
- B. **Commit vào Lob → graph & dashboard refresh → export/share**
- C. Gửi bản sao cho đối thủ cạnh tranh
- D. Không có hành động nào

---

## Phần 6 — Agent workflow & Harness Engineering

**Câu 23.** Harness Engineering được ví với LLM như thế nào?
- A. LLM là toàn bộ máy tính
- B. **LLM là CPU, còn harness là phần còn lại của máy tính** (memory, I/O, permission, logging)
- C. Harness chỉ là màn hình hiển thị
- D. Harness là thứ không cần thiết

**Câu 24.** Theo Harness Engineering, độ tin cậy (reliability) của agent đến từ đâu?
- A. Từ một prompt thật dài
- B. Từ việc liên tục đổi sang model mới hơn
- C. **Từ system design (harness)** — không chỉ từ bản thân model
- D. Từ may mắn

**Câu 25.** Primitive **đầu tiên** khi lắp ráp một agent (theo chuỗi sơ đồ) là gì?
- A. Verification & Observability
- B. **Reasoning Engine** (model trần: prompt → model → text)
- C. Subagents
- D. Orchestration

**Câu 26.** Failure mode "Agent xóa nhầm thư mục" được khắc phục bằng primitive nào?
- A. Instructions
- B. Context Management
- C. **Execution Environment** (isolate worktree & bound permissions)
- D. Durable State

---

## Phần 7 — Agent Memory

**Câu 27.** Working memory tương ứng với gì trong AI Agent?
- A. Vector database
- B. **Context hiện tại** của agent (context window, scratchpad, task state)
- C. Event log theo timeline
- D. Kho lưu skill

**Câu 28.** Câu mô tả "Agent đã trải qua chuyện gì" ứng với loại memory nào?
- A. Working memory
- B. **Episodic memory**
- C. Semantic memory
- D. Procedural memory

**Câu 29.** Procedural memory lưu giữ điều gì?
- A. Task đang xử lý ngay lúc này
- B. Tri thức, facts, concepts về thế giới
- C. **Cách làm việc** — skills, workflows, playbooks, tool-use recipes
- D. Nhật ký lỗi hệ thống

**Câu 30.** Trong quá trình *consolidation* của agent memory, điều gì đúng?
- A. Mọi memory được gộp vào **một** vector DB duy nhất
- B. **Episodic lặp lại → tổng hợp thành Semantic; hành động thành công lặp lại → thành Procedural**
- C. Working memory tồn tại vĩnh viễn
- D. Semantic memory bị xóa sau mỗi phiên chat

---

## Đáp án

| Câu | Đáp án | Giải thích ngắn                                                                                         |
| --- | ------ | ------------------------------------------------------------------------------------------------------- |
| 1   | **B**  | MarkdownOffice = document operating system markdown-native, LLM-first — không phải editor thường.       |
| 2   | **C**  | Doanh nghiệp không thiếu tài liệu; thiếu canonical knowledge layer.                                     |
| 3   | **B**  | Office tối ưu cho mắt người, không đủ cấu trúc/graph/version/permission cho reasoning engine.           |
| 4   | **D**  | Deck nêu: onboarding chậm, handover rủi ro, dữ liệu lỗi thời, thiếu audit — không nói về chi phí cloud. |
| 5   | **B**  | Source of truth là Markdown-native / text-based, diff·parse·validate được.                              |
| 6   | **C**  | LLM-first: Agent là first-class actor, không phải chatbot ngoài rìa.                                    |
| 7   | **C**  | Markdown *portable*, KHÔNG khóa vendor — nên C là điều sai (đáp án cần chọn).                           |
| 8   | **B**  | Tài liệu là node trong document graph, structured data.                                                 |
| 9   | **C**  | MarkdownDoc ↔ Word / Google Docs.                                                                       |
| 10  | **B**  | Bắt đầu từ workflow data: status tracking, ownership matrix, risk scoring.                              |
| 11  | **B**  | Slide được sinh từ report + sheet + graph, có source reference & stale detection.                       |
| 12  | **A**  | Task + tài liệu + tiến độ nằm cùng knowledge layer có version.                                          |
| 13  | **B**  | MarkdownWorkflow = automation layer cho quy trình lặp lại.                                              |
| 14  | **C**  | Agent làm việc theo transaction, không tự ý commit.                                                     |
| 15  | **B**  | Cortexpod ↔ GitHub (hub cho document + Agent).                                                          |
| 16  | **B**  | Lob ↔ Git-like version layer cho structured documents.                                                  |
| 17  | **C**  | Agent Proposal ↔ Pull Request.                                                                          |
| 18  | **B**  | Cần block/AST/semantic diff + policy-based merge, không đủ với line-diff.                               |
| 19  | **C**  | Transaction ghi: actor · intent · source · validation · approval · rollback pointer.                    |
| 20  | **B**  | Use case demo là Employee Handover.                                                                     |
| 21  | **B**  | Scan permission-aware theo permission · scope · policy.                                                 |
| 22  | **B**  | Approve → commit Lob → refresh graph/dashboard → export/share.                                          |
| 23  | **B**  | LLM là CPU, harness là phần còn lại của máy tính.                                                       |
| 24  | **C**  | Reliability đến từ system design, không chỉ từ model.                                                   |
| 25  | **B**  | Bắt đầu từ Reasoning Engine (model trần), rồi lắp thêm primitive.                                       |
| 26  | **C**  | Execution Environment → isolate worktree & bound permissions.                                           |
| 27  | **B**  | Working memory = context hiện tại của agent.                                                            |
| 28  | **B**  | "Đã trải qua chuyện gì" = Episodic memory.                                                              |
| 29  | **C**  | Procedural = "biết làm việc như thế nào" (skills/workflows).                                            |
| 30  | **B**  | Consolidation: episodic → semantic; hành động thành công lặp lại → procedural.                          |

---

### Phân bố chủ đề

| Chủ đề                                   | Câu   |
| ---------------------------------------- | ----- |
| Vấn đề (fragmented knowledge)            | 1–4   |
| Core thesis (markdown-native, LLM-first) | 5–7   |
| Hệ sinh thái sản phẩm                    | 8–14  |
| Cortexpod · Lob · Git-like               | 15–19 |
| Demo workflow (handover)                 | 20–22 |
| Agent workflow · Harness                 | 23–26 |
| Agent memory                             | 27–30 |
