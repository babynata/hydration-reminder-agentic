---
name: hydration-reminder-agentic
description: Create and manage fully customized, interactive water/hydration drinking reminder systems for any AI agent platform. Use when the user says "喝水提醒", "设置喝水", "water reminder", "帮我提醒喝水", "hydration reminder", or wants to create/modify/stop water intake tracking. The skill first consults the user about their drinking background, goals, conditions, and frequency, then designs a personalized daily hydration plan, and runs a daily feedback loop: remind → ask for intake feedback → dynamically adjust remaining reminders → bedtime summary. Supports any target amount, any schedule, any messaging channel available to the agent. Make sure to use this skill whenever the user mentions water, drinking, hydration, 喝水, 肌酸, creatine, or wants to set up any kind of daily health/reminder routine involving fluid intake.
---

# Hydration Reminder — Agent-Agnostic Edition

## Overview
A fully personalized water-drinking reminder system that works on **any AI agent platform**. Not a one-size-fits-all plan. The skill first interviews the user, then designs a custom daily hydration schedule, deploys recurring reminders through whatever scheduling mechanism the host platform supports, and runs an interactive feedback loop every day.

## Trigger Conditions
Activate when user says any of:
- 喝水提醒, 设置喝水, water reminder, hydration reminder
- 帮我提醒喝水, 定制喝水计划, 个性化喝水
- 修改喝水, 更新喝水, 调整提醒
- 停止喝水提醒, 关掉喝水, 暂停喝水
- 查看喝水进度, 今天喝了多少, 喝水总结

## Philosophy
The user must **feel** how much they drink each day. Every message shows:
- **Today's total so far**
- **Progress toward daily goal**
- **Visual progress bar**
- **How much remains**

Tone should be **warm, supportive, and encouraging** — like a friendly health coach, not a rigid drill sergeant. Celebrate progress. Never shame.

## Platform Capabilities & Honest Limitations

**Always be transparent about what the AI can and cannot do:**

- Most AI agents **cannot proactively push notifications** to the user when they are not actively chatting. Do not promise push reminders unless the platform genuinely supports them.
- When proactive push is **not available**, the assistant must:
  1. Acknowledge this limitation honestly and early.
  2. Offer to create a **structured plan** the user can import into their own tools.
  3. Suggest practical fallback options: phone alarms, calendar events, smart watch reminders, hydration apps (WaterMinder, Hydro Coach), or browser extensions.
  4. In **Claude Code specifically**, mention that `/loop` or `CronCreate` can be used to set recurring check-ins within the terminal session.
- When proactive scheduling **is available** (e.g., Claude Code's `CronCreate`, platform cron jobs, scheduled tasks), use it and explain exactly how it works.

## Workflow

### Phase 1: Consultation (Key Node — Always Ask At Least One Follow-Up)
Before creating anything, interview the user. Present questions clearly.

**Even if the user provides context upfront** (e.g., "我健身，目标2L"), you must still ask at least 1-2 follow-up questions to fill gaps. Do not jump straight to a plan preview without any follow-up.

**Required questions (ask at least the ones not already answered):**
1. **喝水背景** (Background)
   - "你平时喝水习惯怎么样？容易忘记还是不喜欢喝？"
   - "有没有特殊情况？比如吃肌酸、健身、服药、孕期、医嘱限水？"
2. **喝水目标** (Goal)
   - "你每天想喝多少水？（单位：ml 或 杯）"
   - "有没有短期目标？比如改善皮肤、减脂、配合运动？"
3. **喝水条件** (Conditions)
   - "你用什么杯子？有刻度/吸管/大容量杯吗？"
   - "主要在什么场景喝水？家里、办公室、外出？"
   - "有没有固定无法喝水的时间？比如开会、通勤、午睡？"
4. **喝水频率/提醒偏好** (Frequency & Channel)
   - "你希望每天提醒几次？"
   - "提醒时间偏好？早上、工作时、饭后、睡前？"
   - "通过什么方式接收提醒？（用当前对话渠道，或其他可用渠道）"

**Consultation output:**
Compile into a brief "喝水档案" and present to user for confirmation:

```
你的定制喝水计划预览：
━━━━━━━━━━━━━━━━━━━━━━
🎯 日目标：{X}ml（约{X/250}杯）
⏰ 提醒时间：{时间列表}
📱 推送渠道：{当前对话渠道 + 其他可用渠道}
📝 特殊备注：{肌酸/健身/医嘱/孕期等}
━━━━━━━━━━━━━━━━━━━━━━
确认后生效，明天开始推送。
```

### Phase 2: Plan Design
Based on consultation, design the schedule.

**Default design principles (5-6 reminders per day):**
- Morning (09:00): Start the day, 1 larger drink
- Noon (12:00): **Checkpoint — ask for morning feedback**
- Afternoon (13:00): Post-lunch refill
- Evening (18:00): **Checkpoint — ask for afternoon feedback**
- Night (21:00): Final push toward goal
- Bedtime (22:30): **Summary — today's total + tomorrow preview**

**Adjust based on user conditions:**
- Office worker → reminders aligned with work breaks (e.g., 09:00/11:00/14:00/16:00/18:00)
- Gym days → extra reminder pre/post workout, ask for training time
- Creatine user → 17:00 creatine + water reminder (ask if they prefer training pre/post instead)
- Hates plain water → suggest infusion, tea, lemon water; explicitly state these count
- Small cup → more frequent, smaller amounts per reminder
- Always forgets → aggressive morning push (2-3 reminders before noon), evening catch-up
- Pregnancy/limited water → smaller amounts, gentler tone, earlier bedtime summary, always include medical disclaimer

**Total reminders should generally not exceed 7 per day** unless the user explicitly requests high frequency.

### Phase 3: Deploy Recurring Reminders
Use the host platform's scheduling mechanism to set up daily recurring reminders at the planned times. The exact mechanism depends on what the agent platform supports:

- **Cron jobs** (if platform supports, e.g., Claude Code `CronCreate`)
- **Scheduled tasks / timers** (if platform supports)
- **Calendar reminders** (if platform integrates with calendars)
- **Manual recurring prompts** (fallback: set repeating alarms or check-ins)

**Always explain the deployment method to the user.** If using Claude Code's `CronCreate`, show the cron expression. If using `/loop`, show the interval. If the platform cannot schedule, provide a copy-pasteable schedule for the user's phone/calendar.

Store the schedule configuration in a persistent file so the agent remembers the plan across sessions.

### Phase 4: Daily Interactive Loop

**Every reminder message must include:**
```
🥤 喝水提醒
━━━━━━━━━━━━━━━━━━━━━━
███░░░░░░░ 500ml / 1500ml (33%)

⏰ 本次：喝 {amount}ml
🎯 还剩：{remaining}ml
━━━━━━━━━━━━━━━━━━━━━━

喝完后回复我数字，我帮你记录。
```

**Checkpoint reminders (12:00, 18:00):**
```
⏰ 喝水检查
━━━━━━━━━━━━━━━━━━━━━━
███░░░░░░░ 500ml / 1500ml (33%)

上午/下午的目标 {target}ml 喝了多少？
请回复具体数字（如：300），我调整后面的提醒。
```

**When user replies with a number:**
- Record the amount
- Update today's total
- Recalculate remaining
- Adjust future reminder messages dynamically
- If actual < planned, acknowledge the gap warmly and say future reminders will be increased to compensate
- If actual >= planned, celebrate!

**Bedtime summary (22:30, or earlier if user prefers):**
```
🌙 今日喝水总结
━━━━━━━━━━━━━━━━━━━━━━
████████░░ 1300ml / 1500ml (86%)

✅ 达成率：86%
📊 分段记录：上午500 | 下午400 | 晚间300 | 睡前100
📝 明日建议：上午多喝100ml，争取100%达标
━━━━━━━━━━━━━━━━━━━━━━

明天继续当前计划？回复"继续"或"调整"。
```

### Phase 5: Next-Day Kickoff
If user replies "继续" → reset daily state, same plan continues
If user replies "调整" → return to Phase 1 consultation

## State Management

Maintain a daily state record. The exact storage mechanism depends on the platform (file, database, memory), but the data structure should be:

```json
{
  "date": "YYYY-MM-DD",
  "target": 1500,
  "total": 1300,
  "records": {
    "09:00": { "planned": 500, "actual": 500 },
    "12:00": { "planned": 0, "actual": 0, "feedback_requested": true },
    "13:00": { "planned": 500, "actual": 400 },
    "18:00": { "planned": 0, "actual": 0, "feedback_requested": true },
    "21:00": { "planned": 300, "actual": 300 },
    "22:30": { "planned": 100, "actual": 100 }
  }
}
```

## Progress Bar Format
```
██████░░░░ 900ml / 1500ml (60%)
```
- 10 characters total
- Filled = completed percentage (rounded to nearest 10%)
- Empty = remaining percentage

## Message Design Rules
1. **Every message shows total + progress bar + remaining**
2. **Checkpoint messages explicitly ask for feedback**
3. **Use simple numbers, no decimals**
4. **Friendly, warm, and encouraging. Celebrate progress. Never shame.**
5. **Bedtime summary is mandatory, not optional**
6. **Always include contextual advice for special conditions** (pregnancy tips, creatine hydration science, office ergonomics, etc.)

## Edge Cases

- **User replies non-numeric at checkpoint:** ask again gently with an example
- **No reply before next reminder:** assume default phase amount, continue tracking
- **User says "今天不喝了" / "skip today":**
  1. Show current progress bar and total
  2. Set remaining to 0 for today
  3. Send warm acknowledgement: "收到，休息一天 💤 今天已喝 {total}ml，明天继续加油！"
  4. Resume normal schedule tomorrow
- **State missing:** auto-init with default target 1500ml, explain to user
- **User changes goal mid-day:** update target, inform user with new progress bar, continue tracking from current total
- **Messaging channel unavailable:** log error, retry on next scheduled time, inform user of the issue
- **User asks to stop / 停止 / 关掉 / 暂停:**
  1. Confirm stopping immediately
  2. Show a brief summary of the paused plan (target, times)
  3. Clearly state: "随时回复'开启喝水提醒'即可恢复，或回复'调整'重新定制"
  4. Do NOT start a new consultation
- **User asks to modify / 修改 / 更新 / 调整:**
  1. Acknowledge the existing plan
  2. Apply requested changes
  3. If removing reminders, explicitly mention how the remaining water target is redistributed across the remaining times
  4. Present updated preview and ask for confirmation

## Example Consultation Flow

**User**: "帮我设置喝水提醒"

**Assistant**:
```
好，先了解一下你的情况：

1. 你平时喝水习惯怎么样？容易忘还是不喜欢喝？
2. 每天想喝多少？（一般建议1500-2000ml）
3. 有没有特殊情况？比如健身、吃肌酸、服药？
4. 希望什么时间提醒？（默认：09:00/12:00/13:00/18:00/21:00/22:30）

回复任意一条，我们边聊边定。
```

**User**: "我健身，吃肌酸，目标2L"

**Assistant**:
```
收到！健身+肌酸确实需要充足水分。再确认两个小问题：

- 你一般几点训练？我可以把肌酸提醒对齐到训练前或训练后。
- 主要在健身房还是家里练？

基于你说的，先给你一个预览：
━━━━━━━━━━━━━━━━━━━━━━
🎯 日目标：2000ml（约8杯）
⏰ 提醒：09:00/12:00/13:00/17:00(肌酸)/18:00/21:00/22:30
📝 备注：健身+肌酸，17:00肌酸+300ml水
━━━━━━━━━━━━━━━━━━━━━━

确认后明天生效。回复"确认"。
```

**User**: "确认"

**Assistant**: Deploys recurring reminders using platform's scheduling mechanism (e.g., cron, /loop, or manual schedule), explains the method to the user, and confirms next check-in time.
