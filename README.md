# OneTHU 插件市场

OneTHU 应用内「插件市场」页签的数据源仓库。名单文件 `registry.json` 经人工审查后
收录社区插件；应用端拉取该文件展示、搜索，并直接从插件仓库安装。

## 收录提交

1. Fork 本仓库，在 `registry.json` 的 `plugins` 数组追加条目：

   ```json
   {
     "id": "onethu.your-plugin",
     "name": "插件名",
     "version": "1.0.0",
     "author": "作者",
     "description": "一句话说明",
     "repo": "your-github/your-repo",
     "entry": "plugin.js",
     "tags": ["分类"]
   }
   ```

2. 提交 Pull Request。审查要点（人工，当前阶段）：
   - 仓库存在且 `entry` 指向的模块可拉取、可解析；
   - `manifest` 与条目信息一致（id/name/version）；
   - 权限声明与功能匹配，无超范围权限；
   - 无混淆代码、无远程动态拼装代码、无凭据收集行为。
3. 合并后即收录；应用端「刷新」或等待缓存过期（5 分钟）可见。

## 仓库格式（插件作者）

插件仓库根目录提供 `plugin.js`（或 `index.js` / `main.js`），内容为单文件 ES 模块：
`manifest` 导出 + 默认导出激活函数，与「粘贴安装」格式完全一致。参见
`plugins/hello/plugin.js` 与 OneTHU 主仓库 `docs/plugin-development.md`。

`repo` 字段支持 `user/repo`、完整 URL、`@branch`、`/tree/branch/sub/path` 形态；
`entry` 缺省为 `plugin.js`。
