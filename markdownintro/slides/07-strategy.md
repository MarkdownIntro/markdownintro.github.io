---
layout: section
---

# 7 · Product strategy
Wedge · GTM · roadmap · platform vision

---
layout: default
---

# Product strategy - target users & wedge

<div class="grid grid-cols-2 gap-6 mt-3">

<div>

### 🎯 Target users
<div class="mt-2 text-xs">

| Đối tượng      | Cần gì                    | MarkdownOffice cấp     |
| -------------- | ------------------------- | ---------------------- |
| User phổ thông | UI giống Office           | Doc/Slide/Sheet viewer |
| Engineer       | Markdown · diff · version | Source · AST · Lob     |
| Manager        | Report · approval         | Dashboard · proposal   |
| AI Agent       | Structured context        | AST · graph · metadata |
| Enterprise     | Security · audit          | Permission · policy    |

</div>

</div>

<div>

### 🪓 Enterprise wedge
<div class="mt-2 flex flex-col gap-1.5 text-xs">
<div class="px-2 py-1 rounded bg-indigo-50 dark:bg-indigo-900/20"><b>Employee handover</b> - ROI rõ, pain thật</div>
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
<span class="opacity-70 text-xs">System of record cho organizational knowledge - càng dùng càng khó rời.</span>
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
Monetizable through permission, audit, compliance, workflow và agent governance - không chỉ "AI editor".
-->

---
layout: default
---

# Platform roadmap

<div class="flex justify-center mt-6">
<img src="../diagrams/roadmap.svg" class="w-full max-h-[220px] object-contain" alt="roadmap" />
</div>

<div class="mt-3 text-center text-sm opacity-70">Wedge nhỏ giá trị cao → mở rộng thành full enterprise document monorepo, không vỡ kiến trúc.</div>

---
layout: default
---

# The ratchet loop - harness evolution

<div class="text-sm mt-2">

Từ <b>demo agent</b> → <b>production agent</b> là khoảng cách của cả một lớp hệ thống, không phải "thêm vài tính năng".

<div class="mt-2 text-xs">

| Demo agent          | Production agent          |
| ------------------- | ------------------------- |
| Prompt dài = "não"  | Instruction hierarchy     |
| Tool không validate | Typed schema              |
| Không durable state | Resume · audit · rollback |
| Không verification  | Verification bắt buộc     |
| Quyền rộng          | Least privilege           |

</div>

</div>


<!--
Mỗi lần agent lỗi → thêm một guardrail vào harness → không bao giờ lùi lại. Đó là ratchet.
-->

---
layout: default
---

# Final vision

<div class="text-center mt-3 text-lg opacity-90 max-w-3xl mx-auto">
Tương lai của tài liệu doanh nghiệp là <b>structured · versioned · Agent-native</b> - nơi con người và AI Agent cùng tạo, review, version, trình bày và tự động hóa.
</div>

<!-- <div class="mt-6 flex justify-center">
<div class="inline-block text-sm">

</div>
</div> -->


| Trước đây               | Sau MarkdownOffice                    |
| ----------------------- | ------------------------------------- |
| Tệp tài liệu phân tán   | Monorepo tài liệu tập trung           |
| Sao chép thủ công       | Sinh nội dung dựa trên document graph |
| Quản lý phiên bản yếu   | Giao dịch được bảo chứng bởi Lob      |
| Chỉ con người chỉnh sửa | Cộng tác giữa con người và Agent      |
| Slide tĩnh              | Bài thuyết trình liên kết với nguồn   |
| Kiểm toán không rõ ràng | Truy vết đầy đủ toàn bộ thay đổi      |

<div class="text-center">
<span class="text-xl font-bold">GitHub for enterprise documents. Microsoft Office for AI Agents.</span>
</div>

<!--
Cơ hội xây lại nền tảng tài liệu doanh nghiệp từ đầu cho thời đại AI Agent.
-->

---
layout: center
class: text-center
---

# Cảm ơn - Q&A

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
Kết deck: mời thảo luận. Nhấn mạnh MarkdownOffice là category mới - AI-native document operating system.
--> 
