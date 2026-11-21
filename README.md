# yiyang-jlc-eda

Yiyang 的嘉立创 EDA 专业版 Skill：将原理图和 PCB 绘制规范，与 API 网关连接、图元编辑、检查及工程导出方式放在同一个技能包中。

## 包含内容

- [设计规范与入口](SKILL.md)：选型确认、A4 模块原理图、Net 标签、专用电源符号、紧凑标注、器件对齐、逐脚丝印及手工走线要求。
- [连接与重连](references/connection.md)：探测本地桥接服务、识别 EDA 窗口、核对工程和活动文档，以及错误处理。
- [实际操作流程](references/operations.md)：器件库、符号与封装、原理图导入 PCB、布局和板框、手工布线、铺铜、DRC、网络一致性及原生工程导出。
- [API 速查](references/api-usage.md)：常用签名、单位、枚举和异步图元修改方式；接口快照基于 `easyeda-api` 1.1.36，按需核对当前官方文档。
- `scripts/eda_bridge.py`：Python 标准库客户端，执行前核对工程和文档 UUID，多窗口时要求明确目标。
- `scripts/bridge-server.mjs`：本地 API 桥接服务，监听 `127.0.0.1`。来源见 [第三方说明](THIRD_PARTY_NOTICES.md)。

禁止自动布线及自动寻路；禁止锐角走线，尽量避免直角转弯，通过微调线段减少多余拐点。几何检查基于实际铜线交点、分支及焊盘/过孔附近，不只检查脚本中的单条路径。

## 使用

将本仓库的 Skill 内容放入支持 `SKILL.md` 的工具技能目录，目录名使用 `yiyang-jlc-eda`。也可解压 [打包技能](https://github.com/AL1325651958/yiyang-jlc-eda/blob/main/dist/yiyang-jlc-eda.skill) 后使用；`.skill` 是 ZIP，解压后的顶层目录即该技能目录。

在对话中使用 `$yiyang-jlc-eda`，或明确要求按该 Skill 连接嘉立创 EDA、绘制或修改工程。开始时读取 `SKILL.md`，按任务查阅关联说明。

实际 EDA 操作需要嘉立创 EDA 专业版客户端已打开、API 网关扩展已加载。桥接服务需要 Node.js 18+，Python 客户端需要 Python 3.8+；尚无本地桥接时，在 Skill 根目录安装依赖并按 [连接说明](references/connection.md) 后台启动服务。通常先运行：

```powershell
python scripts/eda_bridge.py discover
python scripts/eda_bridge.py windows
python scripts/eda_bridge.py probe --window-id <当前连接的EDA窗口ID>
```

上述示例的窗口 ID 由 `windows` 返回，不能照抄旧工程。桥接已经运行时复用现有服务，无需再次启动。

本仓库交付可复用 Skill，不包含某块板的工程、会话快照、账号信息或固定窗口/工程 ID。

## 更新打包

```powershell
python scripts/package_skill.py --output dist/yiyang-jlc-eda.skill
```

打包脚本仅收录 Skill、说明、运行依赖清单及脚本，不收录 `.git`、`node_modules`、日志、会话快照或旧技能包。修改后应校验技能入口和引用，运行受影响脚本的实际检查，再重新打包。
