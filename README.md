# Hydration Reminder Agentic

**[中文](#)** | **[English](#english)

---

## 这是什么

一个完全个性化、可跨平台使用的 AI 喝水提醒系统。它不会给你一张千篇一律的 8 杯水计划表，而是先访谈你的生活习惯，再量身定制每日 Hydration Schedule，并在对话中持续跟踪进度、动态调整。

> **一句话概括**：让 AI 当你的私人喝水教练，而不是一个只会响的闹钟。

## 解决什么问题

| 痛点 | 传统方案 | 本 Skill 的做法 |
|------|---------|----------------|
| 久坐一天只喝 2 杯水，完全想不起来 | 手机闹钟，响一次 dismiss 一次 | 先了解你的作息，把提醒对齐到工作/健身/休息的**自然间隙** |
| 健身吃肌酸，需要大量补水，但不知道什么时候喝 | 自己记，经常漏 | 在计划中**自动插入肌酸+水提醒**，对齐训练时间 |
| 讨厌白开水，看到提醒更不想喝 | 硬推白开水 | 直接告诉你**茶、柠檬水、气泡水都算数** |
| 孕期/医嘱限水，不敢乱喝 | 一刀切 8 杯 |  conservative 小杯量计划，带医疗免责声明 |
| 用小杯子（200ml），每次都喝不完 | 每次提醒 500ml | 改为**高频少量**，一次一杯刚好 |
| 想知道今天到底喝了多少 | 纯靠感觉 | 每次对话都带**进度条 + 今日总量 + 剩余目标** |

## 怎么使用

### Claude Code 用户（推荐）

```bash
# 1. 克隆仓库
git clone https://github.com/babynata/hydration-reminder-agentic.git

# 2. 安装 Skill
mkdir -p ~/.claude/skills/hydration-reminder-agentic
cp hydration-reminder-agentic/SKILL.md ~/.claude/skills/hydration-reminder-agentic/

# 3. 重启 Claude Code 或输入 /skills 重载
```

然后直接说：
- "帮我设置喝水提醒"
- "我健身吃肌酸，目标 2 升"
- "water reminder"

### Kimi / Claude.ai / ChatGPT / 其他平台用户

这些平台不支持外部 Skill，但**复制 `SKILL.md` 的内容作为系统提示词即可**完全复刻同样的体验：

1. 打开 [SKILL.md](./SKILL.md)，全选复制
2. 粘贴到对应平台的 "系统提示词 / System Prompt / Custom Instructions" 中
3. 新建对话，说"帮我设置喝水提醒"

> **进阶用法**：也可以把 `SKILL.md` 内容贴进一个长期对话，每次打开这个对话就相当于"开启喝水教练"。

## 工作流预览

### ① 咨询（Consultation）
AI 会先问你 4 个维度的问题，即使你已经提供了部分信息，也会追问 1-2 个细节补全：

```
1. 你平时喝水习惯怎么样？容易忘还是不喜欢喝？
2. 每天想喝多少？（一般建议 1500-2000ml）
3. 有没有特殊情况？比如健身、吃肌酸、服药、孕期？
4. 希望什么时间提醒？（默认：09:00/12:00/13:00/18:00/21:00/22:30）
```

### ② 计划预览（Plan Preview）
根据你的回答生成定制计划，确认后才生效：

```
你的定制喝水计划预览：
━━━━━━━━━━━━━━━━━━━━━━
🎯 日目标：2000ml（约8杯）
⏰ 提醒时间：09:00 / 12:00 / 13:00 / 17:00(肌酸) / 18:00 / 21:00 / 22:30
📱 推送渠道：当前对话窗口
📝 特殊备注：健身+肌酸，17:00肌酸+300ml水
━━━━━━━━━━━━━━━━━━━━━━
```

### ③ 每日互动循环（Daily Loop）
每次提醒都带实时进度：

```
🥤 喝水提醒
━━━━━━━━━━━━━━━━━━━━━━
██████░░░░ 900ml / 1500ml (60%)

⏰ 本次：喝 300ml
🎯 还剩：600ml
━━━━━━━━━━━━━━━━━━━━━━

喝完后回复我数字，我帮你记录。
```

### ④ 睡前总结（Summary）
```
🌙 今日喝水总结
━━━━━━━━━━━━━━━━━━━━━━
████████░░ 1300ml / 1500ml (86%)

✅ 达成率：86%
📊 分段记录：上午500 | 下午400 | 晚间300 | 睡前100
📝 明日建议：上午多喝100ml，争取100%达标
━━━━━━━━━━━━━━━━━━━━━━
```

## 支持的特殊场景

| 场景 | 处理方式 |
|------|---------|
| 上班族 | 对齐工作休息，午休前后不打扰 |
| 健身 + 肌酸 | 训练前后额外提醒，肌酸+水绑定 |
| 孕期 / 医嘱限水 | 温和小量，带医疗免责声明 |
| 讨厌白开水 | 茶、柠檬水、气泡水全部计入 |
| 小杯（200ml） | 高频少量，一次一杯 |
| 老是忘 | 上午密集推送（2-3次 pre-noon） |
| 中途改目标 | 即时更新，重新分配剩余提醒 |
| 今天不喝了 | 尊重请求，显示当前进度，明天继续 |
| 停止提醒 | 确认停止，展示暂停计划，随时可恢复 |

## 触发词

说任意以下短语即可激活：
- 喝水提醒、设置喝水、帮我提醒喝水、定制喝水计划
- water reminder、hydration reminder、set up water reminder
- 修改喝水、更新喝水、调整提醒
- 停止喝水提醒、关掉喝水、暂停喝水
- 查看喝水进度、今天喝了多少、喝水总结

---

## English

<details>
<summary>Click to expand English version</summary>

### What is this

A fully personalized, cross-platform AI hydration reminder system. Instead of giving you a generic "8 glasses a day" plan, it interviews you about your lifestyle first, then designs a custom daily hydration schedule and tracks your progress interactively.

> **In one sentence**: Let AI be your personal hydration coach, not just another alarm clock.

### Problem it solves

| Pain point | Traditional approach | This skill |
|-----------|---------------------|------------|
| Sit all day, forget to drink entirely | Phone alarm, dismissed once and forgotten | Aligns reminders with your **natural work/gym/rest rhythm** |
| Gym + creatine, need hydration timing | Self-managed, often missed | **Auto-inserts creatine+water reminder** aligned to training time |
| Hates plain water | Keeps pushing plain water | Explicitly counts **tea, lemon water, sparkling water** |
| Pregnancy / doctor's limit | One-size-fits-all 8 cups | Conservative small-cup plan with medical disclaimer |
| Small cup (200ml), can't finish 500ml | 500ml per reminder | **High-frequency small amounts**, one cup at a time |
| Want to know today's actual intake | Pure guesswork | Every message shows **progress bar + total + remaining** |

### How to use

**For Claude Code users (recommended):**

```bash
git clone https://github.com/babynata/hydration-reminder-agentic.git
mkdir -p ~/.claude/skills/hydration-reminder-agentic
cp hydration-reminder-agentic/SKILL.md ~/.claude/skills/hydration-reminder-agentic/
```

Restart Claude Code or type `/skills` to reload. Then say:
- "water reminder"
- "I work out and take creatine, target 2L"
- "帮我设置喝水提醒"

**For Kimi / Claude.ai / ChatGPT / other platforms:**

These platforms don't support external skills, but you can **copy the contents of `SKILL.md` as a system prompt** for the exact same experience:

1. Open [SKILL.md](./SKILL.md), select all, copy
2. Paste into "System Prompt / Custom Instructions" in your platform
3. Start a new chat and say "set up water reminder"

> **Pro tip**: You can also paste `SKILL.md` into a persistent conversation thread and return to it daily as your hydration coach.

### Workflow

**Phase 1: Consultation** — The AI asks about your background, goals, conditions, and frequency preferences. Even if you provide some context upfront, it asks 1-2 follow-ups to fill gaps.

**Phase 2: Plan Preview** — Generates a custom schedule. Only activates after your confirmation.

**Phase 3: Daily Interactive Loop** — Every reminder includes a real-time progress bar:

```
🥤 Water Reminder
━━━━━━━━━━━━━━━━━━━━━━
██████░░░░ 900ml / 1500ml (60%)

⏰ This time: Drink 300ml
🎯 Remaining: 600ml
━━━━━━━━━━━━━━━━━━━━━━

Reply with a number when done, I'll log it.
```

**Phase 4: Bedtime Summary** — Full daily recap with achievement rate, segment records, and tomorrow's suggestion.

### Supported scenarios

| Scenario | How it's handled |
|----------|-----------------|
| Office worker | Aligned with work breaks |
| Gym + creatine | Pre/post workout reminders, creatine+water paired |
| Pregnancy / doctor's limit | Gentle small amounts, medical disclaimer |
| Hates plain water | Tea, lemon water, sparkling water all count |
| Small cup (200ml) | More frequent, smaller amounts |
| Always forgets | Aggressive morning push (2-3 pre-noon reminders) |
| Modify mid-day | Update target, redistribute remaining reminders |
| Skip today | Respects request, shows current progress, resumes tomorrow |
| Stop reminders | Confirms, shows paused plan, resume anytime |

### Trigger phrases

Say any of these to activate:
- water reminder, hydration reminder, set up water reminder
- 喝水提醒, 设置喝水, 帮我提醒喝水, 定制喝水计划
- modify water, update reminder, adjust plan
- stop water reminder, pause reminder
- check progress, how much today, daily summary

</details>

---

## Tech Spec

- **Skill format**: Claude Code `SKILL.md` with YAML frontmatter
- **State storage**: Platform-dependent (file, memory, or conversation history)
- **Scheduling**: Platform abstraction — uses whatever the host supports (cron, timers, loops, or manual fallback)
- **Language**: Bilingual instructions; AI responds in the user's language

## License

MIT
