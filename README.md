# hcloud — Claude Code Skills for Huawei Cloud

Claude Code skills for operating Huawei Cloud services and building enterprise consoles.

## Skills

### hcloud
Operate Huawei Cloud services using hcloud CLI (KooCLI). Covers 150+ services including ECS, CCE, VPC, ELB, RDS, OBS, IAM, SWR, DNS, NAT, EIP, CES, and more. Includes safety rules for destructive operations and dynamic API discovery via `--help` / `--skeleton`.

### huawei-cloud-style
Complete Huawei Cloud console design system — layout, colors, typography, navigation, tables, cards, buttons, forms, and login pages. Use when building any management dashboard or admin panel.

### sre-console
SRE operations console template with standard 13-page structure: overview, user/pod/model/invite management, metrics, logs, alerts, cloud resources, cost monitoring, infrastructure, audit logs, and system status. Depends on `huawei-cloud-style` for visual design.

## Installation

```bash
/install jackylk/claude-plugins
```

## Usage

Tell Claude:
- "帮我查一下华为云的 ECS 实例" → triggers `hcloud`
- "用华为云风格创建一个管理后台" → triggers `huawei-cloud-style`
- "创建一个SRE运维控制台" → triggers `sre-console`
