# 记忆库操作规范（精简）

Dream 写入/整理时遵守。完整版可自行扩展到自己的 `CONVENTIONS.md`。

## 写入判定（事件 → 位置）

| 事件 | 位置 |
|---|---|
| 完成重要操作 | `log/YYYY-MM-DD.md` |
| 发现已验证事实/偏好 | `facts/` |
| 犯错或被纠正 | `lessons/` + 日志标 `[教训]` |
| 项目状态/配置变更 | `projects/` |
| 产生未验证想法 | `ideas/` |
| 对话结束 | `transcripts/`（自动）|

**判定优先级**：犯错或被纠正→`lessons/`；项目总览→`projects/`；已验证→`facts/`；未验证→`ideas/`

## frontmatter schema

```
---
name: <kebab-slug>          # 必填，wiki-link 锚定此字段
description: <一句话>        # 必填，索引据此判断相关性
metadata:
  type: fact|lesson|project|idea
  created: YYYY-MM-DD
  updated: YYYY-MM-DD       # 可选，修改时记（追加优于修改）
  verified: N               # lessons 专用：第 N 次验证通过（可信度）
  status: stale             # 仅 stale 时标，active 省略
---
```

## wiki-link 规范

- 锚 frontmatter `name`：`[[name-slug]]`
- 不用路径式、不带目录前缀
- name 全局唯一；迁移目录 name 不变（解耦）

## 生命周期

- 日志：14 天归档到 `archive/`
- 知识过时：标 `status: stale` + 指向新条目，**不删**
- 知识修改：记 `updated` 或文件内 changelog（**追加优于修改**，保留演进）
- `verified: N` 是可信度标记（检索时高 verified 更可信），不强制固化到 CLAUDE.md（避免膨胀）
- **归档优于删除**：永不 `rm`，`mv` 到 trash

## 命名规范

- 英文文件名，中文内容
- kebab-case：全小写、连字符分隔
- 日志/逐字稿：`YYYY-MM-DD.md`

## 检索策略

`MEMORY.md` 索引 → `facts/`/`lessons/`/`projects/` →（dream 时）`log/` →（需原始细节）`transcripts/`。
**不要每次读所有文件**，先索引后详情。
