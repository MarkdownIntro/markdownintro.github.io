# Agent Memory - bộ nhớ của AI Agent

## 1. Working Memory - bộ nhớ làm việc

**Trong não:**
Working memory là bộ nhớ ngắn hạn dùng để giữ thông tin đang xử lý ngay lúc này. Ví dụ: bạn đang đọc một đoạn code, giữ trong đầu tên biến, logic hàm, mục tiêu sửa bug.

**Trong AI Agent:**
Working memory tương ứng với **context hiện tại của agent**.

Ví dụ:

```txt
User goal: tạo MarkdownSheet
Current task: tìm kiến trúc spreadsheet engine
Constraints: Markdown-native, CSV-first, formula, chart, collaboration
Current reasoning state: đang so sánh Luckysheet, Univer, HyperFormula
```

Trong hệ thống Agent, working memory thường nằm ở:

```txt
LLM context window
scratchpad
current task state
active conversation state
temporary variables
short-term planner state
```

**Chức năng chính:**

| Vai trò                    | Ý nghĩa                                                     |
| -------------------------- | ----------------------------------------------------------- |
| Giữ mục tiêu hiện tại      | Agent biết mình đang làm gì                                 |
| Giữ constraint             | Không quên yêu cầu của user                                 |
| Giữ intermediate reasoning | Dùng để chia nhỏ task                                       |
| Điều phối tool calls       | Biết bước tiếp theo cần gọi search, code, file, calendar... |

**Vấn đề lớn:**
Working memory dễ bị quá tải vì context window có giới hạn. Vì vậy Agent cần cơ chế:

```txt
summarize
compress
prioritize
drop irrelevant context
promote important info to long-term memory
```

## 2. Episodic Memory - bộ nhớ sự kiện / trải nghiệm

**Trong não:**
Episodic memory là ký ức về các sự kiện cụ thể: chuyện gì đã xảy ra, khi nào, ở đâu, với ai, trong ngữ cảnh nào.

Ví dụ:

```txt
Hôm qua tôi đã phân tích MarkdownOffice legal requirements.
Tuần trước tôi đã hỏi về R2 private bucket.
Ngày 18/06/2026 tôi quyết định nghỉ MoMo để tập trung AI Agent và 3D Tiles.
```

**Trong AI Agent:**
Episodic memory là **lịch sử tương tác và hành động của agent theo timeline**.

Ví dụ:

```json
{
  "time": "2026-07-06",
  "event": "User asked about MarkdownSheet architecture",
  "entities": ["MarkdownSheet", "MarkdownOffice", "CSV", "formula engine"],
  "outcome": "Recommended spreadsheet engine architecture"
}
```

**Dùng để trả lời các câu như:**

```txt
Lần trước tôi đã nói gì về MarkdownOffice?
Tôi đã chọn hướng nào cho MarkdownSheet?
Trước đây agent đã thử workflow nào?
Task này đã thất bại ở bước nào?
```

Trong Agent system, episodic memory thường lưu dưới dạng:

```txt
conversation logs
event timeline
task execution traces
tool-call history
decision history
user interaction history
agent observation-action records
```

**Workflow điển hình:**

```txt
User interaction
→ extract important event
→ store timestamped memory
→ index by entity / topic / time
→ retrieve when context similar
```

**Vai trò quan trọng trong Agent:**

| Vai trò                  | Ý nghĩa                       |
| ------------------------ | ----------------------------- |
| Personalization          | Nhớ lịch sử của user          |
| Continuity               | Tiếp tục project từ lần trước |
| Debugging                | Biết agent đã làm gì          |
| Learning from experience | Nhận ra workflow nào hiệu quả |
| Auditability             | Có log giải thích quyết định  |

## 3. Semantic Memory - bộ nhớ tri thức / khái niệm

**Trong não:**
Semantic memory là kiến thức tổng quát, không nhất thiết gắn với một sự kiện cụ thể.

Ví dụ:

```txt
Excel là spreadsheet software.
Markdown là lightweight markup language.
CRDT dùng cho collaborative editing.
Erlang/OTP phù hợp hệ thống concurrent, fault-tolerant.
```

**Trong AI Agent:**
Semantic memory là **knowledge base của agent**.

Nó lưu các facts, concepts, relationships, rules, definitions.

Ví dụ:

```json
{
  "concept": "MarkdownOffice",
  "type": "product ecosystem",
  "components": ["MarkdownDoc", "MarkdownSlide", "MarkdownSheet"],
  "principles": ["Markdown-native", "AST-driven", "AI-agent-friendly"]
}
```

Semantic memory thường nằm trong:

```txt
vector database
knowledge graph
document index
RAG system
entity store
facts database
ontology
```

**Khác với episodic memory:**

| Episodic Memory               | Semantic Memory                            |
| ----------------------------- | ------------------------------------------ |
| Gắn với sự kiện               | Gắn với tri thức                           |
| Có thời gian cụ thể           | Có thể không cần thời gian                 |
| “Tôi đã làm gì?”              | “Tôi biết gì?”                             |
| Log / timeline                | Knowledge base / graph                     |
| Ví dụ: user hỏi về R2 hôm qua | Ví dụ: R2 là object storage của Cloudflare |

**Trong Agent workflow:**

```txt
retrieve relevant knowledge
→ ground reasoning
→ generate answer
→ update knowledge if new stable fact appears
```

Semantic memory giúp Agent không chỉ phản hồi theo từng phiên chat, mà có thể hình thành **world model** hoặc **project model**.

Ví dụ với MarkdownOffice:

```txt
MarkdownOffice
 ├── MarkdownDoc
 ├── MarkdownSlide
 ├── MarkdownSheet
 ├── AST pipeline
 ├── document graph
 ├── version history
 └── AI-native workflows
```

Đây là semantic structure, không chỉ là lịch sử chat.

## 4. Procedural Memory - bộ nhớ kỹ năng / quy trình

**Trong não:**
Procedural memory là bộ nhớ về cách làm một việc. Ví dụ:

```txt
Cách đi xe đạp
Cách gõ phím
Cách debug code
Cách viết prompt nghiên cứu
Cách thiết kế backend WebSocket
```

Nó không chỉ là “biết cái gì”, mà là “biết làm như thế nào”.

**Trong AI Agent:**
Procedural memory tương ứng với:

```txt
skills
tools
routines
playbooks
policies
workflows
automation recipes
agent capabilities
```

Ví dụ:

```txt
Workflow: Research open-source alternatives
1. Search latest projects
2. Compare license
3. Analyze architecture
4. Identify reusable modules
5. Recommend starter stack
```

Hoặc:

```txt
Workflow: Fix bug in codebase
1. Reproduce issue
2. Inspect logs
3. Locate failing module
4. Patch code
5. Run tests
6. Summarize change
```

Trong Agent system, procedural memory có thể được lưu dưới dạng:

```txt
prompt templates
tool-use policies
workflow graphs
code functions
agents as reusable skills
automation scripts
reflection-derived heuristics
```

**Vai trò chính:**

| Vai trò                   | Ý nghĩa                           |
| ------------------------- | --------------------------------- |
| Reusability               | Agent không phải nghĩ lại từ đầu  |
| Consistency               | Làm việc theo quy trình ổn định   |
| Skill acquisition         | Tạo kỹ năng mới sau nhiều lần làm |
| Automation                | Biến kinh nghiệm thành workflow   |
| Multi-agent orchestration | Mỗi agent có skill chuyên biệt    |

Ví dụ với Agent phát triển sản phẩm:

```txt
ResearchAgent.procedure = tìm nguồn, đọc tài liệu, trích xuất insight
CodeAgent.procedure = sửa code, chạy test, refactor
DesignAgent.procedure = tạo UI spec, component map, interaction flow
MemoryAgent.procedure = trích xuất, phân loại, lưu và truy xuất memory
```

## 5. Mapping tổng quát sang kiến trúc AI Agent

Có thể mô hình hóa như sau:

```txt
AI Agent Memory Architecture

Input / Observation
        ↓
Working Memory
        ↓
Reasoning / Planning
        ↓
Retrieve:
   - Episodic Memory
   - Semantic Memory
   - Procedural Memory
        ↓
Action / Tool Use
        ↓
Reflection
        ↓
Memory Update
```

Bảng mapping:

| Brain Memory | Agent Memory     | Lưu cái gì?               | Thời gian sống | Ví dụ                              |
| ------------ | ---------------- | ------------------------- | -------------- | ---------------------------------- |
| Working      | Context state    | Task hiện tại             | Rất ngắn       | Đang phân tích MarkdownSheet       |
| Episodic     | Event log        | Trải nghiệm đã xảy ra     | Dài hạn        | User từng hỏi về R2                |
| Semantic     | Knowledge base   | Tri thức, facts, concepts | Dài hạn        | MarkdownOffice gồm Doc/Slide/Sheet |
| Procedural   | Skills/workflows | Cách làm                  | Dài hạn        | Cách research open-source stack    |

## 6. Workflow memory hoàn chỉnh cho Agent

Một Agent tốt nên có pipeline memory như sau:

```txt
1. Observe
   Nhận input từ user, tool, file, website, hệ thống.

2. Encode
   Trích xuất entity, intent, facts, events, constraints.

3. Classify
   Phân loại thông tin:
   - working?
   - episodic?
   - semantic?
   - procedural?

4. Store
   Lưu vào đúng memory layer.

5. Retrieve
   Khi có task mới, lấy lại memory liên quan.

6. Reason
   Kết hợp memory + current context để lập kế hoạch.

7. Act
   Gọi tool, viết code, tạo tài liệu, gửi email, cập nhật hệ thống.

8. Reflect
   Đánh giá kết quả: đúng/sai, hiệu quả/chưa hiệu quả.

9. Consolidate
   Biến trải nghiệm thành tri thức hoặc quy trình mới.
```

Điểm quan trọng là **không phải memory nào cũng nên lưu giống nhau**.

Ví dụ:

```txt
"User đang hỏi về agent memory" 
→ working memory

"User ngày 06/07/2026 hỏi về working, episodic, semantic, procedural memory"
→ episodic memory

"Agent memory có 4 loại chính: working, episodic, semantic, procedural"
→ semantic memory

"Khi giải thích agent memory, nên dùng bảng mapping não ↔ agent"
→ procedural memory
```

## 7. Thiết kế thực tế cho Agent memory system

Một kiến trúc thực dụng có thể là:

```txt
Working Memory:
- in-context state
- short-term task buffer
- summarized current session

Episodic Memory:
- append-only event log
- timestamp
- user/project/task/action/outcome
- searchable by time and entity

Semantic Memory:
- vector DB + knowledge graph
- facts, concepts, relationships
- entity resolution
- versioned knowledge

Procedural Memory:
- workflow registry
- tool-use recipes
- reusable agent skills
- successful task patterns
```

Có thể triển khai schema đơn giản:

```ts
type AgentMemory =
  | WorkingMemory
  | EpisodicMemory
  | SemanticMemory
  | ProceduralMemory

type WorkingMemory = {
  taskId: string
  goal: string
  constraints: string[]
  currentState: string
  temporaryNotes: string[]
}

type EpisodicMemory = {
  eventId: string
  timestamp: string
  actor: string
  action: string
  context: string
  outcome: string
  relatedEntities: string[]
}

type SemanticMemory = {
  entityId: string
  concept: string
  facts: string[]
  relations: Array<{
    type: string
    target: string
  }>
  confidence: number
}

type ProceduralMemory = {
  skillId: string
  name: string
  trigger: string
  steps: string[]
  tools: string[]
  successCriteria: string[]
}
```

## 8. Cách 4 memory phối hợp trong một Agent

Ví dụ user hỏi:

```txt
Hãy tiếp tục thiết kế MarkdownSheet từ ý tưởng lần trước.
```

Agent cần dùng cả 4 loại memory:

```txt
Working memory:
- Task hiện tại là tiếp tục thiết kế MarkdownSheet.

Episodic memory:
- Lần trước user đã nói MarkdownSheet là spreadsheet trong MarkdownOffice.

Semantic memory:
- MarkdownSheet dùng CSV, .msheet wrapper, formula engine, chart, AST.

Procedural memory:
- Dùng workflow phân tích sản phẩm:
  1. Define core model
  2. Compare existing tools
  3. Design architecture
  4. Propose MVP
  5. Define roadmap
```

Không có episodic memory, agent sẽ không biết “lần trước” là gì.
Không có semantic memory, agent không hiểu MarkdownSheet là gì.
Không có procedural memory, agent không biết nên tiếp tục theo quy trình nào.
Không có working memory, agent không giữ được mục tiêu hiện tại.

## 9. Một điểm rất quan trọng: consolidation

Trong não, memory không chỉ được lưu ngay lập tức. Nó được **củng cố** qua thời gian. Trong AI Agent cũng nên có quá trình tương tự.

Ví dụ:

```txt
Nhiều episodic memories lặp lại:
- User thường hỏi về MarkdownOffice
- User ưu tiên Markdown-native
- User thích architecture rõ ràng
- User dùng SvelteKit, Erlang/OTP, Cloudflare

Sau consolidation:

Semantic memory:
- User đang xây dựng MarkdownOffice, một hệ sinh thái Office AI-native dựa trên Markdown.

Procedural memory:
- Khi giúp user về sản phẩm, nên trả lời theo hướng:
  architecture → MVP → tech stack → roadmap → risk.
```

Nói cách khác:

```txt
Episodic memories → tổng hợp thành semantic memory
Repeated successful actions → tổng hợp thành procedural memory
```

Đây là phần rất quan trọng nếu muốn xây dựng Agent có khả năng “trưởng thành” theo thời gian.

## 10. Kết luận ngắn gọn

Có thể hiểu 4 loại memory như sau:

```txt
Working memory    = Agent đang nghĩ gì ngay lúc này
Episodic memory   = Agent đã trải qua chuyện gì
Semantic memory   = Agent biết gì về thế giới/user/project
Procedural memory = Agent biết làm việc như thế nào
```

Trong Agent development, 4 loại memory này không nên được gom chung thành một vector database duy nhất. Nên tách thành các lớp riêng:

```txt
Context state       → working memory
Timeline/event log  → episodic memory
Knowledge graph/RAG → semantic memory
Workflow/skills     → procedural memory
```

Thiết kế đúng memory architecture sẽ giúp Agent có các năng lực quan trọng:

```txt
nhớ ngữ cảnh
tiếp tục công việc cũ
cá nhân hóa theo user
học từ trải nghiệm
tích lũy tri thức
tự động hóa workflow
tạo kỹ năng mới
phối hợp multi-agent
```

Một Agent mạnh không chỉ cần LLM tốt. Nó cần một **memory workflow tốt**, vì memory chính là thứ biến một chatbot phản hồi từng lượt thành một hệ thống agent có tính liên tục, học hỏi và phát triển theo thời gian.
