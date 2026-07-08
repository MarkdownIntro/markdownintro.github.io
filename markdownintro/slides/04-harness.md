---
layout: section
---

# 4 · Agent workflow integration
Harness Engineering - biến reasoning engine thành agent đáng tin

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

# Agentic loop - vòng lặp cốt lõi

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

| Dòng loop                 | Primitive                 |
| ------------------------- | ------------------------- |
| load_policy_and_task      | **Instructions**          |
| retrieve_relevant_context | **Context Delivery/Mgmt** |
| execute_in_sandbox        | **Tools + Execution Env** |
| update_state              | **Durable State**         |
| toàn vòng lặp             | **Orchestration**         |
| verify                    | **Verification**          |
| mọi bước                  | **Observability**         |

</div>

</div>

<div class="mt-3 text-center text-sm opacity-70">Instructions phân lớp: system · developer · user · task · tool · safety · format · domain policy.</div>

---
layout: two-cols
layoutClass: gap-6
---

# Instructions - policy layer

<div class="text-sm mt-2">

Không phải "system prompt dài". Là <b>policy layer</b> có thứ bậc để giải quyết xung đột rõ ràng.

<div class="mt-2 text-xs">

| Lớp                | Ưu tiên        |
| ------------------ | -------------- |
| Safety instruction | 🔴 cao nhất     |
| System instruction | 🟠 nền tảng     |
| Developer / Tool   | 🟡 cấu hình     |
| User / Task        | 🟢 theo lần gọi |

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

# Context management - chống drift

<div class="text-sm mt-2">

Duy trì tập trung xuyên suốt task dài - khác về bản chất với context delivery.

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
<b>Tool Interface</b> - typed schema · validate input · structured output · <b>tách read/write</b> · human confirm cho irreversible
</div>
</div>

<div class="flex flex-col items-center">
<img src="/content/5.%20Execution%20Environment.png" class="max-h-[210px] w-auto rounded-lg shadow-lg" alt="Execution Environment" />
<div class="mt-2 text-xs text-center">
<b>Execution Env</b> - sandbox · permission boundary · secret mgmt · resource quota · deterministic replay
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

| Loại dữ liệu      | Storage                 |
| ----------------- | ----------------------- |
| Workflow state    | DB có transaction       |
| Artifact          | Object storage          |
| Hành động/sự kiện | Event log (append-only) |
| Semantic memory   | Vector DB               |
| Audit trail       | Immutable log           |
| Rollback state    | Versioned store         |

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

# Skills - procedures tái sử dụng

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

| Harness primitive                     | Hiện thân trong MarkdownOffice                                |
| ------------------------------------- | ------------------------------------------------------------- |
| Instructions (policy layer)           | Permission scope + document policy + workflow template        |
| Context delivery                      | Document graph retrieval, permission-aware, structured blocks |
| Tool interface (read/write tách biệt) | Read = query graph · Write = **Agent proposal**               |
| Execution environment                 | Cortexpod agent workspace (scoped, sandboxed)                 |
| Durable state                         | **Lob** version + transaction store                           |
| Verification                          | Validation + policy check + human review                      |
| Observability / Audit                 | Audit trail per repo/doc/section/block                        |

</div>

<div class="mt-3 text-center text-sm opacity-70">Đây là lý do MarkdownOffice là nền tảng tự nhiên để vận hành document agent trong enterprise.</div>
