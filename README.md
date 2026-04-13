# 小浪双拼 for Rime

基于薄荷拼音 `oh-my-rime` 配置体系整理的小浪双拼方案。

这个仓库不是完整的 Rime 整包，而是一个可叠加到现有薄荷/雾凇生态上的增量 schema。核心目标是把 Windows 搜狗 11.11 可识别的小浪双拼键位，稳定迁移到 Rime，并保留薄荷常用能力。

## 特性

- 小浪双拼键位适配
- 保留 `rime_mint` 词库
- 中英混输
- 日期、时间、农历、计算器
- Emoji / 简繁切换
- 五笔 / 笔画 / 拆字反查
- 拆字滤镜开关

## 依赖

这个方案依赖薄荷拼音 `oh-my-rime` 提供的词库、Lua 脚本和部分反查组件，至少需要这些对象已经存在：

- `rime_mint`
- `melt_eng`
- `wubi98_mint`
- `radical_pinyin`
- `stroke`
- `rime.lua` 及其相关 Lua translators / filters

如果你的环境里已经装好了薄荷拼音，再接入这个仓库就最省事。

## 安装

1. 先按 Mintimate 的文档安装好 `oh-my-rime`。
2. 把 [double_pinyin_xiaolang.schema.yaml](./double_pinyin_xiaolang.schema.yaml) 复制到你的 Rime 用户目录。
3. 打开自己的 `default.custom.yaml`，把 `double_pinyin_xiaolang` 合并进 `schema_list`。
4. 可以参考 [examples/default.custom.yaml](./examples/default.custom.yaml)，但不要整份覆盖自己的现有配置。
5. 重新部署 Rime。

常见用户目录：

- macOS Squirrel: `~/Library/Rime`
- Windows Weasel: `%APPDATA%\\Rime`
- Linux Fcitx5 Rime: `~/.local/share/fcitx5/rime`

## 已适配功能

- 小浪双拼声母 / 韵母映射
- `zh / ch / sh` 特殊键位
- 零声母映射
- `iao / iang / uai / eng` 等易冲突韵母顺序修正
- 中英混输
- 时间日期类 Lua 指令
- 五笔、笔画、拆字反查
- `Control+Shift+E` Emoji 开关
- `Control+Shift+C` 拆字滤镜开关
- `Control+Shift+1` 简繁切换

## 已知限制

- 按你提供的小浪表，零声母 `en` 和 `eng` 同码都是 `un`。当前方案默认回显为 `en`，但查词时两者都会参与匹配。
- 目前没有加入“小浪专属音形辅码”。原因不是做不到，而是没有一套明确、公开、可验证的小浪辅码规则，强接其他双拼体系的辅码会污染输入行为。

## 使用说明

接入后，除了正常双拼输入，你还可以直接使用薄荷原有指令：

- `/sj` 时间
- `/rq` 日期
- `/dt` 日期时间
- `/xq` 星期
- `/nl` 农历
- `=1+2*3` 计算器
- `R1234.56` 数字金额大写
- `Uw...` 五笔反查
- `Ui...` 笔画反查
- `Uu...` 拆字反查

## 文件说明

- [double_pinyin_xiaolang.schema.yaml](./double_pinyin_xiaolang.schema.yaml): 小浪双拼主方案
- [examples/default.custom.yaml](./examples/default.custom.yaml): `schema_list` 接入示例
- [LICENSE](./LICENSE): GPL-3.0 许可证文本

## 致谢

- Mintimate / `oh-my-rime`
- iDvel / `rime-ice`
- 双拼方案作者提供的 Windows 搜狗 11.11 小浪双拼键位文件(ini格式)
- OpenAI Codex

## 许可证

这个仓库里的 schema 明确基于 Mintimate 的薄荷拼音配置改写，并沿用了其组件命名和实现方式。为避免与上游许可证冲突，本仓库采用 GPL-3.0。

上游参考：

- Mintimate `oh-my-rime`: <https://github.com/Mintimate/oh-my-rime>
- iDvel `rime-ice`: <https://github.com/iDvel/rime-ice>
- Mintimate 安装文档: <https://www.mintimate.cc/zh/guide/installRime.html>
