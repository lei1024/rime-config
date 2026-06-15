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

接入后，除了正常双拼输入，你还可以使用薄荷原有的 Lua 指令、计算器、金额转换和反查功能。

### 时间日期指令

当前方案的时间日期引导键是 `o`：

```yaml
key_binder:
  shijian_keys: ["o"]
```

所以默认要输入 `odt` 才是日期时间，`/dt` 不会触发日期时间 Lua。斜杠 `/...` 默认优先走上游 `symbols.yaml` 符号表。

如果你想同时支持 `/dt`、`/rq`、`/sj` 这类写法，可以把 `double_pinyin_xiaolang.schema.yaml` 里的配置改成：

```yaml
key_binder:
  shijian_keys: ["o", "/"]
```

改完后重新部署 Rime。

| 输入 | 数字别名 | 功能 |
| --- | --- | --- |
| `orq` | `o77` | 今天日期，包含公历格式、农历、干支、时辰提示 |
| `orc` |  | 今天日期，走相对日期查询逻辑 |
| `orc3+` / `orc3p` / `orc3=` |  | 3 天后的日期；数字可替换 |
| `orc3-` / `orc3o` |  | 3 天前的日期；数字可替换 |
| `osj` | `o75` | 当前时间，包含时分秒和当前时辰 |
| `odt` | `o38` | 当前日期时间 |
| `ott` | `o88` | 时间戳，包含 Unix 秒、毫秒、RFC3339、本地紧凑格式 |
| `outc` |  | 世界时钟，显示 UTC、本地时间和常见城市时间 |
| `onl` | `o65` | 今天农历、干支、节气和时辰 |
| `oxq` | `o97` | 今天星期和 ISO 周数 |
| `oww` | `o99` | 今年第几周 |
| `ojq` | `o55` | 最近节气和倒计时 |
| `ojr` | `o57` | 即将到来的节日和倒计时 |
| `oday` | `o329` | 今日综合信息：日期、农历、年度进度、节日、节气、三伏等 |

### 指定日期查询

`N...` 不需要 `o` 前缀，直接输入即可：

| 输入 | 功能 |
| --- | --- |
| `N0101` | 查询当前年份的 1 月 1 日，包含公历、农历和干支候选 |
| `N2026` | 查询 2026 年 1 月 1 日 |
| `N202606` | 查询 2026 年 6 月 1 日 |
| `N20260615` | 查询 2026 年 6 月 15 日 |
| `lunar` | 直接查询今天农历，来自 `chineseLunarCalendar_translator` |

日期不存在时，候选窗口会提示 `日期不存在`。

### 日期时间格式

`orq`、`orc...`、`osj`、`odt` 的候选格式来自 schema 中的这些配置：

- `date_formats`: 日期候选格式
- `time_formats`: 时间候选格式
- `datetime_formats`: 日期时间候选格式

可用占位符：

| 占位符 | 含义 |
| --- | --- |
| `Y` / `y` | 四位年份 / 两位年份 |
| `m` / `n` | 两位月份 / 不补零月份 |
| `d` / `j` | 两位日期 / 不补零日期 |
| `H` / `G` | 24 小时制，两位 / 不补零 |
| `I` / `l` | 12 小时制，两位 / 不补零 |
| `M` / `S` | 分钟 / 秒 |
| `p` / `P` | `am`/`pm` 或 `AM`/`PM` |
| `O` / `o` | 时区偏移，如 `+08:00` / `+0800` |
| `A` | 中文时段，如凌晨、上午、下午 |

### 计算器

计算器使用 `=` 作为前缀：

| 输入 | 功能 |
| --- | --- |
| `=1+2*3` | 四则运算 |
| `=(1+2)^3` | 幂运算和括号 |
| `=sqrt(2)` | 平方根 |
| `=sin(pi/2)` | 三角函数，支持 `pi`、`e` |
| `=gys(12,18,30)` | 最大公因数 |
| `=gbs(12,18,30)` | 最小公倍数 |
| `=avg(1,2,3,4)` | 平均值 |
| `=var(1,2,3,4)` | 方差 |
| `=fact(5)` | 阶乘 |
| `=zhs(5,2)` | 组合数 |
| `=pls(5,2)` | 排列数 |
| `=sjs(1,100)` | 随机数 |
| `=jzzh(255,10,16)` | 进制转换 |

计算器还继承了薄荷上游的方程、几何、数列、单位换算、24 点等高级函数，语法通常是 `=函数名(参数1,参数2,...)`。

### 数字和金额大写

金额转换使用 `R` 作为前缀：

| 输入 | 功能 |
| --- | --- |
| `R1234` | 数字小写、数字大写、金额小写、金额大写 |
| `R1234.56` | 带角分厘毫的金额转换 |

### 反查

| 输入 | 功能 |
| --- | --- |
| `Uw...` | 五笔 98 反查 |
| `Ui...` | 笔画反查，笔画键为 `h s p n z`，分别对应 `一 丨 丿 丶 乙` |
| `Uu...` | 拆字反查 |

### 符号输入

`/...` 默认是符号输入，具体可用项来自你安装的 `oh-my-rime` 的 `symbols.yaml`。常见用法包括：

| 输入 | 功能 |
| --- | --- |
| `/1`、`/2`、`/10` | 数字、序号、上下标等符号 |
| `/fh` | 常用符号 |
| `/jt` | 箭头 |
| `/sx` | 数学符号 |
| `/py` | 拼音声调字母 |
| `/zy` | 注音符号 |
| `/xz` | 星座符号 |
| `/tq` | 天气符号 |

### 快捷开关

| 快捷键 | 功能 |
| --- | --- |
| `Control+Shift+E` | Emoji 候选开关 |
| `Control+Shift+C` | 拆字滤镜开关 |
| `Control+Shift+1` | 简繁切换 |
| `Control+Shift+exclam` | 简繁切换，兼容部分平台对 `!` 的按键名 |

## 文件说明

- [double_pinyin_xiaolang.schema.yaml](./double_pinyin_xiaolang.schema.yaml): 小浪双拼主方案
- [examples/default.custom.yaml](./examples/default.custom.yaml): `schema_list` 接入示例
- [docs/xiaolang-keymap.md](./docs/xiaolang-keymap.md): 小浪双拼码表与键盘图
- [docs/assets/xiaolang-keymap.png](./docs/assets/xiaolang-keymap.png): PNG 键盘图
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
