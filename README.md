# Hydration Reminder Agentic

A fully personalized, agent-agnostic water-drinking reminder skill for AI assistants. Works on **Claude Code**, **Claude.ai**, **Cursor**, **GitHub Copilot Chat**, or any AI agent platform that supports skill instructions.

> Not a one-size-fits-all plan. It interviews you first, then designs a custom daily hydration schedule with interactive feedback loops, progress tracking, and adaptive reminders.

---

## Features

- **Personalized Consultation** — Asks about your drinking habits, goals, health conditions, cup size, work schedule, and reminder preferences before creating anything.
- **Smart Plan Design** — Adapts to office workers, gym-goers, creatine users, pregnant women, people who hate plain water, small-cup users, forgetful drinkers, etc.
- **Visual Progress Tracking** — Every message shows today's total, a progress bar, and remaining amount.
- **Daily Interactive Loop** — Remind → ask for intake feedback → dynamically adjust remaining reminders → bedtime summary.
- **Cross-Platform** — Works on Claude Code (`/loop`, `CronCreate`), Claude.ai, or any agent platform. Honestly discloses platform limitations and provides fallback options (phone alarms, calendar, WaterMinder, Hydro Coach).
- **Bilingual** — Full Chinese (喝水提醒) and English (water reminder) support.

---

## Quick Start

### For Claude Code Users

1. Clone or download this repo:
   ```bash
   git clone https://github.com/babynata/hydration-reminder-agentic.git
   ```

2. Copy the skill to your Claude Code skills directory:
   ```bash
   mkdir -p ~/.claude/skills/hydration-reminder-agentic
   cp hydration-reminder-agentic/SKILL.md ~/.claude/skills/hydration-reminder-agentic/
   ```

3. Restart Claude Code or type `/skills` to reload.

4. Say anything like:
   - "帮我设置喝水提醒"
   - "water reminder"
   - "我健身，吃肌酸，目标2升"

### For Claude.ai / Other Platforms

Since these platforms don't support external skills directly, copy the contents of [`SKILL.md`](./SKILL.md) into a new Project, or paste it as a system prompt. The skill instructions will guide the AI through the full workflow.

---

## How It Works

### Phase 1: Consultation
The assistant interviews you with 4 key questions (background, goal, conditions, frequency) before designing anything. Even if you provide context upfront, it asks 1-2 follow-ups to fill gaps.

### Phase 2: Plan Design
Based on your answers, it designs a schedule like:

```
你的定制喝水计划预览：
━━━━━━━━━━━━━━━━━━━━━━
🎯 日目标：2000ml（约8杯）
⏰ 提醒时间：09:00 / 12:00 / 13:00 / 17:00(肌酸) / 18:00 / 21:00 / 22:30
📱 推送渠道：当前对话窗口
📝 特殊备注：健身+肌酸，17:00肌酸+300ml水
━━━━━━━━━━━━━━━━━━━━━━
```

### Phase 3: Deploy
On Claude Code, it can use `CronCreate` or `/loop` to schedule real recurring reminders. On other platforms, it provides a copy-pasteable schedule for your phone/calendar.

### Phase 4: Daily Loop
Every reminder includes:

```
🥤 喝水提醒
━━━━━━━━━━━━━━━━━━━━━━
██████░░░░ 900ml / 1500ml (60%)

⏰ 本次：喝 300ml
🎯 还剩：600ml
━━━━━━━━━━━━━━━━━━━━━━

喝完后回复我数字，我帮你记录。
```

### Phase 5: Summary
At bedtime (22:30), you get a full daily summary with achievement rate, segment records, and tomorrow's suggestion.

---

## Supported Scenarios

| Scenario | How It's Handled |
|----------|-----------------|
| Office worker | Reminders aligned with work breaks |
| Gym + creatine | Dedicated creatine+water reminder, asks for training time |
| Pregnancy / doctor's limit | Gentle tone, smaller amounts, medical disclaimer |
| Hates plain water | Tea, lemon water, infusions all count |
| Small cup (200ml) | More frequent reminders, exactly 1 cup each |
| Always forgets | Aggressive morning push (3 pre-noon reminders) |
| Modify plan mid-day | Adjust target, redistribute amounts, update preview |
| Skip today | Respects request, shows current progress, resumes tomorrow |
| Stop reminders | Confirms immediately, shows resume path |

---

## Trigger Phrases

Say any of these to activate:

- 喝水提醒, 设置喝水, 帮我提醒喝水, 定制喝水计划
- water reminder, hydration reminder, set up water reminder
- 修改喝水, 更新喝水, 调整提醒
- 停止喝水提醒, 关掉喝水, 暂停喝水
- 查看喝水进度, 今天喝了多少, 喝水总结

---

## Tech Spec

- **Skill format**: Standard Claude Code `SKILL.md` with YAML frontmatter
- **State storage**: Platform-dependent (file, memory, or database). JSON schema provided in skill.
- **Scheduling**: Platform abstraction — uses whatever the host supports (cron, timers, loops, or manual fallback).
- **Language**: Bilingual instructions; AI responds in the user's language.

---

## Contributing

This skill was built using the [skill-creator](https://github.com/anthropics/claude-code/tree/main/skill-creator) workflow with iterative eval-driven improvement. If you have ideas for new scenarios, better phrasing, or platform-specific adaptations, feel free to open an issue or PR.

---

## License

MIT
