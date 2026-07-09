---
layout: section
---

# 4 · Agent workflow
Giải phẫu một AI Agent - lắp ráp kiến trúc từng lớp một

---
layout: default
---

# Agent workflow là gì?

<div class="text-sm mt-2">

**Agent workflow** không phải một câu trả lời một lượt. Đó là một **vòng lặp có mục tiêu** (agentic loop): agent tự chọn hành động, gọi tool, quan sát kết quả, cập nhật trạng thái, và lặp lại đến khi hoàn thành.

</div>

<div class="grid grid-cols-2 gap-6 mt-3">

<div class="text-xs">

```text
nhận goal
 → load instructions
 → retrieve context
 → plan next action
 → chọn tool / reasoning step
 → execute
 → observe result
 → update state
 → verify progress
 → tiếp tục · dừng · escalate cho người
```

</div>

<div class="text-sm flex flex-col justify-center gap-3">

<div class="px-3 py-2 rounded bg-rose-50 dark:bg-rose-900/20">
Một <b>LLM trần</b> chỉ sinh token kế tiếp - không tự có loop, memory, tool hay verification.
</div>

<div class="px-3 py-2 rounded border-l-4 border-indigo-500 bg-indigo-50 dark:bg-indigo-900/20">
Ta phải <b>lắp ráp</b> quanh model từng lớp <b>primitive</b>. 13 sơ đồ sau xây dựng kiến trúc đó - mỗi sơ đồ thêm đúng một lớp.
</div>

</div>

</div>

<!--
Đây là section "giải phẫu": mỗi slide thêm một primitive, biến reasoning engine thành một agent hoàn chỉnh.
-->

---
layout: default
---

# ⓪ Reasoning Engine - điểm khởi đầu

<div class="flex justify-center mt-2">
<img src="/content/0.%20Resoning%20Engine.png" class="w-full max-h-[360px] object-contain" alt="Reasoning Engine" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-indigo-400">
<code>(PROMPT) → [MODEL] → (TEXT)</code>. Bản chất LLM chỉ là <b>probabilistic reasoning engine</b>: nhận token, sinh token có xác suất cao nhất. Chưa có memory, tool hay verification - <b>chưa phải agent</b>.
</div>

---
layout: default
---

# ① + Instructions - lớp policy

<div class="flex justify-center mt-2">
<img src="/content/1.%20Instructions.png" class="w-full max-h-[350px] object-contain" alt="Instructions" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-indigo-400">
Thêm <b>[INSTRUCTIONS]</b> phía trên prompt: <b>policy layer</b> định nghĩa vai trò, quy tắc được phép, điều cấm, và <b>stop condition</b>. Phân lớp có thứ bậc: system · developer · user · task · tool · <b>safety</b>.
</div>

---
layout: default
---

# ② + Context Delivery - đưa đúng dữ liệu vào

<div class="flex justify-center mt-2">
<img src="/content/2.%20Context%20Delivery.png" class="w-full max-h-[350px] object-contain" alt="Context Delivery" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-teal-400">
Thêm <b>[CONTEXT DELIVERY]</b> - nạp <b>Docs · Files · Logs</b> vào model song song với Instructions. Model chỉ suy luận tốt trên context nó <b>thực sự nhìn thấy</b> → chống hallucination do thiếu dữ liệu.
</div>

---
layout: default
---

# ③ + Context Management - nén qua phễu

<div class="flex justify-center mt-2">
<img src="/content/3.%20Context%20Management.png" class="w-full max-h-[350px] object-contain" alt="Context Management" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-teal-400">
Nhiều nguồn được <b>nén qua phễu</b> trước khi vào model: <b>Retrieval · Ranking · Compaction</b>. Giữ đúng thông tin quan trọng, tránh tràn cửa sổ context và hiện tượng <i>"lost in the middle"</i>.
</div>

---
layout: default
---

# ④ + Tool Interface - biến model thành actor

<div class="flex justify-center mt-2">
<img src="/content/4.%20Tool%20Interface.png" class="w-full max-h-[350px] object-contain" alt="Tool Interface" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-amber-400">
Thêm <b>[TOOL INTERFACE]</b> dưới model: mỗi tool có <b>Schema · Arguments · Target</b> rõ ràng, input được validate trước khi chạy. Đây là lúc model tác động ra thế giới bên ngoài context của nó.
</div>

---
layout: default
---

# ⑤ + Execution Environment - cô lập hành động

<div class="flex justify-center mt-2">
<img src="/content/5.%20Execution%20Environment.png" class="w-full max-h-[350px] object-contain" alt="Execution Environment" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-amber-400">
Bọc tool trong <b>[EXECUTION ENVIRONMENT]</b> (viền nét đứt = ranh giới cô lập): <b>Sandboxes · Containers · Credentials</b>. Agent chỉ thấy & chạm được đúng thứ môi trường cho phép - không chạm production trực tiếp.
</div>

---
layout: default
---

# ⑥ + Durable State - nhớ tiến trình ngoài context

<div class="flex justify-center mt-2">
<img src="/content/6.%20Durable%20State.png" class="w-full max-h-[350px] object-contain" alt="Durable State" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-emerald-400">
Thêm <b>[DURABLE STATE]</b> (mũi tên hai chiều với model): <b>Checkpoints · Task State · Memory</b> nằm <b>ngoài</b> context window → agent có thể <b>resume sau lỗi</b>, rollback và audit quyết định.
</div>

---
layout: default
---

# ⑦ + Orchestration - điều phối toàn cục

<div class="flex justify-center mt-2">
<img src="/content/7.%20Orchestration.png" class="w-full max-h-[350px] object-contain" alt="Orchestration" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-sky-400">
Cột <b>[ORCHESTRATION]</b> nối tất cả qua <b>Lifecycle Hooks · Retries · Approval Gates</b>: chọn bước kế tiếp, retry đúng bước lỗi (không chạy lại từ đầu), và chèn điểm chờ phê duyệt của con người.
</div>

---
layout: default
---

# ⑧ + Subagents - chia nhỏ năng lực

<div class="flex justify-center mt-2">
<img src="/content/8.%20Subagents.png" class="w-full max-h-[350px] object-contain" alt="Subagents" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-sky-400">
<b>[SUBAGENTS]</b> - orchestrator ủy thác qua <b>Delegation · Handoffs · Specialists</b>. Mỗi sub-agent là một kiến trúc thu nhỏ với context & quyền riêng. Lưu ý: multi-agent <b>không tự nhiên tốt hơn</b> - cần protocol rõ ràng và <b>verifier độc lập</b>.
</div>

---
layout: default
---

# ⑨ + Skills & Procedures - quy trình tái sử dụng

<div class="flex justify-center mt-2">
<img src="/content/9.%20Skills%20%26%20Procedures.png" class="w-full max-h-[350px] object-contain" alt="Skills & Procedures" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-violet-400">
<b>[SKILLS & PROCEDURES]</b> - <b>Playbooks · Workflows · Recipes</b>: quy trình đã kiểm chứng để dùng tool đúng cách, đúng thứ tự, kèm bước kiểm tra. Agent không phải <i>tự phát minh quy trình</i> mỗi lần → giảm hallucination.
</div>

---
layout: default
---

# ⑩ + Verification & Observability - làm cho agent đáng tin

<div class="flex justify-center mt-2">
<img src="/content/10.%20Verification%20%26%20Observability.png" class="w-full max-h-[350px] object-contain" alt="Verification & Observability" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-indigo-400">
Dải <b>[VERIFICATION & OBSERVABILITY]</b> chạy dọc đáy toàn hệ thống: nhận <b>CI/Builds</b>, xuất <b>Traces · Eval Dashboards</b>. Nhiều lớp lọc độc lập trước khi coi output là "xong" - điều kiện bắt buộc cho production.
</div>

---
layout: default
---

# ⑫ The Ratchet Loop - harness tiến hóa

<div class="flex justify-center mt-2">
<img src="/content/12.%20The%20Ratchet%20Loop%20-%20Harness%20Evolution.png" class="w-full max-h-[350px] object-contain" alt="The Ratchet Loop - Harness Evolution" />
</div>

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-emerald-400">
Toàn bộ primitive nối thành một <b>vòng tiến hóa</b> (chevron một chiều = <i>ratchet</i>): mỗi lỗi production được "khóa lại" thành một cải tiến harness, không bao giờ lùi. Harness trưởng thành dần qua từng lần chạy.
</div>

---
layout: default
---

# ⑬ Diagnostic Matrix - lỗi nào, thiếu primitive nào

<!-- <div class="flex justify-center mt-2"> -->
<!-- <img src="/content/13.%20The%20Practical%20Diagnostic%20Matrix.png" class="w-full max-h-[340px] object-contain" alt="The Practical Diagnostic Matrix" /> -->

| Dạng lỗi quan sát được                    | Primitive còn thiếu | Cách khắc phục                                       |
| ----------------------------------------- | ------------------- | ---------------------------------------------------- |
| Agent đoán sai quy tắc của dự án          | Chỉ dẫn / Ngữ cảnh  | Gắn hướng dẫn của repo vào prompt.                   |
| Cửa sổ input bị vượt quá giới hạn / nhiễu | Quản lý ngữ cảnh    | Thêm lớp truy xuất và nén ngữ cảnh.                  |
| Agent xóa nhầm thư mục                    | Môi trường thực thi | Cô lập worktree và giới hạn quyền.                   |
| Agent quên kế hoạch giữa chừng            | Trạng thái bền vững | Lưu checkpoint bên ngoài context window.             |
| Tác vụ bị treo khi chờ con người          | Điều phối           | Triển khai lifecycle hooks và các cổng phê duyệt.    |
| Bug lặp lại qua 5 phiên làm việc          | Tiến hóa Harness    | Chuyển trace lỗi thành một Skill có thể tái sử dụng. |
<!-- </div> -->

<div class="mt-3 mx-auto max-w-4xl text-sm px-3 py-2 rounded bg-slate-50 dark:bg-slate-800 border-l-4 border-rose-400">
Khi agent lỗi, hỏi <b>"thiếu primitive nào?"</b> thay vì đổ lỗi cho model: <i>Failure Mode → Missing Primitive → Fix</i>. Ví dụ: "wiped the wrong directory" → <b>Execution Environment</b> → isolate worktree & bound permissions.
</div>

<!--
13 sơ đồ = bản đồ đầy đủ để đối chiếu khi debug. Chuyển tiếp sang phần Harness: ba kiến trúc agent thực chiến.
-->
