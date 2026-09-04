# CodeLab 官方插件仓库

CodeLab（代码实验室）官方插件集合。对标 TrieCode 插件生态，所有插件通过 `manifest.json` 声明能力，宿主统一分发渲染——UI 与 AI 同源获得插件能力。

## 已发布插件

| 插件 | 版本 | 说明 |
|------|------|------|
| [arduino-cli-toolchain](./arduino-cli-toolchain) | 0.4.0 | Arduino（AVR / ESP32 / ESP8266）编译烧录、板卡与库管理、串口监视 |

## 插件结构

```
my-toolchain/
├── manifest.json    # 插件声明（必填）
├── index.ts         # 插件入口（backend 命令注册）
├── ui/              # Web 视图页面（可选）
└── assets/          # 插件自带资源（可选）
```

## manifest.json 核心字段

```json
{
  "id": "arduino-cli-toolchain",
  "name": "Arduino CLI 工具链",
  "version": "0.4.0",
  "developer": "CodeLab",
  "icon": "🔌",
  "capabilities": {
    "compile": { "transport": "backend", "service": "toolchain", "method": "compile" },
    "upload":  { "transport": "backend", "service": "toolchain", "method": "upload" }
  },
  "devicePanel": {
    "selectors": [
      { "id": "board", "title": "开发板", "source": { "transport": "backend", "service": "toolchain", "method": "boardSearch" }, "searchable": true, "key": "fqbn" },
      { "id": "port",  "title": "端口",   "source": { "transport": "backend", "service": "serialport", "method": "list" }, "key": "port" }
    ],
    "actions": [
      { "id": "open-monitor", "title": "串口监视器", "internal": { "service": "serialLog", "method": "open" } }
    ]
  }
}
```

## 七类通用通道

| transport | 说明 |
|-----------|------|
| `cli` | 一次性命令行（spawn 数组参数防注入） |
| `http` | REST API（全局 fetch，尊重代理） |
| `grpc` | 插件自带 .proto，宿主动态加载 |
| `mcp` | 调用 MCP 服务器工具 |
| `mqtt` | 通用 MQTT 设备通道 |
| `internal` | 宿主标准能力（串口 / 日志 / 状态 / Arduino） |
| `backend` | 调用插件自声明的 stdio 后端（JSON-RPC） |

## 安装方式

1. 插件市场一键安装
2. 设置 → 插件 → 从文件夹安装

## 开发你的插件

参考 [arduino-cli-toolchain](./arduino-cli-toolchain) 的 `manifest.json` 和 `index.ts`，声明能力后宿主自动渲染设备面板、编译/烧录按钮与 AI 工具。

---

© 2026 BlueCode 代码实验室 · 通用软件开发智能体
