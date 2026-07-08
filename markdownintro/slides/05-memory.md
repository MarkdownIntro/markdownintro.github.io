---
layout: section
---

# 5 · Agent Memory
Working · Episodic · Semantic · Procedural

---
layout: default
---

# Kiến trúc memory của Agent

Một agent mạnh không chỉ cần LLM tốt - cần một <b>memory workflow</b> tốt.

<div class="flex justify-center mt-3">
<img src="../diagrams/memory-architecture.svg" class="w-full max-h-[240px] object-contain" alt="memory-architecture" />
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
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">👤 Personalization - nhớ lịch sử user</div>
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">🔗 Continuity - tiếp tục project cũ</div>
<div class="px-2 py-1 rounded bg-orange-50 dark:bg-orange-900/20">🐞 Debugging - biết agent đã làm gì</div>
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

| Episodic         | Semantic            |
| ---------------- | ------------------- |
| Gắn sự kiện      | Gắn tri thức        |
| Có thời gian     | Không cần thời gian |
| "Tôi đã làm gì?" | "Tôi biết gì?"      |
| Log / timeline   | Knowledge graph     |

</div>

<div class="mt-3 px-2 py-1.5 rounded bg-blue-50 dark:bg-blue-900/20 text-xs">
→ Trong MarkdownOffice: chính là <b>document graph</b> - world model của tổ chức.
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
<div class="px-2 py-1 rounded bg-green-50 dark:bg-green-900/20">♻️ Reusability - không nghĩ lại từ đầu</div>
<div class="px-2 py-1 rounded bg-green-50 dark:bg-green-900/20">📏 Consistency - quy trình ổn định</div>
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
<img src="../diagrams/memory-substrate.svg" class="w-full max-h-[220px] object-contain" alt="memory-substrate" />
</div>

<div class="mt-2 text-center text-sm opacity-70">Structured · versioned · auditable → memory không "tan" khi phiên chat kết thúc.</div>
