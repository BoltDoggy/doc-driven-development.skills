# 项目初始化（DDD 结构搭建）

在项目没有 `CLAUDE.md` 和 `docs/` 目录时执行。此流程为项目建立文档驱动开发的基础结构。

## 执行顺序

### 1. 判断项目类型

扫描根目录，判断是"已有代码的项目"还是"新建项目"：

- **已有代码的项目**：存在源码目录（`src/`、`lib/`、`cmd/`、`main/`）或已知代码文件（`.rs`、`.go`、`.py`、`.ts`、`.js` 等），或存在配置文件（`package.json`、`Cargo.toml`、`go.mod`、`pyproject.toml`）
- **新建项目**：不存在以上任何内容

### 2. Git 检查

```
git rev-parse --is-inside-work-tree
```

- 是 Git 仓库：记录最近 5 条 commit 信息（`git log --oneline -5`）
- 不是：记录"非 Git 仓库"，继续执行

### 3. 创建 CLAUDE.md

将附录中的标准 DDD 工作流写入 `CLAUDE.md`，包含以下章节：
- 开发工作流（文档驱动开发）
- 文档 Commit 规范
- 文档结构（各目录职责）
- 代码组织原则（根据识别的技术栈）
- 当前项目状态（表格）
- 常用命令（根据识别的配置文件）
- 约束

已有代码的项目还需扫描源码目录结构和技术栈，让 CLAUDE.md 内容有实际依据。

### 4. 创建 docs/ 目录骨架

```
mkdir -p docs/requirements docs/design docs/adr
mkdir -p docs/api  # 仅当识别出项目包含 API/服务端依赖时
```

### 5. 生成初始文档（仅已有代码的项目）

**设计文档**（`docs/design/overall-architecture.md`）：
- 扫描代码中的类型、接口、结构体、类定义
- 扫描 TODO/FIXME
- 基于扫描结果生成设计文档，所有内容必须能从代码中找到依据

**需求文档**（`docs/requirements/core-features.md`）：
- 基于设计文档推导核心功能和目标用户
- **不可提及**具体编程语言、框架、库

### 6. Git 初始化与提交（仅新建项目）

如果还不是 Git 仓库：
```
git init
git add CLAUDE.md docs/
git commit -m "docs: initialize DDD documentation structure"
```

### 7. 校验与汇报

检查 `CLAUDE.md` 及相关文档是否存在且非空。向用户汇报初始化结果。

---

## 附录：写入 CLAUDE.md 的标准 DDD 工作流

```markdown
## 开发工作流：文档驱动开发

本仓库采用文档驱动开发工作流。所有代码变更必须按以下原则执行：

1. **先写需求文档。** 在任何设计或代码之前，先在 `docs/requirements/` 中产出需求文档，回答：解决什么问题？目标用户是谁？P0 必须具备哪些功能？P1 有哪些？明确不做什么？验收标准是什么？需求文档**不得提及**编程语言、框架、库等实现细节。
2. **再写设计文档。** 只有在需求文档经用户确认后，才在 `docs/design/` 中撰写设计文档，涵盖：架构、接口、数据模型、错误处理、技术取舍。设计必须能追溯到需求文档。
3. **写 ADR（如有）。** 设计文档中涉及"在多个可行方案中选择其一"的决策，必须在 `docs/adr/` 中撰写 ADR。
4. **等待用户确认。** 在获得用户明确确认之前，不得进入实现阶段。
5. **严格按文档实现。** 生成代码和测试时必须与已确认的设计保持一致。
6. **一致性校验。** 实现完成后，检查代码是否与设计文档一致。

## 文档 Commit 规范

- 每次修改 `docs/` 目录下的文件后，必须立即执行 `git add` 和 `git commit`
- Commit message 前缀使用 `docs:`
- 禁止将文档变更与实现代码混在同一个 commit 中
```

## 常用命令速查

| 语言栈 | 命令 |
|--------|------|
| Node.js | `npm install` / `npm run build` / `npm test` |
| Rust | `cargo build` / `cargo test` / `cargo check` |
| Python | `pip install -e .` / `pytest` / `black .` |
| Go | `go build ./...` / `go test ./...` / `go mod tidy` |
| 无法识别 | 写入"待补充" |
