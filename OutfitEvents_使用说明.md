# OutfitDefs.json — abilities 事件使用手册

> 适用对象：`/assets/Data/Resources/OutfitDefs.json`
> 字段：`"abilities": [ ... ]`
> 说明：`abilities` 数组的每个元素是一个能力对象，核心是 `"event"` 字段。本文列出 `OutfitAbilityEvent` 枚举中**经反编译核实、可正常加载不闪退**的 38 个取值，每个给出用法与中文功能释义。
> 逆向依据：能力元素反序列化器 `0x1adea00` + 事件派发器 `0x1e02000`。

---

## 通用结构

每个能力元素是一个 JSON 对象，可写以下键（**全部可选**，缺省安全）：

```json
"abilities": [
  {
    "event": "SocialXxx",              // 必须：行为事件
    "prop_event": "SocialYyy",         // 可选：道具形态的次级事件
    "type": "<行为类型>",               // 可选：能力大类识别码
    "sub_type": "<子类型>",            // 可选：能力子类（乐器音色/循环等）
    "icon": "<UI资源标识>",            // 可选：图标
    "unlock": "<解锁词条>",            // 可选：解锁/关系词条
    "supports_sustain": false,         // 可选：是否支持按住持续
    "cost": 0                          // 可选：每次触发消耗（整数）
  }
]
```

> 核心负载就在 `event` 本身；`type`/`sub_type`/`prop_event` 只在你确实需要"带参数的能力"时才按对应事件补齐。

---

## 38 个安全事件一览

| 事件 | 中文意思 | 是否需要额外参数 |
|---|---|---|
| `SocialApplyBuff` | 给友方/目标施加增益 | 建议 `prop_event`/`unlock` |
| `SocialArmWrestle` | 掰手腕 | 一般可不填 |
| `SocialAttitude` | 姿态动作 | 可不填 |
| `SocialBlank` | 空事件（占位，无动作） | 可不填 |
| `SocialCandle` | 举起/点亮蜡烛 | 可不填 |
| `SocialCloseMenu` | 关闭菜单 | 可不填 |
| `SocialConsumables` | 使用消耗品 | 建议 `type` |
| `SocialDance` | 跳舞 | 可不填 |
| `SocialDarkHornBlast` | 吹响暗号角 | 建议 `prop_event` |
| `SocialDarkHornMenu` | 打开暗号角菜单 | 可不填 |
| `SocialDropItem` | 丢弃道具 | 可不填 |
| `SocialEmote` | 触发表情动作 | 建议 `sub_type`（指定表情） |
| `SocialEmoteLoop` | 循环表情动作 | 建议 `sub_type` + `supports_sustain` |
| `SocialEmoteRhythmicLoop` | 节奏型循环表情（跟拍重复） | 建议 `sub_type` + `supports_sustain` |
| `SocialEndHide` | 结束隐藏状态 | 可不填 |
| `SocialFireworks` | 点燃/释放烟花 | 建议 `prop_event` |
| `SocialFireworksMenu` | 打开烟花菜单 | 可不填 |
| `SocialGift` | 赠送礼物 | 可不填 |
| `SocialGoHome` | 回家（传送回充能处） | 可不填 |
| `SocialGrapplingHookMenu` | 打开抓钩菜单 | 可不填 |
| `SocialHide` | 隐藏（隐身/藏身） | 可不填 |
| `SocialInstrument` | 演奏乐器 | 建议 `type`（乐器）/ `sub_type` |
| `SocialMuscle` | 展示肌肉/力量 | 可不填 |
| `SocialOpenConstellationFriendMenu` | 打开星座好友菜单 | 可不填 |
| `SocialOpenMenu` | 打开菜单 | 可不填 |
| `SocialPickUp` | 拾起道具 | 可不填 |
| `SocialPlaceable` | 放置/摆放可放置物 | 建议 `type`（放置物） |
| `SocialPropMenu` | 打开道具菜单 | 可不填 |
| `SocialSeek` | 寻找 / 搜索目标 | 可不填 |
| `SocialShowPresence` | 展示在线状态/存在 | 可不填 |
| `SocialSignalFlags` | 挥信号旗 / 旗语 | 可不填 |
| `SocialSitDown` | 坐下 | 可不填 |
| `SocialSparkler` | 手持仙女棒/烟花棒 | 可不填 |
| `SocialSunglasses` | 戴上太阳镜 | 可不填 |
| `SocialSurprise` | 惊讶 / 惊喜动作 | 可不填 |
| `SocialTeleportToLevel` | 传送到指定关卡/地图 | 建议 `type`（目标） |
| `SocialTeleportToSpirit` | 传送到指定先祖 | 建议 `type`（目标） |
| `SocialToysMenu` | 打开玩具菜单 | 可不填 |

---

## 逐事件用法与翻译

### `SocialApplyBuff`
> 中文：给友方 / 目标施加增益（Buff）
```json
"abilities": [
  { "event": "SocialApplyBuff", "prop_event": "SocialApplyBuff", "unlock": "<关系词条>" }
]
```

### `SocialArmWrestle`
> 中文：掰手腕（和玩家互动小游戏）
```json
"abilities": [ { "event": "SocialArmWrestle" } ]
```

### `SocialAttitude`
> 中文：姿态动作（做出特定站姿/态度）
```json
"abilities": [ { "event": "SocialAttitude" } ]
```

### `SocialBlank`
> 中文：空事件 —— 占位用，不触发任何行为
```json
"abilities": [ { "event": "SocialBlank" } ]
```

### `SocialCandle`
> 中文：举起 / 点亮蜡烛
```json
"abilities": [ { "event": "SocialCandle" } ]
```

### `SocialCloseMenu`
> 中文：关闭当前菜单
```json
"abilities": [ { "event": "SocialCloseMenu" } ]
```

### `SocialConsumables`
> 中文：使用消耗品（如食物/道具）
```json
"abilities": [ { "event": "SocialConsumables", "type": "<消耗品类型>" } ]
```

### `SocialDance`
> 中文：跳舞
```json
"abilities": [ { "event": "SocialDance" } ]
```

### `SocialDarkHornBlast`
> 中文：吹响「暗号角」（发出的音波冲击）
```json
"abilities": [ { "event": "SocialDarkHornBlast", "prop_event": "SocialDarkHornBlast" } ]
```

### `SocialDarkHornMenu`
> 中文：打开「暗号角」菜单
```json
"abilities": [ { "event": "SocialDarkHornMenu" } ]
```

### `SocialDropItem`
> 中文：丢弃手中的道具 / 物品
```json
"abilities": [ { "event": "SocialDropItem" } ]
```

### `SocialEmote`
> 中文：触发一次表情动作
```json
"abilities": [ { "event": "SocialEmote", "sub_type": "<表情标识>" } ]
```

### `SocialEmoteLoop`
> 中文：循环播放表情动作
```json
"abilities": [ { "event": "SocialEmoteLoop", "sub_type": "<表情标识>", "supports_sustain": true } ]
```

### `SocialEmoteRhythmicLoop`
> 中文：节奏型循环表情 —— 跟随节拍重复的连续动作
```json
"abilities": [ { "event": "SocialEmoteRhythmicLoop", "sub_type": "<节奏表情>", "supports_sustain": true } ]
```

### `SocialEndHide`
> 中文：结束隐藏状态（从隐形/藏身中现身）
```json
"abilities": [ { "event": "SocialEndHide" } ]
```

### `SocialFireworks`
> 中文：点燃 / 释放烟花
```json
"abilities": [ { "event": "SocialFireworks", "prop_event": "SocialFireworks" } ]
```

### `SocialFireworksMenu`
> 中文：打开烟花选择菜单
```json
"abilities": [ { "event": "SocialFireworksMenu" } ]
```

### `SocialGift`
> 中文：赠送礼物给其他玩家
```json
"abilities": [ { "event": "SocialGift" } ]
```

### `SocialGoHome`
> 中文：回家 —— 传送回自己的充能处/家园
```json
"abilities": [ { "event": "SocialGoHome" } ]
```

### `SocialGrapplingHookMenu`
> 中文：打开抓钩 / 绳钩菜单
```json
"abilities": [ { "event": "SocialGrapplingHookMenu" } ]
```

### `SocialHide`
> 中文：隐藏自己（隐身 / 蹲伏藏身）
```json
"abilities": [ { "event": "SocialHide" } ]
```

### `SocialInstrument`
> 中文：演奏乐器（如竖琴、钢琴等）
```json
"abilities": [
  { "event": "SocialInstrument", "type": "<乐器类型>", "sub_type": "<模式/音色>", "supports_sustain": true }
]
```

### `SocialMuscle`
> 中文：展示肌肉 / 力量（健美动作）
```json
"abilities": [ { "event": "SocialMuscle" } ]
```

### `SocialOpenConstellationFriendMenu`
> 中文：打开「星座」好友菜单
```json
"abilities": [ { "event": "SocialOpenConstellationFriendMenu" } ]
```

### `SocialOpenMenu`
> 中文：打开通用菜单
```json
"abilities": [ { "event": "SocialOpenMenu" } ]
```

### `SocialPickUp`
> 中文：拾起 / 捡起道具
```json
"abilities": [ { "event": "SocialPickUp" } ]
```

### `SocialPlaceable`
> 中文：放置 / 摆放可放置物（如野餐篮）
```json
"abilities": [ { "event": "SocialPlaceable", "type": "<放置物类型>", "cost": 1 } ]
```

### `SocialPropMenu`
> 中文：打开道具选择菜单
```json
"abilities": [ { "event": "SocialPropMenu" } ]
```

### `SocialSeek`
> 中文：寻找 / 搜索目标
```json
"abilities": [ { "event": "SocialSeek" } ]
```

### `SocialShowPresence`
> 中文：展示存在 —— 显示自己在线 / 让动作被看见
```json
"abilities": [ { "event": "SocialShowPresence" } ]
```

### `SocialSignalFlags`
> 中文：挥动信号旗 / 打旗语
```json
"abilities": [ { "event": "SocialSignalFlags" } ]
```

### `SocialSitDown`
> 中文：坐下
```json
"abilities": [ { "event": "SocialSitDown" } ]
```

### `SocialSparkler`
> 中文：手持仙女棒 / 烟花棒
```json
"abilities": [ { "event": "SocialSparkler" } ]
```

### `SocialSunglasses`
> 中文：戴上太阳镜（外观表现）
```json
"abilities": [ { "event": "SocialSunglasses" } ]
```

### `SocialSurprise`
> 中文：做出惊讶 / 惊喜的动作
```json
"abilities": [ { "event": "SocialSurprise" } ]
```

### `SocialTeleportToLevel`
> 中文：传送到指定关卡 / 地图
```json
"abilities": [ { "event": "SocialTeleportToLevel", "type": "<目标关卡>" } ]
```

### `SocialTeleportToSpirit`
> 中文：传送到指定先祖
```json
"abilities": [ { "event": "SocialTeleportToSpirit", "type": "<目标先祖>" } ]
```

### `SocialToysMenu`
> 中文：打开玩具选择菜单
```json
"abilities": [ { "event": "SocialToysMenu" } ]
```

---

## 注意事项

1. **只写 `event` 最稳**：除乐器、放置、Buff、传送等"带参数能力"外，表现类事件直接 `{ "event": "SocialXxx" }` 即可，不会跳错分支，也不会因缺参崩。
2. **`type` / `sub_type` / `icon` 的值**需按游戏实际资源/枚举填写，本文只确认了键名与位置。
3. **不要使用** `SocialFeed*`、`SocialRecorder*`、`SocialBlockPlayer`、带 `Rpc` 后缀等其它枚举成员——它们不是 `OutfitAbilityEvent`，写入后无效或有风险。
4. 加载阶段缺省字段是安全的（每个键都有默认值）；崩溃只发生在某个事件运行期用到了你留空的必要参数。