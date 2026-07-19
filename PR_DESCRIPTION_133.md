# PR Description — 支持管理 Hermes 多 Profiles 的技能

## Summary
本 PR 为 skills-manager 增加 Hermes 多 profiles 技能管理能力。当前仅能较好覆盖 Hermes 默认 Agent/profile，新增后可发现、同步、安装和状态管理多个 Hermes profiles 下的技能。

Closes #133.

## 问题背景
用户反馈：当 Hermes 创建了多个 profiles 时，skills-manager 目前无法一并管理这些 profile 下的技能，导致：
- 只能管理默认 profile；
- 多 profile 用户需要手动维护技能；
- skills-manager 与 Hermes 的实际使用场景不匹配。

## 目标
1. 支持识别 Hermes 下多个 profiles。
2. 支持按 profile 维度查看与管理技能（安装、更新、移除、状态同步）。
3. 保持默认 profile 的现有行为兼容。

## 非目标
- 不改变 Hermes 自身 profile 数据结构。
- 不在本次引入跨设备同步或云端 profile 编排能力。

## 方案概述
1. **Profile 发现机制**
   - 扫描并识别 Hermes 的 profile 列表（含默认 profile 与自定义 profiles）。
2. **数据模型扩展**
   - 在 skills-manager 内为 Hermes 技能增加 profile 维度（如 `tool=hermes + profileId/profileName`）。
3. **同步流程增强**
   - 同步任务从“单 profile”升级为“按 profile 批量处理”，并保留逐 profile 错误隔离。
4. **命令/界面能力**
   - 增加 profile 选择、过滤与汇总视图；
   - 操作日志明确标注目标 profile。

## 兼容性
- 对单 profile 用户零破坏，默认行为保持不变。
- 未启用 Hermes 或无自定义 profiles 的场景不受影响。
- 历史记录与缓存通过兼容迁移策略升级到带 profile 维度的模型。

## 风险与缓解
- **风险**：同名技能在不同 profile 中状态冲突。
  - **缓解**：以 `(profile, skill_id)` 作为唯一管理键，避免覆盖。
- **风险**：profile 扫描失败导致部分技能缺失。
  - **缓解**：保留最近一次成功快照并输出明确告警。
- **风险**：批量同步时单 profile 错误影响整体体验。
  - **缓解**：分 profile 执行与聚合结果，失败不阻断其他 profile。

## 验证计划
- 单元测试：
  - 多 profile 发现逻辑；
  - `(profile, skill)` 唯一键与状态聚合；
  - 迁移/兼容逻辑。
- 集成测试：
  - 默认 + 多自定义 profiles 的安装/更新/移除流程；
  - 部分 profile 异常时其余 profile 正常同步。
- 手工验证：
  1. 在 Hermes 中创建多个 profiles；
  2. 分别安装不同技能；
  3. 在 skills-manager 中验证各 profile 可独立查看和管理；
  4. 验证更新后状态正确回写。

## 发布与文档
- 更新 Hermes 集成文档，补充多 profile 管理说明。
- 发布说明中标注“新增 Hermes 多 profiles 技能管理支持”。

## Checklist
- [x] Full local PR description drafted.
- [ ] 功能实现完成。
- [ ] 测试补充并通过。
- [ ] 文档与变更日志更新完成。
