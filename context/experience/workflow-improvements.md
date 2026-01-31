# 工作流改进

**Last Updated**: 2026-01-31

## 概述

记录项目开发过程中发现的工作流优化和效率提升方法。这些改进应该有可衡量的效果。

> 🚀 **模板说明**: 记录那些显著提升开发效率的改进。包含改进前后的对比和量化效果（如果可能）。

## 模板格式

```markdown
### {改进名称}

**类别**: {开发流程/协作方式/工具优化/自动化等}
**问题**: {改进前存在什么问题}

**改进方案**:
{详细描述改进措施}

**实施步骤**:
1. {步骤1}
2. {步骤2}
3. {步骤3}

**效果**:
- 📈 改进前: {具体指标}
- 📈 改进后: {具体指标}
- 💡 额外收益: {其他好处}

**成本**: {实施成本/维护成本}
**推荐程度**: ⭐⭐⭐⭐⭐ (1-5星)
```

---

## 目录

- [开发流程](#开发流程)
- [协作方式](#协作方式)
- [工具优化](#工具优化)
- [自动化](#自动化)

## 开发流程

### 当前工作流

**Plan → Work → Review → Compound** 四阶段模型

待添加具体流程改进经验...

## 协作方式

待添加协作改进经验...

## 工具优化

### 索引优先的上下文加载策略

**类别**: 工具优化/上下文工程
**问题**: 传统方式加载整个知识库目录导致上下文污染，浪费大量 tokens，降低 AI 效能

**改进方案**:
实施"索引优先"策略 - 总是先读取轻量级索引文件，然后根据任务需求选择性加载具体领域文件。

**实施步骤**:
1. 创建 `context-loader` skill，封装索引优先策略
2. 建立三级加载体系：主索引 → 领域索引 → 具体领域文件
3. 在所有工作阶段（Plan/Work/Review/Compound）开始时使用此 skill
4. 使用 grep 搜索优化，先查询再决定是否加载完整文件

**效果**:
- 📈 改进前: 加载所有文件（15+ 文件，18,000+ tokens）
- 📈 改进后: 按需加载（3-5 文件，2,500-4,000 tokens）
- 💡 额外收益: 
  - Token 使用减少 80-85%
  - 上下文窗口保持清洁添加索引优先上下文加载策略 | System |
| 2026-01-30 | 
  - AI 响应质量提升
  - 处理速度更快

**成本**: 
- 初始投入：创建 skill 和建立索引体系（一次性）
- 维护成本：极低（skill 自动适应知识库结构）

**推荐程度**: ⭐⭐⭐⭐⭐ (5星 - 强烈推荐所有项目使用)

**使用方法**:
```bash
# 所有阶段开始时
@context-loader

# 或者明确请求
"Load context for fixing authentication bug"
"Check what we know about testing strategy"
```

**相关资源**:
- [context-loader skill](../../../.github/skills/context-loader/SKILL.md)
- [Anthropic Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [learn.prompt.md](../../../.github/prompts/learn.prompt.md)

---

## 协作方式

### 要求确认而非虚构信息的规划原则

**类别**: 协作方式/AI 交互模式
**问题**: `/plan` 命令在缺少具体信息时会编造细节（如虚构 Copilot hook 配置），导致：
- 规划文档包含错误信息
- 用户信任度下降
- 后续实施困难（基于错误假设）

**改进方案**:
创建全局 `never-fabricate.instructions.md` 文件，使所有 AI 交互（不仅是 `/plan`）都遵循"永不编造信息"原则。

**实施步骤**:
1. ✅ 创建 `.github/instructions/never-fabricate.instructions.md`
2. ✅ 定义核心原则：质量优于完整性
3. ✅ 提供详细的"何时停止并询问"指南
4. ✅ 包含请求澄清的模板和示例
5. ✅ 简化 plan.prompt.md，移除重复内容，引用全局 instructions
6. ✅ 使原则适用于所有工作流阶段（Plan/Work/Review/Compound）

**架构决策**:
- **为什么用 instructions 而非 prompt**: 
  - Instructions 应用于所有聊天会话（全局生效）
  - Prompt 仅在特定命令时生效（局部生效）
  - 不编造信息是普适原则，应该全局强制执行
- **职责分离**: 
  - Instructions = 全局行为准则
  - Prompts = 特定工作流程

**效果**:
- 📈 改进前: 仅 `/plan` 有防护，其他命令可能编造信息
- 📈 改进后: 所有 AI 交互都遵循"永不编造"原则
- 💡 额外收益: 
  - 跨工作流的一致性（Plan/Work/Review 都适用）
  - 代码更简洁（prompts 不重复此逻辑）
  - 更容易维护（一处修改，全局生效）
  - 新 prompts 自动继承此原则

**成本**: 
- 初始投入：创建 instructions 文件（一次性，约 45 分钟）
- 维护成本：极低（全局原则，很少需要修改）
- 交互成本：可能需要额外的澄清往返，但换来准确性

**推荐程度**: ⭐⭐⭐⭐⭐ (5星 - 关键架构改进)

**核心原则**:
```
质量优于完整性 - 宁可输出不完整但准确，
也不要完整但充满虚构信息

Engineering Integrity - Be honest about what you know and don't know
```

**使用场景对比**:

| 场景 | 改进前 | 改进后 |
|------|--------|--------|
| `/plan` 规划功能 | 可能编造 hook 配置 | 停止并请求具体配置 |
| `/work` 编写代码 | 可能编造 API 规范 | 停止并请求 API 文档 |
| `/review` 代码审查 | 可能编造安全标准 | 停止并请求合规要求 |
| 文档编写 | 可能编造使用示例 | 停止并请求真实代码 |
| 所有聊天 | 不一致的行为 | 统一的诚信标准 |

**文件结构**:
```
.github/
├── instructions/
│   └── never-fabricate.instructions.md  ← 新增：全局生效
└── prompts/
    └── plan.prompt.md                    ← 简化：引用全局原则
```

**相关资源**:
- [never-fabricate.instructions.md](../../../.github/instructions/never-fabricate.instructions.md) - 全局原则定义
- [plan.prompt.md](../../../.github/prompts/plan.prompt.md) - 简化后的规划 prompt
- [AI Safety Best Practices](../../../.github/instructions/ai-prompt-engineering-safety-best-practices.instructions.md)

**延伸思考**:
这个改进体现了软件工程的 DRY 原则（Don't Repeat Yourself）：
- 全局原则 → Instructions（单一真实来源）
- 特定工作流 → Prompts（引用全局原则）
- 避免在每个 prompt 中重复相同的行为准则

---

## 工具优化

待添加工具使用优化经验...

## 自动化

### GitHub Actions

项目使用 GitHub Actions 进行自动化验证。

待添加自动化改进经验...

---

**使用说明**: 
- 使用 `/learn` 命令添加新的工作流改进
- 记录显著提升效率的方法和工具

**变更日志**:
| 日期 | 变更 | 作者 |
|------|------|------|
| 2026-01-31 | 升级为全局 instructions：never-fabricate.instructions.md | System |
| 2026-01-31 | 添加"要求确认而非虚构信息的规划原则"改进 | System |
| 2026-01-30 | 添加索引优先上下文加载策略 | System |
| 2026-01-30 | 初始创建 | System |
