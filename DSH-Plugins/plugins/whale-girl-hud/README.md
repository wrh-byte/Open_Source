# 小鲸鱼娘桌宠 · Whale Girl HUD

透明底的女仆装小鲸鱼娘，悬浮在 DSH Web / Desktop 界面右下角。点击她，气泡里显示当前账号额度与本会话运行状态。

## 功能

- **桌宠**：透明底立绘、无卡片边框，轻微上下漂浮；Agent 运行时漂浮加快，脚边出现绿色脉冲点。
- **点击查看额度**：气泡显示充值钱包余额（多币种用 ` · ` 分隔）与赠送余额；未登录时显示「未登录」。
- **运行状态**：当前主会话运行中 / 空闲。
- **刷新**：打开气泡时立即刷新，打开期间每 60 秒刷新一次；关闭后不再请求。
- **收起**：再点一次、点 ×、点页面空白处或按 Esc。
- 界面文字中/英双语，跟随 DSH 语言；深/浅色主题都使用宿主 token。

## 安装

### 方式 A：tarball（分享包）

把 `whale-girl-hud-1.3.0.tgz` 发给对方，然后任选一种：

- **DSH Desktop / Web 插件页**：Settings → Plugins → 安装入口，填入 tarball 的绝对路径，例如
  `C:\Users\you\Downloads\whale-girl-hud-1.3.0.tgz`；
- **CLI**：
  ```sh
  dsh plugin --profile web add "/path/to/whale-girl-hud-1.3.0.tgz"
  ```
- **让 Agent 装**：把 tarball 路径给启用 `plugin_manager` 工具的会话，执行 `install_bundle`。

安装后插件会出现在 Plugins 页的 Installed 列表里；如果没有立刻出现，刷新页面。

### 方式 B：npm（发布后）

```sh
dsh plugin --profile web add whale-girl-hud
```

Desktop 用户在 Settings → Plugins 里搜索 `whale-girl-hud` 安装。

## 兼容性

```json
"engines": { "dsh": ">=0.2.1-rc.3 <0.3.0" }
```

需要账号已在 DSH 中登录；只使用宿主已有的账号 Remote 读取余额，不额外申请凭据。

## 隐私

- 不包含 Host 端逻辑（`index.js` 为空），只有客户端 UI。
- 只调用 DSH 自带的 `ctx.remote.account.getState()` / `getBalance()`；余额不会上传到任何第三方，也不会写入日志。
- 没有遥测、统计或外部网络请求。

## 结构

```text
whale-girl-hud/
  package.json          DSH bundle + client 声明
  cordis.patch.yml      Host 行
  index.js              Host 半（空）
  client.js             客户端：桌宠、气泡、额度读取
  assets/icon.png       插件图标
  assets/pet.webp       透明底立绘（同时以 data URI 内联进 client.js）
  locale/               插件标题/描述（中/英）
```

## 卸载

- Plugins 页对应插件的 Remove；或
  ```sh
  dsh plugin --profile web remove whale-girl-hud
  ```

## 许可

- 代码：MIT，见 [LICENSE](LICENSE)。
- 立绘：版权归插件作者所有，**不在 MIT 范围内**；公开再分发前请确认授权。这是非官方 fan-made 插件，与 DeepSeek 官方无关。
