<div align="center">

<img src="mindflint-logo.png" width="280" />

# mindflint

*mind + flint — A flint doesn't light things up, it sparks them.*  
*The idea is to strike the mind, not fill it.*

</div>

A Claude skill suite based on **Socratic guided learning** — prevents fake learning in the AI era.

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

## Usage

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

## File Structure

```
AILearningTools/
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

---

<div align="center">

<img src="mindflint-logo.png" width="280" />

</div>

## 中文版

> 燧石之用，不在照明，在於引燃。  
> 凡代人思者，予以燭而奪其燧。

Claude skill 套件，基於**學思達教學法**，逼出真正的思考。

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

## 使用流程

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

## 檔案結構

```
AILearningTools/
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
