# DSH 小鲸鱼余额挂件 · 三口皮肤版（dsh-whale-widget-sankou）

> **本仓库为 [MeteorNOX/DeepSeek-Balance-Whale-Widget](https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget)（`dsh-whale-widget`，MIT License，Copyright © 2026 MeteorNOX）的修改版（fork）**。
> 原版 LICENSE 已原样保留在 [LICENSE](./LICENSE)。本仓库的代码修改在下方「与原版的差异」列出；自定义角色素材与语音的使用说明见「素材说明」。

DSH Web 界面右下角的常驻余额挂件：显示 DeepSeek API 余额 + 今日已用 + 每轮对话消耗，并带一只名叫**三口**的桌宠（支持自定义形象/状态/音效）。

## 📺 效果演示

<video src="https://raw.githubusercontent.com/fiven577/dsh-whale-widget-sankou/main/docs/demo.mp4" width="720" controls type="video/mp4" poster="https://raw.githubusercontent.com/fiven577/dsh-whale-widget-sankou/main/docs/demo_cover.png"></video>

> 视频打不开时，可直接下载或点击打开 [docs/demo.mp4](./docs/demo.mp4)。

## 特性

**原版全部能力（保留）：**

- 常驻自启：随 DSH Web 界面打开自动出现在右下角
- 余额 60s 自动刷新 + 点击手动刷新；余额变化数字滚动动画
- 今日已用：小鲸鱼记账 / 实时·令牌 两种模式
- 每轮对话消耗统计与弹泡
- 拖拽 + 四边吸附、按压 Q 弹、汉堡菜单（大小/音量/气泡开关等）
- 峰谷文案皮肤（默认 / 梁文峰谷 / !?强强?!）与峰谷计价
- 随机台词气泡、rua.gif 动图（内容详见原版 README）

**三口皮肤版新增（2026-09）：**

1. **自定义形象**：桌宠本体由鲸鱼换成角色「三口」的透明立绘（构图与尺寸与原鲸鱼一致，可自行替换 `assets/DSniang1.png`）
2. **语音互动**：
   - 音效菜单新增「语音·点击(嗯？)」选项（默认）：单次点击角色播放 `嗯？.wav`，新点击会停掉上一条避免叠音
   - 每轮对话消耗提示出现时同步播放 `吃.wav`
3. **生气状态**：2 秒内连点 ≥6 次 → 形象切换为 `生气.png`，随机播放 `干嘛.wav` / `嗯.wav`（自动轮换不重复），维持 5 秒；生气期间点击音效静音，可再次快速连点重复触发
4. **余额提醒（吃撑）**：菜单新增「余额提醒」——支持固定档位（0.1/1/10/100/1000）或自定义（0.1~1000，精确到 0.1）；自上次提醒起累计消耗满设定金额 → 形象切换 `太撑了.png`、以原版气泡弹出「已吞X元」、随机播放 `这也太多了.wav` / `打嗝.wav`（延迟 1.6s 避免叠音），维持 5 秒
5. **吃透明动画**：每轮消耗提示时，角色会播放一段带 alpha 的「吃」透明动画（`assets/eat.webp`，由素材作者提供的 吃.mov 转码，约 2.1s），随后自动恢复静态形象；加载失败自动降级为仅音效
6. 各类状态相互协调：生气/吃撑互斥、吃动画可被打断、点击热区随形象同步、菜单修复（状态失步与点击被形象像素吞掉导致菜单打不开的问题）

## 与原版的差异

核心代码文件 `lib/index.js` 在原版 0.2.10 基础上修改，主要差异：

- `assets/`：新增角色素材（见下）
- `lib/index.js`：
  - 后端新增路由 `/dsh-whale/state.png`（生气/吃撑形象）、`/dsh-whale/extra.wav`（新增语音）、`/dsh-whale/eat.webp`（透明动画）
  - size 配置扩展 `soundSet=voice` 与 `remindOn/remindYuan` 持久化
  - 前端新增：语音播放（防叠音）、连点生气、余额累计吃撑提醒（原版样式气泡）、吃动画状态机、闲置/点击音效策略、菜单余额提醒 UI（档位+自定义）
  - 修复：菜单开关状态失步导致的“点不开菜单”、点击被形象像素拦截吞掉的问题
- 移除了原作者代码中针对作者本机的绝对路径回退（`D:/TestBox/...`），素材一律从包内 `assets/` 加载

## 素材说明

- **代码**：MIT，原作者版权见 [LICENSE](./LICENSE)；本仓库代码署名与修改记录见本 README。
- **自定义素材（角色「三口」形象图、全部 wav 语音、eat.webp 动画）**：为作者（fiven577）原创/定制素材，随本仓库一并发布。任何使用请同时遵守 MIT（代码部分）与素材作者意愿：**二次分发素材请保留本说明并注明出处**。

## 安装

前提：DSH Desktop / `dsh` CLI、`pnpm`。需先配置 `DEEPSEEK_API_KEY`（拉余额必需）。

```powershell
# 从本仓库安装（仓库公开后）
dsh plugin --profile web add github:fiven577/dsh-whale-widget-sankou

# 或本地安装（仓库根目录就是插件包）
dsh plugin --profile web add link:<本仓库目录绝对路径>
```

安装完成后**完全退出并重开 DSH Desktop**，再 F5 刷新页面，右下角出现桌宠即成功。

验证：

```powershell
curl http://127.0.0.1:3080/dsh-whale/balance.json
curl http://127.0.0.1:3080/dsh-whale/image.png
```

## 使用小抄

| 想干嘛 | 怎么做 |
|---|---|
| 看余额/今日已用 | 点一下桌宠 |
| 峰谷状态 | 点桌宠弹泡后，再点气泡切随机台词（约 45% 概率出现“当前时间段为”） |
| 触发生气 | 2 秒内快速点 6 次（可连续触发） |
| 设置吃撑提醒 | 菜单 → 余额提醒：档位或手输 0.1~1000 |
| 换音效 | 菜单 → 音效：语音·点击(嗯？) / 小黄鸭 / 音效1 |

## 常见问题

- 挂件不出现：确认 `dsh plugin add` 成功且配置里有本包；重启后 F5。
- 形象不显示/回到鲸鱼：确认 `assets/DSniang1.png` 是透明 cut-out（610×610）。
- 生气/吃撑音效与图片不同步：请完全重启后测试；动画加载失败会静默降级为纯音效。
- 想恢复默认鲸鱼：把原版 `assets/DSniang1.png` 放回即可。

## 修改记录

- 2026-09：fork 原版 0.2.10，加入三口皮肤与上述全部功能，发布本仓库。

## License

- 代码：MIT（原作者 Copyright © 2026 MeteorNOX，保留于 [LICENSE](./LICENSE)）
- 本仓库 fork 版本：MIT（附加素材使用说明，见「素材说明」）
