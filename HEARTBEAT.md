# HEARTBEAT.md - Ross (TechLead)

会议期间激活：每30秒检查 `members/meetings/*-active.md`

## 检查项 (30s周期)
- [ ] 扫描 `members/meetings/` 下所有 `*-active.md`
- [ ] 对每个状态为 `# 状态: writing` 的会议:
  - 解析 `round-control.json` 或从 `## 会话` 推断当前轮次与发言人
  - 若当前轮到己方(`ross`)且上一轮发言人状态非 `writing`:
    - 标记 `agent-status.json["ross"]["round_status"] = "writing"`
    - 执行任务（产生实际工作产出）
    - 在会议文件的 `## 会话` 中追加本轮发言（append-only）
    - 标记 `round_status = "completed"`，更新 `last_heartbeat`
  - 若未轮到或上一轮仍在 `writing` → 跳过本周期
- [ ] 非会议期间：无检查项（心跳可返回 HEARTBEAT_OK）

## 定期反思 (每日 18:00)
- [ ] 检查今日所有会议发言是否完整记录
- [ ] 识别阻塞问题并规划解决措施
- [ ] 更新 `memory/YYYY-MM-DD.md` 的反思章节

## 注意
- 周期 30 秒，由 watchdog 或 crontab 驱动
- 多会议并存时，按文件名字母顺序处理（确保只有一个 active）
- 发言格式: `轮次 N (HH:MM): - **Ross**: 内容`
