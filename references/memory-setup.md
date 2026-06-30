# 三层记忆架构搭建（B 方案前置）

Dream 依赖一套三层 markdown 记忆库。未搭建先按本文搭好（约 10 分钟）。

## 目录结构

```
~/.claude/projects/<你的项目>/
├── CLAUDE.md            # 工作规范（每次注入）
└── memory/
    ├── MEMORY.md        # 索引入口（<150 行，每次注入）
    ├── ARCHITECTURE.md  # 蓝图全景（按需读）
    ├── CONVENTIONS.md   # 操作规范（按需读）
    ├── log/             # 日志层：每日摘要（14 天归档）
    ├── facts/           # 知识层：已验证事实·偏好·原则
    ├── lessons/         # 知识层：教训纠错
    ├── projects/        # 知识层：项目快照
    ├── ideas/           # 知识层：未验证想法
    ├── archive/         # 归档（只读不改）
    ├── transcripts/     # ⛔ 原始层：逐字稿，只增不减
    └── .transcript-progress/  # ⚠️ 提取脚本书签，别手碰
```

## 三层分离（核心，不可混同）

| 层 | 目录 | 性质 | 生命周期 | 类比 |
|---|---|---|---|---|
| 日志层 | `log/` | 加工摘要（有取舍）| 14 天→归档 | 日记 |
| 知识层 | `facts/` `lessons/` `projects/` `ideas/` | 蒸馏结论 | 永久 | 教科书 |
| 原始层 | `transcripts/` | 未加工记录 | 永久只增不减 | 行车记录仪 |

⚠ **日志 ≠ 逐字稿**。日志是加工品（有摘要、有取舍），逐字稿是 ground truth（只增不减）。Dream 读日志层（轻），只有需要"到底说了什么"的细节才查逐字稿。

## 蒸馏路径

```
日志层产生新内容 → 提炼进入知识层 → 索引层指向知识层
```

## 搭建步骤

1. 建目录：`memory/{log,facts,lessons,projects,ideas,archive,transcripts}`
2. 写 `MEMORY.md`：索引入口，<150 行，每个知识条目一行指针（hook 摘要）
3. 写 `CONVENTIONS.md`：操作规范（写入判定/wiki-link/frontmatter，见 [`conventions.md`](conventions.md)）
4. 写 `ARCHITECTURE.md`：蓝图全景 + 变更日志（每次架构调整追加一条）
5. 配逐字稿提取：会话结束把原生会话 jsonl 提取成 `transcripts/YYYY-MM-DD.md`（原始层，只增不减）

## 设计哲学（5 条）

1. **手写精选 > 自动流水账**——自动化捕获已多次被废弃，只记值得长期留的
2. **三层分离**——不同生命周期的信息分开，互不污染
3. **解耦**——wiki-link 锚 frontmatter `name`，文件迁移目录而链接不断
4. **索引优先**——MEMORY.md <150 行常驻，详情按需读，回忆 O(1)
5. **归档优于删除**——永不 rm，过时标记，保留可回溯

## 设计本源（为什么需要外部记忆）

| 大模型缺陷 | 表现 | 本架构的解药 |
|---|---|---|
| 会话隔离 | 每次新对话从零开始 | 文件持久化，跨会话保留 |
| 上下文有限 | 长对话遗忘早期内容 | 索引常驻 + 详情按需读 |
| 不自动学习 | 权重不变，重复犯错 | Dream 提炼 lessons + verified 固化 |
| 无元认知 | 不知道自己知道什么 | MEMORY.md 索引 = "我知道什么"的目录 |
