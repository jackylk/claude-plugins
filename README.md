# Claude Plugins — UI Skills

Claude Code skills for building enterprise-grade web consoles.

## Skills

### huawei-cloud-style
Complete Huawei Cloud console design system — layout, colors, typography, navigation, tables, cards, buttons, forms, and login pages. Use when building any management dashboard or admin panel.

### sre-console
SRE operations console template with standard 13-page structure: overview, user/pod/model/invite management, metrics, logs, alerts, cloud resources, cost monitoring, infrastructure, audit logs, and system status. Depends on `huawei-cloud-style` for visual design.

## Installation

```bash
/plugin marketplace add MatrixDriver/claude-plugins
/plugin install jacky-ui-skills
```

Or test locally:
```bash
claude --plugin-dir ~/code/claude-plugins
```

## Usage

Tell Claude:
- "用华为云风格创建一个管理后台" → triggers `huawei-cloud-style`
- "创建一个SRE运维控制台" → triggers `sre-console`
