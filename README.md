# copilot-generate-code

## 背景

[为了让AI干活儿，我竭尽所能——我的 Vibe Coding 认知升级之路](https://yam.gift/2026/01/18/AI/2026-01-18-Upgrade-VibeCoding/)

[认知重建：Speckit 用了三个月，我放弃了——走出工具很强但用不好的困境](https://zhuanlan.zhihu.com/p/1993009461451831150)

[Everything Claude Code](https://github.com/affaan-m/everything-claude-code)

这几天阅读了很多关于构建AI工程化方面的文章后，慢慢了解到底如何让AI正确的干活，之前自己还是依赖自己去写代码方面的，哪怕在实习期间自己也会用AI，但是自己更多的是通过一段话让他写一段代码，对一个新的功能构建还是没有尝试过，怎么说呢，还是不信任吧；

现在慢慢感觉AI的能力已经足够好了，或许自己以前的想法都可以通过AI去帮忙实现，自己做的一个知识梳理以及确认；

看了之后觉得核心还是自己自定义会比较好，相当于有一个集市，当你要做一个菜的时候，自己去集市里面买合适；



## 快速参考

常用的命令：

| 命令           | 用途         | 使用场景     |
| -------------- | ------------ | ------------ |
| `/plan`        | 架构决策     | 开始新功能时 |
| `/tdd`         | 测试驱动开发 | 所有后端代码 |
| `/code-review` | 质量检查     | 代码变更后   |
| `/e2e`         | 端到端测试   | 集成测试时   |
| `/build-fix`   | 修复构建错误 | 构建失败时   |

[Software Engineering Team](https://github.com/github/awesome-copilot/blob/main/collections/software-engineering-team.md)

### 项目管理部分

你可以在这里面[Project Planning & Management](https://github.com/github/awesome-copilot/blob/main/collections/project-planning.md)选择合适的项目管理指令/智能体进行使用

### 测试驱动开发

[Testing & Test Automation](https://github.com/github/awesome-copilot/blob/main/collections/testing-automation.md)

### 质量检查



## 0->1 的全流程

开发一个新功能的最佳实践流程：

```markdown
# 第 1 步：规划（Claude Opus）
/plan "构建市场预测功能，用户可以：
- 浏览活跃市场
- 对结果进行预测
- 追踪预测准确率
- 查看预测历史
技术栈：Next.js 14 + TypeScript + Prisma + PostgreSQL"

# → 输出：详细的实施计划（分阶段）

# 第 2 步：API 契约（手动 + Claude）
# 创建 types/api.ts
export interface Market {
  id: string
  question: string
  endsAt: Date
  status: 'active' | 'closed'
}

export interface Prediction {
  id: string
  marketId: string
  outcome: boolean
  confidence: number
}

# 第 3 步：后端开发（Claude Sonnet + TDD）
/tdd "创建市场 API：
- GET /api/markets - 列出所有市场
- GET /api/markets/:id - 获取单个市场
- POST /api/markets - 创建市场（仅管理员）
- POST /api/predictions - 进行预测
使用 Prisma，添加身份验证和速率限制"

# → 输出：经过测试的后端，覆盖率 80%+

# 第 4 步：前端开发（切换到 Antigravity + Gemini）
# 打开 Antigravity 编辑器
# 在 Antigravity 中向 Gemini 发送以下 prompt：

"生成市场浏览的 React 组件：
1. MarketList 组件 - 卡片网格布局
2. MarketCard 组件 - 单个市场卡片
3. MarketDetail 组件 - 完整市场视图
4. PredictionForm 组件 - 预测表单

技术要求：
- Next.js 14 使用 App Router
- TailwindCSS 样式
- 支持暗色模式
- 响应式设计（移动端优先）
- TypeScript 严格类型
- 无障碍访问（WCAG 2.1 AA）
- 加载和错误状态
- 使用 SWR 进行数据获取"

# → Gemini 在 Antigravity 中生成完整的 React 组件
# → 将生成的组件文件保存到 src/components/market/

# 第 5 步：代码审查（Claude Sonnet）
/code-review src/components/market/
/code-review src/app/api/markets/

# → 输出：发现并修复问题

# 第 6 步：集成（Claude Sonnet）
/plan "连接前端和后端：
- 更新 MarketList 从 /api/markets 获取数据
- 更新 PredictionForm POST 到 /api/predictions
- 添加错误处理
- 添加乐观更新"

# 第 7 步：测试与调试
# 如果测试失败，直接告诉 Claude
"测试失败了：[粘贴错误]，帮我修复"

# E2E 测试
/e2e "用户旅程：浏览市场 → 查看详情 → 进行预测 → 看到确认"

# 第 8 步：安全审查
/plan "对市场功能运行 security-reviewer"

# 第 9 步：部署
# 准备上线！🚀
```

## 专业技巧

1. **从小处着手** - 不要一次性构建所有功能
2. **测试先行** - 始终在实现之前编写测试
3. **频繁审查** - 不要批量代码审查
4. **记录决策** - 记录为什么，而不仅仅是做什么
5. **频繁提交** - 小而原子化的提交
6. **测量性能** - 优化前先分析
7. **安全第一** - 永远不要跳过安全审查
8. **从错误中学习** - 每个 bug 都是一课

## 🚨 常见陷阱

❌ **不要：**

- 跳过规划阶段
- 不写测试就写后端
- 不审查就使用 Gemini 生成的代码
- 忽略 TypeScript 错误
- 跳过安全审查
- 批量大量变更
- 绕过错误而不是修复

✅ **要：**

- 始终从 `/plan` 开始
- 所有后端代码使用 `/tdd`
- 审查所有 AI 生成的代码
- 立即修复类型错误
- 合并前运行安全检查
- 频繁提交并写清楚提交信���
- 遇到错误直接告诉 Claude 调试

## 🎯 成功指标

当你达到以下标准就是成功：

- ✅ 所有测试通过（单元、集成、E2E）
- ✅ 80%+ 代码覆盖率
- ✅ 严格模式下类型检查通过
- ✅ 无控制台错误或警告
- ✅ 安全审查通过
- ✅ 性能基准达标
- ✅ 文档完整
- ✅ 准备上线





## 典型工作流

### 添加新功能

```
# 1. 规划
/plan "添加 [功能名称]"

# 2. 定义类型（手动）
# 编辑 src/types/api.ts

# 3. 构建后端（TDD）
/tdd "为 [功能] 创建 API"

# 4. 生成前端（Gemini）
"为 [功能] 创建 React 组件"

# 5. 审查与集成
/code-review
/plan "连接前端和后端"

# 6. 测试
/e2e "测试 [功能] 用户旅程"

# 7. 安全检查
/plan "运行安全审查"
```

### 修复 Bug

```
# 1. 理解问题
阅读相关文件
重现错误

# 2. 调试（直接告诉 Claude）
"帮我调试：[错误上下文]"

# 3. 应用修复
根据建议编辑文件

# 4. 验证
npm test
npm run type-check

# 5. 添加回归测试
/tdd "为 [bug 场景] 添加测试"
```

### 重构

```
# 1. 代码审查
/code-review

# 2. 识别问题
# 审查输出

# 3. 带测试重构
/tdd "重构 [组件/函数] 同时保持测试"

# 4. 验证没有破坏性变更
npm test
/e2e "运行完整 E2E 测试套件"
```



## 参考

优秀的参考文件：

* [anthropics/skills](https://github.com/anthropics/skills)

* [awesome-copilot](https://github.com/github/awesome-copilot)