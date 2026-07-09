---
layout: section
---

# 6 · Harness Engineering
Kiến trúc Agent thực chiến - biến reasoning engine thành sản phẩm đáng tin

---
layout: default
---

# Harness là gì?

<div class="grid grid-cols-2 gap-6 mt-3">

<div class="text-sm">

Nếu LLM là <b>CPU</b>, thì <b>harness</b> là toàn bộ phần còn lại của máy tính: memory, I/O, scheduler, permission, logging.

<div class="mt-3 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không tự thành agent</div>
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không có memory bền vững</div>
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không tự biết tool nào an toàn</div>
<div class="px-2 py-1 rounded bg-rose-50 dark:bg-rose-900/20">LLM không tự đáng tin nếu thiếu verification</div>
</div>

<div class="mt-3 px-2 py-1.5 rounded border-l-4 border-indigo-500 bg-indigo-50 dark:bg-indigo-900/20 text-xs">
Khác biệt giữa <b>"clever demo"</b> và <b>"dependable product"</b> nằm ở harness, không ở model.
</div>

</div>

<div class="text-sm flex flex-col justify-center gap-2">

### Ba lát cắt kiến trúc tiếp theo

<div class="px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border border-gray-200 dark:border-gray-700">
<b>1 · Session ephemeral</b> - cái gì mất, cái gì còn lại
</div>
<div class="px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border border-gray-200 dark:border-gray-700">
<b>2 · LLM Agent Architecture</b> - bản kiến trúc tổng quát
</div>
<div class="px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border border-gray-200 dark:border-gray-700">
<b>3 · Hermes Harness</b> - một hiện thực production cụ thể
</div>

</div>

</div>

<!--
Muốn agent đáng tin hơn: đầu tư đầu tiên không phải đổi model, mà xây system tốt hơn.
-->

---
layout: default
---

# ① AI Agent Session - mọi thứ trong box là ephemeral

<div class="flex justify-center mt-1">
<img src="/harness/Al%20Agent%20Session%20-%20everything%20inside%20the%20box%20is%20ephemeral.png" class="w-full max-h-[360px] object-contain" alt="AI Agent Session - everything inside the box is ephemeral" />
</div>

<div class="mt-2 mx-auto max-w-5xl text-xs px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-sky-400">
Trong <b>session box</b> (ephemeral, mất khi phiên kết thúc): User Prompt + Chat History + System Prompt → <b>Working Memory / Context RAM</b> → LLM Q&amp;A Agent → Reply. Trí nhớ <b>bền vững nằm NGOÀI box</b>: Procedural (Skill.md) · Semantic (vector store) · Episodic (vector store). <b>RAG top-k</b> nạp lại vào working memory; message được lưu về Episodic; <b>Summarizer Agent</b> (model rẻ) distill &amp; consolidate sau N chat.
</div>

---
layout: default
---

# ② LLM Agent Architecture - bản kiến trúc tổng quát

<div class="flex justify-center mt-1">
<img src="/harness/LLM%20Agent%20Architecture.png" class="w-full max-h-[360px] object-contain" alt="LLM Agent Architecture" />
</div>

<div class="mt-2 mx-auto max-w-5xl text-xs px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-indigo-400">
<b>Harness</b> (LangGraph/LangChain/Pydantic) bọc phần ephemeral + <b>Loop</b> (Agentic Tools ↔ LLM Q&amp;A Agent → End Loop Guardrails → Reply) + ba tầng memory + Summarizer. Bên phải là <b>LLM Ops</b>: Trace (Langfuse/LangSmith) → <b>Eval</b> (LLM-as-a-judge) + <b>Observe</b> (tokens, latency, errors) → Diagnose → <b>Gate</b> → Release → cải thiện <i>System Prompt + Config</i> rồi vòng lại.
</div>

---
layout: default
---

# ③ Hermes Harness - một hiện thực production

<div class="flex justify-center mt-1">
<img src="/harness/Hermes%20Harness.png" class="w-full max-h-[360px] object-contain" alt="Hermes Harness" />
</div>

<div class="mt-2 mx-auto max-w-5xl text-xs px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-emerald-400">
Hermes chạy <b>local / Docker / SSH / VPS</b>, Gateway đa kênh (Telegram, WhatsApp, Slack…). Loop dùng <b>Hermes Adaptive Tools</b> (Terminal, Browser, delegate_tasks, search, skill_manage, MCP) + Sub-Agents + Claude Code, kích hoạt bằng <b>Cron</b>. Memory: SOUL.md · SKILL.md (<code>~/.hermes/skills</code>) · MEMORY.md (<code>~/.hermes/memories</code>) · <b>state.db</b> (SQLite + FTS5, keyword no-embedding). LLM Ops tách rời. Điểm nhấn: <b>Hermes agent tự tạo SKILL.md của chính nó</b>.
</div>

---
layout: default
---

# Harness ↔ MarkdownOffice

Từng primitive của harness có một chỗ đứng cụ thể khi vận hành document agent trong enterprise.

<div class="mt-4 text-sm">

| Harness primitive                     | Hiện thân trong MarkdownOffice                                |
| ------------------------------------- | ------------------------------------------------------------- |
| Instructions (policy layer)           | Permission scope + document policy + workflow template        |
| Context delivery / management         | Document graph retrieval, permission-aware, structured blocks |
| Tool interface (read/write tách biệt) | Read = query graph · Write = **Agent proposal**               |
| Execution environment                 | Cortexpod agent workspace (scoped, sandboxed)                 |
| Durable state                         | **Lob** version + transaction store                           |
| Verification / Observability          | Validation + policy check + human review + audit trail        |
| LLM Ops (trace → eval → release)       | Proposal review → approval → commit → dashboard refresh       |

</div>

<div class="mt-3 text-center text-sm opacity-70">MarkdownOffice là nền tảng tự nhiên để vận hành một document agent có harness đầy đủ.</div>

<!--
Ba diagram harness cho thấy chuẩn kiến trúc; MarkdownOffice là hiện thân của chuẩn đó cho enterprise documents.
-->
