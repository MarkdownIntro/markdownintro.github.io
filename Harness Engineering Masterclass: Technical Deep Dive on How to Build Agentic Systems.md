# Harness Engineering Masterclass: Technical Deep Dive on How to Build Agentic Systems

---

## 1. Harness Engineering là gì?

Harness engineering là lớp kỹ thuật nằm giữa user, model, tools, memory, execution runtime và external systems. Nó không phải là prompt engineering mở rộng, mà là một hệ thống hoàn chỉnh đóng vai trò như "operating system" cho agent - kiểm soát input nào được đưa vào model, output nào được chấp nhận, context nào được duy trì, hành động nào được phép thực thi, trạng thái nào được lưu giữ, quyền hạn nào được cấp, và kết quả nào được xác minh trước khi coi là hoàn thành.

Một cách nhìn khác: nếu LLM là CPU, thì harness là toàn bộ phần còn lại của máy tính - bộ nhớ, I/O, scheduler, filesystem, permission model, logging. CPU mạnh không tạo ra một hệ điều hành đáng tin cậy nếu không có các thành phần đó bao quanh nó.

Bốn sự thật kỹ thuật cần làm rõ ngay từ đầu:

- **LLM không tự động trở thành agent.** Một model chỉ sinh token kế tiếp dựa trên context nó nhận được. Khả năng "hành động lặp lại có mục tiêu" - nhận task, chọn action, gọi tool, quan sát kết quả, tiếp tục - không nằm trong model, mà nằm trong vòng lặp bên ngoài do harness điều khiển.
- **LLM không có memory bền vững nếu hệ thống không cung cấp.** Context window là bộ nhớ làm việc tạm thời, biến mất khi phiên kết thúc. Mọi thứ gọi là "agent nhớ được việc hôm qua" đều là kết quả của một hệ thống lưu trữ và truy xuất bên ngoài model.
- **LLM không tự biết tool nào an toàn nếu interface không được thiết kế đúng.** Model chỉ nhìn thấy schema và mô tả tool. Nếu schema mơ hồ, model sẽ đoán tham số. Nếu không có permission boundary, model có thể gọi một tool phá hủy dữ liệu với cùng mức "tự tin" như khi gọi một tool chỉ đọc.
- **LLM không tự đáng tin cậy nếu không có verification và observability.** Model không biết khi nào nó sai. Nó không có cơ chế nội tại để phát hiện hallucination của chính nó. Độ tin cậy phải được encode từ bên ngoài, bằng các lớp kiểm tra độc lập với quá trình sinh ra output.

Sự khác biệt giữa một "clever demo" và một "dependable product" gần như luôn nằm ở harness, không nằm ở model. Hai đội cùng dùng chung một model, cùng một API, có thể tạo ra hai sản phẩm có độ tin cậy khác nhau hoàn toàn - vì một bên đầu tư vào lớp hệ thống bao quanh, bên còn lại chỉ viết một prompt dài và hy vọng model tự xử lý phần còn lại.

---

## 2. Model là reasoning engine, không phải toàn bộ system

Cần định vị chính xác vai trò của LLM trong kiến trúc: nó là một **probabilistic reasoning engine**. Nó nhận một chuỗi token làm input, và sinh ra chuỗi token tiếp theo có xác suất cao nhất theo phân phối đã học. Nó không "biết" sự thật theo nghĩa một database biết; nó ước lượng token nào hợp lý nhất trong ngữ cảnh hiện có.

Từ đó, hallucination không nên được hiểu đơn thuần là "model thiếu kiến thức". Nó là hệ quả của bốn nguyên nhân hệ thống:

1. **Thiếu context** - model không có thông tin cần thiết trong prompt nên phải "điền vào chỗ trống" bằng suy luận thống kê.
2. **Thiếu ràng buộc** - không có constraint nào chặn model tạo ra câu trả lời nghe hợp lý nhưng sai.
3. **Thiếu tool grounding** - model trả lời từ tri thức nội tại thay vì gọi tool để lấy dữ liệu thực tế, vì không có cơ chế bắt buộc grounding.
4. **Thiếu verification** - không có bước nào kiểm tra lại output trước khi nó được coi là final.

Hệ quả kiến trúc quan trọng: **muốn agent đáng tin cậy hơn, đầu tư đầu tiên không phải là "đổi sang model tốt hơn", mà là "xây system tốt hơn".** Một model tốt hơn giảm tần suất lỗi biên (edge-case reasoning error), nhưng không loại bỏ được các lỗi hệ thống - thiếu context, thiếu constraint, thiếu verification vẫn tồn tại bất kể model nào được dùng.

Mức độ cần harness tăng phi tuyến theo độ phức tạp của agent, vì các yếu tố sau cộng dồn:

- Nhiều bước suy luận → sai số tích lũy qua từng bước (compounding error).
- Nhiều tool call → nhiều điểm có thể gọi sai tool hoặc sai tham số.
- Nhiều state trung gian → cần đồng bộ, dễ mất mát hoặc xung đột.
- Nhiều quyền truy cập → bề mặt rủi ro bảo mật lớn hơn.
- Nhiều failure mode → cần rollback, retry, audit, permission, monitoring cho từng loại lỗi riêng biệt.

Một agent chạy 3 bước với 1 tool có thể chấp nhận được với một prompt tốt. Một agent chạy 40 bước với 15 tool, ghi vào production database, và chạy trong 6 giờ không giám sát - nếu không có harness đầy đủ, xác suất thất bại tích lũy gần như chắc chắn xảy ra ở đâu đó trong chuỗi hành động.

---

## 3. Agentic Loop: vòng lặp cốt lõi của AI Agent

Agentic loop là chuỗi bước lặp lại cho đến khi mục tiêu hoàn thành hoặc cần dừng:

1. Receive goal / task
2. Load instructions
3. Retrieve relevant context
4. Plan next action
5. Select tool or reasoning step
6. Execute action
7. Observe result
8. Update state
9. Verify progress
10. Continue, stop, escalate hoặc ask for human input

Harness engineering, về bản chất, chính là **thiết kế và kiểm soát vòng lặp này** - không phải viết prompt hay, mà là quyết định: context nào được nạp ở bước nào, tool nào được phép gọi ở trạng thái nào, khi nào loop phải dừng, khi nào phải escalate cho người.

```pseudo
while not done:
    instruction = load_policy_and_task()
    context = retrieve_relevant_context(state, task)
    action = model.decide(instruction, context, available_tools)

    if requires_tool(action):
        result = execute_in_sandbox(action)
        state = update_state(state, result)
    else:
        state = update_state(state, action.output)

    verification = verify(state, task, constraints)

    if verification.failed:
        repair_or_escalate()

    if verification.complete:
        return final_output
```

Điểm mấu chốt: mỗi dòng pseudo-code ở trên tương ứng với một primitive riêng biệt sẽ được phân tích ở các phần tiếp theo. `load_policy_and_task()` là Instructions. `retrieve_relevant_context()` là Context Delivery + Context Management. `execute_in_sandbox()` là Tool Interfaces + Execution Environments. `update_state()` là Durable State. Toàn bộ vòng lặp được điều khiển bởi Orchestration. `verify()` là Verification. Và mọi bước đều cần được ghi lại bởi Observability.

---

## 4. Instructions: primitive đầu tiên của harness

Instructions không phải là "system prompt" theo nghĩa một đoạn văn dài mô tả tính cách agent. Đó là một **policy layer** định nghĩa: vai trò của agent, mục tiêu cụ thể của task, quy tắc hành động được phép, điều cấm tuyệt đối, tiêu chuẩn định dạng output, cách sử dụng từng tool, cách hỏi lại user khi thiếu thông tin, cách xử lý uncertainty, và điều kiện dừng.

Trong một hệ thống trưởng thành, instruction được phân lớp thay vì gộp chung vào một prompt:

| Lớp                    | Vai trò                                 | Ví dụ                                                         |
| ---------------------- | --------------------------------------- | ------------------------------------------------------------- |
| System instruction     | Ràng buộc nền tảng, không đổi theo task | "Không bao giờ thực thi lệnh xóa dữ liệu mà chưa có xác nhận" |
| Developer instruction  | Cấu hình do đội kỹ thuật đặt ra         | "Luôn trả JSON theo schema X"                                 |
| User instruction       | Yêu cầu trực tiếp từ người dùng         | "Tóm tắt báo cáo này"                                         |
| Task instruction       | Ngữ cảnh cụ thể của tác vụ hiện tại     | "Task ID 4471, deadline hôm nay"                              |
| Tool instruction       | Cách dùng đúng một tool cụ thể          | "Chỉ gọi search_db với limit ≤ 50"                            |
| Safety instruction     | Ràng buộc an toàn, ưu tiên cao nhất     | "Từ chối nếu action có nguy cơ pháp lý"                       |
| Format instruction     | Yêu cầu cấu trúc output                 | "Trả về markdown table"                                       |
| Domain-specific policy | Quy tắc riêng của ngành/tổ chức         | "Tuân thủ chuẩn IFC khi export mô hình 3D"                    |

Phân lớp này quan trọng vì nó cho phép **giải quyết xung đột theo thứ bậc rõ ràng** - khi user instruction mâu thuẫn với safety instruction, hệ thống cần biết ai thắng, thay vì để model tự quyết định dựa trên độ "thuyết phục" của câu chữ.

### Failure mode thường gặp

- **Instruction conflict** - hai chỉ dẫn mâu thuẫn nhau không được giải quyết trước, model buộc phải đoán ưu tiên.
- **Prompt injection** - nội dung từ external context (tài liệu, tool result, trang web) chứa chỉ dẫn giả mạo, và nếu hệ thống không phân biệt "instruction từ nguồn tin cậy" với "dữ liệu cần xử lý", model có thể tuân theo lệnh độc hại nhúng trong dữ liệu.
- **Over-broad instruction** - chỉ dẫn kiểu "hãy làm điều tốt nhất cho user" không cho model biết ranh giới hành động cụ thể.
- **Under-specified task** - thiếu định nghĩa "hoàn thành" là gì, khiến agent không biết khi nào nên dừng.
- **Hidden assumption** - instruction giả định một điều kiện môi trường (ví dụ: "file luôn tồn tại") mà thực tế không đảm bảo.
- **Tool misuse** - instruction mô tả tool không đủ rõ về ràng buộc, dẫn đến gọi tool ngoài phạm vi dự kiến.

### Best practice

- Instruction phải cụ thể, có thứ bậc rõ ràng, không mơ hồ về ưu tiên.
- Tách **policy** (bất biến, áp dụng mọi task) khỏi **task** (thay đổi theo từng lần gọi).
- Tách **format instruction** khỏi **reasoning instruction** - trộn lẫn hai loại này khiến model phải cân bằng giữa suy nghĩ đúng và trình bày đúng cùng lúc, làm giảm chất lượng cả hai.
- Không nhồi rule không liên quan đến task hiện tại - mỗi rule dư thừa là một phần context bị tiêu tốn cho việc không cần thiết.
- Luôn có **explicit stop condition** - không để model tự đoán khi nào task hoàn thành.
- Luôn có **explicit uncertainty behavior** - định nghĩa rõ: khi không chắc, hỏi lại, không đoán, không tự ý hành động.

---

## 5. Context Delivery: đưa đúng thông tin vào model

Model chỉ suy luận tốt trên context nó thực sự nhìn thấy. Context delivery là quá trình **chọn, đóng gói và đưa thông tin liên quan** vào prompt tại đúng thời điểm cần.

### Nguồn context điển hình

User message, conversation history, retrieved documents, files, database records, tool results, memory dài hạn, current state, environment state, business rules, user preferences.

### Vấn đề kỹ thuật cốt lõi

- **Context window hữu hạn** - không thể nhét toàn bộ dữ liệu tổ chức vào một request.
- **Noise làm giảm chất lượng reasoning** - càng nhiều thông tin không liên quan, model càng dễ bị phân tán attention, dẫn tới bỏ sót chi tiết quan trọng nằm giữa một khối context dài (hiện tượng "lost in the middle").
- **Context thiếu dẫn đến hallucination** - model buộc phải "đoán" khi thiếu dữ liệu thật.
- **Context thừa dẫn đến mất focus** - model có xu hướng bám vào chi tiết gần cuối prompt hoặc chi tiết nổi bật thay vì chi tiết quan trọng nhất.
- **Context lỗi thời gây quyết định sai** - dữ liệu cache cũ khiến agent hành động dựa trên trạng thái không còn đúng.

### Kỹ thuật xử lý

- **Retrieval-Augmented Generation (RAG)** - truy xuất tài liệu liên quan từ knowledge base thay vì nhồi toàn bộ corpus vào prompt.
- **Query rewriting** - viết lại câu hỏi user thành truy vấn tối ưu cho retrieval (ví dụ mở rộng từ viết tắt, chuẩn hóa thuật ngữ).
- **Chunk ranking** - xếp hạng các đoạn tài liệu theo độ liên quan trước khi đưa vào prompt, không chỉ theo thứ tự tìm thấy.
- **Metadata filtering** - lọc theo owner, project, timestamp, permission trước khi retrieval, tránh rò rỉ dữ liệu không thuộc phạm vi.
- **Recency filtering** - ưu tiên dữ liệu mới nhất khi có nhiều phiên bản.
- **Context compression** - tóm tắt các đoạn dài thành bản rút gọn giữ nguyên thông tin cốt lõi trước khi đưa vào prompt.
- **Context prioritization** - đặt thông tin quan trọng nhất ở vị trí model chú ý nhiều nhất (đầu hoặc cuối context, tùy kiến trúc).
- **Citation grounding** - buộc model gắn nguồn cho mỗi claim, giảm khả năng bịa thông tin vì có ràng buộc phải chỉ ra xuất xứ.
- **Structured context blocks** - đóng gói context theo block có nhãn rõ ràng (`<task>`, `<tool_result>`, `<memory>`...) thay vì văn bản thô lẫn lộn, giúp model phân biệt loại thông tin.

Ví dụ thực tế: một agent xử lý pull request trên GitHub không nên nhận toàn bộ lịch sử repo. Nó cần: diff của PR hiện tại, file liên quan trực tiếp, coding convention của project, và kết quả CI gần nhất - được đóng gói thành structured block, không phải toàn bộ codebase đổ vào context.

---

## 6. Context Management: giữ agent không bị lạc hướng

Context delivery là đưa thông tin vào tại một thời điểm. **Context management là duy trì sự tập trung xuyên suốt nhiều bước** của một task dài - đây là vấn đề khác về bản chất, vì agent chạy càng lâu thì lượng thông tin tích lũy càng lớn, và context window không thể phình vô hạn.

### Kỹ thuật quản lý

- **Sliding window** - chỉ giữ N bước gần nhất trong context, loại bỏ phần cũ.
- **Working memory** - vùng lưu trữ tạm cho thông tin đang xử lý (biến số, kết quả trung gian).
- **Task state** - trạng thái tiến độ hiện tại của task (bước nào đã xong, bước nào còn lại).
- **Scratchpad** - không gian cho model "viết ra suy nghĩ" trước khi hành động, tách biệt với output cuối cùng.
- **Summarization** - nén lịch sử dài thành tóm tắt định kỳ, giữ lại quyết định quan trọng, bỏ chi tiết vụn.
- **Checkpointing** - lưu trạng thái tại các mốc quan trọng để có thể resume nếu gián đoạn.
- **Goal stack** - cấu trúc dạng stack lưu mục tiêu chính và các sub-goal phát sinh, đảm bảo agent quay lại đúng mục tiêu cha sau khi hoàn thành sub-goal.
- **Attention budgeting** - chủ động giới hạn lượng token dành cho mỗi loại thông tin, tránh một nguồn (ví dụ log lỗi dài) chiếm hết ngân sách context.
- **Context pruning** - loại bỏ thông tin không còn liên quan đến bước hiện tại một cách chủ động, không chỉ dựa vào window trượt theo thời gian.
- **Relevance scoring** - đánh giá lại độ liên quan của từng phần context mỗi khi state thay đổi, không giữ nguyên đánh giá ban đầu.

### Vì sao agent dài hạn dễ drift

- Quên mục tiêu ban đầu khi lịch sử quá dài và mục tiêu bị "chôn" ở đầu context.
- Bám vào chi tiết phụ vì nó xuất hiện gần nhất trong lịch sử.
- Lặp lại hành động đã thực hiện vì không có state đánh dấu "đã làm".
- Gọi tool không cần thiết vì không rõ đã có đủ thông tin hay chưa.
- Không biết khi nào đã hoàn thành vì thiếu goal stack rõ ràng và verifier độc lập.

### Kiến trúc đề xuất

```text
User Goal
   ↓
Task Planner
   ↓
Working Memory
   ↓
Context Retriever
   ↓
Model Reasoning Step
   ↓
Tool Execution
   ↓
Observation
   ↓
State Update
   ↓
Verifier
```

Điểm quan trọng: **Verifier** nằm ở cuối chuỗi và có đường phản hồi ngược lại Task Planner (không vẽ ở trên để giữ đơn giản) - khi verification thất bại, hệ thống quay lại planner để điều chỉnh, không quay lại model reasoning một cách mù quáng.

---

## 7. Tool Interfaces: biến model thành actor

Tool interface là cách model tương tác với thế giới bên ngoài context của chính nó: search, database, file system, code execution, email, calendar, browser, API nội bộ, payment system, deployment system, enterprise tools.

### Yêu cầu của một tool interface tốt

- **Schema rõ ràng** - định nghĩa kiểu dữ liệu chặt chẽ cho từng tham số, không dùng free-text cho những gì có thể enum hóa.
- **Input được validate** trước khi thực thi, không tin tưởng mù quáng tham số model sinh ra.
- **Output có cấu trúc** - JSON hoặc dạng có schema, không phải văn bản tự do khó parse lại.
- **Error message có thể xử lý được** - lỗi phải mô tả rõ nguyên nhân để model (hoặc orchestrator) có thể sửa và retry, không chỉ trả về "Error 500".
- **Permission rõ ràng** - mỗi tool gắn với một tập quyền cụ thể, không phải "agent có toàn quyền hệ thống".
- **Idempotency khi cần** - với các action có thể bị gọi lại (do retry), đảm bảo gọi hai lần không gây hậu quả gấp đôi.
- **Logging đầy đủ** cho mỗi lần gọi.
- **Timeout** - không để một tool call treo vô thời hạn.
- **Retry policy** rõ ràng - số lần thử lại, backoff strategy.
- **Dry-run** cho hành động nguy hiểm - cho phép mô phỏng trước khi thực thi thật.

### Failure mode

- Model gọi sai tool vì mô tả tool trùng lặp hoặc mơ hồ.
- Tham số không hợp lệ vì schema quá lỏng lẻo.
- Tool output quá dài, tràn context ở bước tiếp theo.
- Tool output mơ hồ khiến model diễn giải sai kết quả.
- Tool có side effect nguy hiểm (ghi đè dữ liệu, gửi email thật) mà không có cảnh báo.
- Tool bị prompt injection qua dữ liệu bên ngoài - ví dụ, một trang web agent duyệt qua chứa chỉ dẫn ẩn nhắm vào chính agent.

### Best practice

- Dùng typed schema (JSON Schema hoặc tương đương) cho mọi tool, không dùng tool nhận một chuỗi tự do làm input chính.
- Giới hạn quyền tool theo task cụ thể - một agent xử lý báo cáo không cần quyền xóa file.
- **Tách read tool và write tool** một cách rõ ràng ở tầng kiến trúc, không gộp chung để dễ implement.
- **Human confirmation bắt buộc** cho hành động không thể đảo ngược (xóa dữ liệu, gửi tiền, deploy production).
- Trả về structured result thay vì để model tự parse văn bản tự do từ tool.
- Có **sandbox** riêng cho các tool có khả năng gây hại nếu chạy sai (xem phần 8).

Ví dụ cụ thể: một tool `update_database(table, filter, values)` nếu không có validate và giới hạn phạm vi `filter`, model có thể vô tình sinh ra một filter rỗng và cập nhật toàn bộ bảng thay vì một record. Một interface tốt sẽ bắt buộc `filter` phải match tối đa N record, và từ chối thực thi nếu vượt ngưỡng mà chưa có xác nhận.

---

## 8. Execution Environments: kiểm soát nơi agent hành động

Execution environment là nơi hành động của agent thực sự được chạy: browser sandbox, code interpreter, container, serverless worker, VM, workflow runtime, isolated process, enterprise integration runtime.

### Vai trò của execution environment

- Giới hạn quyền truy cập - agent chỉ thấy được những gì môi trường cho phép thấy.
- Cô lập lỗi - một lỗi trong quá trình thực thi không lan ra ngoài phạm vi sandbox.
- Quản lý tài nguyên - giới hạn CPU, memory, thời gian chạy.
- Theo dõi hành động - mọi lệnh thực thi đều được ghi log tại tầng môi trường, độc lập với log của model.
- Cho phép rollback - môi trường có thể được reset về trạng thái trước khi agent hành động.
- Ngăn agent phá hỏng hệ thống thật - đặc biệt quan trọng khi agent có khả năng chạy code tùy ý.

### Yếu tố kỹ thuật cần thiết kế

- **Sandboxing** - cô lập process/container không có quyền truy cập ngoài phạm vi cho phép.
- **Permission boundary** - access control rõ ràng cho filesystem, network, secrets.
- **Network access** - whitelist domain, không cho phép truy cập tùy ý ra internet nếu không cần thiết.
- **File system isolation** - mount read-only cho dữ liệu nhạy cảm, ghi chỉ vào thư mục làm việc riêng.
- **Secret management** - credentials không bao giờ nằm trong context model nhìn thấy; được inject ở tầng runtime.
- **Resource quota** - giới hạn CPU/RAM/disk để tránh agent chạy vòng lặp vô hạn làm sập hệ thống.
- **Timeout** ở cấp môi trường, độc lập với timeout ở cấp tool.
- **Audit log** ghi lại toàn bộ lệnh thực thi trong môi trường, tách biệt khỏi log của orchestrator.
- **Deterministic replay** - khả năng chạy lại chính xác một chuỗi hành động để debug hoặc kiểm chứng.

Nguyên tắc kiến trúc quan trọng nhất của phần này: **một agent production-grade không nên được phép hành động trực tiếp trên production system nếu không có guardrail.** Pattern thực tế phổ biến: agent hành động trên một staging environment hoặc một bản sao dữ liệu, và chỉ hành động thật trên production sau khi verification (và thường là human approval) đã pass.

---

## 9. Durable State: giữ lại tiến trình công việc

Durable state là bộ nhớ bền vững của workflow - khác về bản chất với context tạm thời nằm trong một prompt, vốn biến mất khi phiên kết thúc.

### Các loại state

Task state, user state, workflow state, tool result state, intermediate artifact, decision log, checkpoint, version history, long-term memory.

### Vì sao cần durable state

- Agent có thể **resume sau lỗi** - không phải chạy lại từ đầu khi gặp sự cố giữa chừng.
- Có thể **audit quyết định** - trả lời được câu hỏi "tại sao agent làm điều này" sau khi sự việc đã xảy ra.
- Có thể **rollback** về một trạng thái trước đó khi phát hiện sai sót.
- Có thể **chia task cho sub-agent** - mỗi sub-agent đọc/ghi vào cùng một state store thay vì truyền toàn bộ lịch sử qua prompt.
- Có thể **tránh làm lại việc đã làm** - kiểm tra state trước khi thực thi một bước tốn kém.
- Có thể **bảo toàn work product** - kết quả trung gian không mất khi phiên chat kết thúc hoặc container bị restart.

### Đề xuất storage theo loại dữ liệu

| Loại dữ liệu                       | Storage phù hợp                                |
| ---------------------------------- | ---------------------------------------------- |
| Workflow state                     | Database quan hệ hoặc key-value có transaction |
| Artifact (file, report, hình ảnh)  | Object storage (S3-compatible)                 |
| Hành động, sự kiện                 | Event log (append-only)                        |
| Semantic memory (tìm theo ý nghĩa) | Vector DB                                      |
| Audit trail                        | Append-only log, immutable                     |
| Trạng thái có thể rollback         | Versioned state store (giữ lịch sử thay đổi)   |

Một sai lầm phổ biến: dùng chính conversation history của model làm "state store" duy nhất. Điều này nhồi thông tin không cần thiết vào context (làm giảm chất lượng reasoning như đã phân tích ở phần 6), và không có transaction guarantee - nếu container restart giữa chừng, toàn bộ tiến trình mất. State cần tách bạch khỏi context: context là "cái model nhìn thấy ngay bây giờ", state là "sự thật về tiến trình công việc", và context được build lại từ state khi cần chứ không phải ngược lại.

---

## 10. Orchestration: điều phối workflow agentic

Orchestration là lớp điều khiển nhiều bước, nhiều agent, nhiều tool và nhiều state cùng lúc.

### Các kiểu orchestration

- **Sequential workflow** - bước A xong mới đến bước B, đơn giản, dễ debug, phù hợp task tuyến tính.
- **Branching workflow** - rẽ nhánh theo điều kiện, ví dụ: nếu verification fail thì rẽ sang nhánh repair.
- **Planner–executor** - một model lập kế hoạch tổng thể, một (hoặc nhiều) model khác thực thi từng bước.
- **ReAct loop** (reason + act) - xen kẽ suy luận và hành động trong cùng một vòng lặp, phù hợp task cần điều chỉnh liên tục theo observation.
- **DAG-based workflow** - các bước có dependency được biểu diễn thành đồ thị có hướng không chu trình, cho phép chạy song song các nhánh độc lập.
- **Event-driven workflow** - bước tiếp theo được kích hoạt bởi sự kiện (webhook, tin nhắn mới, file mới) thay vì gọi tuần tự.
- **Human-in-the-loop workflow** - chèn bước chờ con người xác nhận tại các điểm rủi ro cao.
- **Multi-agent workflow** - nhiều agent chuyên biệt phối hợp (xem phần 11).

### Trách nhiệm của orchestrator

Chọn bước tiếp theo, quản lý dependency giữa các bước, quản lý retry khi một bước fail, quản lý timeout cho từng bước, quản lý error recovery (fallback path), quản lý state (đọc/ghi durable state), gọi verifier sau mỗi bước quan trọng, và quyết định stop condition - khi nào coi là hoàn thành, khi nào escalate cho người.

### Prompt-only agent vs Orchestrated agent

|                            | Prompt-only agent                    | Orchestrated agent                      |
| -------------------------- | ------------------------------------ | --------------------------------------- |
| Tốc độ triển khai          | Nhanh                                | Chậm hơn, cần thiết kế trước            |
| Độ phức tạp code           | Thấp                                 | Cao hơn                                 |
| Khả năng debug             | Khó - lỗi ẩn trong một prompt dài    | Dễ - mỗi bước có log riêng              |
| Khả năng retry chọn lọc    | Không - thường phải chạy lại toàn bộ | Có - retry đúng bước fail               |
| Độ tin cậy ở task phức tạp | Giảm nhanh khi task dài              | Ổn định hơn nhờ kiểm soát từng bước     |
| Phù hợp                    | Demo, prototype, task ngắn           | Production, task nhiều bước, nhiều tool |

Orchestration không phải là "làm mọi thứ phức tạp hơn cho vui" - nó là công cụ đánh đổi tốc độ phát triển ban đầu lấy khả năng vận hành lâu dài. Một prototype hợp lý có thể bắt đầu prompt-only; nhưng khi task có trên một vài bước tool-call phụ thuộc lẫn nhau, thiếu orchestration gần như chắc chắn dẫn đến lỗi khó tái hiện trong production.

---

## 11. Sub-agents: chia nhỏ năng lực

Sub-agent là một agent chuyên biệt cho một phần cụ thể của nhiệm vụ tổng thể: research agent, coding agent, review agent, planning agent, data extraction agent, verification agent, security agent, UX writing agent, deployment agent.

### Lợi ích

- **Giảm context overload** - mỗi sub-agent chỉ cần context liên quan đến phần việc của nó, không phải toàn bộ lịch sử của task tổng.
- **Tăng specialization** - instruction và skill layer của mỗi sub-agent được tối ưu riêng cho một loại việc, thay vì một instruction chung phải bao quát mọi tình huống.
- **Dễ kiểm soát quyền** - sub-agent nào cần quyền gì được cấp riêng, không có agent nào có toàn quyền hệ thống.
- **Dễ test từng phần** - có thể viết test riêng cho coding agent mà không phụ thuộc vào research agent.
- **Dễ parallelize** - các sub-agent độc lập có thể chạy song song, giảm latency tổng.
- **Dễ thay thế component** - nâng cấp research agent không ảnh hưởng đến coding agent, miễn interface giữa chúng không đổi.

### Cảnh báo quan trọng

**Multi-agent không tự động tốt hơn single-agent.** Chia nhỏ mà không thiết kế kỹ giao tiếp giữa các agent thường tạo ra nhiều lỗi hơn là ít lỗi hơn, vì:

- Cần **protocol giao tiếp rõ ràng** - định dạng message giữa các agent phải có schema, không phải văn bản tự do dễ hiểu sai.
- Cần **shared state** - nếu mỗi sub-agent giữ state riêng không đồng bộ, các agent có thể hành động dựa trên thông tin mâu thuẫn nhau.
- Cần **conflict resolution** - khi hai sub-agent đưa ra kết luận khác nhau (ví dụ review agent và coding agent bất đồng), cần có cơ chế giải quyết đã định nghĩa trước, không để hệ thống treo.
- Cần **final authority** - một điểm quyết định cuối cùng (thường là orchestrator hoặc một agent cấp cao hơn) có quyền override khi có xung đột.
- Cần **verifier độc lập** - không để một sub-agent tự kiểm tra công việc của chính nó là đủ; lý tưởng là một agent hoặc cơ chế tách biệt đóng vai trò verify.

Ví dụ cụ thể: trong một pipeline coding agent → review agent, nếu review agent dùng chung model và chung context với coding agent, nó có xu hướng "đồng ý" với chính suy luận đã sinh ra code đó (thiên lệch xác nhận). Review agent hiệu quả cần context độc lập, tiêu chí đánh giá tách biệt, và lý tưởng là một model call riêng không thấy được lý do coding agent đã đưa ra quyết định, chỉ thấy được kết quả cuối để đánh giá khách quan.

---

## 12. Skill Layers: reusable procedures cho agent

Skill layer là tập hợp các procedure có thể tái sử dụng, giúp agent làm việc nhất quán thay vì tự phát minh lại quy trình mỗi lần.

Phân biệt rõ ràng:

```text
Tool: GitHub API
Skill: Cách review một pull request an toàn
       (đọc diff → kiểm tra breaking change →
        chạy test → kiểm tra convention →
        viết comment có cấu trúc → không tự merge)
```

**Tool là capability** - khả năng kỹ thuật để làm một việc (gọi API, đọc file, chạy code). **Skill là procedure** - quy trình đã được kiểm chứng để dùng capability đó đúng cách, đúng thứ tự, đúng tiêu chuẩn, với các bước kiểm tra đã biết trước.

### Ví dụ skill thực tế

Cách đọc và trích xuất nội dung từ PDF theo đúng thứ tự (kiểm tra loại tài liệu trước khi chọn phương pháp trích xuất). Cách tạo slide theo chuẩn định dạng nhất quán. Cách viết code review theo checklist cố định. Cách deploy app qua các bước staging → smoke test → production. Cách gọi API nội bộ với đúng authentication flow. Cách kiểm tra security cho một đoạn code trước khi merge. Cách tạo report theo template chuẩn của tổ chức. Cách chẩn đoán và sửa lỗi build theo thứ tự ưu tiên nguyên nhân phổ biến nhất trước.

### Vì sao skill layer giảm hallucination

- Agent không phải **tự phát minh quy trình** mỗi lần gặp một loại task - giảm biến thiên và giảm khả năng bỏ sót bước quan trọng.
- Có **checklist rõ ràng** để đối chiếu trước khi coi task hoàn thành.
- Có **tiêu chuẩn output** đã định nghĩa sẵn, giảm khả năng model tự bịa ra định dạng.
- Có **known failure modes** đã được ghi lại từ kinh nghiệm trước, giúp agent (hoặc verifier) nhận diện lỗi quen thuộc nhanh hơn.
- Có **verification steps** gắn liền với từng skill, không phải verification chung chung áp dụng cho mọi loại task.

Về mặt kiến trúc, skill layer thường được triển khai dưới dạng tài liệu có cấu trúc (procedure, ví dụ, ràng buộc, bước kiểm tra) mà agent load vào context khi phát hiện task khớp với loại skill đó - tương tự một thư viện runbook mà kỹ sư vận hành hệ thống vẫn dùng, chỉ khác là được viết để một model đọc và thực thi theo.

---

## 13. Verification: làm cho agent có thể tin được

Verification là lớp kiểm tra output và hành động của agent trước khi được coi là hoàn thành - đây là primitive quyết định agent có "đáng tin" hay chỉ "nghe có vẻ đúng".

### Các kiểu verification

Schema validation, unit test, type check, lint, fact checking, source citation check, constraint check, security check, regression test, human approval, tool-based verification, cross-agent review.

### Nguyên tắc thiết kế

- **Không nên để cùng một model vừa tạo vừa tự xác nhận hoàn toàn.** Model có xu hướng nhất quán với suy luận nó vừa đưa ra (thiên lệch xác nhận nội tại), nên tự đánh giá của chính nó có độ tin cậy thấp hơn một kiểm tra độc lập.
- **Dùng external verifier khi có thể** - công cụ hoặc quy trình tách biệt khỏi quá trình sinh ra output.
- **Dùng deterministic check cho những gì có thể kiểm tra bằng máy** - schema, type, test case, lint rule. Đây là verification rẻ, nhanh, và không có sai số.
- **Dùng human review cho việc rủi ro cao** - quyết định tài chính, pháp lý, hoặc hành động không thể đảo ngược.

### Pipeline verification thực tế

```text
Coding agent writes patch
→ Test runner executes tests
→ Static analyzer checks code
→ Review agent inspects diff
→ Human approves deployment
```

Mỗi bước trong chuỗi trên là một "lưới lọc" độc lập. Patch có thể pass test runner nhưng fail static analyzer (ví dụ code đúng logic nhưng vi phạm convention bảo mật). Patch có thể pass cả hai nhưng vẫn bị review agent gắn cờ vì thay đổi ảnh hưởng đến một module nhạy cảm cần con người xem lại. Thiết kế nhiều lớp verification độc lập, thay vì một lớp duy nhất, là cách thực tế để giảm false negative (lỗi lọt qua) trong hệ thống production.

---

## 14. Observability: nhìn thấy agent đang làm gì

Observability là điều kiện bắt buộc để vận hành agent trong môi trường production - không phải tính năng tùy chọn.

### Thông tin cần log

User task, instruction version (đã dùng phiên bản policy nào), context đã sử dụng, tool calls, tool parameters, tool results, state transitions, errors, retries, verification result, final output, latency, cost, token usage.

### Metric cần theo dõi

Success rate, failure rate, hallucination rate (tỷ lệ output bị verification hoặc human đánh dấu sai), tool error rate, retry count, human escalation rate, average task duration, cost per task, context utilization (bao nhiêu phần trăm context được thực sự dùng đến trong reasoning), verification pass rate.

Không có observability, ba việc trở nên bất khả thi:

- **Không thể debug agent** - khi agent thất bại ở một task cụ thể, không có cách nào biết nó đã thấy context gì, gọi tool gì, tại sao ra quyết định đó.
- **Không thể cải thiện agent** - không đo được cải tiến nào (đổi instruction, đổi model, đổi retrieval strategy) thực sự làm giảm failure rate hay chỉ là cảm giác chủ quan.
- **Không thể tin agent trong môi trường enterprise** - tổ chức không thể đưa một hệ thống vào production nếu không trả lời được câu hỏi "khi nó sai, chúng ta biết được không, và biết trong bao lâu".

Về mặt thực hành, log của agent nên được coi ngang hàng với log của một distributed system truyền thống - có trace ID xuyên suốt một task, có thể join giữa log của orchestrator, tool, và execution environment để tái dựng toàn bộ hành trình của một request.

---

## 15. Harness Engineering Architecture tổng thể

```text
User
 ↓
Agent Gateway
 ↓
Instruction Layer
 ↓
Context Manager
 ↓
Planner / Orchestrator
 ↓
LLM Reasoning Engine
 ↓
Tool Router
 ↓
Execution Environment
 ↓
External Tools / APIs
 ↓
Observation Collector
 ↓
Durable State Store
 ↓
Verifier
 ↓
Final Response / Artifact
```

Phân tích từng layer - input, output, và failure mode nó chịu trách nhiệm xử lý:

| Layer                 | Input                    | Output                   | Failure mode xử lý                     |
| --------------------- | ------------------------ | ------------------------ | -------------------------------------- |
| Agent Gateway         | Request từ user/hệ thống | Task chuẩn hóa           | Request sai định dạng, thiếu auth      |
| Instruction Layer     | Task + policy config     | Instruction đã phân lớp  | Instruction conflict, thiếu ràng buộc  |
| Context Manager       | Instruction, task state  | Context block đã lọc/nén | Context tràn, noise, thông tin cũ      |
| Planner/Orchestrator  | Context, goal            | Kế hoạch bước tiếp theo  | Mất mục tiêu, không biết dừng khi nào  |
| LLM Reasoning Engine  | Instruction + context    | Action đề xuất           | Hallucination, sai suy luận            |
| Tool Router           | Action đề xuất           | Tool call hợp lệ         | Gọi sai tool, sai tham số              |
| Execution Environment | Tool call                | Kết quả thực thi         | Side effect nguy hiểm, tràn tài nguyên |
| External Tools/APIs   | Request thực thi         | Dữ liệu/kết quả thô      | Tool lỗi, timeout, dữ liệu độc hại     |
| Observation Collector | Kết quả thực thi         | Observation chuẩn hóa    | Kết quả quá dài, mơ hồ                 |
| Durable State Store   | Observation              | State đã cập nhật        | Mất state, xung đột ghi đồng thời      |
| Verifier              | State, constraint        | Pass/fail + lý do        | Lỗi lọt qua, false positive            |
| Final Response        | State đã verify          | Output cho user          | Trả kết quả chưa được xác minh         |

Kiến trúc này không bắt buộc mọi hệ thống phải triển khai đủ 12 layer ngay từ đầu. Nó là bản đồ đầy đủ để đối chiếu: khi một agent gặp lỗi trong production, câu hỏi đầu tiên nên là "lỗi này thuộc layer nào, và layer đó có tồn tại trong hệ thống của mình không" - thay vì mặc định đổ lỗi cho model.

---

## 16. Từ demo agent đến production agent

### Demo Agent

Một prompt dài đóng vai trò toàn bộ "não" của agent. Một vài tool đơn giản, thường không có validation chặt. Không có durable state - mọi thứ sống trong context của một phiên chạy. Không có audit - không ai biết agent đã làm gì sau khi phiên kết thúc. Không có verification - output được coi là đúng nếu "nghe hợp lý". Không có permission rõ ràng - agent thường có quyền truy cập rộng hơn cần thiết vì việc giới hạn quyền tốn công thiết kế. Kết quả: hoạt động tốt trong video demo (dữ liệu sạch, kịch bản đã được thử trước), nhưng dễ lỗi ngay khi gặp dữ liệu thật, input bất ngờ, hoặc chạy trong thời gian dài không giám sát.

### Production Agent

Instruction hierarchy rõ ràng, phân lớp theo phần 4. Context retrieval có kiểm soát, không nhồi toàn bộ dữ liệu vào prompt. Tool interface có typed schema, validate input/output. Execution sandbox cô lập hành động khỏi hệ thống thật. Durable state cho phép resume, audit, rollback. Orchestration điều phối nhiều bước, nhiều agent một cách tường minh. Skill layer chuẩn hóa quy trình cho từng loại task lặp lại. Verification là lớp bắt buộc trước khi trả kết quả. Observability đầy đủ cho mọi bước. Human-in-the-loop tại các điểm rủi ro cao. Permission và security boundary được thiết kế theo nguyên tắc least privilege cho từng agent/tool.

Khoảng cách giữa hai cấp độ này không phải là "thêm một vài tính năng" - đó là khoảng cách giữa một prototype chứng minh ý tưởng khả thi, và một hệ thống có thể vận hành với dữ liệu thật, người dùng thật, và hậu quả thật khi sai sót xảy ra.

---

## 17. Kết luận

Harness engineering là bước tiến hóa tất yếu của AI engineering, khi trọng tâm chuyển từ "làm sao để model trả lời hay hơn" sang "làm sao để một hệ thống dùng model đó hoạt động đáng tin cậy trong thực tế".

Vài điểm cốt lõi cần giữ lại:

- **Model intelligence là điều kiện cần, không phải điều kiện đủ.** Một model mạnh hơn không tự động tạo ra một agent đáng tin cậy hơn nếu hệ thống bao quanh nó vẫn thiếu context management, tool interface chặt chẽ, và verification độc lập.
- **Agent reliability đến từ system design, không đến từ một prompt hay.** Độ tin cậy là thuộc tính của kiến trúc - instruction, context, tool, execution, state, orchestration, verification, observability - cộng lại, không phải thuộc tính của riêng model.
- **Harness là nơi reliability được encode.** Mọi ràng buộc về an toàn, mọi cơ chế kiểm tra, mọi khả năng phục hồi sau lỗi đều nằm ở lớp hệ thống này, không nằm trong trọng số của model.
- **Một agentic system tốt là sự kết hợp của tám thành phần chính**: model, context, tools, runtime, state, orchestration, verification, và observability - thiếu bất kỳ thành phần nào, hệ thống trở nên giòn (brittle) ở một điểm nào đó, chỉ là chưa lộ ra trong lúc demo.

Về lâu dài, giá trị kỹ thuật sẽ dịch chuyển: người biết viết prompt hay vẫn có giá trị, nhưng người biết **thiết kế harness** - xây được lớp hệ thống biến một reasoning engine xác suất thành một agent có thể giao việc thật, giám sát được, kiểm chứng được, và mở rộng được - mới là người quyết định một sản phẩm AI có sống được ngoài phòng demo hay không.