# 从 pokegold_1.01 移植的背景音乐开关

**修改对象：** Mega / Polished Crystal 项目（本压缩包的源码）。

- 普通 Options 可滚动菜单中的 **Music: On/Off**（位于 Sound 和 Battle Effects 之间）。用左右键切换；初始默认 On。
- 关闭时仅屏蔽背景音乐的硬件声道输出，不停止音乐播放进度；开启时接着当前进度播放。
- 保留音效、宝可梦叫声，以及 Mega 原本的 Mono/Stereo 声道设置。静音期间将全局音量维持最大，避免音乐淡出同时影响音效和叫声（沿用金版 1.01 的处理方式）。
- 使用 `wOptions3` 的空闲 bit 3（`MUSIC_OFF`），由项目已有的 `sOptions3`/`sBackupOptions3` 存档流程保存，不增加 RAM 或 SRAM 字节，不修改旧存档布局。
- 金版原来需要第二页来容纳音乐选项；Mega 已经使用滚动选项菜单，所以直接增加一项，不需要改成金版的两页菜单，也未加入金版的版本号。

## 文件改动

- `constants/ram_constants.asm`：标志位定义。
- `ram/wram0.asm`：选项位说明。
- `engine/menus/options_menu.asm`：选项函数与菜单索引。
- `data/options/option_names.asm`：菜单文字。
- `data/options/options_descriptions.asm`：选项说明。
- `audio/engine.asm`：背景音乐通道路由屏蔽、静音状态下音效/叫声音量保留。

## 验收建议

1. Options -> Music -> Off 时地图、战斗、剧情 BGM 静音；On 时恢复且不会从开头重放。
2. 关闭 BGM 时仍可听到操作音效、战斗技能音效、宝可梦叫声，Mono/Stereo 正常。
3. 音乐淡入淡出、地图切换、低血量提示及广播类音乐中测试上述行为。
4. 游戏保存并重开后检查 Music 状态仍然正确，特别是使用旧存档时。
