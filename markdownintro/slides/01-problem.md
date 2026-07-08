---
layout: section
---

# 1 · Nỗi đau tài liệu doanh nghiệp
Fragmented knowledge - con người mệt mỏi, AI Agent thất bại

---
layout: default
---

# Doanh nghiệp không thiếu tài liệu - thiếu hệ điều hành cho tài liệu

Cùng một sự thật vận hành bị chia nhỏ và sao chép qua hàng chục công cụ, không có <b>canonical source of truth</b>.

<div class="mt-4 flex justify-center">
<img src="../diagrams/fragmented-tools.svg" class="w-full max-h-[300px] object-contain" alt="fragmented-tools" />
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

| Loại thông tin    | Nơi thường nằm                | Vấn đề khi cần tổng hợp            |
| ----------------- | ----------------------------- | ---------------------------------- |
| Report / Spec     | Google Docs, Word, PDF        | Nhiều bản, không rõ bản mới nhất   |
| Slide             | PowerPoint, Google Slides     | Copy-paste, dễ stale               |
| Sheet / Data      | Excel, Google Sheets          | Formula khó audit cross-doc        |
| Task              | Jira, Linear, Trello          | Tách rời khỏi tài liệu mô tả       |
| Chat context      | Slack, Teams                  | Quyết định chìm trong hội thoại    |
| Decision / Wiki   | Notion, Confluence, email     | Không có version thật, không owner |
| Service ownership | Wiki, sheet, tribal knowledge | Mất khi nhân sự nghỉ việc          |
| Incident history  | Jira, Slack, postmortem       | Rời rạc, khó trace                 |

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
<div class="p-3 rounded-lg border border-gray-300 dark:border-gray-600">🕰️ <b>Bản cũ vs bản mới</b><br><span class="opacity-70">`final_v2_revised_latest.docx` - không ai chắc.</span></div>
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
<img src="../diagrams/document-chaos.svg" class="w-full max-h-[360px] object-contain" alt="document-chaos" />
</div>

<div class="text-center text-sm opacity-70 -mt-2">Sao chép chồng chéo · liên kết mờ · không version · không owner · không audit</div>

---
layout: default
---

# Trạng thái mong muốn: enterprise document monorepo

Một cây tri thức duy nhất - có cấu trúc, có version, có graph, có audit - để người và Agent cùng vận hành.

<div class="flex justify-center mt-4">
<img src="../diagrams/document-monorepo.svg" class="w-full max-h-[330px] object-contain" alt="document-monorepo" />
</div>

<!--
Từ chaos sang monorepo: mọi artifact có cấu trúc, có graph, có version và audit.
-->

