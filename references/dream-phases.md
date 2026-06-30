# Dream 6 阶段详解

## Phase 0 · 入睡校准（含架构 onboarding）

**0a. 动态定位项目目录**（绝不写死路径）
- 首选 `$CLAUDE_PROJECT_DIR`（CC 运行时注入），兜底 `cwd`
- 不假设 `projects/-Users-xxx/` 这类用户特有的路径编码（每用户不同）

**0b. 功能检测**（以功能为依据，非路径字符串）
- 查 `<项目目录>/memory/` 三层子目录 + 骨架文件（MEMORY.md/CONVENTIONS.md/ARCHITECTURE.md）是否存在
- ✅ 全在 → 跳 0d
- ❌ 缺失 → 先 0c onboarding

**0c. onboarding（缺失时）**
- `mkdir memory/{log,facts,lessons,projects,ideas,archive,transcripts}` + 各 `.gitkeep`
- 写骨架：`MEMORY.md`（空索引模板）/ `CONVENTIONS.md`（规范，参考 [`conventions.md`](conventions.md)）/ `ARCHITECTURE.md`（蓝图模板）
- CC 机制指引（**只提示，不改用户配置**）：CLAUDE.md 加 dream 触发词 + 配 SessionEnd hook 把会话 jsonl 提取到 `transcripts/` → 询问用户是否采纳

**0d. 已有架构**：读三总揽（MEMORY+CONVENTIONS+ARCHITECTURE）→ `ls` 各层计数 + 关键文件 shasum（回滚锚）→ 红线确认（原始层只读/禁 rm/stale 不删）→ 定范围（记忆/架构/回路/全量）

## Phase 1 · 慢波巩固（海马→皮层）

- 读近 N 天 `log/`（必要时 `transcripts/` 补细节）
- **写入判定**：犯错→`lessons` / 项目→`projects` / 已验证→`facts` / 未验证→`ideas`
- 重复 episodic 合并成 semantic 规律

## Phase 2 · REM 关联

- 跨条目找隐藏关联
- 补/修 wiki-link `[[name]]`
- 生成新 ideas（远距洞察）
- 找孤儿条目（无入链 / 无引用）

## Phase 3 · 突触稳态瘦身

- 知识层去重（grep 相似）、合并近似条目
- `MEMORY.md` 索引瘦身（<150 行，每条一行 hook）
- **索引 >120 行（接近上限）→ 建议分领域索引**（如 `MEMORY-<域>.md`，MEMORY.md 只留域指针）。提示用户，不擅自拆
- 过时条目标 `status: stale` + 指向新条目（**不删**）
- 检查规范被遵守情况，违反标 `[冲突]` 待裁决

## Phase 4 · 类淋巴冲洗（物理清理）

- 清磁盘垃圾：失效缓存 / 僵尸文件 / 旧 build / 孤儿产物
- ⛔ **不碰 `transcripts/`**（原始层红线）
- ⚠️ **不碰 `.transcript-progress/`**（脚本书签）
- 一律 `mv` 到 `trash/$(date +%Y-%m-%d)/`，**禁 rm**
- 输出释放空间 + 清理清单
- **transcripts >50 篇 → 仅提示磁盘占用**：⛔ 红线禁止移动/压缩/归档 transcripts，dream 不擅动，只报告占用由用户决定

## Phase 5 · 髓鞘化 + 架构自检

- 高频手动流程 → 沉淀/优化 skill（"用过 3 次"阈值）
- 校验工具路由表 / hook / 工作流回路健康
- **架构自检**：`ls` 实际 vs `ARCHITECTURE` 蓝图数字对账
- 架构变动 → 全量更新蓝图 + 变更日志

## Phase 6 · 清醒回忆（梦境报告）

- 输出 diff 摘要：新增 / 合并 / 标 stale / 待删 / 清理空间 / 架构变更
- 高危动作（删除 / 架构变更 / 回路改动）列清单**等用户确认**
- 追加 `log/YYYY-MM-DD.md`（dream 本身也是一次重要操作）

## 阶段跳跃（按范围裁剪）

| 范围 | 路径 |
|---|---|
| 纯记忆整理 | 0 → 1 → 2 → 3 → 6 |
| 纯清理 | 0 → 4 → 6 |
| 纯架构/回路 | 0 → 5 → 6 |
| 全量 | 0 → 1 → 2 → 3 → 4 → 5 → 6 |
