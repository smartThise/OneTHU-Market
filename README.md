# OneTHU 插件市场

OneTHU 应用内「插件市场」页签的数据源仓库。

**本仓库只收录插件元信息与源码仓库地址，不收录插件代码。** 插件本体始终存放在
作者自己的 GitHub 仓库；应用安装时直接从作者仓库拉取入口模块。

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

   `repo` 必须指向**存有插件源码的公开 GitHub 仓库**（支持 `user/repo`、完整 URL、
   `@branch`、`/tree/branch/sub/path` 形态）；`entry` 缺省为 `plugin.js`。

2. 提交 Pull Request。审查（当前为人工）要点：

   - `repo` 指向的仓库存在，`entry` 指向的模块可拉取、可解析；
   - `manifest` 导出与条目信息一致（id / name / version）；
   - 权限声明与功能匹配，无超范围权限；
   - 无混淆代码、无远程动态拼装代码、无凭据收集行为。

3. 合并即收录；用户端「刷新」或等待缓存过期（5 分钟）可见。

## 插件仓库格式

见 OneTHU 主仓库 `docs/plugin-development.md` §8.1：仓库根目录提供 `plugin.js`
（或 `index.js` / `main.js`），内容为单文件 ES 模块（`manifest` 导出 + 默认导出
激活函数），与「粘贴安装」格式完全一致。

完整示例仓库：[OneTHU-plugin-hello](https://github.com/smartThise/OneTHU-plugin-hello)。

## 版本号维护（重要）

条目的 `version` 是**人工维护的元数据**，应用端据此判定更新：市场版本高于用户本地
已装版本时，插件卡片显示「可更新 ↑」，市场条目按钮显示「更新」。该字段不随插件仓库
自动同步，因此**插件仓库每次发布新版本后，须向本仓库提交同版本的 `version` 改动**，
否则用户端不出现更新提示（表现为「插件明明升级了，市场里看不到新版」）。

同时应保持条目与插件 `manifest` 三者一致：`id`、`name`、`version`。审查会核对这
三项。

## 数据新鲜度

应用端拉取名单：优先 GitHub contents API，失败降级 `raw.githubusercontent.com`；
本地另有 5 分钟缓存，「刷新」按钮强制跳过缓存。raw 域名的 Fastly 边缘缓存会短时
返回推送前的旧内容且忽略 query 参数，故收录合并后若前端仍显示旧名单，等待缓存过期
或点「刷新」即可。
