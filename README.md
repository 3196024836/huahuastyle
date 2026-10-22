<div align="center">

# Huahua Style

**让 AI 按你的规矩协作，把改动做清楚、边界守清楚、结果验清楚。**

面向 AI 编程助手的个人开发规范 Skill，以 C# / Unity 为主要代码风格参考，覆盖文件授权、代码组织、工具使用、美术制作与 UI 验收流程。

[![版本](https://img.shields.io/github/v/release/3196024836/huahuastyle?color=166534&label=version)](https://github.com/3196024836/huahuastyle/releases)
[![类型](https://img.shields.io/badge/Agent_Skill-instruction--only-2563eb)](SKILL.md)
[![代码参考](https://img.shields.io/badge/Code-C%23_%2F_Unity-52525b)](#代码与工程规范)

[功能](#功能) · [安装](#安装) · [使用](#使用) · [适用条件](#适用条件) · [规范全文](SKILL.md)

</div>

---

## 它解决什么问题

AI 写代码时，麻烦往往不只是语法错误：没问就新建文件、顺手重构无关代码、工具断线后绕过检查继续改，或者只看静态代码就声称“运行正常”。

Huahua Style 把这些协作约定写进一份可复用的 `SKILL.md`：先看现状和授权，再做必要的改动，最后用匹配的证据交付。它是一套有明确个人偏好的工作规范，不是自动格式化器、编译器或权限沙箱。

- **仓库名：** `huahuastyle`
- **Skill 内部名称：** `huahua-code-style`，安装目录和显式调用沿用这个名称。
- **内容形式：** 英文规范正文，默认要求简洁中文代码注释；本首页使用中文。

## 功能

### 文件与协作边界

- 修改前检查既有代码和用户改动，避免无关重构。
- 新建、删除、移动、复制、重命名文件均受当前任务授权约束。
- 新增内容先列完整路径、用途和范围，同一范围已授权后不重复询问。
- 建议不等于执行授权；长期规则变更先说明，再经用户确认。

### 代码与工程规范

- 以 C# 为主要参考：脚本布局、命名、`using` 分组、换行与大括号规则。
- 类、方法、参数与返回值使用准确的 XML 文档；变量和功能分段使用简洁注释。
- Inspector 序列化字段优先用 `[Tooltip]`，避免重复说明。
- 单一职责，稳定共性进入共享层；不过早抽象，不让其他脚本直接改写内部字段。
- 区分必需配置缺失、可选表现缺失和普通业务分支，不堆叠无意义的空值检查。

### Unity 与联机开发

- 开始或恢复 Unity 开发前，通过实际 MCP 调用确认目标工程身份和连接有效性。
- 断线时保留进度、暂停开发操作，在现有授权内诊断重连；写入结果不明时先核实，不盲目重放。
- 本地玩家读取输入；远端不执行本地相机、光标锁定或输入逻辑。
- 共享联机结果由服务器验证和决定，仅在确实需要网络能力时使用 `NetworkBehaviour`。
- 修改后按风险检查编译、场景挂载和实际运行，静态检查不能替代运行验证。

### 美术与 UI 流程

- 禁用桌面和浏览器电脑控制自动化，使用文件、API、CLI 和 MCP。
- 模型优先使用 Blender MCP；原规范也定义了受授权约束的 Blender CLI 后备路径。
- 图片生成与编辑使用原规范指定的 `image-gen-pro` 工作流，不静默替换模型或供应商。
- UI 按“概念确认 → 独立素材 → 网页预览迭代 → 目标栈实现 → 实际验收”推进。
- 每张新 UI 图片独立生成，不从概念图或素材拼板裁切生产素材；动态文字留在 UI 层。

## 安装

以下为 Codex 本地 Skill 目录安装方式。需要 Git；安装前先阅读 [规范全文](SKILL.md)，确认其中的工具偏好和文件授权规则适合你。

### Windows · PowerShell

```powershell
# 用户级安装：供当前用户的多个项目使用。
$skillPath = Join-Path $HOME '.agents/skills/huahua-code-style'
if (Test-Path -LiteralPath $skillPath) {
    throw "目标已存在，请先检查，避免覆盖：$skillPath"
}
New-Item -ItemType Directory -Path (Split-Path -Parent $skillPath) -Force | Out-Null
git clone https://github.com/3196024836/huahuastyle.git $skillPath
```

### macOS / Linux · Shell

```sh
# 用户级安装；目标目录已存在时停止，不覆盖已有内容。
(
  skill_path="$HOME/.agents/skills/huahua-code-style"
  if [ -e "$skill_path" ] || [ -L "$skill_path" ]; then
    printf '%s\n' "目标已存在，请先检查：$skill_path" >&2
    exit 1
  fi
  mkdir -p "$HOME/.agents/skills" &&
    git clone https://github.com/3196024836/huahuastyle.git "$skill_path"
)
```

**只在一个项目使用：** 将仓库克隆到该项目的 `.agents/skills/huahua-code-style`，而不是用户级目录；先检查目标不存在。不要同时安装同名的用户级和项目级副本。

Codex 会检测本地 Skill 变更；若列表未刷新，可重启后再检查。目录发现和元数据格式参见 [Codex 官方 Skill 文档](https://developers.openai.com/codex/skills)。其他支持 `SKILL.md` 的助手可按各自的安装方式加载，具体工具接入需要另外配置。

## 使用

安装后，在任务中显式提到 Skill：

```text
请使用 $huahua-code-style，先检查现有代码和改动，再在本次授权范围内修复这个问题。
```

也可以缩小范围，给出清楚的修改权限：

```text
请使用 $huahua-code-style，只修改现有的 PlayerInteraction.cs。
修复远端玩家误读本地输入的问题，保留已有改动；如需新增文件，先说明路径和用途。
完成后区分编译检查、实际运行和未验证部分。
```

助手读取规范后仍需遵守所在环境的上级指令、项目规则与实际权限。发生冲突时应指出并确认，不能靠本 Skill 扩大授权。

## 适用条件

**适合：** 希望长期保持个人开发习惯、重视文件边界与验证证据，尤其主要使用 C# / Unity 的开发者。

**采用前应了解：**

- 这是一套 Huahua 的个人偏好，不宣称是所有语言或团队的通用最佳实践。例如运行时引用查找、变量注释密度、工具选型都有明确取向。
- `SKILL.md` 本身不包含执行脚本，不会安装或启动 Unity、Blender、MCP 服务或图片生成服务，也不会自行发起 API 请求；被助手采用后，相关工具操作仍可能产生服务费用。
- 通用协作与代码规范可单独使用；只有执行 Unity、联机、美术或 UI 任务时才启用对应部分。
- `unity:*`、Blender MCP 和 `image-gen-pro` 是原规范中的外部依赖，本仓库不捆绑这些工具。缺少它们时应报告受影响的任务，不假装已经连接或完成。
- 本次公开发布保留原规范正文。安装文档与结构检查不代表已完成各助手、Unity 或外部服务的端到端兼容测试。

## 文件结构

```text
huahuastyle/
├── SKILL.md              # 规范正文，内部名称 huahua-code-style
├── agents/openai.yaml    # Skill 显示名称、简介和默认提示
├── README.md             # 功能、安装、示例和适用条件
├── CHANGELOG.md          # 公开版本记录
├── CONTRIBUTING.md       # 反馈与变更约定
├── .gitattributes        # 文本格式约定
└── .gitignore            # 本地和敏感文件过滤
```

## 更新与反馈

有本地定制时，更新前先检查差异，不要用强制重置覆盖自己的规则。查看 [版本记录](CHANGELOG.md) 和 [发布页](https://github.com/3196024836/huahuastyle/releases)。

通过 [Issues](https://github.com/3196024836/huahuastyle/issues) 提交问题；请附使用场景、预期与实际行为，并移除密钥、私人路径和项目机密。变更约定见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可

本仓库公开展示，当前未附开源许可证。公开可见不等于授予复制、修改或再分发许可；如需这些权利，请先联系维护者确认。
