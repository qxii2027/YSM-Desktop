# YSM 桌面宠物

把 Yes Steve Model（YSM）的 `.ysm` 玩家模型投影到 Windows 桌面，做成可以自动行动、可以拖动、可以飞行、可以右键播放动作的桌宠。

本项目不修改 WindowX / YSM 本体；它是一个伴生 NeoForge mod + 一个托盘启动器。

## 运行环境

- Windows 10 / 11 x64
- Minecraft 1.21.1
- NeoForge 21.1.265
- [WindowX](https://space.bilibili.com/1703785283) 2.1.10（桌宠投影窗口）
- Yes Steve Model 2.6.5（模型与动作）
- 可选：PCL2 启动器（本项目的启动器自带离线启动，不依赖 PCL）

## 快速开始

1. 确认实例的 `mods` 目录已经有：
   - `windowx-2.1.10-full-1.21.1-NeoForge.jar`
   - `ysm-2.6.5-neoforge+mc1.21.1-release.jar`
2. 在本项目目录运行安装脚本（需要 PowerShell）：
   ```powershell
   powershell -ExecutionPolicy Bypass -File scripts\install.ps1
   ```
   脚本会把 `dist\ysm-desktop-pet-1.0.0.jar` 复制到实例 `mods\`，把启动器复制到 `%LOCALAPPDATA%\YSMDesktopPet\`。
3. 运行 `%LOCALAPPDATA%\YSMDesktopPet\YSMDesktopPet.exe`。
4. 右键任务栏托盘图标 → **启动游戏**。
5. 游戏会自动创建并进入 `YSM_Desktop_Pet` 空世界；主游戏窗口移到屏幕外，桌宠出现在桌面底部。
6. 右键桌宠 → **模型** → 选择你的 `.ysm` 模型。选择会保存在该世界中。
7. 需要开机自启时，托盘 → **设置** → 勾选“开机自动运行本启动器”和“登录后等待延迟自动启动游戏”。

> 启动器使用离线用户名 `DesktopPet`（可在设置里修改），与 PCL 的 LittleSkin 账号互不影响。单机 YSM 模型选择按玩家 UUID 保存，换用户名会重新选择模型。

## 操作

| 操作 | 说明 |
|---|---|
| 左键单击桌宠窗口 | 让桌宠窗口获得焦点；之后方向键可用，鼠标可以移开 |
| ← / → | 桌宠在桌面上左右移动；角色自动转向并播放走路动画 |
| 双击 ← 或 → | 疾跑（同一方向快速双击；窗口速度约 1.7 倍，角色切到跑步动画） |
| ↑（非飞行） | 地面小跳 |
| ↑ / ↓（飞行） | 上升 / 下降 |
| 右键桌宠 | 在桌宠旁边打开独立的动作菜单窗口（不打开游戏窗口，桌宠仍然可见） |
| 菜单右上角“退出” | 第一次点击变成“确认”，3 秒内再点一次退出游戏 |
| 左键拖动桌宠 | 自由拖动；松手后非飞行状态会坠落到地面 |
| Esc | 关闭菜单 / YSM 原生界面 |
| `Ctrl+Alt+P` | 显示 / 隐藏桌宠（恢复游戏窗口） |
| `Ctrl+Alt+O` | 停止桌宠并恢复游戏窗口 |
| `Ctrl+Alt+R` | 随机播放一个动作 |
| `Ctrl+Alt+F` | 切换飞行状态 |

动作菜单底部：随机 / 停止 / 设置 / 模型（YSM 原生模型选择）/ 自动:开/关 / 飞行:开/关 / 关闭。

菜单会跟随桌宠的位置出现在它上方（空间不足时在下方），并自动夹在工作区内，不会跑到屏幕外。界面为扁平简约风格：浅灰白纯色面板、细边框、深色文字，无渐变、无阴影、无彩色高亮，避免整块黑色面板。

YSM 的二级菜单会直接切换自制菜单页面（带“返回”）。动作菜单里的“配置”卡片会打开自制的模型设置页，按模型自带的 `ysm.json` 配置表单生成复选框、滑杆和下拉框；修改会立即写入 YSM 并保存。设置页底部还有“全局设置”，提供 YSM 客户端的全部常用选项（动画音量、自身/他人模型、手部、投掷物、载具、第一人称动画、加载界面、兼容渲染器、优先显示模型 ID 等）。“模型”按钮打开 YSM 原生模型列表（搜索、皮肤、下载、预览等原生功能全部保留），关闭后回到桌宠菜单。

## 窗口与虚拟桌面

- 进入桌宠模式后，Minecraft 主窗口会移到屏幕外，并从任务栏隐藏；任务栏只保留托盘启动器。
- 桌宠窗口和动作菜单窗口同样不进入任务栏。
- 桌宠使用独立的 AppUserModelID（`YSM.DesktopPet`）并固定到所有虚拟桌面：用 `Win+Tab` 切到其他桌面时，桌宠仍然显示在同一位置；菜单窗口也随应用一起固定。
- 退出桌宠 / 恢复游戏窗口时，主窗口的任务栏样式和位置会还原。

## 桌面物理

- 桌宠窗口的底部就是角色的脚底。
- 进入桌宠模式时游戏音乐音量自动设为 0，退出/恢复游戏窗口时还原。
- 非飞行时受重力影响，会落到“地面”。
- 地面位置由配置 `groundMargin` 控制：窗口底部距离工作区底部（任务栏上沿）的像素数，默认 `0`。
- 拖动窗口到高处后松手：
  - 非飞行：坠落回地面；
  - 飞行：停在松手位置。
- 空闲时视线跟随鼠标；播放 YSM 动作期间视线交给动画，不再追踪鼠标；停止动作或开始行走后恢复。
- 飞行状态使用 Minecraft 创造模式飞行，会触发 YSM 的飞行动画；关闭飞行后回到生存模式。

## 大模型适配

- 默认开启 `autoFitModel`：读取 YSM 模型的 `height_scale` / `width_scale`，自动调整相机正交视野，让高/宽模型完整显示。
- 如果某个模型仍然被裁切：
  - 在启动器 **设置 → 桌宠** 中关闭“自动适配模型大小”，把“正交半高”调大（例如 `2.5` 或更高）；
  - 或直接编辑实例目录 `config/ysm_desktop_pet.json` 的 `orthoHalfHeight`。
- 窗口尺寸（`windowWidth` / `windowHeight`）修改后，需要重新进入桌宠（停止后再次开启）才生效；正交半高会自动热更新。

## 配置文件

实例目录：`config/ysm_desktop_pet.json`

```json
{
  "enabled": true,
  "petWorld": "YSM_Desktop_Pet",
  "autoOpenPetWorld": true,
  "windowWidth": 300,
  "windowHeight": 420,
  "menuWidth": 960,
  "menuHeight": 540,
  "orthoHalfHeight": 1.25,
  "autoFitModel": true,
  "autoMove": true,
  "randomAnimationMinSeconds": 180,
  "randomAnimationMaxSeconds": 300,
  "flying": false,
  "gravity": 2400.0,
  "maxFallSpeed": 2200.0,
  "flySpeed": 340.0,
  "walkSpeedPx": 150.0,
  "sprintMultiplier": 1.7,
  "jumpSpeed": 700.0,
  "groundMargin": 0,
  "dragEnabled": true,
  "gazeFollowMouse": true,
  "hideMainWindow": true,
  "mainWindowWidth": 1280,
  "mainWindowHeight": 720,
  "ipcPort": 25566
}
```

修改后可以在托盘里点“设置”保存，或重启桌宠；窗口尺寸和飞行启动状态需要重新进入桌宠，正交半高、地面、重力、拖动、视线会热更新。

## 启动器

- 位置：`%LOCALAPPDATA%\YSMDesktopPet\YSMDesktopPet.exe`
- 设置保存在：`%LOCALAPPDATA%\YSMDesktopPet\launcher.json`
- 启动日志：`%LOCALAPPDATA%\YSMDesktopPet\launcher.log`
- 托盘菜单：启动游戏 / 显示隐藏桌宠 / 打开动作菜单 / 切换飞行 / 恢复游戏窗口 / 打开桌宠配置 / 设置 / 退出
- 开机自启写入：`HKCU\Software\Microsoft\Windows\CurrentVersion\Run\YSMDesktopPet`
- 与 mod 的本地控制通道：`127.0.0.1:25566`（命令 `pet` / `menu` / `fly` / `restore` / `reload` / `quit`）

## 日志与排查

- mod 日志：实例目录 `logs/latest.log`，搜索 `[YSM Pet]`。
- 启动器日志：`%LOCALAPPDATA%\YSMDesktopPet\launcher.log`。
- 桌宠窗口没出现：
  - 确认 WindowX 2.1.10 已加载，日志里有 `[YSM Pet] window created`；
  - 确认 YSM 2.6.5 已加载，日志里有 `[YSM Pet] YSM bridge initialised: true`；
  - 世界名必须是 `YSM_Desktop_Pet`（或修改配置 `petWorld`）。
- 右键菜单打不开：先左键点一下桌宠窗口让它获得焦点，再右键。
- 主游戏窗口不见了：托盘 → **恢复游戏窗口**，或按 `Ctrl+Alt+O`。
- 桌宠动作列表为空：模型可能还在加载/同步，等几秒再打开菜单；或在 YSM 模型界面里确认动作配置。
- 想用 PCL / LittleSkin 账号：可以照常用 PCL 启动并进入 `YSM_Desktop_Pet`；mod 会在该世界中启用桌宠。但需要保证 `pauseOnLostFocus=false`（mod 进入桌宠时会自动设置）。

## 卸载

```powershell
powershell -ExecutionPolicy Bypass -File scripts\uninstall.ps1
```

默认只删除 mod jar 和自启 registry 值，保留启动器文件和桌宠世界。需要一并删除启动器/世界时：

```powershell
powershell -ExecutionPolicy Bypass -File scripts\uninstall.ps1 -RemoveLauncher -RemovePetWorld
```

## 二次分发

生成分发包：

```powershell
powershell -ExecutionPolicy Bypass -File scripts\package.ps1
```

输出：`dist\YSMDesktopPet-1.0.0.zip`，包含：

- `ysm-desktop-pet-1.0.0.jar`：本 mod
- `YSMDesktopPet.exe`：自包含启动器（不需要目标机器安装 .NET）
- `install.ps1`：交互式安装脚本
- `README.md`、`分发说明.txt`

分发包**不包含** WindowX 和 YSM，因为它们是第三方 mod。接收方需要：

1. 准备 Windows 10/11 x64、Java 21，以及 Minecraft 1.21.1 + NeoForge 21.1.265 的 PCL 实例（建议开启“版本隔离”）。
2. 先把 WindowX 2.1.10 和 YSM 2.6.5 放入同一个实例的 `mods` 目录。
3. 解压分发包，运行：
   ```powershell
   powershell -ExecutionPolicy Bypass -File .\install.ps1
   ```
   脚本会询问实例目录，自动识别版本 ID，把 mod 放进 `mods`，把启动器装到 `%LOCALAPPDATA%\YSMDesktopPet`，并写入 `launcher.json`。
   也可以直接指定：
   ```powershell
   powershell -ExecutionPolicy Bypass -File .\install.ps1 -InstanceDir "D:\...\versions\1.21.1-NeoForge_21.1.265"
   ```
4. 运行 `%LOCALAPPDATA%\YSMDesktopPet\YSMDesktopPet.exe`，托盘右键 → 启动游戏。
5. 在启动器“设置”里可以改实例目录、`.minecraft` 目录、Java、版本 ID、离线用户名和开机自启。

注意：

- **不要未经授权把 WindowX / YSM 的 jar 打进自己的分发包**；让接收方自行安装，或先取得作者许可。
- 本启动器按 PCL“版本隔离”结构工作：实例目录同时作为游戏目录。若使用官方启动器或未隔离的 `.minecraft`，需要把实例目录和 `.minecraft` 目录分别填成版本目录和 `.minecraft`；当前启动器以“实例目录中的版本 JSON + 同一目录作为游戏目录”为准。
- 版本目录被重命名也能启动：启动器会扫描实例目录中的版本 JSON；也可在设置里手动填写“版本 ID”。
- 需要便携/绿色版时，可设置环境变量 `YSMPET_LAUNCHER_HOME` 指向自定义配置目录，启动器会把 `launcher.json`、日志和 natives 放在那里。
- 本项目绑定 YSM 2.6.5 的混淆类名；升级 YSM 后需要同步更新 `YsmBridge`，否则动作/设置功能会失效。

## 从源码构建

```powershell
# 1) 构建 mod（需要本机已有 1.21.1 NeoForge 实例）
powershell -ExecutionPolicy Bypass -File mod\build.ps1

# 2) 构建启动器
cd launcher
dotnet publish -c Release -r win-x64 --self-contained true -o ..\dist\launcher

# 3) 安装
powershell -ExecutionPolicy Bypass -File scripts\install.ps1
```

构建脚本默认通过 `build\instance` / `build\mc` 目录联接访问本机实例；也可以通过 `YSMPET_INSTANCE_DIR` / `YSMPET_MC_DIR` 环境变量指定。

## 已知限制

- 只在主显示器工作区内活动。
- 游戏音乐只在桌宠模式期间静音；退出桌宠后恢复原音量。
- 疾跑使用与 MC 原版一致的双击方向键手势；窗口移动速度按 `sprintMultiplier` 提升。
- 方向键只在桌宠窗口获得焦点时生效：点一下桌宠让它获得焦点，之后鼠标可以移开；点击其他窗口即失焦，方向键不会移动桌宠。
- YSM 反射绑定 2.6.5；升级 YSM 后如动作菜单失效，需要更新 `YsmBridge` 中的混淆类名/方法名。
- 桌宠世界为创造/生存切换使用；飞行状态会切换游戏模式，介意时可以只用非飞行模式。
- 原生 YSM 模型选择界面由 YSM 提供；模型应用方式以 YSM UI 为准。
