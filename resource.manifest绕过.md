0.32.2演示，别的版本同理
分析完成。这个  libBootloader.so （游戏为 Sky/TGC， com/tgc/sky/SystemIO_android ）里有一个"资源同步"（ResourceSync）工作线程，负责读取  resource.manifest 、向服务器拉取新清单并下载资源包。我已经定位到它的总开关，并生成了一份绕过补丁。
 
核心结论
 
决定"是否读取 resource.manifest"的不是清单文件本身，而是一个运行时配置键  enable_resource_sync ，以及一个总闸函数：
 
总闸函数  0x194f3a0 （反编译）：读取配置  enable_resource_sync 
​
配置为  true （含未设置时的默认路径）→ 构建 ResourceSync Worker、返回  1  → 引导状态机进入 state  0xb （执行清单同步）
​
配置为  false  → 打印日志  "ResourceSync disabled" 、返回  0  → 状态机进入 state  0x18 （跳过同步，直接用本地文件继续启动）
​
调用点  0xc0d77c （主引导状态机）：plaintext
  



bl 0x194f3a0         ; 判断是否同步
tst w0, #1
csel w8, w9, w8, ne  ; w8 = 0xb(同步) 或 0x18(跳过)
 
​
同步 Worker 本体在  0x22c1fb8 ~ 0x22c3e44 ，其中真正读清单的函数（ LoadCurrentManifest ）先拼  Data/%s%s  得到  Data/resource.manifest ，失败时还会拼  Data/Manifests/%s/%s ；然后  "Syncing resources"  → 拉  .new  清单 → 反序列化 → 对比 → 下载/删除资源。
 
也就是说：只要让  enable_resource_sync  走 "disabled" 分支，游戏就不会再读、也不会再拉取  resource.manifest ，而是直接用本地已有的资源文件启动。
 
两种改法
 
方案一：改配置（非侵入，最推荐）
 
把  enable_resource_sync  设为  false 。它是通过通用 config-map 查询的布尔键，若游戏允许本地/远程配置覆盖，直接置  false  即可看到日志  ResourceSync disabled 。若该值来自服务端远程配置而无法本地覆盖，则用下面的二进制补丁强制。
 
方案二：二进制补丁（强制生效，已帮你打好）
 
位置 VA 文件偏移 原字节 改后字节 作用 
A  0x194f3a0   0x194B3A0   ff4302d1fd7b07a9   00008052c0035fd6  函数入口直接  mov w0,#0; ret ，永远返回"禁用" 
B  0x194f3c8   0x194B3C8   c0010034   1f2003d5  把  cbz  分支 NOP 掉， true  时也落入"禁用"路径 
 
其中 Patch A 是主补丁（8 字节），Patch B 是双保险（4 字节），两者都已应用并通过校验。
 
