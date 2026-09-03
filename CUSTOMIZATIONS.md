# 晚晚版自定义改动

这份文件记录在同步 Ombre Brain 上游版本时必须复查的本地改动，避免以后再次依赖提交历史考古。

## 必须保留

### `notify`

- 位置：`src/server.py`
- 用途：给晚晚手机发送 ntfy 推送。
- 迁移要求：保持现有实现原样，不重构配置，不修改默认标题、topic、超时或返回文案。
- 现有配置：`NTFY_URL`，默认 `https://ntfy.sh`；topic 固定为 `wanwan-alert`；默认标题为 `小克`。

### `check_phone`

- 位置：`src/server.py`
- 用途：从 Supabase 查询晚晚最近的手机使用记录。
- 迁移要求：保持现有实现原样，不重构配置，不修改表名、查询字段、超时或返回文案。
- 现有配置：`SUPABASE_URL`、`SUPABASE_ANON_KEY`；表名固定为 `Linwan-ke`。

### Fork 工作流

- 不保留上游的 `.github/workflows/docker-publish.yml`，避免个人 fork 尝试向原作者的 Docker Hub 命名空间发布镜像。
- 保留 `.github/workflows/tests.yml`，用于在升级和合并前验证自定义版本。

## 已由上游接管，不再重复打补丁

- 旧版 `hold(name=...)`：新版使用 `hold(title=...)` 原生支持显式命名，包括普通记忆与 feel。
- 旧版 `_strip_md_fence` 前缀文字兼容：新版 `clean_llm_json()` 已能提取模型回复中的首个完整 JSON 值。
- 旧版 Render `starter` 档位修改：上游 `render.yaml` 已默认使用 `starter`。
- 旧版 `OMBRE_MCP_NO_AUTH=1`：新版改用官方变量 `OMBRE_MCP_REQUIRE_AUTH=false`，不再保留旧代码分支。

## 升级基线

- 本轮基于上游 `3.6.12`。
- 记忆数据仍位于持久化 Volume，不把任何 buckets、密钥或个人记忆提交到代码仓库。
