---
theme: seriph
background: /images/MarkdownOffice.jpg
title: "MarkdownOffice: LLM-first, markdown-native document agent cho enterprise monorepo"
info: |
  ## MarkdownOffice
  LLM-first, markdown-native document operating system cho enterprise monorepo.

  Deck trình bày:
  1. Vì sao tri thức doanh nghiệp bị phân mảnh và AI Agent khó vận hành.
  2. MarkdownOffice: hệ điều hành tài liệu markdown-native, LLM-first.
  3. Hệ sinh thái: MarkdownDoc / Sheet / Slide / Jira / Report / Workflow / Agent.
  4. Cortexpod + Lob: primitive kiểu Git cho document monorepo.
  5. Agent workflow integration theo Harness Engineering.
  6. Agent memory: working / episodic / semantic / procedural.
  7. Product strategy & platform vision.
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
mdc: true
---

# MarkdownOffice

## LLM-first, markdown-native document agent cho enterprise monorepo

<div class="mt-6 text-lg opacity-80 max-w-3xl mx-auto">
Biến tài liệu doanh nghiệp thành <b>structured · versioned · auditable · machine-readable</b> knowledge — để con người và AI Agent cùng tạo, review, version và tự động hóa.
</div>

<div class="mt-10 flex justify-center gap-3 text-sm">
  <span class="px-3 py-1 rounded-full bg-white/10 border border-white/20">Markdown-native</span>
  <span class="px-3 py-1 rounded-full bg-white/10 border border-white/20">LLM-first</span>
  <span class="px-3 py-1 rounded-full bg-white/10 border border-white/20">Git-like versioning</span>
  <span class="px-3 py-1 rounded-full bg-white/10 border border-white/20">Agent-collaborative</span>
</div>

<!--
MarkdownOffice không phải một Markdown editor mới. Đây là nỗ lực xây lại nền tảng tài liệu doanh nghiệp cho thời đại human + AI Agent.
-->

---
layout: default
---

# Deck này trả lời điều gì?

<div class="grid grid-cols-2 gap-4 mt-8 text-sm">

<div v-click class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">🧩</div>
<b>1 · Vấn đề</b><br>
<span class="opacity-70">Vì sao tri thức doanh nghiệp phân mảnh, con người mất context và AI Agent gần như không thể vận hành đáng tin cậy.</span>
</div>

<div v-click class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">🏛️</div>
<b>2 · Giải pháp</b><br>
<span class="opacity-70">MarkdownOffice: document operating system markdown-native, LLM-first cho enterprise monorepo.</span>
</div>

<div v-click class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">⚙️</div>
<b>3 · Kiến trúc</b><br>
<span class="opacity-70">Hệ sinh thái sản phẩm + Cortexpod & Lob làm primitive kiểu Git cho document.</span>
</div>

<div v-click class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">🤖</div>
<b>4 · Agent</b><br>
<span class="opacity-70">Harness Engineering + 4 lớp memory: cách nhúng Agent vào workflow tài liệu an toàn.</span>
</div>

</div>

<div v-click class="mt-6 text-center opacity-80">
Và cuối cùng: <b>product strategy</b>, enterprise wedge và <b>platform vision</b> dài hạn.
</div>

<!--
Định vị deck: không phải product demo nhỏ, mà là câu chuyện kiến trúc + tầm nhìn nền tảng.
-->

---
layout: section
---

# 1 · Nỗi đau tài liệu doanh nghiệp
Fragmented knowledge — con người mệt mỏi, AI Agent thất bại

---
layout: default
---

# Doanh nghiệp không thiếu tài liệu — thiếu hệ điều hành cho tài liệu

Cùng một sự thật vận hành bị chia nhỏ và sao chép qua hàng chục công cụ, không có <b>canonical source of truth</b>.

<div class="mt-4 flex justify-center">
<svg viewBox="0 0 920 300" class="w-full max-h-[300px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="ar1" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 Z" fill="#94a3b8"/></marker>
  </defs>
  <!-- scattered tools -->
  <g font-size="13" text-anchor="middle">
    <g>
      <rect x="20" y="20" width="130" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="85" y="42">Google Docs</text>
      <rect x="20" y="70" width="130" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="85" y="92">PowerPoint</text>
      <rect x="20" y="120" width="130" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="85" y="142">Excel / Sheets</text>
      <rect x="20" y="170" width="130" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="85" y="192">Slack / Teams</text>
      <rect x="20" y="220" width="130" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="85" y="242">Jira / Linear</text>
    </g>
    <g>
      <rect x="180" y="45" width="130" height="34" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="245" y="67">Confluence</text>
      <rect x="180" y="120" width="130" height="34" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="245" y="142">Email</text>
      <rect x="180" y="195" width="130" height="34" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="245" y="217">Code repo / README</text>
    </g>
    <g>
      <rect x="340" y="70" width="130" height="34" rx="8" fill="#fffbeb" stroke="#f59e0b"/><text x="405" y="92">File server</text>
      <rect x="340" y="170" width="130" height="34" rx="8" fill="#fffbeb" stroke="#f59e0b"/><text x="405" y="192">Local laptop</text>
    </g>
  </g>
  <!-- arrows to void -->
  <g stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ar1)" fill="none">
    <path d="M472,90 C560,110 560,120 610,135"/>
    <path d="M472,187 C560,170 560,160 610,150"/>
    <path d="M312,62 C520,70 540,120 610,135"/>
    <path d="M312,212 C520,205 540,160 610,150"/>
  </g>
  <!-- void -->
  <circle cx="720" cy="145" r="95" fill="#fef2f2" stroke="#ef4444" stroke-dasharray="6 5"/>
  <text x="720" y="130" text-anchor="middle" font-size="20" fill="#b91c1c" font-weight="700">Fragmented</text>
  <text x="720" y="158" text-anchor="middle" font-size="20" fill="#b91c1c" font-weight="700">Knowledge</text>
  <text x="720" y="185" text-anchor="middle" font-size="12" fill="#ef4444">No source of truth</text>
</svg>
</div>

<!--
Vấn đề không phải từng tool kém. Vấn đề là không có canonical knowledge layer để người, hệ thống và Agent cùng làm việc.
-->

---
layout: default
---

# Bối cảnh công cụ bị phân mảnh

Mỗi loại tri thức nằm ở một nơi khác nhau, với schema, permission, history và API khác nhau.

<div class="mt-4 text-sm">

| Loại thông tin | Nơi thường nằm | Vấn đề khi cần tổng hợp |
| --- | --- | --- |
| Report / Spec | Google Docs, Word, PDF | Nhiều bản, không rõ bản mới nhất |
| Slide | PowerPoint, Google Slides | Copy-paste, dễ stale |
| Sheet / Data | Excel, Google Sheets | Formula khó audit cross-doc |
| Task | Jira, Linear, Trello | Tách rời khỏi tài liệu mô tả |
| Chat context | Slack, Teams | Quyết định chìm trong hội thoại |
| Decision / Wiki | Notion, Confluence, email | Không có version thật, không owner |
| Service ownership | Wiki, sheet, tribal knowledge | Mất khi nhân sự nghỉ việc |
| Incident history | Jira, Slack, postmortem | Rời rạc, khó trace |

</div>

<div class="mt-4 p-3 rounded-lg border-l-4 border-amber-500 bg-amber-50 dark:bg-amber-900/20 text-sm">
⚠️ <b>Không có canonical layer</b> → không biết đâu là bản đúng, ai sửa gì, và Agent nên tin vào dữ liệu nào.
</div>

---
layout: two-cols
layoutClass: gap-8
---

# Vì sao con người mất context

<div class="mt-4 flex flex-col gap-3 text-sm">
<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600">🔍 <b>Săn lùng thông tin</b><br><span class="opacity-70">Phải mở 6–8 tool để ghép lại một bức tranh.</span></div>
<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600">🕰️ <b>Bản cũ vs bản mới</b><br><span class="opacity-70">`final_v2_revised_latest.docx` — không ai chắc.</span></div>
<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600">🧠 <b>Tribal knowledge</b><br><span class="opacity-70">Kiến thức nằm trong đầu người, không được ghi lại.</span></div>
<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600">🔗 <b>Mất liên kết</b><br><span class="opacity-70">Slide không biết lấy từ sheet nào, report nào.</span></div>
</div>

::right::

<div class="mt-16 p-5 rounded-xl bg-gradient-to-br from-slate-100 to-slate-200 dark:from-slate-800 dark:to-slate-700">

### Cái giá phải trả

- Onboarding chậm
- Handover rủi ro cao
- Quyết định dựa trên dữ liệu lỗi thời
- Không có audit trail khi cần

<div class="mt-4 text-center">
<span class="text-3xl font-bold text-rose-600">~20%+</span><br>
<span class="text-xs opacity-70">thời gian tri thức bị đốt vào việc đi tìm lại thông tin</span>
</div>

</div>

<!--
Context switching liên tục giữa các tool là thuế vô hình đánh vào mọi knowledge worker.
-->

---
layout: default
---

# Vì sao AI Agent thất bại với tài liệu phân mảnh

Office/Docs tối ưu cho <b>mắt người</b>, không cho <b>reasoning engine</b>. Agent chỉ "đọc text" chứ không "vận hành tài liệu".

<div class="grid grid-cols-2 gap-6 mt-6">

<div>

### Agent không xác định được
<div class="mt-2 flex flex-col gap-2 text-sm">
<div class="px-3 py-2 rounded bg-rose-50 dark:bg-rose-900/20 border border-rose-200 dark:border-rose-800">❓ Đâu là version mới nhất / đã duyệt</div>
<div class="px-3 py-2 rounded bg-rose-50 dark:bg-rose-900/20 border border-rose-200 dark:border-rose-800">❓ Ai owner, quyền truy cập tới đâu</div>
<div class="px-3 py-2 rounded bg-rose-50 dark:bg-rose-900/20 border border-rose-200 dark:border-rose-800">❓ Bảng nào là source of truth</div>
<div class="px-3 py-2 rounded bg-rose-50 dark:bg-rose-900/20 border border-rose-200 dark:border-rose-800">❓ Dữ liệu nào stale, đổi gì ảnh hưởng gì</div>
</div>

</div>

<div>

### Hệ quả (theo Harness Engineering)
<div class="mt-2 flex flex-col gap-2 text-sm">
<div class="px-3 py-2 rounded bg-amber-50 dark:bg-amber-900/20 border border-amber-200 dark:border-amber-800">🌀 <b>Thiếu context</b> → hallucination</div>
<div class="px-3 py-2 rounded bg-amber-50 dark:bg-amber-900/20 border border-amber-200 dark:border-amber-800">🚫 <b>Thiếu grounding</b> → bịa dữ kiện</div>
<div class="px-3 py-2 rounded bg-amber-50 dark:bg-amber-900/20 border border-amber-200 dark:border-amber-800">🔒 <b>Thiếu permission model</b> → rủi ro</div>
<div class="px-3 py-2 rounded bg-amber-50 dark:bg-amber-900/20 border border-amber-200 dark:border-amber-800">📝 <b>Thiếu audit/rollback</b> → không thể tin</div>
</div>

</div>

</div>

<!--
"Agent reliability đến từ system design, không từ một prompt hay". Tài liệu phân mảnh chính là system design tồi cho Agent.
-->

---
layout: default
---

# Trạng thái hiện tại: document chaos

<div class="flex justify-center mt-6">
<svg viewBox="0 0 920 360" class="w-full max-h-[360px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arChaos" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 Z" fill="#cbd5e1"/></marker>
  </defs>
  <!-- messy web of duplicated docs -->
  <g font-size="12" text-anchor="middle">
    <rect x="60" y="40" width="120" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="120" y="60">report_v1.docx</text>
    <rect x="330" y="30" width="130" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="395" y="50">report_final.pdf</text>
    <rect x="640" y="55" width="150" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="715" y="75">report_FINAL_v2.docx</text>
    <rect x="120" y="150" width="120" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="180" y="170">deck.pptx</text>
    <rect x="420" y="170" width="140" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="490" y="190">deck-copy.pptx</text>
    <rect x="700" y="185" width="140" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="770" y="205">slack thread</text>
    <rect x="70" y="270" width="140" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="140" y="290">tracker.xlsx</text>
    <rect x="360" y="290" width="150" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="435" y="310">email attachment</text>
    <rect x="620" y="290" width="150" height="30" rx="6" fill="#f1f5f9" stroke="#94a3b8"/><text x="695" y="310">jira comment</text>
  </g>
  <g stroke="#cbd5e1" stroke-width="1.3" fill="none" marker-end="url(#arChaos)">
    <path d="M180,55 C260,55 280,45 330,45"/>
    <path d="M460,45 C560,45 580,60 640,70"/>
    <path d="M180,70 C200,110 190,130 190,150"/>
    <path d="M240,165 C330,165 350,180 420,185"/>
    <path d="M560,185 C630,190 650,195 700,200"/>
    <path d="M140,180 C140,220 140,240 140,270"/>
    <path d="M210,285 C290,290 300,300 360,305"/>
    <path d="M510,305 C560,305 580,300 620,300"/>
    <path d="M770,215 C780,250 740,275 695,290"/>
  </g>
</svg>
</div>

<div class="text-center text-sm opacity-70 -mt-2">Sao chép chồng chéo · liên kết mờ · không version · không owner · không audit</div>

---
layout: default
---

# Trạng thái mong muốn: enterprise document monorepo

Một cây tri thức duy nhất — có cấu trúc, có version, có graph, có audit — để người và Agent cùng vận hành.

<div class="flex justify-center mt-4">
<svg viewBox="0 0 900 340" class="w-full max-h-[330px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arMono" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 Z" fill="#6366f1"/></marker>
  </defs>
  <rect x="330" y="20" width="240" height="46" rx="10" fill="#eef2ff" stroke="#6366f1" stroke-width="2"/>
  <text x="450" y="42" text-anchor="middle" font-size="15" font-weight="700" fill="#3730a3">company-knowledge</text>
  <text x="450" y="59" text-anchor="middle" font-size="11" fill="#6366f1">document monorepo · single source of truth</text>

  <g font-size="12.5" text-anchor="middle">
    <rect x="60"  y="140" width="150" height="40" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="135" y="158">/services</text><text x="135" y="173" font-size="10" fill="#0d9488">*.mdoc</text>
    <rect x="235" y="140" width="150" height="40" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="310" y="158">/runbooks</text><text x="310" y="173" font-size="10" fill="#0d9488">*.mdoc</text>
    <rect x="410" y="140" width="150" height="40" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="485" y="158">/sheets</text><text x="485" y="173" font-size="10" fill="#0d9488">*.msheet</text>
    <rect x="585" y="140" width="150" height="40" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="660" y="158">/slides</text><text x="660" y="173" font-size="10" fill="#0d9488">*.mslide</text>
    <rect x="760" y="140" width="120" height="40" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="820" y="158">/decisions</text><text x="820" y="173" font-size="10" fill="#0d9488">ADR</text>
  </g>

  <g stroke="#6366f1" stroke-width="1.4" fill="none" marker-end="url(#arMono)">
    <path d="M450,66 C450,90 135,100 135,138"/>
    <path d="M450,66 C450,95 310,105 310,138"/>
    <path d="M450,66 L485,138"/>
    <path d="M450,66 C450,95 660,105 660,138"/>
    <path d="M450,66 C450,90 820,100 820,138"/>
  </g>

  <g font-size="12" text-anchor="middle">
    <rect x="150" y="250" width="160" height="40" rx="20" fill="#fef3c7" stroke="#f59e0b"/><text x="230" y="275">🔐 Permission + Owner</text>
    <rect x="370" y="250" width="160" height="40" rx="20" fill="#e0e7ff" stroke="#6366f1"/><text x="450" y="275">🕸️ Knowledge Graph</text>
    <rect x="590" y="250" width="160" height="40" rx="20" fill="#dcfce7" stroke="#22c55e"/><text x="670" y="275">🗂️ Version + Audit</text>
  </g>
</svg>
</div>

<!--
Từ chaos sang monorepo: mọi artifact có cấu trúc, có graph, có version và audit.
-->

---
layout: section
---

# 2 · Giới thiệu MarkdownOffice
Document Operating System — markdown-native, LLM-first

---
layout: two-cols
layoutClass: gap-6
---

# MarkdownOffice là gì?

<div class="mt-4 text-sm">

Không phải Markdown editor. Đây là <b>hệ điều hành tài liệu</b> cho doanh nghiệp:

- Tài liệu là <b>structured data</b>, không phải file cô lập
- <b>Markdown-native</b> làm source of truth, có thể diff/parse/validate
- <b>LLM-first</b>: Agent là first-class actor
- Có permission · version · graph · audit từ lõi

</div>

<div class="mt-4 p-3 rounded-lg border-l-4 border-indigo-500 bg-indigo-50 dark:bg-indigo-900/20 text-sm italic">
"Microsoft Office reimagined for AI Agents, running on a GitHub-like hub, powered by a Git-like version layer for structured documents."
</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/images/MarkdownOffice.jpg" class="max-h-[420px] w-auto rounded-xl shadow-2xl" alt="MarkdownOffice" />
</div>

<!--
Người dùng vẫn thấy Word/PowerPoint/Excel. Bên dưới là Markdown-native, machine-readable, agent-accessible.
-->

---
layout: default
---

# Core thesis: markdown-native + LLM-first

<div class="grid grid-cols-2 gap-6 mt-4">

<div>

### Vì sao Markdown-native?
<div class="text-sm mt-2 flex flex-col gap-1.5">
<div class="px-3 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20">✅ Human-readable & machine-readable</div>
<div class="px-3 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20">✅ Diff-friendly → version control tự nhiên</div>
<div class="px-3 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20">✅ Portable, không lock vendor</div>
<div class="px-3 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20">✅ LLM đã "quen" cấu trúc Markdown</div>
<div class="px-3 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20">✅ Extensible: frontmatter, block ID, schema</div>
</div>

</div>

<div>

### Kiến trúc phân lớp
<svg viewBox="0 0 380 250" class="w-full max-h-[240px]" font-family="ui-sans-serif, system-ui">
  <g font-size="12" text-anchor="middle">
    <rect x="40" y="6"   width="300" height="30" rx="6" fill="#e0e7ff" stroke="#6366f1"/><text x="190" y="26">Doc · Slide · Sheet (surfaces)</text>
    <rect x="40" y="42"  width="300" height="30" rx="6" fill="#ede9fe" stroke="#8b5cf6"/><text x="190" y="62">Rendering Layer</text>
    <rect x="40" y="78"  width="300" height="30" rx="6" fill="#f0fdfa" stroke="#14b8a6"/><text x="190" y="98">AST + Domain Model</text>
    <rect x="40" y="114" width="300" height="30" rx="6" fill="#f0fdfa" stroke="#14b8a6"/><text x="190" y="134">Document Graph</text>
    <rect x="40" y="150" width="300" height="30" rx="6" fill="#dcfce7" stroke="#22c55e"/><text x="190" y="170">Lob · Version Management</text>
    <rect x="40" y="186" width="300" height="30" rx="6" fill="#dbeafe" stroke="#3b82f6"/><text x="190" y="206">Cortexpod · Repository Hub</text>
    <rect x="40" y="222" width="300" height="24" rx="6" fill="#fef3c7" stroke="#f59e0b"/><text x="190" y="238" font-size="11">Permission · Audit · Compliance</text>
  </g>
</svg>

</div>

</div>

<!--
Một knowledge engine, nhiều Office view. Người dùng không bị bắt nhìn Markdown thô.
-->

---
layout: default
---

# MarkdownOffice ecosystem map

<div class="flex justify-center mt-2">
<svg viewBox="0 0 900 330" class="w-full max-h-[300px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arEco" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c7d2fe"/></marker>
  </defs>
  <circle cx="450" cy="165" r="66" fill="#4f46e5"/>
  <text x="450" y="160" text-anchor="middle" font-size="16" font-weight="700" fill="#fff">Markdown</text>
  <text x="450" y="180" text-anchor="middle" font-size="16" font-weight="700" fill="#fff">Office</text>

  <g stroke="#c7d2fe" stroke-width="2" marker-end="url(#arEco)">
    <line x1="450" y1="100" x2="450" y2="60"/>
    <line x1="512" y1="130" x2="600" y2="80"/>
    <line x1="516" y1="165" x2="620" y2="165"/>
    <line x1="512" y1="200" x2="600" y2="250"/>
    <line x1="388" y1="200" x2="300" y2="250"/>
    <line x1="384" y1="165" x2="280" y2="165"/>
    <line x1="388" y1="130" x2="300" y2="80"/>
  </g>

  <g font-size="12.5" text-anchor="middle">
    <rect x="375" y="16" width="150" height="42" rx="10" fill="#eef2ff" stroke="#6366f1"/><text x="450" y="33" font-weight="700">MarkdownDoc</text><text x="450" y="49" font-size="10" fill="#6366f1">Word · report/spec</text>
    <rect x="595" y="40" width="150" height="42" rx="10" fill="#eef2ff" stroke="#6366f1"/><text x="670" y="57" font-weight="700">MarkdownSheet</text><text x="670" y="73" font-size="10" fill="#6366f1">Excel · tables</text>
    <rect x="620" y="144" width="150" height="42" rx="10" fill="#eef2ff" stroke="#6366f1"/><text x="695" y="161" font-weight="700">MarkdownSlide</text><text x="695" y="177" font-size="10" fill="#6366f1">PowerPoint · deck</text>
    <rect x="595" y="248" width="150" height="42" rx="10" fill="#eef2ff" stroke="#6366f1"/><text x="670" y="265" font-weight="700">MarkdownJira</text><text x="670" y="281" font-size="10" fill="#6366f1">task · workflow</text>
    <rect x="230" y="248" width="150" height="42" rx="10" fill="#eef2ff" stroke="#6366f1"/><text x="305" y="265" font-weight="700">MarkdownReport</text><text x="305" y="281" font-size="10" fill="#6366f1">exec · dashboard</text>
    <rect x="130" y="144" width="150" height="42" rx="10" fill="#eef2ff" stroke="#6366f1"/><text x="205" y="161" font-weight="700">MarkdownWorkflow</text><text x="205" y="177" font-size="10" fill="#6366f1">automation</text>
    <rect x="155" y="40" width="150" height="42" rx="10" fill="#eef2ff" stroke="#6366f1"/><text x="230" y="57" font-weight="700">MarkdownAgent</text><text x="230" y="73" font-size="10" fill="#6366f1">AI actor</text>
  </g>
</svg>
</div>

<div class="grid grid-cols-2 gap-3 text-xs mt-1">
<div class="px-3 py-1.5 rounded bg-blue-50 dark:bg-blue-900/20">🏛️ <b>Cortexpod</b> — GitHub-like hub cho document + Agent</div>
<div class="px-3 py-1.5 rounded bg-green-50 dark:bg-green-900/20">🔀 <b>Lob</b> — Git-like version layer cho structured docs</div>
</div>

<!--
Bảy surface chia sẻ chung một engine: content · AST · graph · permission · Lob · Cortexpod · agent layer.
-->

---
layout: two-cols
layoutClass: gap-6
---

# MarkdownDoc

## Word-like · Markdown-native

<div class="text-sm mt-2">
Report · spec · policy · runbook · handover · knowledge base — với source `.mdoc` có block ID.
</div>

```md
---
title: Service Handover Report
owner: platform-team
status: active
---
# Executive Summary
## Known Risks   {#risk}
- SLA: 99.9% · dep: PaymentService
```

<div class="mt-3 text-xs flex flex-col gap-1.5">
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">👀 <b>View:</b> Word-like, paginated, PDF export</div>
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">🌳 <b>Source:</b> Markdown + AST, diffable, agent-readable</div>
</div>

::right::

<div class="flex flex-col items-center justify-center h-full gap-3">
<img src="/images/MarkdownDoc.jpg" class="max-h-[230px] w-auto rounded-lg shadow-xl" alt="MarkdownDoc" />
<img src="/images/Doc%20Poster.png" class="max-h-[160px] w-auto rounded-lg shadow-lg" alt="Doc Poster" />
</div>

<!--
User có trải nghiệm Word. Agent có structured source để đọc, sửa, validate và propose.
-->

---
layout: two-cols
layoutClass: gap-6
---

# MarkdownSheet

## Excel-like · CSV-first

<div class="text-sm mt-2">
Không clone toàn bộ Excel ngay. Bắt đầu từ <b>workflow data</b>: status tracking, ownership matrix, risk scoring.
</div>

<div class="mt-2 text-xs">

| Cột | Vai trò |
| --- | --- |
| Service / Owner | ai nắm cái gì |
| Documentation % | mức tài liệu hóa |
| Risk Score | crit × complex × gap |
| Completion % | tiến độ handover |

</div>

<div class="mt-2 p-2 rounded bg-teal-50 dark:bg-teal-900/20 text-xs">
🔗 Formula <b>dependency tracking</b>: đổi 1 ô → biết sheet/chart/slide/report nào cần refresh.
</div>

::right::

<div class="flex flex-col items-center justify-center h-full gap-3">
<img src="/images/MarkdownSheet.png" class="max-h-[230px] w-auto rounded-lg shadow-xl" alt="MarkdownSheet" />
<img src="/images/Sheet%20Poster.png" class="max-h-[160px] w-auto rounded-lg shadow-lg" alt="Sheet Poster" />
</div>

<!--
MarkdownSheet là structured table engine, không chỉ spreadsheet.
-->

---
layout: two-cols
layoutClass: gap-6
---

# MarkdownSlide

## PowerPoint-like · sinh từ knowledge graph

<div class="text-sm mt-2">
Deck được <b>generate từ report + sheet + document graph</b> — không còn copy-paste tay.
</div>

<div class="mt-3 text-xs flex flex-col gap-1.5">
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">📊 Chart lấy trực tiếp từ MarkdownSheet</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">🔗 Mỗi slide có <b>source reference</b></div>
<div class="px-2 py-1 rounded bg-amber-50 dark:bg-amber-900/20">🕰️ <b>Stale detection</b> khi source đổi</div>
<div class="px-2 py-1 rounded bg-emerald-50 dark:bg-emerald-900/20">🔄 Refresh theo policy · giữ snapshot audit</div>
</div>

<div class="mt-3 text-xs opacity-70">Deck này chính là một MarkdownSlide (chạy trên Slidev).</div>

::right::

<div class="flex flex-col items-center justify-center h-full gap-3">
<img src="/images/MarkdownSlide.png" class="max-h-[230px] w-auto rounded-lg shadow-xl" alt="MarkdownSlide" />
<img src="/images/Slide%20Poster.png" class="max-h-[160px] w-auto rounded-lg shadow-lg" alt="Slide Poster" />
</div>

<!--
Slide không phải endpoint tĩnh, mà là view có version của knowledge pipeline.
-->

---
layout: default
---

# MarkdownJira — task & workflow gắn với tài liệu

Task không tách rời tài liệu mô tả nó. Issue là một node trong cùng document graph.

<div class="grid grid-cols-3 gap-4 mt-6 text-sm">

<div class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">📋</div>
<b>Issue = document-linked</b><br>
<span class="opacity-70">Mỗi task liên kết tới service doc, runbook, incident, owner.</span>
</div>

<div class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">🔗</div>
<b>Traceable</b><br>
<span class="opacity-70">Từ task → decision record → report → slide, tất cả trong monorepo.</span>
</div>

<div class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">🤖</div>
<b>Agent-actionable</b><br>
<span class="opacity-70">Agent đọc task, cập nhật status sheet, mở proposal — có audit.</span>
</div>

</div>

<div class="mt-6 p-3 rounded-lg border-l-4 border-indigo-500 bg-indigo-50 dark:bg-indigo-900/20 text-sm">
So với Jira truyền thống: task, mô tả, tài liệu và tiến độ nằm <b>cùng một knowledge layer có version</b>, không phải các silo tách rời.
</div>

---
layout: default
---

# MarkdownWorkflow & MarkdownReport

<div class="grid grid-cols-2 gap-6 mt-4">

<div>

### ⚙️ MarkdownWorkflow
<div class="text-sm mt-2 opacity-80">Automation layer cho các quy trình lặp lại:</div>
<div class="mt-2 text-xs flex flex-col gap-1.5">
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">Handover · Onboarding</div>
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">Incident review · Postmortem</div>
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">Policy approval · Compliance check</div>
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">Stale detection · Ownership refresh</div>
</div>

</div>

<div>

### 📈 MarkdownReport
<div class="text-sm mt-2 opacity-80">Executive & dashboard surface:</div>
<div class="mt-2 text-xs flex flex-col gap-1.5">
<div class="px-2 py-1 rounded bg-sky-50 dark:bg-sky-900/20">Document health · stale docs</div>
<div class="px-2 py-1 rounded bg-sky-50 dark:bg-sky-900/20">Missing owners · pending approvals</div>
<div class="px-2 py-1 rounded bg-sky-50 dark:bg-sky-900/20">High-risk services · open handovers</div>
<div class="px-2 py-1 rounded bg-sky-50 dark:bg-sky-900/20">Agent activity summary</div>
</div>

</div>

</div>

<div class="mt-5 p-3 rounded bg-slate-100 dark:bg-slate-800 text-xs font-mono">
Team Knowledge Health · 12 services · 3 thiếu backup owner · 5 runbook outdated · 4 agent proposals pending
</div>

---
layout: default
---

# MarkdownAgent — AI Agent là first-class actor

Agent không phải chatbot bên ngoài tài liệu. Nó làm việc theo <b>transaction</b>, không chỉ "generate text".

<div class="flex justify-center mt-4">
<svg viewBox="0 0 900 190" class="w-full max-h-[180px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arAg" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 Z" fill="#6366f1"/></marker>
  </defs>
  <g font-size="12" text-anchor="middle">
    <rect x="10"  y="70" width="118" height="48" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="69" y="90">Read context</text><text x="69" y="106" font-size="10" fill="#6366f1">(graph, perm)</text>
    <rect x="150" y="70" width="118" height="48" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="209" y="90">Generate</text><text x="209" y="106" font-size="10" fill="#6366f1">doc/sheet/slide</text>
    <rect x="290" y="70" width="118" height="48" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="349" y="90">Validate</text><text x="349" y="106" font-size="10" fill="#6366f1">schema/policy</text>
    <rect x="430" y="70" width="118" height="48" rx="8" fill="#fef3c7" stroke="#f59e0b"/><text x="489" y="90">Open</text><text x="489" y="106" font-size="10" fill="#b45309">proposal</text>
    <rect x="570" y="70" width="118" height="48" rx="8" fill="#fef3c7" stroke="#f59e0b"/><text x="629" y="90">Human</text><text x="629" y="106" font-size="10" fill="#b45309">review</text>
    <rect x="710" y="70" width="118" height="48" rx="8" fill="#dcfce7" stroke="#22c55e"/><text x="769" y="90">Lob commit</text><text x="769" y="106" font-size="10" fill="#15803d">+ audit</text>
  </g>
  <g stroke="#6366f1" stroke-width="1.6" marker-end="url(#arAg)">
    <line x1="128" y1="94" x2="148" y2="94"/>
    <line x1="268" y1="94" x2="288" y2="94"/>
    <line x1="408" y1="94" x2="428" y2="94"/>
    <line x1="548" y1="94" x2="568" y2="94"/>
    <line x1="688" y1="94" x2="708" y2="94"/>
  </g>
</svg>
</div>

<div class="grid grid-cols-4 gap-2 text-xs mt-3">
<div class="px-2 py-1.5 rounded bg-gray-100 dark:bg-gray-800 text-center">Read · Retrieve</div>
<div class="px-2 py-1.5 rounded bg-gray-100 dark:bg-gray-800 text-center">Propose · Edit (scoped)</div>
<div class="px-2 py-1.5 rounded bg-gray-100 dark:bg-gray-800 text-center">Validate</div>
<div class="px-2 py-1.5 rounded bg-gray-100 dark:bg-gray-800 text-center">Commit (nếu policy cho phép)</div>
</div>

<!--
Agents propose, humans approve, Lob commits. Đây là cách đưa AI vào enterprise an toàn.
-->

---
layout: default
---

# Cortexpod — GitHub cho enterprise documents & Agents

Control plane cho toàn bộ lifecycle của enterprise knowledge — không chỉ lưu file.

<div class="flex justify-center mt-4">
<svg viewBox="0 0 900 260" class="w-full max-h-[250px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arCx" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#93c5fd"/></marker>
  </defs>
  <circle cx="450" cy="130" r="58" fill="#2563eb"/>
  <text x="450" y="126" text-anchor="middle" font-size="16" font-weight="700" fill="#fff">Cortexpod</text>
  <text x="450" y="146" text-anchor="middle" font-size="10" fill="#dbeafe">control plane</text>
  <g stroke="#93c5fd" stroke-width="2" marker-end="url(#arCx)">
    <line x1="450" y1="72" x2="450" y2="45"/>
    <line x1="508" y1="105" x2="600" y2="60"/>
    <line x1="508" y1="130" x2="620" y2="130"/>
    <line x1="508" y1="155" x2="600" y2="200"/>
    <line x1="392" y1="155" x2="300" y2="200"/>
    <line x1="392" y1="130" x2="280" y2="130"/>
    <line x1="392" y1="105" x2="300" y2="60"/>
  </g>
  <g font-size="12" text-anchor="middle">
    <rect x="385" y="16" width="130" height="34" rx="8" fill="#dbeafe" stroke="#3b82f6"/><text x="450" y="37">Repos</text>
    <rect x="595" y="38" width="130" height="34" rx="8" fill="#dbeafe" stroke="#3b82f6"/><text x="660" y="59">Proposals</text>
    <rect x="615" y="113" width="130" height="34" rx="8" fill="#dbeafe" stroke="#3b82f6"/><text x="680" y="134">Reviews</text>
    <rect x="595" y="188" width="130" height="34" rx="8" fill="#dbeafe" stroke="#3b82f6"/><text x="660" y="209">Agent workspace</text>
    <rect x="235" y="188" width="130" height="34" rx="8" fill="#dbeafe" stroke="#3b82f6"/><text x="300" y="209">Knowledge graph</text>
    <rect x="155" y="113" width="130" height="34" rx="8" fill="#dbeafe" stroke="#3b82f6"/><text x="220" y="134">Audit</text>
    <rect x="175" y="38" width="130" height="34" rx="8" fill="#dbeafe" stroke="#3b82f6"/><text x="240" y="59">Permission</text>
  </g>
</svg>
</div>

<!--
Cortexpod hiểu document type, owner, reviewer, status, dependency, stale state, agent activity, Lob history.
-->

---
layout: default
---

# Lob — Git-like version management cho tài liệu

Git line-diff không đủ. Lob cần <b>block / AST / semantic diff</b> và <b>policy-based merge</b>.

<div class="grid grid-cols-2 gap-6 mt-4">

<div>

### Commit graph & branch
<svg viewBox="0 0 400 200" class="w-full max-h-[190px]" font-family="ui-sans-serif, system-ui">
  <line x1="30" y1="40" x2="370" y2="40" stroke="#cbd5e1" stroke-width="3"/>
  <line x1="120" y1="40" x2="200" y2="110" stroke="#a78bfa" stroke-width="3"/>
  <line x1="200" y1="110" x2="320" y2="110" stroke="#a78bfa" stroke-width="3"/>
  <line x1="320" y1="110" x2="360" y2="40" stroke="#22c55e" stroke-width="3" stroke-dasharray="5 4"/>
  <g>
    <circle cx="60" cy="40" r="10" fill="#64748b"/><circle cx="120" cy="40" r="10" fill="#64748b"/><circle cx="250" cy="40" r="10" fill="#64748b"/><circle cx="360" cy="40" r="11" fill="#2563eb"/>
    <circle cx="200" cy="110" r="10" fill="#8b5cf6"/><circle cx="320" cy="110" r="10" fill="#8b5cf6"/>
  </g>
  <text x="60" y="24" font-size="10" text-anchor="middle" fill="#64748b">main</text>
  <text x="200" y="135" font-size="10" text-anchor="middle" fill="#8b5cf6">handover/v1</text>
  <text x="360" y="24" font-size="10" text-anchor="middle" fill="#2563eb">merge</text>
</svg>

</div>

<div>

### Diff vượt khỏi line-diff
<div class="text-xs mt-1">

| Artifact | Diff cần có |
| --- | --- |
| MarkdownDoc | section · block · semantic |
| MarkdownSlide | slide · layout · source |
| MarkdownSheet | row · cell · formula · schema |
| Agent | reasoning · provenance |

</div>
<div class="mt-2 px-2 py-1 rounded bg-emerald-50 dark:bg-emerald-900/20 text-xs">
Transaction ghi: actor · intent · source · validation · approval · rollback pointer.
</div>

</div>

</div>

<!--
Lob là "Git for documents + Agent workflow", không phải wrapper của Git.
-->

---
layout: default
---

# Git-like document operations — mapping

<div class="mt-4 text-sm">

| Thế giới hiện tại | MarkdownOffice ecosystem |
| --- | --- |
| Microsoft Office / Google Workspace | **MarkdownOffice** |
| Word · PowerPoint · Excel | MarkdownDoc · Slide · Sheet |
| GitHub | **Cortexpod** |
| Git | **Lob** |
| Pull Request | Agent Proposal / document change request |
| Commit | Auditable document transaction |
| Branch | Parallel document/workflow version |
| Merge | Policy-based reconciliation |
| Code review | Document / slide / sheet review |
| Wiki | Living operational knowledge graph |

</div>

<!--
Chúng ta không build editor. Chúng ta build stack mới: Office-like UI + GitHub-like hub + Git-like version layer.
-->

---
layout: default
---

# Monorepo workspace model

Repository trong Cortexpod không chỉ chứa file — nó hiểu type, owner, dependency, permission.

<div class="grid grid-cols-2 gap-6 mt-3">

<div class="text-xs">

```text
/company-knowledge
  /services
    payment-service.mdoc
    notification-service.mdoc
  /runbooks
    deployment-runbook.mdoc
    incident-response.mdoc
  /sheets
    service-ownership.msheet
    transfer-status.msheet
  /slides
    q3-executive.mslide
    handover.mslide
  /decisions
    adr-001-database.mdoc
  /policies
    security-policy.mdoc
```

</div>

<div class="text-sm">

### Cortexpod hiểu thêm gì?
<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">📌 document type · owner · reviewer · team</div>
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">✅ status · approval state</div>
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">🔗 dependency · generated artifact</div>
<div class="px-2 py-1 rounded bg-amber-50 dark:bg-amber-900/20">🕰️ stale state · permission boundary</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">🤖 agent activity · Lob commit history</div>
</div>

<div class="mt-3 p-2 rounded border-l-4 border-emerald-500 bg-emerald-50 dark:bg-emerald-900/20 text-xs">
Một source of truth · dễ search bằng graph · Agent có workspace chuẩn · version nhất quán · audit sâu.
</div>

</div>

</div>

---
layout: section
---

# 3 · Demo workflow
Create → structure → link → version → agent → export

---
layout: default
---

# Demo workflow overview — Employee Handover

Use case ROI rõ ràng: giảm knowledge loss, tăng tốc chuyển giao, tạo audit trail.

<div class="flex justify-center mt-3">
<svg viewBox="0 0 900 250" class="w-full max-h-[240px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arDemo" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 Z" fill="#6366f1"/></marker>
  </defs>
  <g font-size="11.5" text-anchor="middle">
    <rect x="20"  y="20" width="150" height="42" rx="8" fill="#f1f5f9" stroke="#94a3b8"/><text x="95" y="38">Source docs</text><text x="95" y="53" font-size="9.5" fill="#64748b">runbook·incident·task</text>
    <rect x="210" y="20" width="150" height="42" rx="8" fill="#e0e7ff" stroke="#6366f1"/><text x="285" y="38">Permission-aware</text><text x="285" y="53" font-size="9.5" fill="#6366f1">agent scan</text>
    <rect x="400" y="20" width="150" height="42" rx="8" fill="#e0e7ff" stroke="#6366f1"/><text x="475" y="38">Document graph</text><text x="475" y="53" font-size="9.5" fill="#6366f1">context</text>
    <rect x="590" y="20" width="150" height="42" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="665" y="38">MarkdownSheet</text><text x="665" y="53" font-size="9.5" fill="#0d9488">transfer matrix</text>

    <rect x="590" y="105" width="150" height="42" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="665" y="123">MarkdownDoc</text><text x="665" y="138" font-size="9.5" fill="#0d9488">handover report</text>
    <rect x="400" y="105" width="150" height="42" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="475" y="123">MarkdownSlide</text><text x="475" y="138" font-size="9.5" fill="#0d9488">presentation</text>
    <rect x="210" y="105" width="150" height="42" rx="8" fill="#fef3c7" stroke="#f59e0b"/><text x="285" y="123">Cortexpod</text><text x="285" y="138" font-size="9.5" fill="#b45309">proposal</text>

    <rect x="210" y="190" width="150" height="42" rx="8" fill="#fef3c7" stroke="#f59e0b"/><text x="285" y="208">Human review</text><text x="285" y="223" font-size="9.5" fill="#b45309">&amp; approval</text>
    <rect x="400" y="190" width="150" height="42" rx="8" fill="#dcfce7" stroke="#22c55e"/><text x="475" y="208">Lob commit</text>
    <rect x="590" y="190" width="150" height="42" rx="8" fill="#dcfce7" stroke="#22c55e"/><text x="665" y="208">Audit + graph</text><text x="665" y="223" font-size="9.5" fill="#15803d">update · track</text>
  </g>
  <g stroke="#6366f1" stroke-width="1.5" fill="none" marker-end="url(#arDemo)">
    <line x1="170" y1="41" x2="208" y2="41"/>
    <line x1="360" y1="41" x2="398" y2="41"/>
    <line x1="550" y1="41" x2="588" y2="41"/>
    <path d="M665,62 L665,103"/>
    <line x1="588" y1="126" x2="552" y2="126"/>
    <line x1="398" y1="126" x2="362" y2="126"/>
    <path d="M285,147 L285,188"/>
    <line x1="360" y1="211" x2="398" y2="211"/>
    <line x1="550" y1="211" x2="588" y2="211"/>
  </g>
</svg>
</div>

<!--
Agent tạo cả một handover PACKAGE liên kết với nhau, không phải một file rời rạc.
-->

---
layout: two-cols
layoutClass: gap-6
---

# Demo · Bước 1 — Authoring document

<div class="text-sm mt-2">
Tạo document → cấu trúc bằng Markdown + frontmatter + block ID. Agent scan permission-aware.
</div>

```md
---
title: Employee Handover Report
owner: platform-team
status: draft
generated_by: HandoverAgent
---

# Executive Summary
# Scope of Ownership
# Known Risks           {#risk}
# Open Tasks            {#tasks}
# Recommended Next Owner {#next}
# Appendix: Sources     {#src}
```

::right::

<div class="mt-14 text-sm">

### Knowledge Scan Summary
<div class="mt-2 p-3 rounded-lg bg-slate-100 dark:bg-slate-800 text-xs font-mono">
8 service documents found<br>
4 runbooks found<br>
12 open tasks found<br>
3 incidents found<br>
2 outdated docs detected<br>
1 missing backup owner<br>
<span class="text-rose-500">5 docs excluded by permission</span>
</div>

<div class="mt-3 px-2 py-1.5 rounded bg-amber-50 dark:bg-amber-900/20 text-xs">
⚖️ Agent không tìm kiếm mù — scan theo <b>permission · scope · policy</b>.
</div>

</div>

---
layout: default
---

# Demo · Bước 2 — Linking context (document graph)

Mỗi tài liệu là một node. Liên kết cho phép impact analysis, stale detection, provenance.

<div class="flex justify-center mt-4">
<svg viewBox="0 0 880 250" class="w-full max-h-[240px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arGraph" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#94a3b8"/></marker>
  </defs>
  <g stroke="#94a3b8" stroke-width="1.5" marker-end="url(#arGraph)">
    <line x1="220" y1="125" x2="60" y2="45"/>
    <line x1="220" y1="125" x2="60" y2="125"/>
    <line x1="220" y1="125" x2="60" y2="205"/>
    <line x1="290" y1="110" x2="440" y2="45"/>
    <line x1="290" y1="125" x2="440" y2="125"/>
    <line x1="290" y1="140" x2="440" y2="205"/>
    <line x1="510" y1="45" x2="660" y2="45"/>
    <line x1="510" y1="205" x2="660" y2="205"/>
  </g>
  <g font-size="11" text-anchor="middle">
    <rect x="205" y="105" width="90" height="42" rx="10" fill="#4f46e5"/><text x="250" y="122" fill="#fff" font-weight="700">Service</text><text x="250" y="137" fill="#c7d2fe" font-size="9">ownership.mdoc</text>

    <rect x="0"   y="24" width="120" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="60" y="45">Architecture</text>
    <rect x="0"   y="108" width="120" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="60" y="129">Runbook</text>
    <rect x="0"   y="188" width="120" height="34" rx="8" fill="#eef2ff" stroke="#6366f1"/><text x="60" y="209">Incident history</text>

    <rect x="440" y="24" width="120" height="34" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="500" y="45">SLA doc</text>
    <rect x="440" y="108" width="120" height="34" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="500" y="129">Ownership sheet</text>
    <rect x="440" y="188" width="120" height="34" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="500" y="209">Monitoring</text>

    <rect x="660" y="24" width="130" height="34" rx="8" fill="#fef3c7" stroke="#f59e0b"/><text x="725" y="45">Handover slide</text>
    <rect x="660" y="188" width="130" height="34" rx="8" fill="#fef3c7" stroke="#f59e0b"/><text x="725" y="209">Dashboard</text>
  </g>
</svg>
</div>

<div class="text-center text-sm opacity-70 mt-1">"Nếu owner nghỉ việc → tài liệu nào ảnh hưởng? Slide lấy data từ sheet nào? Sheet đã update sau incident chưa?"</div>

---
layout: default
---

# Demo · Bước 3 — Versioning & comparison

Lob so sánh <b>semantic</b>, không chỉ text. Reviewer thấy đúng ý nghĩa thay đổi.

<div class="grid grid-cols-2 gap-6 mt-4 text-xs">

<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="font-bold mb-2">📄 MarkdownDoc — section "Known Risks"</div>
<div class="font-mono bg-slate-100 dark:bg-slate-800 p-2 rounded">
<span class="text-emerald-600">+ Added 2 new risks</span><br>
<span class="text-rose-500">- Removed outdated deploy warning</span><br>
+ Linked to incident INC-2026-07<br>
<span class="text-amber-600">≈ Risk level: Medium → High</span>
</div>
</div>

<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="font-bold mb-2">📊 MarkdownSheet — rows</div>
<div class="font-mono bg-slate-100 dark:bg-slate-800 p-2 rounded">
≈ payment-service transfer owner updated<br>
<span class="text-emerald-600">≈ documentation: 60% → 85%</span><br>
<span class="text-amber-600">≈ risk score: 8.4 → 5.1 (recalc)</span>
</div>
</div>

<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600 col-span-2">
<div class="font-bold mb-2">📽️ MarkdownSlide — slide 7</div>
<div class="font-mono bg-slate-100 dark:bg-slate-800 p-2 rounded">
≈ risk chart updated · source sheet v12 → v13 · speaker note regenerated · <span class="text-emerald-600">slide marked reviewed</span>
</div>
</div>

</div>

<!--
Merge theo policy: legal content cần legal reviewer, financial number cần finance approval, report từ stale sheet không được merge.
-->

---
layout: default
---

# Demo · Bước 4 — Agent-assisted editing (proposal)

Agent proposal ≈ Pull Request cho document/slide/sheet/workflow.

<div class="grid grid-cols-2 gap-6 mt-3">

<div class="text-xs">

```text
Proposal: Generate handover package
────────────────────────────────
Files created:
 - handover-report.mdoc
 - service-ownership.msheet
 - handover.mslide
Files updated:
 - service-index.mdoc
Affected graph nodes:
 - payment-service
 - notification-service
Validation:
 ✓ schema passed
 ⚠ 2 missing-documentation warnings
Suggested reviewers:
 - Team Lead, Next Owner
```

</div>

<div class="text-sm">

### Reviewer có thể
<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">👁️ Xem diff · source · generated artifact</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">💬 Comment vào section / slide / row</div>
<div class="px-2 py-1 rounded bg-emerald-50 dark:bg-emerald-900/20">✅ Approve toàn bộ / một phần</div>
<div class="px-2 py-1 rounded bg-amber-50 dark:bg-amber-900/20">🔁 Yêu cầu Agent chỉnh · lock section</div>
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">↩️ Reject / rollback</div>
</div>

<div class="mt-3 px-2 py-1.5 rounded border-l-4 border-emerald-500 bg-emerald-50 dark:bg-emerald-900/20 text-xs">
Sau approve → commit vào Lob → graph & dashboard refresh → export/share (PDF, deck, HTML).
</div>

</div>

</div>

---
layout: section
---

# 4 · Agent workflow integration
Harness Engineering — biến reasoning engine thành agent đáng tin

---
layout: two-cols
layoutClass: gap-6
---

# Harness Engineering là gì?

<div class="text-sm mt-2">

Nếu LLM là <b>CPU</b>, harness là <b>toàn bộ phần còn lại của máy tính</b>: memory, I/O, scheduler, permission, logging.

<div class="mt-3 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không tự thành agent</div>
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không có memory bền vững</div>
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không tự biết tool nào an toàn</div>
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không tự đáng tin nếu thiếu verification</div>
</div>

<div class="mt-3 px-2 py-1.5 rounded border-l-4 border-indigo-500 bg-indigo-50 dark:bg-indigo-900/20 text-xs">
Khác biệt giữa "clever demo" và "dependable product" nằm ở <b>harness</b>, không ở model.
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/0.%20Resoning%20Engine.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Reasoning Engine" />
</div>

<!--
Muốn agent đáng tin hơn: đầu tư đầu tiên không phải đổi model, mà xây system tốt hơn.
-->

---
layout: default
---

# Agentic loop — vòng lặp cốt lõi

Harness engineering = thiết kế & kiểm soát vòng lặp này, không phải viết prompt hay.

<div class="grid grid-cols-2 gap-6 mt-3">

<div class="text-xs">

```text
while not done:
  instruction = load_policy_and_task()
  context = retrieve_relevant_context(state)
  action = model.decide(instruction, context, tools)

  if requires_tool(action):
    result = execute_in_sandbox(action)
    state = update_state(state, result)

  verification = verify(state, task, constraints)
  if verification.failed:  repair_or_escalate()
  if verification.complete: return output
```

</div>

<div class="text-xs">

| Dòng loop | Primitive |
| --- | --- |
| load_policy_and_task | **Instructions** |
| retrieve_relevant_context | **Context Delivery/Mgmt** |
| execute_in_sandbox | **Tools + Execution Env** |
| update_state | **Durable State** |
| toàn vòng lặp | **Orchestration** |
| verify | **Verification** |
| mọi bước | **Observability** |

</div>

</div>

<div class="mt-3 text-center text-sm opacity-70">Instructions phân lớp: system · developer · user · task · tool · safety · format · domain policy.</div>

---
layout: two-cols
layoutClass: gap-6
---

# Instructions — policy layer

<div class="text-sm mt-2">

Không phải "system prompt dài". Là <b>policy layer</b> có thứ bậc để giải quyết xung đột rõ ràng.

<div class="mt-2 text-xs">

| Lớp | Ưu tiên |
| --- | --- |
| Safety instruction | 🔴 cao nhất |
| System instruction | 🟠 nền tảng |
| Developer / Tool | 🟡 cấu hình |
| User / Task | 🟢 theo lần gọi |

</div>

<div class="mt-2 px-2 py-1.5 rounded bg-amber-50 dark:bg-amber-900/20 text-xs">
⚠️ Chống <b>prompt injection</b>: phân biệt "instruction từ nguồn tin cậy" với "dữ liệu cần xử lý".
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/1.%20Instructions.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Instructions" />
</div>

<!--
Luôn có explicit stop condition + explicit uncertainty behavior. Tách policy khỏi task.
-->

---
layout: two-cols
layoutClass: gap-6
---

# Context delivery pipeline

<div class="text-sm mt-2">

Model chỉ suy luận tốt trên context nó thực sự nhìn thấy.

<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">🔎 RAG · query rewriting · chunk ranking</div>
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">🎛️ Metadata & recency filtering</div>
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">🗜️ Context compression & prioritization</div>
<div class="px-2 py-1 rounded bg-teal-50 dark:bg-teal-900/20">📎 Citation grounding · structured blocks</div>
</div>

<div class="mt-2 px-2 py-1.5 rounded bg-rose-50 dark:bg-rose-900/20 text-xs">
Thiếu context → hallucination · Thừa context → "lost in the middle".
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/2.%20Context%20Delivery.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Context Delivery" />
</div>

---
layout: two-cols
layoutClass: gap-6
---

# Context management — chống drift

<div class="text-sm mt-2">

Duy trì tập trung xuyên suốt task dài — khác về bản chất với context delivery.

<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">🪟 Sliding window · working memory</div>
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">📝 Scratchpad · summarization</div>
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">💾 Checkpointing · goal stack</div>
<div class="px-2 py-1 rounded bg-violet-50 dark:bg-violet-900/20">✂️ Context pruning · relevance scoring</div>
</div>

<div class="mt-2 px-2 py-1.5 rounded bg-amber-50 dark:bg-amber-900/20 text-xs">
Agent dài hạn dễ: quên mục tiêu · lặp hành động · gọi tool thừa · không biết khi nào xong.
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/3.%20Context%20Management.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Context Management" />
</div>

---
layout: default
---

# Tool-use & execution environment

<div class="grid grid-cols-2 gap-5 mt-2">

<div class="flex flex-col items-center">
<img src="/content/4.%20Tool%20Interface.png" class="max-h-[210px] w-auto rounded-lg shadow-lg" alt="Tool Interface" />
<div class="mt-2 text-xs text-center">
<b>Tool Interface</b> — typed schema · validate input · structured output · <b>tách read/write</b> · human confirm cho irreversible
</div>
</div>

<div class="flex flex-col items-center">
<img src="/content/5.%20Execution%20Environment.png" class="max-h-[210px] w-auto rounded-lg shadow-lg" alt="Execution Environment" />
<div class="mt-2 text-xs text-center">
<b>Execution Env</b> — sandbox · permission boundary · secret mgmt · resource quota · deterministic replay
</div>
</div>

</div>

<div class="mt-3 p-2 rounded-lg border-l-4 border-rose-500 bg-rose-50 dark:bg-rose-900/20 text-xs">
Nguyên tắc: agent production-grade <b>không hành động trực tiếp trên production</b> nếu thiếu guardrail. Chạy trên staging/bản sao, chỉ chạm production sau verification + human approval.
</div>

<!--
Trong MarkdownOffice: "execution environment" chính là Cortexpod workspace có scope + permission; "write tool" đi qua proposal.
-->

---
layout: two-cols
layoutClass: gap-6
---

# Durable state & auditability

<div class="text-sm mt-2">

State là "sự thật về tiến trình", context là "cái model thấy ngay bây giờ". Context được build lại từ state.

<div class="mt-2 text-xs">

| Loại dữ liệu | Storage |
| --- | --- |
| Workflow state | DB có transaction |
| Artifact | Object storage |
| Hành động/sự kiện | Event log (append-only) |
| Semantic memory | Vector DB |
| Audit trail | Immutable log |
| Rollback state | Versioned store |

</div>

<div class="mt-2 px-2 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20 text-xs">
Nhờ durable state: resume sau lỗi · audit quyết định · rollback · chia việc cho sub-agent.
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/6.%20Durable%20State.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Durable State" />
</div>

<div class="text-xs opacity-60 mt-1 text-center">→ Lob chính là durable + versioned state store cho document.</div>

---
layout: two-cols
layoutClass: gap-6
---

# Orchestration & sub-agents

<div class="text-sm mt-2">

Điều phối nhiều bước / tool / agent / state cùng lúc.

<div class="mt-1 text-xs flex flex-col gap-1">
<div class="px-2 py-1 rounded bg-sky-50 dark:bg-sky-900/20">Sequential · Branching · Planner–Executor</div>
<div class="px-2 py-1 rounded bg-sky-50 dark:bg-sky-900/20">ReAct · DAG · Event-driven · HITL</div>
</div>

<div class="mt-2 px-2 py-1.5 rounded bg-amber-50 dark:bg-amber-900/20 text-xs">
⚠️ Multi-agent <b>không tự động tốt hơn</b>: cần protocol · shared state · conflict resolution · final authority · <b>verifier độc lập</b>.
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/8.%20Subagents.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Subagents" />
</div>

<!--
Review agent không nên dùng chung context với coding agent (thiên lệch xác nhận).
-->

---
layout: two-cols
layoutClass: gap-6
---

# Skills — procedures tái sử dụng

<div class="text-sm mt-2">

<b>Tool = capability</b> (gọi API). <b>Skill = procedure</b> đã kiểm chứng để dùng capability đúng cách.

```text
Tool:  GitHub API
Skill: Cách review PR an toàn
  đọc diff → check breaking change
  → chạy test → check convention
  → comment có cấu trúc → KHÔNG tự merge
```

<div class="mt-2 px-2 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20 text-xs">
Skill giảm hallucination: checklist rõ · output chuẩn · known failure modes · verification gắn liền.
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/9.%20Skills%20%26%20Procedures.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Skills & Procedures" />
</div>

<!--
Trong MarkdownOffice: skill = MarkdownWorkflow template (handover, incident review, onboarding).
-->

---
layout: two-cols
layoutClass: gap-6
---

# Human-in-the-loop & verification

<div class="text-sm mt-2">

Không để cùng model vừa tạo vừa tự xác nhận. Nhiều lớp lọc độc lập.

<div class="mt-2 text-xs font-mono bg-slate-100 dark:bg-slate-800 p-2 rounded">
patch → test runner<br>
&nbsp;&nbsp;→ static analyzer<br>
&nbsp;&nbsp;→ review agent (context độc lập)<br>
&nbsp;&nbsp;→ <b>human approves</b>
</div>

<div class="mt-2 px-2 py-1.5 rounded bg-indigo-50 dark:bg-indigo-900/20 text-xs">
Deterministic check (schema/type/lint) cho việc máy kiểm được · human review cho rủi ro cao (pháp lý, tài chính, irreversible).
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/10.%20Verification%20%26%20Observability.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Verification & Observability" />
</div>

<!--
MarkdownOffice ánh xạ trực tiếp: proposal + policy check + reviewer + audit = verification pipeline.
-->

---
layout: default
---

# MarkdownOffice = harness cho document agent

Từng primitive của Harness Engineering có một chỗ đứng cụ thể trong MarkdownOffice.

<div class="mt-4 text-sm">

| Harness primitive | Hiện thân trong MarkdownOffice |
| --- | --- |
| Instructions (policy layer) | Permission scope + document policy + workflow template |
| Context delivery | Document graph retrieval, permission-aware, structured blocks |
| Tool interface (read/write tách biệt) | Read = query graph · Write = **Agent proposal** |
| Execution environment | Cortexpod agent workspace (scoped, sandboxed) |
| Durable state | **Lob** version + transaction store |
| Verification | Validation + policy check + human review |
| Observability / Audit | Audit trail per repo/doc/section/block |

</div>

<div class="mt-3 text-center text-sm opacity-70">Đây là lý do MarkdownOffice là nền tảng tự nhiên để vận hành document agent trong enterprise.</div>

---
layout: section
---

# 5 · Agent Memory
Working · Episodic · Semantic · Procedural

---
layout: default
---

# Kiến trúc memory của Agent

Một agent mạnh không chỉ cần LLM tốt — cần một <b>memory workflow</b> tốt.

<div class="flex justify-center mt-3">
<svg viewBox="0 0 900 250" class="w-full max-h-[240px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arMem" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 Z" fill="#6366f1"/></marker>
  </defs>
  <g font-size="12" text-anchor="middle">
    <rect x="20"  y="100" width="120" height="46" rx="8" fill="#f1f5f9" stroke="#94a3b8"/><text x="80" y="122">Observation</text><text x="80" y="138" font-size="10" fill="#64748b">input</text>
    <rect x="170" y="100" width="130" height="46" rx="8" fill="#e0e7ff" stroke="#6366f1"/><text x="235" y="122">Working memory</text><text x="235" y="138" font-size="10" fill="#6366f1">context state</text>
    <rect x="330" y="100" width="130" height="46" rx="8" fill="#ede9fe" stroke="#8b5cf6"/><text x="395" y="122">Reason / Plan</text>
    <rect x="490" y="100" width="130" height="46" rx="8" fill="#f0fdfa" stroke="#14b8a6"/><text x="555" y="122">Action / Tool</text>
    <rect x="650" y="100" width="120" height="46" rx="8" fill="#fef3c7" stroke="#f59e0b"/><text x="710" y="122">Reflect</text>
    <rect x="800" y="100" width="90" height="46" rx="8" fill="#dcfce7" stroke="#22c55e"/><text x="845" y="122">Update</text>

    <rect x="300" y="10" width="130" height="34" rx="8" fill="#fff7ed" stroke="#fb923c"/><text x="365" y="31">Episodic</text>
    <rect x="450" y="10" width="130" height="34" rx="8" fill="#eff6ff" stroke="#3b82f6"/><text x="515" y="31">Semantic</text>
    <rect x="600" y="10" width="130" height="34" rx="8" fill="#f0fdf4" stroke="#22c55e"/><text x="665" y="31">Procedural</text>
  </g>
  <g stroke="#6366f1" stroke-width="1.5" fill="none" marker-end="url(#arMem)">
    <line x1="140" y1="123" x2="168" y2="123"/>
    <line x1="300" y1="123" x2="328" y2="123"/>
    <line x1="460" y1="123" x2="488" y2="123"/>
    <line x1="620" y1="123" x2="648" y2="123"/>
    <line x1="770" y1="123" x2="798" y2="123"/>
    <path d="M365,44 L385,98" stroke-dasharray="4 3"/>
    <path d="M515,44 L410,98" stroke-dasharray="4 3"/>
    <path d="M665,44 L420,98" stroke-dasharray="4 3"/>
    <path d="M845,146 C845,210 400,215 235,215 L235,148" />
  </g>
  <text x="500" y="235" text-anchor="middle" font-size="10" fill="#94a3b8">consolidation: episodic → semantic · repeated success → procedural</text>
</svg>
</div>

---
layout: two-cols
layoutClass: gap-6
---

# Working memory

## "Agent đang nghĩ gì ngay lúc này"

<div class="text-sm mt-2">
Bộ nhớ ngắn hạn = context hiện tại của agent.
</div>

```txt
User goal: tạo MarkdownSheet
Current task: tìm spreadsheet engine
Constraints: Markdown-native, CSV-first,
             formula, chart, collaboration
State: đang so sánh Univer vs HyperFormula
```

<div class="mt-2 text-xs opacity-70">Nằm ở: LLM context window · scratchpad · task state · temporary variables.</div>

::right::

<div class="mt-14 text-sm">

### Chức năng
<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">🎯 Giữ mục tiêu hiện tại</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">📌 Giữ constraint của user</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">🧩 Giữ intermediate reasoning</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">🔧 Điều phối tool calls</div>
</div>

<div class="mt-3 px-2 py-1.5 rounded bg-rose-50 dark:bg-rose-900/20 text-xs">
⚠️ Dễ quá tải → cần summarize · compress · prioritize · promote lên long-term.
</div>

</div>

---
layout: two-cols
layoutClass: gap-6
---

# Episodic memory

## "Agent đã trải qua chuyện gì"

<div class="text-sm mt-2">
Lịch sử tương tác & hành động theo timeline.
</div>

```json
{
  "time": "2026-07-06",
  "event": "User asked MarkdownSheet architecture",
  "entities": ["MarkdownSheet","CSV","formula"],
  "outcome": "Recommended engine architecture"
}
```

<div class="mt-2 text-xs opacity-70">Lưu: conversation logs · event timeline · tool-call history · decision history.</div>

::right::

<div class="mt-14 text-sm">

### Vai trò
<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">👤 Personalization — nhớ lịch sử user</div>
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">🔗 Continuity — tiếp tục project cũ</div>
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">🐞 Debugging — biết agent đã làm gì</div>
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">📚 Learning from experience</div>
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">🗂️ Auditability</div>
</div>

<div class="mt-3 px-2 py-1.5 rounded bg-emerald-50 dark:bg-emerald-900/20 text-xs">
→ Trong MarkdownOffice: chính là <b>Lob transaction log</b> (append-only, timestamped).
</div>

</div>

---
layout: two-cols
layoutClass: gap-6
---

# Semantic memory

## "Agent biết gì về thế giới / project"

<div class="text-sm mt-2">
Knowledge base: facts, concepts, relationships, rules.
</div>

```json
{
  "concept": "MarkdownOffice",
  "type": "product ecosystem",
  "components": ["Doc","Slide","Sheet"],
  "principles": ["Markdown-native","AST-driven",
                 "AI-agent-friendly"]
}
```

<div class="mt-2 text-xs opacity-70">Lưu: vector DB · knowledge graph · document index · ontology.</div>

::right::

<div class="mt-14 text-sm">

### Episodic vs Semantic
<div class="mt-2 text-xs">

| Episodic | Semantic |
| --- | --- |
| Gắn sự kiện | Gắn tri thức |
| Có thời gian | Không cần thời gian |
| "Tôi đã làm gì?" | "Tôi biết gì?" |
| Log / timeline | Knowledge graph |

</div>

<div class="mt-3 px-2 py-1.5 rounded bg-blue-50 dark:bg-blue-900/20 text-xs">
→ Trong MarkdownOffice: chính là <b>document graph</b> — world model của tổ chức.
</div>

</div>

---
layout: two-cols
layoutClass: gap-6
---

# Procedural memory

## "Agent biết làm việc như thế nào"

<div class="text-sm mt-2">
Không phải "biết cái gì" mà "biết làm ra sao".
</div>

```txt
Workflow: Fix bug in codebase
1. Reproduce issue
2. Inspect logs
3. Locate failing module
4. Patch code
5. Run tests
6. Summarize change
```

<div class="mt-2 text-xs opacity-70">Lưu: prompt templates · tool-use policies · workflow graphs · reusable skills.</div>

::right::

<div class="mt-14 text-sm">

### Vai trò
<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-green-50 dark:bg-green-900/20">♻️ Reusability — không nghĩ lại từ đầu</div>
<div class="px-2 py-1 rounded bg-green-50 dark:bg-green-900/20">📏 Consistency — quy trình ổn định</div>
<div class="px-2 py-1 rounded bg-green-50 dark:bg-green-900/20">🎓 Skill acquisition</div>
<div class="px-2 py-1 rounded bg-green-50 dark:bg-green-900/20">🤝 Multi-agent orchestration</div>
</div>

<div class="mt-3 px-2 py-1.5 rounded bg-violet-50 dark:bg-violet-900/20 text-xs">
→ Trong MarkdownOffice: chính là <b>MarkdownWorkflow + Skill layer</b>.
</div>

</div>

---
layout: default
---

# MarkdownOffice như một memory substrate có cấu trúc

Cả 4 loại memory đều tìm được một "nhà" bền vững, có version và audit trong MarkdownOffice.

<div class="flex justify-center mt-3">
<svg viewBox="0 0 900 230" class="w-full max-h-[220px]" font-family="ui-sans-serif, system-ui">
  <defs>
    <marker id="arSub" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 Z" fill="#6366f1"/></marker>
  </defs>
  <g font-size="11.5" text-anchor="middle">
    <rect x="20"  y="20" width="180" height="40" rx="8" fill="#e0e7ff" stroke="#6366f1"/><text x="110" y="37">Working memory</text><text x="110" y="52" font-size="9.5" fill="#6366f1">context state</text>
    <rect x="20"  y="75" width="180" height="40" rx="8" fill="#fff7ed" stroke="#fb923c"/><text x="110" y="92">Episodic</text><text x="110" y="107" font-size="9.5" fill="#c2410c">event log</text>
    <rect x="20"  y="130" width="180" height="40" rx="8" fill="#eff6ff" stroke="#3b82f6"/><text x="110" y="147">Semantic</text><text x="110" y="162" font-size="9.5" fill="#1d4ed8">knowledge graph</text>
    <rect x="20"  y="185" width="180" height="40" rx="8" fill="#f0fdf4" stroke="#22c55e"/><text x="110" y="202">Procedural</text><text x="110" y="217" font-size="9.5" fill="#15803d">workflow / skill</text>

    <rect x="520" y="20" width="360" height="40" rx="8" fill="#f8fafc" stroke="#94a3b8"/><text x="700" y="44">Cortexpod agent workspace (scoped session)</text>
    <rect x="520" y="75" width="360" height="40" rx="8" fill="#f8fafc" stroke="#94a3b8"/><text x="700" y="99">Lob transaction log (append-only, timestamped)</text>
    <rect x="520" y="130" width="360" height="40" rx="8" fill="#f8fafc" stroke="#94a3b8"/><text x="700" y="154">Document graph + metadata (world model)</text>
    <rect x="520" y="185" width="360" height="40" rx="8" fill="#f8fafc" stroke="#94a3b8"/><text x="700" y="209">MarkdownWorkflow templates + Skill layer</text>
  </g>
  <g stroke="#6366f1" stroke-width="1.6" marker-end="url(#arSub)">
    <line x1="200" y1="40"  x2="518" y2="40"/>
    <line x1="200" y1="95"  x2="518" y2="95"/>
    <line x1="200" y1="150" x2="518" y2="150"/>
    <line x1="200" y1="205" x2="518" y2="205"/>
  </g>
</svg>
</div>

<div class="mt-2 text-center text-sm opacity-70">Structured · versioned · auditable → memory không "tan" khi phiên chat kết thúc.</div>

---
layout: section
---

# 6 · Product strategy
Wedge · GTM · roadmap · platform vision

---
layout: default
---

# Product strategy — target users & wedge

<div class="grid grid-cols-2 gap-6 mt-3">

<div>

### 🎯 Target users
<div class="mt-2 text-xs">

| Đối tượng | Cần gì | MarkdownOffice cấp |
| --- | --- | --- |
| User phổ thông | UI giống Office | Doc/Slide/Sheet viewer |
| Engineer | Markdown · diff · version | Source · AST · Lob |
| Manager | Report · approval | Dashboard · proposal |
| AI Agent | Structured context | AST · graph · metadata |
| Enterprise | Security · audit | Permission · policy |

</div>

</div>

<div>

### 🪓 Enterprise wedge
<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20"><b>Employee handover</b> — ROI rõ, pain thật</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">Structured report generation</div>
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20">Incident review · onboarding</div>
</div>

<div class="mt-3 px-2 py-1.5 rounded border-l-4 border-emerald-500 bg-emerald-50 dark:bg-emerald-900/20 text-xs">
Không clone toàn bộ Office. Bắt đầu từ <b>một workflow giá trị cao</b>, rồi mở rộng thành document monorepo.
</div>

</div>

</div>

---
layout: default
---

# Enterprise go-to-market & monetization

<div class="grid grid-cols-3 gap-4 mt-4 text-sm">

<div class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">🚪</div>
<b>Land</b><br>
<span class="opacity-70 text-xs">Handover / report agent cho một team. Value trong tuần đầu, không cần thay toàn bộ tooling.</span>
</div>

<div class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">📈</div>
<b>Expand</b><br>
<span class="opacity-70 text-xs">Từ 1 team → nhiều team → document monorepo toàn tổ chức. Graph càng lớn, giá trị càng tăng.</span>
</div>

<div class="p-4 rounded-lg border border-gray-300 dark:border-gray-600">
<div class="text-2xl">🔒</div>
<b>Lock-in giá trị</b><br>
<span class="opacity-70 text-xs">System of record cho organizational knowledge — càng dùng càng khó rời.</span>
</div>

</div>

<div class="mt-5 text-sm">

### 💰 Monetization direction
<div class="mt-2 grid grid-cols-2 gap-2 text-xs">
<div class="px-2 py-1 rounded bg-amber-50 dark:bg-amber-900/20">Seat: editor + viewer</div>
<div class="px-2 py-1 rounded bg-amber-50 dark:bg-amber-900/20">Agent workflow / automation runs</div>
<div class="px-2 py-1 rounded bg-amber-50 dark:bg-amber-900/20">Enterprise: permission · audit · compliance</div>
<div class="px-2 py-1 rounded bg-amber-50 dark:bg-amber-900/20">Agent governance & policy tier</div>
</div>

</div>

<!--
Monetizable through permission, audit, compliance, workflow và agent governance — không chỉ "AI editor".
-->

---
layout: default
---

# Platform roadmap

<div class="flex justify-center mt-6">
<svg viewBox="0 0 920 220" class="w-full max-h-[220px]" font-family="ui-sans-serif, system-ui">
  <line x1="40" y1="110" x2="880" y2="110" stroke="#cbd5e1" stroke-width="3"/>
  <g font-size="10.5" text-anchor="middle">
    <g>
      <circle cx="90" cy="110" r="9" fill="#6366f1"/><text x="90" y="80" font-weight="700">Phase 1</text>
      <text x="90" y="140">MarkdownDoc</text><text x="90" y="154" fill="#64748b">AST · version</text>
    </g>
    <g>
      <circle cx="230" cy="110" r="9" fill="#6366f1"/><text x="230" y="80" font-weight="700">Phase 2</text>
      <text x="230" y="140">Cortexpod</text><text x="230" y="154" fill="#64748b">repo · proposal</text>
    </g>
    <g>
      <circle cx="370" cy="110" r="9" fill="#8b5cf6"/><text x="370" y="80" font-weight="700">Phase 3</text>
      <text x="370" y="140">MarkdownSlide</text><text x="370" y="154" fill="#64748b">generation</text>
    </g>
    <g>
      <circle cx="510" cy="110" r="9" fill="#8b5cf6"/><text x="510" y="80" font-weight="700">Phase 4</text>
      <text x="510" y="140">MarkdownSheet</text><text x="510" y="154" fill="#64748b">workflow tables</text>
    </g>
    <g>
      <circle cx="650" cy="110" r="9" fill="#14b8a6"/><text x="650" y="80" font-weight="700">Phase 5</text>
      <text x="650" y="140">Document graph</text><text x="650" y="154" fill="#64748b">+ automation</text>
    </g>
    <g>
      <circle cx="810" cy="110" r="9" fill="#22c55e"/><text x="810" y="80" font-weight="700">Phase 6</text>
      <text x="810" y="140">Enterprise</text><text x="810" y="154" fill="#64748b">permission · audit</text>
    </g>
  </g>
</svg>
</div>

<div class="mt-3 text-center text-sm opacity-70">Wedge nhỏ giá trị cao → mở rộng thành full enterprise document monorepo, không vỡ kiến trúc.</div>

---
layout: two-cols
layoutClass: gap-6
---

# The ratchet loop — harness evolution

<div class="text-sm mt-2">

Từ <b>demo agent</b> → <b>production agent</b> là khoảng cách của cả một lớp hệ thống, không phải "thêm vài tính năng".

<div class="mt-2 text-xs">

| Demo agent | Production agent |
| --- | --- |
| Prompt dài = "não" | Instruction hierarchy |
| Tool không validate | Typed schema |
| Không durable state | Resume · audit · rollback |
| Không verification | Verification bắt buộc |
| Quyền rộng | Least privilege |

</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/12.%20The%20Ratchet%20Loop%20-%20Harness%20Evolution.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Ratchet Loop" />
</div>

<!--
Mỗi lần agent lỗi → thêm một guardrail vào harness → không bao giờ lùi lại. Đó là ratchet.
-->

---
layout: two-cols
layoutClass: gap-6
---

# Diagnostic matrix — debug theo layer

<div class="text-sm mt-2">

Khi agent lỗi trong production, câu hỏi đầu tiên: <b>"lỗi này thuộc layer nào?"</b> — không mặc định đổ lỗi cho model.

<div class="mt-2 flex flex-col gap-1 text-xs">
<div class="px-2 py-1 rounded bg-slate-100 dark:bg-slate-800">Instruction conflict → Instruction layer</div>
<div class="px-2 py-1 rounded bg-slate-100 dark:bg-slate-800">Hallucination → Context / Verification</div>
<div class="px-2 py-1 rounded bg-slate-100 dark:bg-slate-800">Sai tham số tool → Tool router</div>
<div class="px-2 py-1 rounded bg-slate-100 dark:bg-slate-800">Side effect nguy hiểm → Execution env</div>
<div class="px-2 py-1 rounded bg-slate-100 dark:bg-slate-800">Mất tiến trình → Durable state</div>
</div>

</div>

::right::

<div class="flex items-center justify-center h-full">
<img src="/content/13.%20The%20Practical%20Diagnostic%20Matrix.png" class="max-h-[400px] w-auto rounded-lg shadow-xl" alt="Diagnostic Matrix" />
</div>

---
layout: default
---

# Final vision

<div class="text-center mt-3 text-lg opacity-90 max-w-3xl mx-auto">
Tương lai của tài liệu doanh nghiệp là <b>structured · versioned · Agent-native</b> — nơi con người và AI Agent cùng tạo, review, version, trình bày và tự động hóa.
</div>

<div class="mt-6 flex justify-center">
<div class="inline-block text-sm">

| Before | After |
| --- | --- |
| Scattered files | Centralized document monorepo |
| Manual copy-paste | Graph-driven generation |
| Weak versioning | Lob-backed transactions |
| Human-only editing | Human + Agent collaboration |
| Static slide | Source-linked presentation |
| Unclear audit | Full traceability |

</div>
</div>

<div class="mt-5 text-center">
<span class="text-xl font-bold">GitHub for enterprise documents. Microsoft Office for AI Agents.</span>
</div>

<!--
Cơ hội xây lại nền tảng tài liệu doanh nghiệp từ đầu cho thời đại AI Agent.
-->

---
layout: center
class: text-center
---

# Cảm ơn — Q&A

<div class="mt-4 text-lg opacity-80 max-w-2xl mx-auto">
MarkdownOffice: LLM-first, markdown-native document agent cho enterprise monorepo.
</div>

<div class="mt-8 flex justify-center gap-3 text-sm">
  <span class="px-4 py-2 rounded-full bg-indigo-100 dark:bg-indigo-900/40">MarkdownDoc · Sheet · Slide · Jira</span>
  <span class="px-4 py-2 rounded-full bg-teal-100 dark:bg-teal-900/40">Cortexpod + Lob</span>
  <span class="px-4 py-2 rounded-full bg-amber-100 dark:bg-amber-900/40">Agent + Memory + Harness</span>
</div>

<div class="mt-10 text-sm opacity-60">
Rất mong nhận câu hỏi về kiến trúc, agent workflow, memory substrate và go-to-market.
</div>

<!--
Kết deck: mời thảo luận. Nhấn mạnh MarkdownOffice là category mới — AI-native document operating system.
-->
