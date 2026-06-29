<div align="center">

<img src="mindflint-logo.png" width="280" />

# mindflint

*mind + flint — A flint doesn't light things up, it sparks them.*  
*The idea is to strike the mind, not fill it.*

</div>

A Claude Code and Codex skill suite based on **Socratic guided learning** — prevents fake learning in the AI era.

> Fake learning: you read the AI's answer, felt like you got it, but didn't.

## Core Principle

Never give the answer directly. Ask first, guide, then verify.

```
Question → Ask for current understanding → Guide derivation → Verify → Leave a takeaway question
```

## Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| `mindflint-en` | `/mindflint-en` | Main mode: Socratic guidance, no direct answers |
| `mindflint-check-en` | `/mindflint-check-en` | Quiz: 3 questions (concept/application/transfer) to find real breaks |
| `mindflint-review-en` | `/mindflint-review-en` | Review: scan the conversation, compile weakness list |

## Installation

### Claude Code

**From GitHub:**
```bash
claude plugin marketplace add chungFTF/mindflint
claude plugin install ai-learning-tools
```

**From local path:**
```bash
claude plugin marketplace add ~/path/to/mindflint
claude plugin install ai-learning-tools
```

Restart Claude Code after install. Verify with:
```bash
claude plugin list
```

### Codex

Mindflint can also be installed as a Codex plugin without cloning this repository.
The Codex plugin id is `ai-learning-tools`, and the product/display name is
Mindflint.

**From GitHub:**
```bash
codex plugin marketplace add chungFTF/mindflint --ref main
codex
```

For stable releases, prefer a version tag once one is available:
```bash
codex plugin marketplace add chungFTF/mindflint --ref v0.2.0
```

**From local path for development:**
```bash
codex plugin marketplace add ~/path/to/mindflint
```

Then open the plugin directory:
```text
/plugins
```

Select the `Mindflint Plugins` marketplace and install Mindflint /
`ai-learning-tools`.

To refresh a GitHub-installed marketplace later:
```bash
codex plugin marketplace upgrade mindflint
```

## Usage

### Claude Code

**1. Start learning**
```
/mindflint-en difference between BigQuery partition and cluster
```
Claude asks for your current understanding first — no direct explanation.

**2. Quiz yourself**
```
/mindflint-check-en
```
Three questions, one at a time. Weakness list after all three.

**3. Review weak spots**
```
/mindflint-review-en
```
Scans the conversation, lists breaks and concrete reinforcement suggestions.

**4. Exit guided mode**
```
stop mindflint / normal mode
```

### Codex

Codex does not use the same slash-command model as Claude Code. First invoke the
installed plugin:

```text
@Ai-Learning-Tools
```

Then select a bundled skill:

```text
$mindflint
$mindflint-check
$mindflint-review
$mindflint-en
$mindflint-check-en
$mindflint-review-en
```

Examples:

```text
@Ai-Learning-Tools $mindflint-en difference between BigQuery partition and cluster
@Ai-Learning-Tools $mindflint-check-en quiz me on BigQuery partition vs cluster
@Ai-Learning-Tools $mindflint-review-en review my weak spots from this conversation
```

If skill autocomplete is unavailable in a Codex CLI version, you can still reference
the skill path explicitly:

```text
Use skills/mindflint-en to guide me through BigQuery partition vs cluster.
Use skills/mindflint-check-en to check my understanding.
Use skills/mindflint-review-en to review my weak spots from this conversation.
```

Note: Claude Code uses slash commands such as `/mindflint-en`. Codex uses plugin
invocation with `@` and bundled skill invocation with `$`.

## File Structure

```
AILearningTools/
├── .agents/plugins/marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
├── plugin.yaml
├── skills/
│   ├── mindflint/SKILL.md
│   ├── mindflint-check/SKILL.md
│   ├── mindflint-review/SKILL.md
│   ├── mindflint-en/SKILL.md
│   ├── mindflint-check-en/SKILL.md
│   └── mindflint-review-en/SKILL.md
└── commands/
    ├── mindflint.toml
    ├── mindflint-check.toml
    ├── mindflint-review.toml
    ├── mindflint-en.toml
    ├── mindflint-check-en.toml
    └── mindflint-review-en.toml
```

## Roadmap

- [ ] `mindflint-progress` — cross-session understanding progress tracking (needs persistent storage)
- [ ] Hooks — auto-detect when user skips thinking and asks for the answer directly

## Example Conversation

```
User: I want to understand how GCP Load Balancer differs from AWS Load Balancer.

Guide: Hold on — before we compare. What's your current understanding of AWS Load Balancer?

User: I know AWS Load Balancer connects to target groups, has health checks, and can scale by
      adding instances based on different rules.

Guide: You've got the core skeleton. Now go one step further — why do you think AWS needs three
       different Load Balancers (ALB/NLB/CLB)? They all "distribute traffic across machines,"
       so why not just use one?

       Hint: Think about HTTP requests, UDP packets, TCP database connections — do they all need
       the same kind of handling?

User: Because one machine would get overloaded with too many requests.

Guide: That's why Load Balancers exist — totally correct. But I'm asking why we need different
       kinds. Different angle: does a Load Balancer need to "understand" the same things when
       handling HTTP web requests vs. TCP database connections?

       Hint: If a Load Balancer needs to route based on the path /api/user, what does it need
       to be able to "read"?

User: A needs to understand paths so I'm guessing ALB (L7), B hits by port so maybe NLB?

Guide: Exactly right — you derived it yourself.
       - ALB = L7 (application layer) → understands HTTP path, headers, host
       - NLB = L4 (transport layer) → only sees IP + port, fast but "illiterate"

---

Guide: GCP also has the concept of "layers," but categorizes them differently. Think about this:
       what different requirements might a global service vs. a Taiwan-only service have for a
       Load Balancer?

       Hint: Traffic coming from the US vs. only within Taiwan — where do you want the Load
       Balancer to intercept it?

User: Cross-region?

Guide: Yes! GCP's second dimension is exactly Global vs. Regional.

       With AWS, you'd need Route 53 + a separate ALB per region for global routing. What do you
       think is the fundamental difference between GCP Global LB and DNS round-robin?

       Hint: DNS decides where to go before the connection; Global LB decides when the packet arrives.

User: GCP would be faster?

Guide: Right direction. But DNS has a fatal flaw — a Taiwan user gets routed to Tokyo, Tokyo goes
       down. What happens to the user?

User: (Not sure)

Guide: DNS has TTL caching. Tokyo is down, but your computer already cached "go to Tokyo" — until
       the TTL expires, it keeps hitting the dead server. GCP Global LB uses Anycast: one global IP,
       Google Edge decides in real time which region to send you to. No cache problem.
```

<div align="center">

<img src="mindflint-logo.png" width="280" />

</div>

## 中文版

> 燧石之用，不在照明，在於引燃。  
> 凡代人思者，予以燭而奪其燧。

Claude Code 與 Codex skill 套件，基於**學思達教學法**，逼出真正的思考。

> AI 給的答案，讀完像是懂了。  
> 那不是懂，是借了別人的火把走路——一熄，就迷路。

## 核心理念

不直接給答案。先問，再引導，最後驗證。

```
問題 → 反問現有理解 → 引導推導 → 驗證理解 → 留下思考題
```

## Skills

| Skill | 指令 | 用途 |
|-------|------|------|
| `mindflint` | `/mindflint` | 主模式：蘇格拉底引導，不直接作答 |
| `mindflint-check` | `/mindflint-check` | 測驗：3 題（概念/應用/遷移）找出真正的斷點 |
| `mindflint-review` | `/mindflint-review` | 回顧：掃描對話，整理弱點清單 |

## 安裝

### Claude Code

**從 GitHub 安裝：**
```bash
claude plugin marketplace add chungFTF/mindflint
claude plugin install ai-learning-tools
```

**從本機路徑安裝：**
```bash
claude plugin marketplace add ~/path/to/mindflint
claude plugin install ai-learning-tools
```

安裝後重啟 Claude Code，確認安裝成功：
```bash
claude plugin list
```

### Codex

Mindflint 也可以作為 Codex plugin 使用，而且使用者不需要先 clone 這個 repo。
Codex plugin id 是 `ai-learning-tools`，產品/顯示名稱是 Mindflint。

**從 GitHub 安裝：**
```bash
codex plugin marketplace add chungFTF/mindflint --ref main
codex
```

正式 release 後，建議改用版本 tag：
```bash
codex plugin marketplace add chungFTF/mindflint --ref v0.2.0
```

**從本機路徑安裝，供開發測試使用：**
```bash
codex plugin marketplace add ~/path/to/mindflint
```

接著打開 plugin directory：
```text
/plugins
```

選擇 `Mindflint Plugins` marketplace，安裝 Mindflint / `ai-learning-tools`。

之後若要更新 GitHub 安裝的 marketplace：
```bash
codex plugin marketplace upgrade mindflint
```

## 使用流程

### Claude Code

**1. 開始學習**
```
/mindflint BigQuery 的 partition 和 cluster 差異
```
Claude 會先問你目前的理解，不會直接解釋。

**2. 測驗理解**
```
/mindflint-check
```
出三題，一次一題，等你回答才繼續。結束後給弱點清單。

**3. 回顧弱點**
```
/mindflint-review
```
掃描本次對話，列出斷點和具體補強建議。

**4. 關閉引導模式**
```
stop mindflint / 正常模式
```

### Codex

Codex 不使用 Claude Code 那套 slash-command model。先載入已安裝的 plugin：

```text
@Ai-Learning-Tools
```

再選擇 bundled skill：

```text
$mindflint
$mindflint-check
$mindflint-review
$mindflint-en
$mindflint-check-en
$mindflint-review-en
```

範例：

```text
@Ai-Learning-Tools $mindflint BigQuery 的 partition 和 cluster 差異
@Ai-Learning-Tools $mindflint-check 測驗我 BigQuery partition vs cluster
@Ai-Learning-Tools $mindflint-review 回顧這段對話中我的弱點
```

如果某個 Codex CLI 版本沒有出現 skill autocomplete，也可以用 skill path 明確指定：

```text
Use skills/mindflint to guide me through BigQuery partition 和 cluster 的差異。
Use skills/mindflint-check to check my understanding.
Use skills/mindflint-review to review my weak spots from this conversation.
```

注意：Claude Code 使用 `/mindflint` 這類 slash commands；Codex 使用 `@`
載入 plugin，再用 `$` 選 bundled skill。

## 檔案結構

```
AILearningTools/
├── .agents/plugins/marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
├── plugin.yaml
├── skills/
│   ├── mindflint/SKILL.md
│   ├── mindflint-check/SKILL.md
│   ├── mindflint-review/SKILL.md
│   ├── mindflint-en/SKILL.md
│   ├── mindflint-check-en/SKILL.md
│   └── mindflint-review-en/SKILL.md
└── commands/
    ├── mindflint.toml
    ├── mindflint-check.toml
    ├── mindflint-review.toml
    ├── mindflint-en.toml
    ├── mindflint-check-en.toml
    └── mindflint-review-en.toml
```

## Roadmap

- [ ] `mindflint-progress` — 跨對話的理解進度追蹤（需要 persistent storage）
- [ ] Hooks — 自動偵測用戶是否跳過思考直接要答案

## 對話示範

```
用戶：我想知道 GCP load balancer 的概念對比於 AWS Load Balancer 有何不同

引導者：先別急著比較。你目前對 AWS Load Balancer 的理解是什麼？

用戶：我知道 AWS Load Balancer 可以接上 target group，會有 healthcheck，
      依照不同規則可以縮放、新增 instance

引導者：你掌握了核心骨架。現在往前一步——你覺得為什麼需要三種不同的 Load Balancer
        （ALB/NLB/CLB）？同樣都是「把流量分散到多台機器」，為什麼不用同一種就好？

        提示：想想 HTTP 請求、UDP 封包、TCP 資料庫連線——它們需要的處理方式一樣嗎？

用戶：因為一個機器太多請求會負載過重

引導者：你說的是 Load Balancer 存在的原因，完全正確。不過我問的是為什麼需要不同種類。
        換個角度：處理 HTTP 網頁請求跟處理 TCP 資料庫連線，Load Balancer 需要「懂」的東西一樣嗎？

        提示：如果 Load Balancer 要根據路徑 /api/user 轉到不同機器，它必須能「看懂」什麼？

用戶：A 要懂路徑所以我猜是 ALB (L7)，B 是打到 port 所以我猜是 NLB？

引導者：完全正確，你自己推導出來了。
        - ALB = L7（應用層）→ 看得懂 HTTP path、header、host
        - NLB = L4（傳輸層）→ 只看 IP + port，速度快但「不識字」

---

引導者：GCP 也有「層」的概念，但分類方式不太一樣。想想看：一個全球服務跟一個只在台灣的服務，
        Load Balancer 可能有什麼不同需求？

        提示：流量從美國打進來 vs 只在台灣內部，你希望 Load Balancer 在哪裡接住流量？

用戶：跨 region？

引導者：對！GCP 的第二個維度就是 Global vs Regional。

        AWS 要做全球路由要搭 Route 53 + 每個 region 各自一個 ALB。
        你覺得 GCP Global LB 跟 DNS 輪詢比起來，根本差異是什麼？

        提示：DNS 是在連線之前決定去哪裡，Global LB 是在封包進來當下決定。

用戶：GCP 會更快？

引導者：方向對。但 DNS 方案有個致命問題——台灣用戶被導到東京，東京掛了，用戶會發生什麼事？

用戶：（不知道）

引導者：DNS 有 TTL 快取。東京掛了，但你的電腦已經快取「去東京」這個答案，TTL 沒過前還是繼續
        打掛掉的地方。GCP Global LB 用 Anycast，全球單一 IP，Google Edge 即時決定轉到哪個
        region，沒有快取問題。
```
