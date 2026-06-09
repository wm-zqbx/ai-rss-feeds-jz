# AGENTS.md

## 交流约定

- 默认使用中文简体回复。
- 说明改动时保持简洁，明确列出已执行的验证命令和结果。

## 项目概览

- 这是一个 Bun + TypeScript 项目，用于根据 `configs/` 中的配置生成 `feeds/` 下的 RSS 2.0 XML。
- 核心代码位于 `src/`：
  - `parser.ts`：多模式解析入口。
  - `validator.ts`：RSS 结果校验。
  - `generator.ts`：生成 RSS XML。
  - `run-all.ts`：批量更新和验证 CLI。
  - `add-smart.ts` / `add-feed.ts`：新增 feed。
  - `heal-feed.ts`：修复失效 feed 配置。
  - `update-readme.ts`：更新 README 中的 feed 表格。

## 常用命令

```bash
bun install
bun run typecheck
bun run validate
bun run update
bun run update:one <name>
bun run add <url>
bun run heal <name>
bun run readme
```

## 开发注意事项

- 修改 TypeScript 后至少运行 `bun run typecheck`。
- 修改解析、生成或校验逻辑后，优先运行 `bun run validate`；如涉及某个 feed，可运行 `bun run update:one <name>`。
- 新增或删除 `configs/` / `feeds/` 中的 feed 文件后，运行 `bun run readme` 同步 README 表格。
- `feeds/` 是生成产物，但当前仓库会跟踪这些 XML 文件；不要因为它们是生成文件就随意忽略相关变更。
- `cache/` 用于回归跟踪。除非任务明确涉及快照或验证行为，否则避免手动改动缓存。
- 需要 LLM 的命令依赖 `GITHUB_TOKEN`，例如 `bun run add <url>` 和 `bun run heal <name>`。

## 代码风格

- 保持现有 TypeScript 风格，使用 ES modules。
- 优先复用 `src/types.ts` 中已有类型。
- 对配置 JSON 保持字段清晰、稳定，新增 parser mode 或字段时同步更新类型和相关 README 说明。
- 避免引入不必要的新依赖；如确需新增依赖，更新 `package.json` 和锁文件。
