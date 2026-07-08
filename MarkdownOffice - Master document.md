
## Phần 1 — A đến D

Bản phân tích này bám theo định vị cốt lõi của MarkdownOffice: Markdown là source of truth, tài liệu được parse thành AST, document là graph thay vì file cô lập, AI Agent có thể tương tác an toàn, và mọi thay đổi đều versioned/auditable. 

---

# A. Executive Summary

MarkdownOffice là một **AI-native Document Operating System** cho doanh nghiệp, không chỉ là một bộ editor thay thế Word, PowerPoint hay Excel.

Các ý chính:

1. **MarkdownOffice biến tài liệu thành dữ liệu có cấu trúc**
   Document, slide, sheet, report, workflow và metadata đều được lưu dưới dạng Markdown-native hoặc text-based, có thể diff, version, parse, validate và audit.

2. **MarkdownOffice tạo centralized document monorepo cho doanh nghiệp**
   Thay vì tài liệu nằm rải rác trong Google Docs, Word, PowerPoint, Excel, Notion, Jira, Slack, email và local file, tất cả được gom vào một hub có permission, graph, version history và workflow.

3. **MarkdownDoc, MarkdownSlide, MarkdownSheet là ba interface chính của hệ sinh thái**
   Người dùng vẫn có trải nghiệm quen thuộc như Word, PowerPoint, Excel, nhưng bên dưới là Markdown-native, machine-readable và Agent-accessible.

4. **Cortexpod đóng vai trò GitHub cho enterprise knowledge**
   Cortexpod là centralized repository hub cho document, workflow, service ownership, review, permission, audit, search, agent proposal và knowledge graph.

5. **Lob là version layer cho document và Agent workflow**
   Lob mở rộng khái niệm commit, branch, diff, merge, rollback, review sang tài liệu, slide, sheet, workflow và hoạt động của AI Agent.

6. **AI Agent có workspace chuẩn để tạo report, sheet, slide và proposal**
   Agent không chỉ đọc file rời rạc, mà có thể truy xuất document graph, hiểu permission, tạo artifact, mở proposal, giải thích reasoning và commit sau khi được approve.

7. **MarkdownOffice có thể trở thành nền tảng enterprise knowledge automation**
   Khi toàn bộ knowledge của tổ chức trở thành structured, versioned, auditable data, doanh nghiệp có thể tự động hóa report, handover, compliance, onboarding, incident review, planning và decision tracking.

---

# B. Problem Statement

## B1. Doanh nghiệp hiện nay không thiếu tài liệu, mà thiếu hệ điều hành cho tài liệu

Trong hầu hết doanh nghiệp, knowledge không nằm trong một hệ thống thống nhất. Nó bị phân mảnh trong nhiều tool:

| Loại thông tin    | Nơi thường nằm                  |
| ----------------- | ------------------------------- |
| Report            | Google Docs, Word, PDF          |
| Slide             | PowerPoint, Google Slides       |
| Sheet             | Excel, Google Sheets            |
| Task              | Jira, Linear, Trello            |
| Chat context      | Slack, Teams                    |
| Decision          | Notion, Confluence, email       |
| Service ownership | Wiki, sheet, tribal knowledge   |
| Incident history  | Jira, Slack, postmortem doc     |
| Architecture      | Diagram tool, docs, repo README |
| Handover          | Email, meeting note, local file |

Vấn đề không phải là từng công cụ kém. Vấn đề là **không có một canonical knowledge layer** để con người, hệ thống và AI Agent cùng làm việc.

---

## B2. Tài liệu hiện tại tốt cho UI, nhưng yếu cho automation

Microsoft Office và Google Workspace rất mạnh cho con người vì có UI quen thuộc. Nhưng với AI Agent, chúng có nhiều giới hạn:

Agent khó xác định:

* đâu là version mới nhất;
* section nào đã được duyệt;
* ai sở hữu tài liệu;
* bảng nào là source of truth;
* slide nào được tạo từ report nào;
* dữ liệu nào đang stale;
* thay đổi nào ảnh hưởng đến report, sheet hoặc presentation nào;
* tài liệu nào phụ thuộc vào service, team, project hoặc permission nào.

Khi document không có cấu trúc machine-readable rõ ràng, Agent chỉ có thể “đọc nội dung”, nhưng không thể “vận hành tài liệu” như một hệ thống.

---

## B3. Tài liệu doanh nghiệp thiếu version control thật sự

Git giải quyết rất tốt vấn đề version control cho code. Nhưng với document, đa số doanh nghiệp vẫn phụ thuộc vào:

* file name kiểu `final_v2_revised_latest.docx`;
* comment thủ công;
* history rời rạc trong SaaS;
* approval qua email hoặc chat;
* không có branch rõ ràng;
* không có semantic diff;
* rollback khó;
* audit không đủ sâu;
* Agent-generated content không có attribution đầy đủ.

Trong thời đại human + AI Agent, tài liệu cần một lớp version control mới:

* document-level diff;
* block-level diff;
* AST diff;
* semantic diff;
* branch cho document workflow;
* proposal trước khi commit;
* policy-based merge;
* audit trail cho cả human và Agent.

---

## B4. AI Agent thiếu workspace chuẩn để làm việc

Hiện nay, nếu một Agent muốn tạo handover report hoặc executive slide, nó phải truy cập nhiều nguồn:

* Google Docs;
* Slack;
* Jira;
* GitHub;
* email;
* sheet;
* file storage;
* code repository;
* meeting notes.

Mỗi nguồn có schema khác nhau, permission khác nhau, history khác nhau, format khác nhau và API khác nhau.

Kết quả là Agent dễ gặp các vấn đề:

* context thiếu hoặc sai;
* đọc nhầm tài liệu cũ;
* không biết quyền truy cập;
* không biết dữ liệu nào đáng tin;
* không thể trace nguồn;
* không thể tạo artifact có audit trail;
* không thể mở review request chuẩn;
* không thể rollback nếu tạo sai.

MarkdownOffice giải quyết bằng cách tạo ra một **Agent-native document workspace**: nơi Agent có thể đọc, hiểu, tạo, sửa, validate, propose và commit tài liệu theo policy của tổ chức.

---

# C. Product Vision

## C1. MarkdownOffice là Document Operating System, không phải Markdown editor

Một Markdown editor truyền thống chỉ tập trung vào việc viết và preview Markdown.

MarkdownOffice đi xa hơn: nó biến Markdown thành nền tảng dữ liệu cho toàn bộ document workflow của doanh nghiệp.

Nó bao gồm:

* content authoring;
* structured document model;
* AST pipeline;
* document graph;
* permission model;
* version history;
* AI Agent interaction;
* workflow automation;
* approval;
* audit;
* rendering đa chế độ;
* report, slide và sheet generation.

Vì vậy, MarkdownOffice nên được hiểu là:

> **Một hệ điều hành tài liệu cho doanh nghiệp, nơi mọi knowledge artifact đều có cấu trúc, có version, có graph, có audit và có thể được vận hành bởi AI Agent.**

---

## C2. Markdown-native là lựa chọn chiến lược

Markdown có nhiều lợi thế cho AI-native document system:

### 1. Human-readable

Người dùng kỹ thuật, founder, PM, engineer hoặc analyst đều có thể đọc Markdown trực tiếp.

### 2. Machine-readable

Markdown dễ parse thành AST, dễ extract heading, table, list, code block, reference, frontmatter và metadata.

### 3. Diff-friendly

Text-based format giúp version control tự nhiên hơn binary document.

### 4. Portable

Markdown không bị khóa vào một vendor cụ thể.

### 5. LLM-friendly

Các model ngôn ngữ lớn đã quen với Markdown, heading, bullet, table, code block và cấu trúc text rõ ràng.

### 6. Extensible

Có thể mở rộng bằng frontmatter, block ID, directive, schema, metadata, reference và domain-specific template.

Điểm quan trọng: MarkdownOffice không bắt người dùng phổ thông phải nhìn Markdown thô. Người dùng vẫn có Word-like, PowerPoint-like và Excel-like viewer. Markdown chỉ là **source of truth bên dưới**.

---

## C3. Document không còn là file, mà là node trong graph

Trong MarkdownOffice, một tài liệu không phải file cô lập.

Nó là một node trong document graph, có thể liên kết đến:

* section khác;
* sheet khác;
* slide khác;
* chart;
* task;
* service;
* owner;
* incident;
* decision record;
* API spec;
* runbook;
* approval state;
* policy;
* dependency.

Ví dụ:

Một service ownership document có thể liên kết đến:

* architecture document;
* deployment runbook;
* SLA document;
* monitoring dashboard;
* incident history;
* team ownership sheet;
* API specification;
* onboarding guide.

Khi tài liệu trở thành graph, hệ thống có thể làm được những việc mà Office truyền thống khó làm:

* impact analysis;
* stale detection;
* dependency tracking;
* automatic slide generation;
* automatic report generation;
* permission-aware retrieval;
* compliance validation;
* workflow automation.

---

## C4. MarkdownOffice tạo cầu nối giữa con người và Agent

Con người cần UI quen thuộc.
Agent cần dữ liệu có cấu trúc.
Doanh nghiệp cần permission, version, audit và compliance.

MarkdownOffice đứng ở giữa ba nhu cầu này:

| Đối tượng      | Cần gì                      | MarkdownOffice cung cấp       |
| -------------- | --------------------------- | ----------------------------- |
| User phổ thông | UI giống Office             | Word/Slide/Sheet viewer       |
| Engineer       | Markdown, diff, version     | Text-based source, AST, Lob   |
| Manager        | Report, status, approval    | Dashboard, proposal, review   |
| AI Agent       | Structured context          | AST, graph, metadata          |
| Enterprise     | Security, audit, compliance | Permission, audit log, policy |

Đây là lợi thế chiến lược: MarkdownOffice không cố thay thế Office bằng một editor mới, mà thay thế **data foundation** của enterprise documents.

---

# D. Ecosystem Mapping

## D1. Mapping tổng thể

| Thế giới hiện tại                    | MarkdownOffice ecosystem                 |
| ------------------------------------ | ---------------------------------------- |
| Microsoft Office / Google Workspace  | MarkdownOffice                           |
| Microsoft Word / Google Docs         | MarkdownDoc                              |
| Microsoft PowerPoint / Google Slides | MarkdownSlide                            |
| Microsoft Excel / Google Sheets      | MarkdownSheet                            |
| GitHub                               | Cortexpod                                |
| Git                                  | Lob Version Management                   |
| File storage                         | Document monorepo                        |
| Folder                               | Knowledge graph                          |
| File version                         | Document transaction                     |
| Pull Request                         | Agent proposal / document change request |
| Commit                               | Auditable document transaction           |
| Branch                               | Parallel document/workflow version       |
| Merge                                | Policy-based reconciliation              |
| Comment                              | Review thread / approval workflow        |
| Wiki                                 | Living operational knowledge graph       |

---

## D2. Microsoft Office / Google Workspace vs MarkdownOffice

Microsoft Office và Google Workspace tối ưu cho productivity của con người.

MarkdownOffice tối ưu cho productivity của **human + AI Agent + enterprise workflow**.

| Tiêu chí             | Office / Google Workspace           | MarkdownOffice                  |
| -------------------- | ----------------------------------- | ------------------------------- |
| Source of truth      | Proprietary/binary/SaaS format      | Markdown-native/text-based      |
| Version control      | Có history nhưng không Git-like sâu | Lob-style version management    |
| Agent readability    | Phụ thuộc API và parsing            | AST + graph + metadata          |
| Cross-doc dependency | Yếu, chủ yếu thủ công               | Document graph                  |
| Audit                | Có nhưng không Agent-native         | Human/Agent attribution         |
| Review workflow      | Comment/approval riêng lẻ           | Proposal, review, merge         |
| Rendering            | UI-first                            | Multi-view: UI + source + graph |
| Automation           | Tích hợp bên ngoài                  | Native workflow automation      |
| Portability          | Phụ thuộc vendor                    | Markdown/text-first             |

Điểm khác biệt lớn nhất: Office hiện tại coi tài liệu là artifact để con người chỉnh sửa. MarkdownOffice coi tài liệu là **enterprise knowledge object** có thể được version, query, validate, automate và điều phối bởi Agent.

---

## D3. GitHub vs Cortexpod

GitHub là platform trung tâm cho code collaboration.

Cortexpod nên là platform trung tâm cho **document, workflow, knowledge và Agent collaboration**.

| GitHub                  | Cortexpod                          |
| ----------------------- | ---------------------------------- |
| Code repository         | Document/workflow repository       |
| Pull Request            | Document proposal / Agent proposal |
| Issues                  | Task + document-linked workflow    |
| README                  | Living knowledge entry point       |
| Code owner              | Document/service/workflow owner    |
| CI check                | Validation/compliance/agent check  |
| Branch                  | Multi-version workspace            |
| Commit history          | Auditable document transaction     |
| Repo graph              | Document graph + service graph     |
| Developer collaboration | Human + Agent collaboration        |

Cortexpod không chỉ host MarkdownOffice files. Nó là nơi enterprise quản lý:

* repository tài liệu;
* permission;
* ownership;
* review;
* agent workspace;
* knowledge graph;
* workflow;
* audit;
* compliance;
* analytics;
* presentation generation.

---

## D4. Git vs Lob Version Management

Git được thiết kế cho code. Lob cần được thiết kế cho **document + Agent workflow + multi-version collaboration**.

| Git             | Lob                                  |
| --------------- | ------------------------------------ |
| Line-based diff | AST/block/semantic diff              |
| Code branch     | Document/workflow branch             |
| Commit          | Document transaction                 |
| Merge conflict  | Policy-based document reconciliation |
| Author          | Human/Agent actor                    |
| Git log         | Audit trail + reasoning trace        |
| Pull/Push       | Sync document workspace              |
| Repo state      | Document graph state                 |
| Code review     | Document/slide/sheet review          |
| Rollback        | Artifact + dependency rollback       |

Lob cần giải quyết các thứ Git không tối ưu cho document:

* block-level identity;
* section-level approval;
* table row diff;
* slide diff;
* formula dependency diff;
* chart source tracking;
* generated artifact provenance;
* permission-aware merge;
* long-running parallel versions;
* Agent-generated transaction trace.

---

## D5. Word vs MarkdownDoc

MarkdownDoc là Word-like document product nhưng có Markdown-native source of truth.

Nó phù hợp cho:

* technical spec;
* business proposal;
* service ownership document;
* onboarding guide;
* incident report;
* policy document;
* legal document;
* architecture document.

Người dùng có thể xem như Word.
Agent có thể đọc như structured document.

Sự khác biệt chính:

| Word / Google Docs   | MarkdownDoc                         |
| -------------------- | ----------------------------------- |
| UI-first document    | Markdown-native structured document |
| Human editing        | Human + Agent editing               |
| Format-rich          | Schema + metadata + block ID        |
| Manual linking       | Document graph linking              |
| Basic history        | Lob-backed version history          |
| Comment-based review | Proposal + approval workflow        |

---

## D6. PowerPoint vs MarkdownSlide

MarkdownSlide là PowerPoint-like presentation product nhưng source là Markdown slide syntax.

Nó phù hợp cho:

* executive report;
* project status deck;
* handover presentation;
* technical architecture deck;
* incident review;
* investor deck;
* training material.

Điểm mạnh là slide có thể được generate từ document graph:

* report → slide;
* sheet → chart;
* issue history → timeline;
* service metadata → architecture view;
* risk register → risk slide.

| PowerPoint / Google Slides | MarkdownSlide                      |
| -------------------------- | ---------------------------------- |
| Slide là file tĩnh         | Slide là view từ structured source |
| Manual copy-paste          | Generated from docs/sheets/graph   |
| Khó trace nguồn dữ liệu    | Source-linked slide                |
| Dễ stale                   | Stale detection                    |
| UI-first                   | Markdown + presentation viewer     |
| Human-only authoring       | Agent-generated deck               |

---

## D7. Excel vs MarkdownSheet

MarkdownSheet là Excel-like spreadsheet product nhưng source là CSV-first hoặc `.msheet`.

Nó phù hợp cho:

* transfer status sheet;
* service ownership matrix;
* responsibility assignment matrix;
* project timeline;
* cost estimation;
* KPI tracking;
* formula calculation;
* incident checklist;
* resource planning.

Điểm khác biệt chính:

| Excel / Google Sheets         | MarkdownSheet                 |
| ----------------------------- | ----------------------------- |
| Spreadsheet là file/app riêng | Sheet là structured data node |
| Binary/SaaS dependency        | CSV/Markdown/text-first       |
| Formula khó audit cross-doc   | Formula dependency tracking   |
| Manual chart/report copy      | Chart/report/slide linked     |
| Version history hạn chế       | Lob-backed diff/version       |
| Agent khó hiểu context        | AI-readable table schema      |

MarkdownSheet không cố clone toàn bộ Excel ngay từ đầu. MVP nên tập trung vào table-driven workflow, status tracking, formula nhẹ, chart cơ bản và integration với MarkdownDoc/MarkdownSlide.

---

## D8. Định nghĩa hệ sinh thái trong một câu

MarkdownOffice là lớp application.
Cortexpod là lớp platform.
Lob là lớp version management.
Document graph là lớp knowledge model.
AI Agent là lớp automation.

Tổng thể có thể định nghĩa:

> **MarkdownOffice is Microsoft Office reimagined for AI Agents, running on a GitHub-like enterprise document hub, powered by a Git-like version layer designed for structured documents, workflows and organizational knowledge.**

---

# Phần 2 — E đến F

Phần này tập trung vào kiến trúc hệ thống và ba sản phẩm lõi: **MarkdownDoc, MarkdownSlide, MarkdownSheet**. Trọng tâm là cách MarkdownOffice biến Markdown từ “format viết tài liệu” thành **nền tảng dữ liệu vận hành cho enterprise knowledge và AI Agent**. Kiến trúc này nhất quán với master document của MarkdownOffice: Markdown là source of truth, AST là representation trung tâm, document là graph, hệ thống có collaboration, version control, automation, agent layer và audit/compliance. 

---

# E. System Architecture

## E1. Tổng quan kiến trúc

MarkdownOffice nên được thiết kế theo kiến trúc nhiều layer:

```text
User / Team / Agent
        ↓
Rendering & Interaction Layer
        ↓
Document Application Layer
        ↓
AST & Semantic Processing Layer
        ↓
Document Graph Layer
        ↓
Version Management Layer: Lob
        ↓
Repository / Hub Layer: Cortexpod
        ↓
Storage, Permission, Audit, Compliance
```

Điểm quan trọng: MarkdownOffice không chỉ render file Markdown. Nó cần biến mỗi file thành một **structured knowledge object** có metadata, schema, dependency, version, permission, audit và Agent-accessible interface.

---

## E2. Content Layer

Content layer là nơi lưu source of truth.

Các định dạng chính:

| Loại artifact | Format đề xuất        | Vai trò                              |
| ------------- | --------------------- | ------------------------------------ |
| Document      | `.mdoc`, `.md`        | Report, spec, policy, runbook        |
| Slide         | `.mslide`             | Deck, presentation, executive report |
| Sheet         | `.msheet`, `.csv`     | Table, tracking, matrix, calculation |
| Metadata      | Frontmatter           | Owner, status, permission, schema    |
| Reference     | Block ID / section ID | Stable linking                       |
| Template      | Markdown template     | Chuẩn hóa document                   |
| Schema        | JSON/YAML schema      | Validate cấu trúc                    |

Ví dụ một MarkdownDoc không chỉ là text. Nó có thể chứa:

* title;
* owner;
* status;
* reviewer;
* approval state;
* document type;
* service reference;
* section ID;
* linked sheet;
* linked slide;
* dependency;
* generated-by Agent metadata.

Content layer cần thỏa mãn 5 yêu cầu:

1. **Readable**: con người có thể đọc source.
2. **Diffable**: có thể so sánh thay đổi.
3. **Parseable**: có thể chuyển thành AST.
4. **Portable**: không bị khóa vào vendor.
5. **Extensible**: có thể thêm metadata, block, schema, reference.

---

## E3. AST Layer

AST layer là trái tim kỹ thuật của MarkdownOffice.

Khi user hoặc Agent tạo một file `.mdoc`, `.mslide`, `.msheet`, hệ thống không chỉ lưu text. Nó parse file thành AST.

AST dùng cho:

* rendering;
* editor interaction;
* validation;
* semantic extraction;
* section detection;
* table detection;
* reference detection;
* dependency detection;
* document diff;
* block-level merge;
* policy enforcement;
* Agent context retrieval;
* slide generation;
* report generation;
* sheet generation.

Có thể hiểu:

```text
Markdown Source
      ↓
Markdown Parser
      ↓
Base AST
      ↓
Enriched AST
      ↓
Domain Model
      ↓
Rendering / Agent / Workflow / Versioning
```

### Vì sao AST quan trọng?

Nếu chỉ lưu Markdown thô, Agent có thể đọc text nhưng khó thao tác chính xác.

Nếu có AST, Agent có thể nói:

* sửa section `risk-analysis`;
* cập nhật table `service-ownership`;
* tạo slide từ heading cấp 2;
* lấy các action item chưa hoàn thành;
* kiểm tra document có thiếu phần approval không;
* xác định chart nào lấy dữ liệu từ sheet nào;
* diff thay đổi ở block nào;
* rollback một section thay vì rollback cả file.

AST biến document thành dữ liệu có cấu trúc.

---

## E4. Domain Model Layer

AST là cấu trúc cú pháp. Domain model là cấu trúc nghiệp vụ.

Ví dụ AST biết có một table.
Domain model hiểu đó là **Service Ownership Matrix**.

Ví dụ AST biết có một heading.
Domain model hiểu đó là **Risk Section** của handover report.

Domain model cần biểu diễn:

| Entity      | Ý nghĩa                                |
| ----------- | -------------------------------------- |
| Document    | Một knowledge artifact                 |
| Section     | Một phần có semantic role              |
| Block       | Paragraph, table, chart, callout, task |
| Table       | Structured data                        |
| Slide       | Presentation unit                      |
| Sheet       | Tabular model                          |
| Chart       | Visualization từ data source           |
| Reference   | Link giữa các object                   |
| Task        | Action item                            |
| Approval    | Review state                           |
| Actor       | Human hoặc Agent                       |
| Transaction | Một thay đổi có audit                  |
| Policy      | Rule kiểm soát workflow                |

Domain model là thứ giúp MarkdownOffice vượt khỏi “Markdown editor” và trở thành “Document Operating System”.

---

## E5. Document Graph Layer

Document graph là lớp biến tài liệu thành mạng lưới tri thức.

Mỗi document, section, table, slide, chart, task, service, owner, decision record đều có thể là node hoặc edge trong graph.

Ví dụ:

```text
Service Ownership Doc
   ├── links to Architecture Doc
   ├── links to Deployment Runbook
   ├── links to Incident History
   ├── links to SLA Document
   ├── links to Monitoring Dashboard
   ├── links to Ownership Sheet
   └── generates Handover Slide Deck
```

Graph cần tracking:

* document reference;
* section reference;
* table reference;
* slide source;
* chart source;
* formula dependency;
* service dependency;
* owner relationship;
* approval state;
* permission boundary;
* generated artifact provenance;
* stale data detection;
* impact scope.

### Giá trị của document graph

Document graph giúp trả lời các câu hỏi enterprise rất quan trọng:

* Service này ai đang own?
* Nếu owner nghỉ việc, tài liệu nào bị ảnh hưởng?
* Slide deck này lấy dữ liệu từ sheet nào?
* Sheet này có được cập nhật sau incident mới nhất chưa?
* Report này có section nào chưa approve?
* Agent nào tạo phần này?
* Có thể rollback phần nào mà không ảnh hưởng toàn bộ document?
* Nếu đổi SLA, slide và report nào cần cập nhật?

Đây là điểm Microsoft Office / Google Workspace chưa giải quyết sâu ở cấp kiến trúc.

---

## E6. Rendering Layer

MarkdownOffice cần nhiều rendering mode để phục vụ nhiều loại user khác nhau.

### MarkdownDoc rendering

* Markdown source mode;
* Word-like viewer;
* paginated document viewer;
* enterprise report viewer;
* PDF export;
* structured AI-readable viewer.

### MarkdownSlide rendering

* Markdown slide editor;
* PowerPoint-like viewer;
* presentation mode;
* speaker mode;
* PDF export;
* HTML interactive deck;
* AI-generated slide preview.

### MarkdownSheet rendering

* raw CSV mode;
* Markdown table mode;
* Excel-like grid viewer;
* formula evaluation mode;
* chart mode;
* dashboard mode;
* AI-readable structured table view.

Điểm chiến lược: **MarkdownOffice không bắt user học Markdown**.

Người dùng phổ thông thấy:

* Word-like document;
* PowerPoint-like deck;
* Excel-like grid.

Engineer và Agent thấy:

* Markdown source;
* AST;
* schema;
* graph;
* diff;
* metadata;
* transaction.

---

## E7. Collaboration Layer

Collaboration layer cần hỗ trợ cả human-human và human-Agent collaboration.

Các năng lực chính:

* real-time editing;
* CRDT hoặc OT;
* block-level editing;
* section-level locking;
* comment;
* suggestion mode;
* presence;
* review thread;
* proposal workflow;
* conflict detection;
* permission-aware editing.

Khác với Google Docs chỉ tập trung vào live editing, MarkdownOffice cần thêm chiều sâu cho enterprise workflow:

* ai được sửa section nào;
* Agent chỉ được propose hay được commit;
* section nào cần reviewer;
* bảng nào không được thay đổi nếu chưa có approval;
* document nào thuộc compliance scope;
* merge conflict được xử lý theo policy nào.

---

## E8. Version Management Layer — Lob

Lob là lớp version management cho MarkdownOffice.

Nó cần hỗ trợ:

* commit;
* branch;
* document transaction;
* block-level diff;
* AST diff;
* semantic diff;
* slide diff;
* table row diff;
* sheet formula diff;
* merge;
* conflict resolution;
* rollback;
* signed transaction;
* human/agent attribution;
* policy-based merge;
* long-running parallel version.

### Vì sao không dùng Git nguyên bản?

Git rất mạnh cho code, nhưng chưa đủ tự nhiên cho document workflow:

| Vấn đề            | Git truyền thống   | Lob cần làm tốt hơn          |
| ----------------- | ------------------ | ---------------------------- |
| Diff              | line-based         | block/AST/semantic diff      |
| Document approval | không native       | section/block approval       |
| Slide             | file diff khó hiểu | slide-level diff             |
| Sheet             | CSV diff thô       | row/cell/formula diff        |
| Agent attribution | author string      | human/Agent identity         |
| Permission        | repo-level         | document/section/block-level |
| Merge             | text merge         | policy-based reconciliation  |
| Long workflow     | branch thủ công    | multi-version workspace      |

Lob là “Git for document + Agent workflow”, không chỉ là Git wrapper.

---

## E9. Agent Interaction Layer

Agent interaction layer là lớp cho AI Agent làm việc an toàn với tài liệu.

Agent cần các quyền khác nhau:

| Permission       | Ý nghĩa                               |
| ---------------- | ------------------------------------- |
| Read             | Đọc document theo permission          |
| Retrieve context | Lấy context từ graph                  |
| Propose          | Đề xuất thay đổi                      |
| Edit             | Sửa trong phạm vi được phép           |
| Validate         | Kiểm tra schema/compliance            |
| Approve          | Chỉ Agent đặc biệt hoặc human mới có  |
| Commit           | Ghi transaction vào Lob               |
| Rollback         | Phục hồi thay đổi nếu policy cho phép |

Agent không nên chỉ “generate text”. Agent phải làm việc theo transaction:

```text
Read context
→ Build reasoning summary
→ Generate artifact
→ Validate schema
→ Detect affected files
→ Create proposal
→ Human review
→ Commit to Lob
→ Update graph
→ Audit trail
```

Điểm khác biệt lớn: Agent không hoạt động như chatbot bên ngoài tài liệu. Agent trở thành **first-class actor** trong document operating system.

---

## E10. Workflow Automation Layer

Workflow automation layer xử lý các quy trình như:

* handover;
* onboarding;
* incident review;
* policy approval;
* report generation;
* slide generation;
* compliance check;
* stale document detection;
* recurring executive update;
* service ownership refresh;
* project status synthesis.

Ví dụ workflow handover:

```text
Trigger: employee transfer / resignation
      ↓
Permission-aware scan
      ↓
Build ownership map
      ↓
Generate handover report
      ↓
Generate transfer sheet
      ↓
Generate presentation deck
      ↓
Validate missing information
      ↓
Open proposal
      ↓
Review & approval
      ↓
Commit to Lob
      ↓
Update document graph
```

Workflow layer giúp MarkdownOffice trở thành enterprise automation platform, không chỉ document editor.

---

## E11. Audit & Compliance Layer

Enterprise adoption cần audit và compliance từ đầu.

Audit layer cần tracking:

* ai sửa;
* Agent nào sửa;
* sửa file nào;
* sửa block nào;
* lý do sửa;
* source context nào được dùng;
* validation nào đã chạy;
* reviewer nào approve;
* policy nào được áp dụng;
* commit nào chứa thay đổi;
* rollback point ở đâu;
* artifact nào được generate từ nguồn nào.

MarkdownOffice cần audit ở cấp:

* repository;
* document;
* section;
* block;
* table row;
* slide;
* formula;
* Agent transaction;
* approval workflow.

Đây là điểm quan trọng để bán vào enterprise: hệ thống không chỉ “AI tạo nhanh hơn”, mà còn “AI làm việc có kiểm soát, có trace, có rollback”.

---

## E12. Permission Layer

Permission cần đi sâu hơn file-level.

Các mức permission nên có:

| Level          | Ví dụ                                     |
| -------------- | ----------------------------------------- |
| Workspace      | Ai được vào repository                    |
| Folder/module  | Team nào được truy cập                    |
| Document       | Ai được đọc/sửa                           |
| Section        | Section pháp lý chỉ legal team sửa        |
| Block          | Một bảng salary chỉ HR thấy               |
| Sheet column   | Cột compensation bị ẩn                    |
| Slide          | Slide investor chỉ leadership thấy        |
| Agent scope    | Agent chỉ được propose, không được commit |
| Workflow scope | Handover Agent chỉ đọc tài liệu liên quan |

Permission phải tích hợp với graph.

Ví dụ: nếu slide được generate từ sheet có dữ liệu nhạy cảm, slide cũng phải kế thừa policy hoặc bị đánh dấu cần review.

---

## E13. Data Flow tổng thể

Một flow chuẩn trong MarkdownOffice:

```text
Markdown Source
   ↓
AST
   ↓
Domain Model
   ↓
Document Graph
   ↓
Rendering Views
   ↓
Agent Workflow
   ↓
Proposal
   ↓
Review
   ↓
Lob Commit
   ↓
Audit + Graph Update
```

Flow từ document → sheet → slide:

```text
MarkdownDoc Report
   ↓ extract structured sections
MarkdownSheet Status Table
   ↓ compute metrics / charts
MarkdownSlide Executive Deck
   ↓ presentation mode
Team Review / Meeting
   ↓ approved updates
Lob Commit + Audit Trail
```

Đây là data flow quan trọng cho slide thuyết trình: nó cho thấy MarkdownOffice không phải ba app riêng lẻ, mà là một **unified knowledge pipeline**.

---

# F. Core Products

# F1. MarkdownDoc

## Vai trò

MarkdownDoc là sản phẩm tương ứng với Microsoft Word / Google Docs trong hệ sinh thái MarkdownOffice.

Nhưng MarkdownDoc không chỉ là trình soạn thảo văn bản. Nó là **structured enterprise document engine**.

MarkdownDoc dùng để tạo:

* business report;
* technical specification;
* architecture document;
* product requirement document;
* legal document;
* policy document;
* onboarding guide;
* service ownership document;
* incident report;
* handover report;
* internal knowledge base;
* decision record;
* compliance document.

---

## F1.1 Markdown-native source of truth

Mỗi MarkdownDoc có source là `.mdoc` hoặc `.md`.

Bên dưới có thể chứa:

* frontmatter metadata;
* headings;
* sections;
* block IDs;
* callouts;
* tables;
* task lists;
* references;
* code blocks;
* diagrams;
* schema declarations;
* approval metadata.

Ví dụ logical structure:

```text
Document
   ├── Metadata
   ├── Executive Summary
   ├── Context
   ├── Architecture
   ├── Risk
   ├── Action Items
   ├── Linked Sheets
   └── Approval State
```

MarkdownDoc cần có stable block ID để Agent và Lob có thể thao tác chính xác ở cấp section/block.

---

## F1.2 Viewer giống Word

MarkdownDoc cần cung cấp Word-like viewer để user phổ thông cảm thấy quen thuộc.

Các năng lực cần có:

* page layout;
* typography;
* heading style;
* table style;
* image embedding;
* comment;
* suggestion;
* document outline;
* export PDF;
* export DOCX-like output nếu cần;
* print-ready report;
* enterprise template.

Nhưng sự khác biệt là Word-like viewer chỉ là một view. Source of truth vẫn là Markdown.

---

## F1.3 Agent interaction

Agent có thể làm các tác vụ:

* tạo report từ document graph;
* tóm tắt service ownership;
* update incident section;
* phát hiện missing documentation;
* viết executive summary;
* convert meeting notes thành action items;
* tạo proposal chỉnh sửa;
* check schema;
* check compliance;
* liên kết document với sheet/slide;
* đánh dấu stale section;
* tạo handover report.

Ví dụ:

```text
Agent reads:
- architecture docs
- runbooks
- incident history
- ownership sheet

Agent generates:
- handover report
- risk section
- action items
- recommended next owner

Agent opens:
- document proposal for review
```

---

## F1.4 MVP cho MarkdownDoc

MVP nên build trước vì đây là lõi của toàn hệ thống.

Scope đề xuất:

1. Markdown editor + preview.
2. Word-like document viewer.
3. Frontmatter metadata.
4. Document outline.
5. Block ID.
6. Basic AST parsing.
7. Version history.
8. Comment/suggestion.
9. Basic Agent draft/proposal.
10. Export PDF.

Không nên bắt đầu bằng clone toàn bộ Word. Nên bắt đầu bằng **structured report + enterprise document workflow**.

---

# F2. MarkdownSlide

## Vai trò

MarkdownSlide là sản phẩm tương ứng với Microsoft PowerPoint / Google Slides.

Nó dùng để tạo:

* executive deck;
* project status presentation;
* technical architecture presentation;
* incident review deck;
* handover presentation;
* investor deck;
* training material;
* workshop slide;
* product roadmap deck.

MarkdownSlide cần đặc biệt mạnh ở khả năng **generate presentation từ document graph**.

---

## F2.1 Markdown-native slide source

MarkdownSlide có source là `.mslide`.

Source có thể dựa trên Markdown slide syntax:

```text
---
title: Handover Presentation
owner: Platform Team
source_doc: service-handover.mdoc
source_sheet: ownership-transfer.msheet
---

# Title Slide

---

# Scope of Ownership

---

# Critical Services

---

# Risk and Timeline
```

Mỗi slide nên có:

* slide ID;
* title;
* layout;
* content blocks;
* speaker notes;
* source reference;
* chart reference;
* generated-by metadata;
* stale status;
* approval state.

---

## F2.2 Viewer giống PowerPoint

MarkdownSlide cần có PowerPoint-like viewer:

* slide thumbnail sidebar;
* canvas-based slide editor;
* layout templates;
* theme;
* presenter mode;
* speaker notes;
* full-screen presentation;
* PDF export;
* HTML interactive export;
* image export;
* PPTX-compatible export nếu roadmap cho phép.

Người dùng business không cần biết slide source là Markdown. Họ chỉ cần thấy slide đẹp, dễ trình bày, dễ chỉnh.

---

## F2.3 Agent interaction

MarkdownSlide là nơi Agent có thể tạo deck từ tri thức có sẵn.

Agent có thể:

* tạo executive deck từ report;
* tạo project update từ status sheet;
* tạo handover deck từ service ownership graph;
* tạo architecture presentation từ system docs;
* tạo incident review deck từ postmortem + timeline;
* tạo investor deck từ product docs;
* tạo training deck từ onboarding docs.

Điểm mạnh nhất:

```text
Document graph → Structured outline → Slide plan → Slide deck → Review → Presentation
```

Agent không chỉ tạo slide text. Agent cần tạo:

* narrative flow;
* slide structure;
* visual suggestion;
* speaker notes;
* source references;
* stale detection;
* reviewer suggestion.

---

## F2.4 Slide không phải artifact tĩnh

Trong PowerPoint truyền thống, slide thường bị copy-paste từ nhiều nguồn. Sau một thời gian, dữ liệu dễ stale.

Trong MarkdownSlide, slide có thể liên kết với:

* source document;
* source sheet;
* chart;
* KPI table;
* incident timeline;
* service metadata;
* dependency graph.

Nếu source thay đổi, slide có thể:

* được đánh dấu stale;
* yêu cầu refresh;
* tự update theo policy;
* mở proposal để review thay đổi;
* giữ snapshot cũ cho audit.

Đây là lợi thế cực mạnh cho enterprise reporting.

---

## F2.5 MVP cho MarkdownSlide

MarkdownSlide MVP nên tập trung vào:

1. Markdown slide syntax.
2. Presentation mode.
3. Basic layout templates.
4. Speaker notes.
5. Export PDF.
6. Generate deck from MarkdownDoc.
7. Insert table/chart from MarkdownSheet.
8. Source reference tracking.
9. Stale indicator cơ bản.
10. Agent-generated outline + slide proposal.

Không cần clone toàn bộ PowerPoint từ đầu. Nên tập trung vào **AI-generated report deck** và **technical/business presentation workflow**.

---

# F3. MarkdownSheet

## Vai trò

MarkdownSheet là sản phẩm tương ứng với Microsoft Excel / Google Sheets.

Nhưng MarkdownSheet không nên bắt đầu bằng mục tiêu clone Excel toàn bộ. Excel là sản phẩm cực lớn. MarkdownSheet nên bắt đầu từ use case enterprise workflow:

* status tracking;
* ownership matrix;
* transfer checklist;
* project timeline;
* KPI table;
* cost estimation;
* risk scoring;
* resource planning;
* formula nhẹ;
* chart cơ bản;
* dashboard từ structured table.

MarkdownSheet là **structured table engine** cho MarkdownOffice.

---

## F3.1 CSV-first / Markdown-wrapped source of truth

MarkdownSheet có thể dùng:

* `.csv` cho raw table data;
* `.msheet` cho wrapper có metadata, schema, formula, chart, permission;
* Markdown table cho bảng nhỏ;
* frontmatter cho owner, status, source, schema.

Ví dụ conceptual model:

```text
Sheet
   ├── Metadata
   ├── Tables
   ├── Columns
   ├── Rows
   ├── Formula
   ├── Validation Rules
   ├── Chart Definitions
   ├── Dashboard Blocks
   └── Source References
```

`.msheet` có thể đóng vai trò wrapper:

```text
---
title: Service Ownership Transfer
type: ownership_matrix
owner: platform-team
schema: service-transfer-v1
---

@source ./service-transfer.csv

@chart risk-by-service
@formula completion_percentage
```

CSV giữ data đơn giản. `.msheet` giữ semantic layer.

---

## F3.2 Viewer giống Excel

MarkdownSheet cần Excel-like grid viewer:

* rows;
* columns;
* cell editing;
* sorting;
* filtering;
* frozen header;
* column type;
* validation;
* formula preview;
* chart;
* dashboard;
* import/export CSV;
* copy-paste from Excel;
* basic table formatting.

Nhưng điểm khác biệt: sheet không phải app riêng lẻ. Sheet là node trong document graph.

Một table trong MarkdownSheet có thể được dùng bởi:

* MarkdownDoc report;
* MarkdownSlide deck;
* dashboard;
* workflow automation;
* Agent reasoning;
* compliance report.

---

## F3.3 Agent interaction

Agent có thể dùng MarkdownSheet để tạo và cập nhật structured data.

Ví dụ trong handover workflow, Agent tạo sheet gồm:

* service name;
* owner hiện tại;
* backup owner;
* transfer owner;
* business criticality;
* technical complexity;
* documentation status;
* test coverage status;
* deployment risk;
* open incidents;
* pending tasks;
* handover completion percentage.

Agent cũng có thể tính:

* số ngày còn lại;
* số buổi handover cần có;
* số service cần chuyển giao;
* effort estimate;
* priority score;
* risk score;
* workload distribution;
* transfer progress.

---

## F3.4 Formula và dependency tracking

MarkdownSheet không cần clone toàn bộ Excel formula ngay từ đầu.

MVP nên hỗ trợ:

* arithmetic basic;
* percentage;
* weighted score;
* date difference;
* status aggregation;
* lookup đơn giản;
* conditional formatting cơ bản;
* chart từ table;
* validation rule.

Quan trọng hơn formula là **dependency tracking**.

Ví dụ:

```text
handover_completion = completed_items / total_items
risk_score = business_criticality * technical_complexity * documentation_gap
days_remaining = resignation_date - today
```

Nếu `documentation_gap` thay đổi, hệ thống biết:

* sheet nào bị ảnh hưởng;
* chart nào cần update;
* slide nào đang stale;
* report section nào cần refresh.

Đây là điểm biến MarkdownSheet thành workflow data engine, không chỉ spreadsheet.

---

## F3.5 MVP cho MarkdownSheet

Scope đề xuất:

1. CSV import/export.
2. `.msheet` wrapper.
3. Excel-like grid viewer.
4. Column type.
5. Basic formula.
6. Sorting/filtering.
7. Chart cơ bản.
8. Link table vào MarkdownDoc.
9. Link chart vào MarkdownSlide.
10. Version diff cho row/cell/table.
11. Agent-generated tracking sheet.
12. Basic dashboard view.

MarkdownSheet nên đi theo hướng “enterprise structured table + workflow matrix” trước, sau đó mới mở rộng dần về Excel compatibility.

---

# F4. Ba sản phẩm không nên tách rời

Sai lầm lớn là build MarkdownDoc, MarkdownSlide, MarkdownSheet như ba app riêng biệt.

Chúng phải dùng chung:

* content layer;
* AST parser;
* metadata model;
* document graph;
* permission model;
* Lob version layer;
* Cortexpod repository;
* Agent interaction layer;
* audit/compliance system;
* template/schema engine.

Nên hiểu:

```text
MarkdownDoc = narrative knowledge
MarkdownSheet = structured/tabular knowledge
MarkdownSlide = presentation knowledge
```

Ba loại knowledge này liên kết với nhau.

Ví dụ:

```text
Incident Report.mdoc
      ↓ extracts action items
Incident Checklist.msheet
      ↓ creates charts/timeline
Incident Review Deck.mslide
      ↓ presented to leadership
Review feedback
      ↓ updates report + checklist
Lob commit
      ↓ audit trail + graph update
```

Đây chính là sức mạnh của MarkdownOffice ecosystem.

---

# F5. Product architecture theo hướng “one engine, many surfaces”

MarkdownOffice nên có một engine lõi, nhiều surface:

```text
Core Engine
   ├── MarkdownDoc Surface
   ├── MarkdownSlide Surface
   ├── MarkdownSheet Surface
   ├── Chat Surface
   ├── Dashboard Surface
   ├── Calendar Surface
   └── Agent Surface
```

Core Engine gồm:

* Markdown parser;
* AST enrichment;
* document graph;
* schema validation;
* permission;
* version;
* audit;
* workflow;
* agent transaction.

Surface chỉ là cách hiển thị và tương tác.

Đây là chiến lược quan trọng vì nó giúp MarkdownOffice mở rộng mà không bị vỡ kiến trúc.

---

# F6. Tóm tắt phần kiến trúc cho slide

Có thể trình bày phần này thành một slide kiến trúc như sau:

## Slide title

**MarkdownOffice Architecture: One Knowledge Engine, Multiple Office Views**

## Key message

MarkdownOffice không chỉ là bộ editor. Nó là một document operating system có một source of truth Markdown-native, một AST engine, một document graph, một version layer và nhiều rendering mode giống Word, PowerPoint, Excel.

## Visual suggestion

Một sơ đồ layer:

```text
MarkdownDoc | MarkdownSlide | MarkdownSheet
             ↓
Rendering Layer
             ↓
AST + Domain Model
             ↓
Document Graph
             ↓
Lob Version Management
             ↓
Cortexpod Repository Hub
             ↓
Permission + Audit + Compliance
```

## Speaker note

“Người dùng nhìn thấy Word, PowerPoint và Excel-like experience. Nhưng bên dưới, toàn bộ tài liệu là structured data có thể version, audit, validate và cho AI Agent thao tác an toàn.”

---

# Phần 3 — G đến H

Phần này phân tích cách **Cortexpod** và **Lob Version Management** trở thành hạ tầng nền cho MarkdownOffice, sau đó đi sâu vào use case **employee handover** như một ví dụ có thể dùng trực tiếp trong slide báo cáo. Các thành phần này bám theo nguyên tắc lõi của MarkdownOffice: document có cấu trúc, có graph, có agent transaction, có version history, có audit và có workflow automation. 

---

# G. Cortexpod + Lob Integration

## G1. Tư duy tổng thể

MarkdownOffice là application layer.
Cortexpod là collaboration platform layer.
Lob là version management layer.

Có thể hiểu theo mapping:

```text
MarkdownOffice = Microsoft Office / Google Workspace alternative
Cortexpod      = GitHub-like hub for documents, workflows and Agents
Lob           = Git-like version system for structured documents
```

Ba lớp này không nên tách rời. Chúng tạo thành một hệ thống thống nhất:

```text
MarkdownDoc / MarkdownSlide / MarkdownSheet
        ↓
Cortexpod Workspace
        ↓
Lob Version Management
        ↓
Document Graph + Audit + Permission
```

Trong mô hình này:

* User tạo và chỉnh tài liệu qua MarkdownOffice.
* Cortexpod quản lý repository, permission, review, workspace, agent workflow.
* Lob ghi nhận version, diff, branch, merge, rollback và audit transaction.
* Agent hoạt động như một actor có quyền hạn, proposal, reasoning và attribution rõ ràng.

---

## G2. Cortexpod là centralized document hub

Cortexpod nên được định vị như **GitHub for enterprise documents and AI Agent workflows**.

Nếu GitHub là nơi team phần mềm quản lý code, issue, pull request, review và deployment workflow, thì Cortexpod là nơi doanh nghiệp quản lý:

* document repository;
* slide repository;
* sheet repository;
* service ownership;
* workflow;
* decision record;
* knowledge graph;
* team permission;
* agent workspace;
* review request;
* audit trail;
* compliance evidence;
* generated report;
* generated presentation;
* document analytics.

Cortexpod không chỉ là nơi lưu file. Nó là **control plane** cho enterprise knowledge.

---

## G3. Repository trong Cortexpod không chỉ chứa file

Một repository trong Cortexpod có thể chứa:

```text
/company-knowledge
  /services
    payment-service.mdoc
    user-profile-service.mdoc
    notification-service.mdoc

  /runbooks
    deployment-runbook.mdoc
    incident-response.mdoc

  /slides
    q3-executive-report.mslide
    handover-presentation.mslide

  /sheets
    service-ownership.msheet
    transfer-status.msheet
    risk-register.msheet

  /decisions
    adr-001-database-choice.mdoc
    adr-002-cache-layer.mdoc

  /policies
    security-policy.mdoc
    data-retention-policy.mdoc
```

Nhưng bên dưới, Cortexpod không chỉ thấy folder và file. Nó hiểu:

* document type;
* owner;
* reviewer;
* team;
* status;
* approval state;
* dependency;
* generated artifact;
* stale state;
* permission boundary;
* Agent activity;
* Lob commit history.

Đây là điểm khác biệt lớn so với file storage hoặc cloud drive.

---

## G4. Document monorepo

MarkdownOffice nên dùng khái niệm **document monorepo**.

Document monorepo là nơi toàn bộ tri thức vận hành của doanh nghiệp được gom vào một cấu trúc có version, permission và graph.

Nó bao gồm:

* technical documents;
* business reports;
* policies;
* runbooks;
* onboarding docs;
* decision records;
* service ownership docs;
* status sheets;
* executive decks;
* incident reviews;
* workflow templates;
* generated artifacts.

Ưu điểm:

1. **Một source of truth rõ ràng**
   Không còn nhiều bản rải rác ở Google Drive, Slack, email hoặc laptop cá nhân.

2. **Dễ tìm kiếm và liên kết**
   Document không chỉ search bằng keyword mà còn bằng graph relationship.

3. **Agent có workspace chuẩn**
   Agent không cần tự dò qua nhiều SaaS khác nhau.

4. **Version control nhất quán**
   Mọi thay đổi đi qua Lob transaction.

5. **Audit và compliance mạnh hơn**
   Doanh nghiệp biết ai sửa gì, khi nào, vì sao, dựa trên nguồn nào.

6. **Dễ tạo report, slide, sheet tự động**
   Vì document, sheet và slide cùng nằm trong một knowledge pipeline.

---

## G5. Permission-aware repository

Cortexpod phải có permission model sâu hơn folder/file permission truyền thống.

Các cấp permission cần có:

| Cấp           | Ví dụ                                  |
| ------------- | -------------------------------------- |
| Organization  | Ai thuộc công ty                       |
| Workspace     | Ai được vào workspace                  |
| Repository    | Team nào truy cập repo                 |
| Folder/module | Team nào đọc phần service              |
| Document      | Ai được đọc/sửa document               |
| Section       | Legal section chỉ legal team sửa       |
| Table         | Finance table chỉ finance team thấy    |
| Column        | Compensation column bị ẩn              |
| Slide         | Investor slide chỉ leadership thấy     |
| Agent         | Agent chỉ được propose                 |
| Workflow      | Handover Agent chỉ đọc scope liên quan |

Điểm quan trọng: permission phải đi cùng document graph.

Ví dụ:

Nếu một slide được tạo từ sheet có dữ liệu confidential, slide đó phải:

* kế thừa permission;
* bị đánh dấu sensitive;
* yêu cầu reviewer phù hợp;
* không được export/public nếu policy không cho phép.

---

## G6. Agent workspace trong Cortexpod

Cortexpod cần có workspace riêng cho Agent.

Agent không nên hoạt động như một chatbot bên ngoài. Agent cần làm việc trong một không gian có:

* task scope;
* permission scope;
* input documents;
* allowed actions;
* generated artifacts;
* reasoning summary;
* validation result;
* proposal;
* review thread;
* audit trail;
* rollback point.

Ví dụ Agent task:

```text
Task: Generate employee handover package

Inputs:
- service ownership docs
- runbooks
- incident history
- open tasks
- transfer policy
- team ownership sheet

Allowed actions:
- read permitted documents
- create .mdoc report
- create .msheet tracking sheet
- create .mslide presentation
- open proposal

Not allowed:
- commit directly
- access salary documents
- modify production runbook without approval
```

Cách này giúp doanh nghiệp kiểm soát Agent bằng policy thay vì chỉ dựa vào prompt.

---

## G7. Agent proposal thay cho Agent auto-commit

Trong enterprise, Agent không nên tự ý sửa tài liệu quan trọng rồi commit ngay.

Workflow tốt hơn:

```text
Agent reads context
      ↓
Agent generates changes
      ↓
Agent validates output
      ↓
Agent creates proposal
      ↓
Human reviews
      ↓
Policy checks
      ↓
Approve / request changes / reject
      ↓
Commit to Lob
```

Agent proposal nên bao gồm:

* summary;
* changed files;
* generated files;
* affected sections;
* source references;
* confidence score;
* validation result;
* risk level;
* missing information;
* suggested reviewers;
* rollback plan.

Nó tương tự Pull Request, nhưng dành cho document, slide, sheet và workflow.

---

## G8. Lob là version layer cho document transaction

Lob cần ghi nhận mọi thay đổi dưới dạng **document transaction**.

Một transaction không chỉ là text diff. Nó nên chứa:

* actor: human hoặc Agent;
* intent: vì sao thay đổi;
* source context;
* affected files;
* affected blocks;
* semantic change;
* validation result;
* approval state;
* timestamp;
* rollback pointer;
* signature nếu cần;
* policy decision.

Ví dụ:

```text
Transaction: handover-package-generation
Actor: HandoverAgent
Approved by: Team Lead
Files created:
- handover-report.mdoc
- service-ownership-transfer.msheet
- handover-presentation.mslide

Files updated:
- service-index.mdoc
- ownership-map.msheet

Source references:
- payment-service.mdoc
- incident-history.mdoc
- jira-export.msheet

Policy:
- Agent cannot commit without human approval
- Sensitive docs excluded
```

Đây là audit trail mà doanh nghiệp cần để tin tưởng AI-generated documents.

---

## G9. Branch và multi-version workspace

Document workflow trong doanh nghiệp thường kéo dài. Một report, policy, proposal hoặc handover package có thể có nhiều version song song.

Lob cần hỗ trợ:

* branch cho document;
* branch cho workflow;
* branch cho Agent-generated proposal;
* branch cho draft;
* branch cho scenario planning;
* branch cho policy revision;
* branch cho slide deck variant.

Ví dụ:

```text
main
 ├── handover/thanh-v1
 ├── handover/thanh-risk-focused
 ├── handover/thanh-executive-summary
 └── handover/thanh-engineering-detail
```

Mỗi branch có thể chứa:

* report variant;
* slide variant;
* sheet calculation variant;
* different risk assumptions;
* different timeline;
* different reviewer comments.

Lob phải quản lý việc merge theo policy, không chỉ line merge.

---

## G10. Document diff cần vượt khỏi line diff

Line diff chưa đủ cho enterprise documents.

Lob cần nhiều loại diff:

| Artifact      | Diff cần có                                        |
| ------------- | -------------------------------------------------- |
| MarkdownDoc   | section diff, block diff, semantic diff            |
| MarkdownSlide | slide diff, layout diff, content diff, source diff |
| MarkdownSheet | row diff, cell diff, formula diff, schema diff     |
| Metadata      | owner/status/permission diff                       |
| Graph         | dependency diff, reference diff                    |
| Agent         | reasoning/provenance diff                          |
| Approval      | review state diff                                  |

Ví dụ với MarkdownDoc:

```text
Section changed:
- "Known Risks" updated
- Added 2 new risks
- Removed outdated deployment warning
- Linked to incident INC-2026-07

Semantic change:
- Risk level changed from Medium to High
```

Ví dụ với MarkdownSlide:

```text
Slide 7 changed:
- Updated risk chart
- Source sheet changed from v12 to v13
- Speaker note regenerated
- Slide marked as reviewed
```

Ví dụ với MarkdownSheet:

```text
Rows changed:
- payment-service transfer owner updated
- documentation status changed from 60% to 85%
- risk score recalculated from 8.4 to 5.1
```

---

## G11. Merge theo policy

Document merge không thể chỉ dựa vào text.

Lob cần policy-based merge.

Ví dụ policy:

* Agent-generated legal content phải có legal reviewer.
* Sheet có financial number phải có finance approval.
* Slide dùng confidential data không được export public.
* Section đã approved không được Agent sửa trực tiếp.
* Runbook production cần service owner approve.
* Formula thay đổi phải chạy validation.
* Report generated từ stale sheet không được merge.

Merge process:

```text
Proposal
  ↓
AST diff
  ↓
Policy check
  ↓
Validation
  ↓
Reviewer approval
  ↓
Merge to target branch
  ↓
Graph update
  ↓
Audit log
```

Đây là nơi Lob khác Git: merge không chỉ là hợp nhất text, mà là hợp nhất tri thức có permission, schema và policy.

---

## G12. Rollback và recovery

Rollback trong MarkdownOffice cần nhiều cấp:

| Cấp rollback               | Ví dụ                                      |
| -------------------------- | ------------------------------------------ |
| Repository rollback        | Quay lại trạng thái repo trước transaction |
| Document rollback          | Khôi phục một document                     |
| Section rollback           | Khôi phục một section                      |
| Sheet row rollback         | Khôi phục một dòng                         |
| Formula rollback           | Khôi phục formula                          |
| Slide rollback             | Khôi phục một slide                        |
| Agent transaction rollback | Hoàn tác toàn bộ artifact Agent tạo        |
| Graph rollback             | Khôi phục dependency state                 |

Ví dụ nếu Agent tạo handover package sai, hệ thống có thể rollback:

* report;
* transfer sheet;
* presentation deck;
* graph links;
* metadata updates;
* generated artifact references.

Rollback này phải để lại audit trail, không được xóa dấu vết.

---

## G13. Cortexpod dashboard

Cortexpod nên có dashboard cho enterprise knowledge:

* document health;
* stale documents;
* missing owners;
* unreviewed Agent proposals;
* pending approvals;
* high-risk services;
* outdated runbooks;
* generated reports;
* slide decks linked to stale data;
* open handover workflows;
* compliance violations;
* Agent activity summary.

Ví dụ dashboard cho team lead:

```text
Team Knowledge Health
- 12 services
- 3 services missing backup owner
- 5 runbooks outdated
- 2 handover workflows active
- 4 Agent proposals pending review
- 1 executive deck stale
- 7 documents missing approval
```

Đây là sản phẩm có giá trị rất lớn với manager và enterprise.

---

# H. Agent Workflow Example — Employee Handover

## H1. Vì sao employee handover là use case rất mạnh

Employee handover là use case lý tưởng để trình bày MarkdownOffice vì nó chạm vào nhiều vấn đề thật của doanh nghiệp:

* tri thức nằm rải rác;
* service ownership không rõ;
* runbook thiếu cập nhật;
* project task chưa chuyển giao;
* incident context nằm trong Slack/Jira/email;
* người nghỉ việc giữ nhiều tribal knowledge;
* tài liệu hiện tại không đủ structured;
* manager cần report;
* team cần checklist;
* leadership cần slide;
* HR/compliance cần audit.

Handover cũng là workflow có ROI rõ ràng: giảm rủi ro mất tri thức, giảm downtime, tăng tốc onboarding người nhận, tạo audit trail cho quá trình chuyển giao.

---

## H2. Current-state workflow: cách doanh nghiệp thường làm

Trong mô hình hiện tại:

```text
Người sắp nghỉ
  ↓
Tự viết handover note
  ↓
Gửi Google Doc / Word / email
  ↓
Team lead hỏi thêm trên Slack
  ↓
Một sheet transfer được tạo thủ công
  ↓
Một vài meeting được lên lịch
  ↓
PowerPoint được copy-paste từ docs/sheets
  ↓
Sau khi nghỉ, team vẫn thiếu thông tin
```

Vấn đề:

* thiếu source of truth;
* thiếu service ownership map;
* không biết tài liệu nào đã cập nhật;
* không biết rủi ro nào còn mở;
* slide dễ stale;
* sheet không link với report;
* không có audit đầy đủ;
* không có dependency graph;
* Agent khó hỗ trợ vì dữ liệu phân tán.

---

## H3. Target workflow với MarkdownOffice + Cortexpod + Lob

Workflow mục tiêu:

```text
Trigger handover workflow
      ↓
Permission-aware knowledge scan
      ↓
Build service ownership map
      ↓
Generate MarkdownSheet transfer matrix
      ↓
Generate MarkdownDoc handover report
      ↓
Generate MarkdownSlide presentation
      ↓
Generate calculation sheet
      ↓
Open Agent proposal
      ↓
Human review and approval
      ↓
Commit to Lob
      ↓
Present via MarkdownSlide
      ↓
Track completion and stale data
```

Điểm khác biệt: Agent không tạo một file rời rạc. Agent tạo một **handover package** có report, sheet, slide, metadata, source reference, graph link và audit trail.

---

## H4. Step 1 — Permission-aware knowledge scan

Agent bắt đầu bằng việc scan tri thức theo permission.

Agent không được đọc mọi thứ. Nó chỉ đọc:

* tài liệu người đó sở hữu;
* service người đó maintain;
* runbook liên quan;
* architecture document liên quan;
* incident history được phép;
* task đang mở;
* sheet liên quan;
* slide liên quan;
* decision record liên quan;
* meeting note trong scope;
* dependency graph trong scope.

Agent cần loại trừ:

* tài liệu ngoài quyền;
* thông tin salary;
* private HR record;
* legal sensitive document;
* confidential customer data nếu không thuộc workflow;
* personal note không được tổ chức cho phép.

Output của bước này:

```text
Knowledge Scan Summary
- 8 service documents found
- 4 runbooks found
- 12 open tasks found
- 3 incidents found
- 2 outdated docs detected
- 1 missing backup owner
- 5 source documents excluded by permission
```

Slide message:

> Agent không tìm kiếm mù. Agent scan knowledge theo permission, scope và policy.

---

## H5. Step 2 — Build service ownership map

Sau khi scan, Agent xây dựng service ownership map.

Service ownership map trả lời:

* người này đang own service nào;
* service nào critical;
* service nào có backup owner;
* service nào thiếu documentation;
* service nào có deployment risk;
* service nào có incident gần đây;
* service nào cần handover trước;
* ai nên là next owner.

Ownership map là graph, nhưng có thể render thành sheet hoặc visual.

Ví dụ fields:

| Field                | Ý nghĩa                 |
| -------------------- | ----------------------- |
| Service name         | Tên service             |
| Current owner        | Owner hiện tại          |
| Backup owner         | Người backup            |
| Transfer owner       | Người nhận chuyển giao  |
| Business criticality | Mức quan trọng business |
| Technical complexity | Độ phức tạp kỹ thuật    |
| Documentation status | Mức hoàn thiện tài liệu |
| Deployment risk      | Rủi ro deploy           |
| Open incidents       | Incident chưa đóng      |
| Pending tasks        | Task còn mở             |
| Handover priority    | Ưu tiên chuyển giao     |

Output:

```text
service-ownership-transfer.msheet
```

Slide message:

> MarkdownSheet biến tri thức rời rạc thành bảng chuyển giao có thể tính toán, track và audit.

---

## H6. Step 3 — Generate MarkdownSheet transfer matrix

Agent tạo một MarkdownSheet cho transfer workflow.

Sheet này không chỉ là bảng. Nó là workflow data model.

Các cột đề xuất:

| Column            | Purpose                 |
| ----------------- | ----------------------- |
| Service           | Service cần chuyển giao |
| Current Owner     | Người đang nắm          |
| New Owner         | Người nhận              |
| Backup Owner      | Người backup            |
| Criticality       | Mức quan trọng          |
| Complexity        | Độ phức tạp             |
| Documentation %   | Mức tài liệu hóa        |
| Runbook Status    | Trạng thái runbook      |
| Test Coverage     | Trạng thái test         |
| Last Incident     | Incident gần nhất       |
| Open Tasks        | Task còn mở             |
| Risk Score        | Điểm rủi ro             |
| Handover Sessions | Số buổi cần chuyển giao |
| Completion %      | Tiến độ                 |
| Review Status     | Trạng thái review       |

Formula đơn giản:

```text
Risk Score =
Business Criticality × Technical Complexity × Documentation Gap

Completion % =
Completed Handover Items / Total Handover Items

Priority =
Risk Score + Open Incident Weight + Business Criticality
```

Giá trị:

* team lead thấy tiến độ;
* người nhận biết ưu tiên;
* Agent biết task nào còn thiếu;
* slide có thể lấy chart từ sheet;
* report có thể lấy summary từ sheet;
* Lob có thể diff từng row/cell.

---

## H7. Step 4 — Generate MarkdownDoc handover report

Agent tạo MarkdownDoc handover report.

Cấu trúc đề xuất:

```text
# Employee Handover Report

## Executive Summary
## Scope of Ownership
## Service List
## Architecture Overview
## Main Dependencies
## Deployment Process
## Monitoring and Alerts
## Incident History
## Known Risks
## Open Tasks
## Missing Documentation
## Recommended Next Owners
## Transfer Timeline
## Operational Checklist
## Action Items
## Appendix: Source References
```

Report cần có:

* source reference;
* generated-by Agent metadata;
* confidence level;
* missing information;
* reviewer suggestion;
* linked transfer sheet;
* linked slide deck;
* approval state.

Report không chỉ để đọc. Nó là canonical handover document.

Slide message:

> MarkdownDoc biến scan result thành report có cấu trúc, có nguồn, có reviewer và có audit trail.

---

## H8. Step 5 — Generate MarkdownSlide handover presentation

Agent tạo MarkdownSlide deck cho buổi handover meeting.

Cấu trúc slide:

| Slide | Nội dung                          |
| ----- | --------------------------------- |
| 1     | Title: Employee Handover Overview |
| 2     | Context: Why handover is needed   |
| 3     | Scope of ownership                |
| 4     | Service ownership map             |
| 5     | Critical systems                  |
| 6     | Dependency graph                  |
| 7     | Current project status            |
| 8     | Open incidents and risks          |
| 9     | Transfer checklist                |
| 10    | Timeline                          |
| 11    | Ownership transition plan         |
| 12    | Recommended next actions          |
| 13    | Q&A                               |

Mỗi slide nên có:

* title;
* key message;
* visual;
* speaker note;
* source reference;
* stale status;
* approval state.

Ví dụ:

```text
Slide 8: Open Risks

Key Message:
3 services have high transfer risk due to missing runbook and recent incidents.

Visual:
Risk matrix from service-ownership-transfer.msheet

Speaker Note:
Focus discussion on payment-service and notification-service before final working day.
```

Slide message:

> MarkdownSlide không phải file copy-paste. Nó là presentation view được tạo từ report, sheet và document graph.

---

## H9. Step 6 — Generate calculation sheet

Agent tạo thêm calculation sheet để lên kế hoạch chuyển giao.

Sheet này tính:

* số ngày còn lại trước ngày nghỉ;
* số service cần chuyển giao;
* số buổi handover cần có;
* effort estimate;
* risk score;
* priority score;
* workload distribution;
* completion percentage;
* missing documentation count;
* review status;
* owner assignment balance.

Ví dụ:

| Metric             | Meaning                        |
| ------------------ | ------------------------------ |
| Days remaining     | Còn bao nhiêu ngày             |
| Total services     | Tổng service cần chuyển giao   |
| High-risk services | Service rủi ro cao             |
| Required sessions  | Số buổi handover cần có        |
| Available sessions | Số buổi có thể tổ chức         |
| Completion rate    | Tiến độ                        |
| Documentation gap  | Phần tài liệu thiếu            |
| Owner overload     | Người nhận có bị quá tải không |

Output:

```text
handover-planning.msheet
```

Giá trị:

* manager có kế hoạch thực tế;
* team biết cần ưu tiên gì;
* Agent có thể nhắc việc;
* slide timeline được generate tự động;
* audit ghi lại cơ sở tính toán.

---

## H10. Step 7 — Validation

Trước khi mở proposal, Agent cần validate output.

Validation gồm:

### Document validation

* có đủ required sections không;
* có executive summary không;
* có source reference không;
* có missing information không;
* có owner/reviewer không.

### Sheet validation

* schema đúng không;
* cột bắt buộc đủ không;
* formula có lỗi không;
* row nào thiếu owner không;
* service nào thiếu risk score không.

### Slide validation

* đủ slide chính không;
* slide có source không;
* chart có data không;
* slide nào stale không;
* có speaker notes không.

### Policy validation

* Agent có quyền tạo artifact không;
* có dùng tài liệu ngoài permission không;
* có lộ dữ liệu sensitive không;
* có cần reviewer đặc biệt không;
* có file nào không được auto-update không.

Output:

```text
Validation Summary
- 3 files generated
- 2 source docs stale
- 1 service missing backup owner
- 1 slide requires leadership review
- No permission violation detected
```

---

## H11. Step 8 — Open Agent proposal

Agent mở proposal trong Cortexpod.

Proposal gồm:

```text
Title:
Generate handover package for [employee/team]

Summary:
Created handover report, transfer matrix and presentation deck.

Files created:
- handover-report.mdoc
- service-ownership-transfer.msheet
- handover-presentation.mslide
- handover-planning.msheet

Files updated:
- service-index.mdoc
- team-ownership-overview.msheet

Affected graph nodes:
- payment-service
- notification-service
- deployment-runbook
- incident-history

Validation:
- Passed schema validation
- Requires owner review
- 2 missing documentation warnings

Suggested reviewers:
- Team Lead
- Current Owner
- Next Owner
- Platform Architect
```

Proposal này tương tự Pull Request nhưng dành cho document workflow.

Nó cho phép reviewer:

* xem diff;
* xem source;
* xem generated artifacts;
* comment vào section/slide/row;
* request changes;
* approve;
* merge;
* rollback nếu cần.

---

## H12. Step 9 — Review and approval

Human review là bước bắt buộc trong enterprise.

Reviewer có thể xem:

* MarkdownDoc report diff;
* MarkdownSheet row/cell diff;
* MarkdownSlide slide diff;
* Agent reasoning summary;
* source references;
* confidence score;
* missing information;
* policy check result;
* stale warnings.

Reviewer có thể:

* approve toàn bộ;
* approve một phần;
* yêu cầu Agent chỉnh;
* reject proposal;
* assign reviewer khác;
* lock một section;
* mark section as verified.

Ví dụ comment:

```text
Reviewer comment:
Payment service deployment section is incomplete.
Please include rollback procedure from latest runbook.
```

Agent có thể tạo revision mới trong cùng proposal.

---

## H13. Step 10 — Commit to Lob

Sau khi được approve, thay đổi được commit vào Lob.

Commit không chỉ là text snapshot. Nó là document transaction.

Commit metadata:

```text
Commit: handover-package-approved
Actor: HandoverAgent + Team Lead
Approved by: Team Lead
Generated artifacts:
- handover-report.mdoc
- service-ownership-transfer.msheet
- handover-presentation.mslide

Source references:
- 8 service docs
- 4 runbooks
- 3 incidents
- 12 tasks

Policy:
- Human approval required
- Sensitive docs excluded
- Stale docs flagged

Rollback:
- rollback transaction ID available
```

Sau commit:

* graph được update;
* ownership metadata được update;
* slide source links được lưu;
* sheet formulas được lưu;
* audit trail được ghi nhận;
* dashboard được refresh.

---

## H14. Step 11 — Present via MarkdownSlide

Team mở MarkdownSlide bằng PowerPoint-like viewer để trình bày.

Trong presentation mode, slide có thể hiển thị:

* service map;
* risk matrix;
* transfer timeline;
* checklist;
* open issues;
* next actions.

Điểm khác biệt:

* slide có source link;
* chart lấy từ sheet;
* status lấy từ graph;
* nếu sheet đổi, slide được đánh dấu stale;
* nếu report update, deck có thể tạo proposal refresh;
* sau meeting, decision/action item có thể ghi ngược vào document graph.

Presentation không còn là endpoint tĩnh. Nó trở thành một phần của knowledge workflow.

---

## H15. Step 12 — Track completion after meeting

Sau buổi handover, MarkdownOffice tiếp tục track tiến độ:

* service nào đã chuyển giao;
* owner nào đã accept;
* runbook nào đã update;
* task nào còn mở;
* session nào đã hoàn thành;
* risk nào đã giảm;
* documentation gap nào còn tồn tại;
* slide deck nào đã stale;
* report section nào cần update.

Agent có thể tạo follow-up proposal:

```text
Follow-up proposal:
- Mark 3 services as transferred
- Update backup owner
- Close 5 checklist items
- Refresh risk chart
- Update final handover report
```

Đây là điểm biến MarkdownOffice từ “document generator” thành “workflow operating system”.

---

# H16. Data flow trong employee handover

Có thể trình bày data flow như sau:

```text
Source Documents
(runbooks, service docs, incidents, tasks)
        ↓
Permission-aware Agent Scan
        ↓
Document Graph Context
        ↓
MarkdownSheet
(service ownership + planning)
        ↓
MarkdownDoc
(handover report)
        ↓
MarkdownSlide
(handover presentation)
        ↓
Cortexpod Proposal
        ↓
Human Review
        ↓
Lob Commit
        ↓
Audit + Graph Update
```

Thông điệp chính:

> Agent không chỉ tạo một bản báo cáo. Agent tạo một chuỗi artifact liên kết với nhau: sheet để track, document để giải thích, slide để trình bày, proposal để review và Lob commit để audit.

---

# H17. Vì sao use case này thuyết phục founder/investor/engineering team

## Với founder

Use case này thể hiện rõ pain point doanh nghiệp:

* mất tri thức khi nhân sự rời đi;
* handover thủ công;
* thiếu ownership;
* thiếu audit;
* thiếu automation;
* rủi ro vận hành cao.

Nó cũng thể hiện rõ product value:

* giảm knowledge loss;
* tăng tốc chuyển giao;
* tạo report/slide/sheet tự động;
* kiểm soát bằng review và audit;
* phù hợp enterprise.

## Với investor

Use case này chứng minh MarkdownOffice không chỉ là “AI document editor”.

Nó là:

* enterprise workflow automation platform;
* system of record cho organizational knowledge;
* AI-native collaboration infrastructure;
* monetizable through permission, audit, compliance, workflow and agent governance.

## Với engineering team

Use case này giúp xác định architecture rõ ràng:

* AST;
* graph;
* permission;
* version transaction;
* proposal workflow;
* diff;
* rendering;
* agent orchestration;
* audit.

Nó giúp team không build lan man theo kiểu clone Office, mà tập trung vào workflow có giá trị.

---
