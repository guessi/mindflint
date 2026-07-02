<div align="center">

<img src="icon.jpg" width="280" alt="Mindflint logo" />

# mindflint

*mind + flint — A flint doesn't light things up, it sparks them.*  
*The idea is to strike the mind, not fill it.*

**[English](#english) · [中文](#中文)**

</div>

---

<a id="english"></a>

## English

A Claude Code and Codex skill suite based on **Socratic guided learning** — prevents fake learning in the AI era.

> Fake learning: you read the AI's answer, felt like you got it, but didn't.

If you already use [Claude Code](https://claude.com/product/claude-code) or [Codex](https://developers.openai.com/codex) as your AI coding assistant, Mindflint adds a guided-learning mode on top of it — it doesn't replace or require anything else.

### Core Principle

Never give the answer directly. Set context first, then guide thinking, then verify.

```text
Question → Landscape (2-4 sentences) → Guiding question → Guide derivation → Verify → Key takeaways + next step
```

When a concept is complete, the guide summarizes key points and prompts the user to run `/mindflint-check` or `/mindflint-review`.

### Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| `mindflint` | `/mindflint` | Main mode: Socratic guidance, no direct answers |
| `mindflint-check` | `/mindflint-check` | Quiz: 3 questions (concept/application/transfer) to find real breaks |
| `mindflint-review` | `/mindflint-review` | Review: scan the conversation, compile weakness list |

All skills respond in the language the user writes in (Chinese or English).

### Installation

Requires [Claude Code](https://claude.com/product/claude-code) or [Codex](https://developers.openai.com/codex) already installed and set up on your machine. Run the commands below in your terminal (not inside a chat session).

#### Claude Code

**From GitHub:**

```bash
claude plugin marketplace add chungFTF/mindflint
claude plugin install mindflint
```

**From local path:**

```bash
claude plugin marketplace add ~/path/to/mindflint
claude plugin install mindflint
```

Restart Claude Code after install. Verify with:

```bash
claude plugin list
```

#### Codex

Mindflint can also be installed as a Codex plugin without cloning this repository.
The Codex plugin id is `mindflint`.

**From GitHub:**

```bash
codex plugin marketplace add chungFTF/mindflint --ref main
codex
```

For stable releases, prefer a version tag if one is available for the release you want:

```bash
codex plugin marketplace add chungFTF/mindflint --ref v0.2.0
```

**From local path for development:**

```bash
codex plugin marketplace add ~/path/to/mindflint
```

Then open the plugin directory, select the `Mindflint Plugins` marketplace and install Mindflint. This works the same way for both the GitHub and local path installs above.

To refresh a GitHub-installed marketplace later:

```bash
codex plugin marketplace upgrade mindflint
```

### Uninstall

#### Claude Code

```bash
claude plugin uninstall mindflint
```

#### Codex

Open the plugin directory, select the `Mindflint Plugins` marketplace, and uninstall Mindflint.

### Usage

The BigQuery/AWS examples below are just illustrations — swap in any topic you're trying to learn.

#### Claude Code

##### 1. Start learning

```text
/mindflint difference between BigQuery partition and cluster
```

Claude sets context first, then asks for your thinking — no direct explanation.

##### 2. Quiz yourself

```text
/mindflint-check
```

Three questions, one at a time. If you don't specify a topic and none is clear from the conversation, the skill asks which topic to quiz first. Weakness list after all three.

##### 3. Review weak spots

```text
/mindflint-review
```

Scans the conversation, lists breaks and concrete reinforcement suggestions. If the conversation doesn't have enough material yet, it tells you to run `/mindflint` or `/mindflint-check` first.

##### 4. Exit guided mode

```text
stop learn / normal mode
```

#### Codex

First invoke the installed plugin:

```text
@Mindflint
```

Then select a bundled skill:

```text
$mindflint
$mindflint-check
$mindflint-review
```

Examples:

```text
@Mindflint $mindflint difference between BigQuery partition and cluster
@Mindflint $mindflint-check quiz me on BigQuery partition vs cluster
@Mindflint $mindflint-review review my weak spots from this conversation
```

If skill autocomplete is unavailable, reference the skill path explicitly:

```text
Use skills/mindflint to guide me through BigQuery partition vs cluster.
Use skills/mindflint-check to check my understanding.
Use skills/mindflint-review to review my weak spots from this conversation.
```

### File Structure

```text
mindflint/
├── .agents/plugins/marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
└── skills/
    ├── mindflint/SKILL.md
    ├── mindflint-check/SKILL.md
    └── mindflint-review/SKILL.md
```

### Roadmap

- [ ] `mindflint-progress` — cross-session understanding progress tracking (needs persistent storage)
- [ ] Hooks — auto-detect when user skips thinking and asks for the answer directly

### Example Conversation

```text
User: I want to understand how GCP Load Balancer differs from AWS Load Balancer.

Guide: Before we compare, let me set the stage. AWS Load Balancer is a family of
       three products — ALB, NLB, and CLB — each operating at a different layer of
       the network stack. The layer it operates at determines what it can "see" and
       what routing decisions it can make.

       Given that, what do you think determines which layer a load balancer operates at?

User: I know AWS Load Balancer connects to target groups, has health checks, and can
      scale by adding instances based on different rules.

Guide: You've got the operational skeleton — target groups, health checks, scaling.
       That's the "what it does." Now let's get at the "how it sees traffic."

       A load balancer routing HTTP requests based on /api/user path needs to read
       something specific. What does it need to be able to read that a load balancer
       handling raw TCP connections doesn't?

User: A needs to understand paths so I'm guessing ALB (L7), B hits by port so maybe NLB?

Guide: Exactly right — you derived it yourself.
       - ALB = L7 (application layer) → understands HTTP path, headers, host
       - NLB = L4 (transport layer) → only sees IP + port, fast but "illiterate"

── Concept complete ──

Key takeaways
• ALB operates at L7 (HTTP/HTTPS) — can route by path, host, headers
• NLB operates at L4 (TCP/UDP) — sees only IP + port, lower latency
• Choose by what the load balancer needs to "read" to make routing decisions

Keep thinking:
1. When would you pick NLB over ALB even for an HTTP service?
2. What happens to WebSocket connections on an ALB vs NLB?
3. CLB is deprecated — what does that tell you about the direction AWS is going?

---
Ready for the next concept, or want to check that you really got this one?
→ /mindflint-check — three questions to test your understanding
→ /mindflint-review — surface your weak spots from this session
```

---

<a id="中文"></a>

## 中文

> 燧石之用，不在照明，在於引燃。  
> 凡代人思者，予以燭而奪其燧。

Claude Code 與 Codex skill 套件，基於**學思達教學法**，逼出真正的思考。

> AI 給的答案，讀完像是懂了。  
> 那不是懂，是借了別人的火把走路——一熄，就迷路。

如果你已經在用 [Claude Code](https://claude.com/product/claude-code) 或 [Codex](https://developers.openai.com/codex) 當你的 AI 開發助手，Mindflint 是加在上面的引導學習模式——不取代、也不需要額外安裝其他東西。

### 核心理念

不直接給答案。先鋪脈絡，再引導思考，最後驗證。

```text
問題 → 鋪景（2-4 句脈絡）→ 引導問題 → 引導推導 → 驗證理解 → 重點整理 + 下一步提示
```

每個概念結束時，引導者會整理重點，並提示用戶使用 `/mindflint-check` 測驗或 `/mindflint-review` 回顧弱點。

### Skills

| Skill | 指令 | 用途 |
|-------|------|------|
| `mindflint` | `/mindflint` | 主模式：蘇格拉底引導，不直接作答 |
| `mindflint-check` | `/mindflint-check` | 測驗：3 題（概念/應用/遷移）找出真正的斷點 |
| `mindflint-review` | `/mindflint-review` | 回顧：掃描對話，整理弱點清單 |

所有 skill 會自動配合用戶的語言（中文或英文）回應。

### 安裝

需要先安裝好 [Claude Code](https://claude.com/product/claude-code) 或 [Codex](https://developers.openai.com/codex) 其中一個並完成設定。下面的指令都是在終端機（terminal）執行，不是在對話框裡輸入。

#### Claude Code

**從 GitHub 安裝：**

```bash
claude plugin marketplace add chungFTF/mindflint
claude plugin install mindflint
```

**從本機路徑安裝：**

```bash
claude plugin marketplace add ~/path/to/mindflint
claude plugin install mindflint
```

安裝後重啟 Claude Code，確認安裝成功：

```bash
claude plugin list
```

#### Codex

Mindflint 也可以作為 Codex plugin 使用，不需要先 clone 這個 repo。Codex plugin id 是 `mindflint`。

**從 GitHub 安裝：**

```bash
codex plugin marketplace add chungFTF/mindflint --ref main
codex
```

正式 release 後，建議改用版本 tag（若該版本已標記 tag）：

```bash
codex plugin marketplace add chungFTF/mindflint --ref v0.2.0
```

**從本機路徑安裝，供開發測試使用：**

```bash
codex plugin marketplace add ~/path/to/mindflint
```

接著打開 plugin directory，選擇 `Mindflint Plugins` marketplace，安裝 Mindflint。上面兩種安裝方式（GitHub 或本機路徑）之後都走這個步驟。

之後若要更新 GitHub 安裝的 marketplace：

```bash
codex plugin marketplace upgrade mindflint
```

### 解除安裝

#### Claude Code

```bash
claude plugin uninstall mindflint
```

#### Codex

打開 plugin directory，選擇 `Mindflint Plugins` marketplace，解除安裝 Mindflint。

### 使用流程

以下的 BigQuery／AWS 只是示範主題，實際使用時換成你想學的任何主題都可以。

#### Claude Code

##### 1. 開始學習

```text
/mindflint BigQuery 的 partition 和 cluster 差異
```

Claude 會先鋪脈絡、再問你的理解，不會直接解釋。

##### 2. 測驗理解

```text
/mindflint-check
```

出三題，一次一題，等你回答才繼續。如果沒有指定主題、對話中也沒有明確主題，會先反問你要測驗哪個主題。結束後給弱點清單。

##### 3. 回顧弱點

```text
/mindflint-review
```

掃描本次對話，列出斷點和具體補強建議。如果對話還不夠，會提示先用 `/mindflint` 或 `/mindflint-check` 建立資料。

##### 4. 關閉引導模式

```text
stop learn / 正常模式
```

#### Codex

先載入已安裝的 plugin：

```text
@Mindflint
```

再選擇 bundled skill：

```text
$mindflint
$mindflint-check
$mindflint-review
```

範例：

```text
@Mindflint $mindflint BigQuery 的 partition 和 cluster 差異
@Mindflint $mindflint-check 測驗我 BigQuery partition vs cluster
@Mindflint $mindflint-review 回顧這段對話中我的弱點
```

如果某個 Codex CLI 版本沒有出現 skill autocomplete，也可以用 skill path 明確指定：

```text
Use skills/mindflint to guide me through BigQuery partition 和 cluster 的差異。
Use skills/mindflint-check to check my understanding.
Use skills/mindflint-review to review my weak spots from this conversation.
```

### 檔案結構

```text
mindflint/
├── .agents/plugins/marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
└── skills/
    ├── mindflint/SKILL.md
    ├── mindflint-check/SKILL.md
    └── mindflint-review/SKILL.md
```

### Roadmap

- [ ] `mindflint-progress` — 跨對話的理解進度追蹤（需要 persistent storage）
- [ ] Hooks — 自動偵測用戶是否跳過思考直接要答案

### 對話示範

```text
用戶：我想知道 GCP load balancer 的概念對比於 AWS Load Balancer 有何不同

引導者：在比較之前，先鋪個背景。AWS Load Balancer 其實是三個產品家族——ALB、NLB、CLB，
        每個運作在網路堆疊的不同層。它運作的層決定了它能「看見」什麼，以及能做什麼路由決策。

        在這個背景下，你覺得決定 Load Balancer 運作在哪一層的關鍵因素是什麼？

用戶：我知道 AWS Load Balancer 可以接上 target group，會有 healthcheck，
      依照不同規則可以縮放、新增 instance

引導者：你掌握了操作骨架——target group、health check、縮放，這些是「它做什麼」。
        現在來看「它怎麼看流量」。

        一個 Load Balancer 要根據 /api/user 路徑轉到不同機器，它必須能讀取什麼？
        而一個處理原始 TCP 連線的 Load Balancer 又不需要讀什麼？

用戶：A 要懂路徑所以我猜是 ALB (L7)，B 是打到 port 所以我猜是 NLB？

引導者：完全正確，你自己推導出來了。
        - ALB = L7（應用層）→ 看得懂 HTTP path、header、host
        - NLB = L4（傳輸層）→ 只看 IP + port，速度快但「不識字」

── 這個概念走完了 ──

重點整理
• ALB 運作在 L7（HTTP/HTTPS）—— 可以根據 path、host、header 做路由
• NLB 運作在 L4（TCP/UDP）—— 只看 IP + port，延遲更低
• 選擇依據：Load Balancer 需要「讀懂」什麼才能做路由決策

帶走思考：
1. 什麼情況下，就算是 HTTP 服務也應該選 NLB？
2. WebSocket 連線在 ALB 和 NLB 上的行為有什麼差異？
3. CLB 已被棄用——這告訴你 AWS 的方向是什麼？

---
要繼續下一個概念，還是先確認一下你真的懂了？
→ /mindflint-check 出三道題測驗你
→ /mindflint-review 整理你這輪的弱點
```

<div align="center">

<img src="icon.jpg" width="280" alt="Mindflint logo" />

</div>
