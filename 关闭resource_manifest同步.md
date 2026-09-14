# 关闭 resource.manifest 同步逆向补丁

## 目标

让游戏启动时跳过 `resource.manifest` 的读取与拉取，直接使用本地资源文件。

## 定位方法（适用任意版本）

1. 搜索字符串 `enable_resource_sync`（同步开关的配置键）。
2. 找它的交叉引用。AArch64 下字符串由 `adrp xN, page` + `add xN, xN, #lo12` 两条指令加载，扫描该指令对即可定位引用点。
3. 引用点落在「同步总闸函数」内。该函数读配置后决定：`true` 进入同步状态机，`false` 打印 `ResourceSync disabled` 并跳过。
4. 把函数入口改为 `mov w0, #0` + `ret`（补丁 A）；再把函数内紧跟配置读取的 `cbz w0` 改为 `nop`（补丁 B，双保险）。

## 补丁（本版实例）

| 补丁 | 文件偏移 | 原字节 | 新字节 | 汇编语义 |
| --- | --- | --- | --- | --- |
| A | `0x194B3A0` | `ff4302d1fd7b07a9` | `00008052c0035fd6` | 函数入口改为 `mov w0,#0` + `ret` |
| B | `0x194B3C8` | `c0010034` | `1f2003d5` | `cbz w0` → `nop` |

## 效果

启动日志出现 `ResourceSync disabled`，不再出现 `Syncing resources` 及下载流程。

## 提醒

改完需对 APK 重新做 v2/v3 签名。