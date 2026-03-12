---
name: sre-console
description: Build an SRE operations console (运维控制台) with standard menu structure and pages. Use this skill when the user wants to create an SRE console, operations dashboard, admin panel for infrastructure management, or any monitoring/management console for a cloud-native application. Triggers on phrases like "SRE控制台", "运维控制台", "operations console", "admin console for infrastructure", or when building a management interface that needs metrics, logs, alerts, cloud resources, or user management pages. This skill depends on the huawei-cloud-style skill for visual design.
---

# SRE Console Template

This skill defines the standard structure for an SRE operations console. It specifies what pages to build, what data each page shows, and how the navigation should be organized.

**Prerequisite**: This skill uses the `huawei-cloud-style` skill for all visual design decisions (colors, typography, tables, layout). Read that skill first for styling guidance.

## Standard Menu Structure

The SRE console uses grouped sidebar navigation with 4 groups:

```typescript
const NAV_GROUPS = [
  {
    group: null,  // No header for top-level
    items: [{ label: "总览", path: "/sre" }],
  },
  {
    group: "业务管理",
    items: [
      { label: "用户管理", path: "/sre/users" },
      { label: "Pod 管理", path: "/sre/pods" },
      { label: "模型配置", path: "/sre/models" },
      { label: "邀请码", path: "/sre/invites" },
    ],
  },
  {
    group: "监控运维",
    items: [
      { label: "指标", path: "/sre/metrics" },
      { label: "日志", path: "/sre/logs" },
      { label: "告警", path: "/sre/alerts" },
    ],
  },
  {
    group: "系统管理",
    items: [
      { label: "云资源", path: "/sre/cloud" },
      { label: "成本监控", path: "/sre/cost" },
      { label: "基础设施", path: "/sre/infra" },
      { label: "审计日志", path: "/sre/audit" },
      { label: "系统状态", path: "/sre/system" },
    ],
  },
];
```

Adapt the menu items to the specific application. The group structure and naming conventions should remain consistent. If the application doesn't use Pods or Lobsters, replace those items with equivalent resource management pages.

## Page Specifications

### 1. Overview (总览) — `/sre`

The landing page provides an at-a-glance summary.

**Components:**
- **Metric cards** (grid, 4 columns): registered users, active pods/instances, available models/services, remaining invites/quotas
- **Recent users table** (left half): username, auth provider nickname, registration time, resource status
- **Resource status table** (right half): resource ID, owner, status (dot indicator), type

**Key patterns:**
- Use `Promise.all` to load stats, users, and resources in parallel
- Show only the 5 most recent entries in each table
- Format dates with `toLocaleString("zh-CN")`

### 2. User Management (用户管理) — `/sre/users`

Full user CRUD with role management.

**Columns:** username, auth provider name, registration time, has resource (yes/no badge), is admin (yes/no badge), actions
**Actions:** toggle admin role, delete user, delete user's resource
**Patterns:**
- Row hover effect
- Confirmation dialog before destructive actions
- Badge colors: green (#52c41a/#f6ffed) for yes, red (#e6393d/#fff2f0) for no, blue (#0073e6/#e6f4ff) for admin

### 3. Pod/Instance Management (Pod 管理) — `/sre/pods`

Resource instance management with status filtering.

**Filter bar:** status tabs (All, Running, Creating, Stopped)
**Columns:** instance ID (monospace), owner, status (dot indicator), model/type, IP, creation time, actions
**Actions:** delete instance
**Footer:** total count display

### 4. Model/Service Configuration (模型配置) — `/sre/models`

CRUD for available service configurations.

**Header:** title + "Add" button (brand red)
**Columns:** ID, name, model ID (monospace), default badge, availability (dot indicator), sort order, actions (edit/delete)
**Modal dialog:** for create/edit with form fields
**Modal style:** fixed overlay, centered card (480px), white background, shadow

### 5. Invite Codes (邀请码) — `/sre/invites`

Invite code generation and management.

**Header:** title + "Generate" button (brand red)
**Columns:** code (monospace, bold), creator, used by, used time, status badge, actions (revoke)
**Patterns:** prompt() for count input, alert() for showing generated codes

### 6. Metrics (指标) — `/sre/metrics`

Application and system performance metrics.

**Components:**
- **Stat cards:** request rate (req/s), P95 latency (ms), error rate (%), total requests
- **Top endpoints table:** endpoint (monospace), request count, avg latency
- **Business metrics cards:** total users, new today, total resources, active instances, channels
- **System resources:** CPU/memory/disk progress bars
- Refresh button in header

### 7. Logs (日志) — `/sre/logs`

API request log viewer with filtering, pagination, and detail panel.

**Filter bar:** endpoint search input + search button
**Columns:** endpoint (monospace), tenant, space, status code (color-coded), response time (ms), timestamp, actions (detail link)
**Pagination:** prev/next buttons with "page N of total" display
**Detail side panel** (slides in from right, 480px):
- Two tabs: "请求详情" and "响应详情"
- Request tab: status badge (SUCCESS/ERROR), duration, tenant, space, log ID, endpoint, status code, timestamp, request headers (formatted JSON), request body (formatted JSON)
- Response tab: response headers (formatted JSON), response body (formatted JSON)
- JSON formatter: `try { JSON.stringify(JSON.parse(raw), null, 2) } catch { raw }`

**Backend**: Middleware captures request/response headers+body (10KB max, sensitive headers filtered), stored in usage_logs table. Admin API joins with tenant/space for display names.

### 8. Alerts (告警) — `/sre/alerts`

Alert rule management and history.

**Two sections:**

**Alert Rules table:**
- Columns: rule name (editable), metric (monospace), condition, cooldown (editable), webhook URL (editable), enabled toggle, test button
- Click-to-edit pattern for inline editing (dashed underline, click opens input)

**Alert History table:**
- Columns: rule name, severity badge, message, status badge, fired time, resolved time
- Severity styles: critical (red), warning (orange), info (gray)
- Status styles: firing (red), resolved (green)

### 9. Cloud Resources (云资源) — `/sre/cloud`

Architecture visualization and resource inventory.

**Components:**
- **Architecture diagram:** flow visualization using Box + Arrow components showing the deployment topology
- **Resource table:** name, region, service type, resource type (monospace), status (dot), spec, **console link** (opens cloud provider console in new tab)
- Include external services (databases, SaaS) as additional rows
- Each service type maps to its provider's console URL

### 10. Cost Monitoring (成本监控) — `/sre/cost`

Cost estimation and breakdown (self-calculated, not from billing API).

**Components:**
- **Overview cards:** hourly/daily/monthly cost estimates
- **Breakdown table:** resource type, description, hourly/daily/monthly costs
- **Trend chart:** 30-day line chart (canvas-based)
- **User allocation table:** username, runtime hours, estimated cost
- Null-safe rendering: always use `?.toFixed() ?? "-"`

### 11. Infrastructure (基础设施) — `/sre/infra`

Physical/virtual infrastructure monitoring.

**Components:**
- **Control plane section:** CPU/memory/disk progress bars for main server
- **Container resources table:** pod name (monospace), owner, CPU cores, memory MB, status (dot)

### 12. Audit Log (审计日志) — `/sre/audit`

Full API request audit trail with filtering and pagination.

**Filter bar:** username input, method select (GET/POST/PATCH/PUT/DELETE), path input, status code input, datetime range inputs, search button
**Columns:** timestamp, username, method (colored badge), path (monospace), status code (colored by range), IP, duration (ms)
**Method badge colors:** GET=#0073e6, POST=#52c41a, PATCH=#e37318, PUT=#722ed1, DELETE=#e6393d
**Pagination:** prev/next buttons with "page N of total" display

### 13. System Status (系统状态) — `/sre/system`

Health check and system information.

**Components:**
- Health status indicator (large dot + text)
- System info cards: version, uptime, database status, etc.
- Simple key-value display format

## Backend Integration Pattern

### API Client

Create a centralized API client (`sre-client.ts`):
```typescript
const API_BASE = "/api/proxy";

function adminHeaders(): Record<string, string> {
  const token = typeof window !== "undefined"
    ? localStorage.getItem("app_admin_token") : null;
  return {
    "Content-Type": "application/json",
    ...(token ? { Authorization: `Bearer ${token}` } : {}),
  };
}

async function adminFetch(path: string, init?: RequestInit) {
  const r = await fetch(`${API_BASE}${path}`, {
    ...init,
    headers: { ...adminHeaders(), ...init?.headers },
  });
  if (r.status === 401 || r.status === 403) {
    localStorage.removeItem("app_admin_token");
    window.location.href = "/sre";
    throw new Error("Unauthorized");
  }
  if (!r.ok) {
    const err = await r.json().catch(() => ({ detail: "Request failed" }));
    throw new Error(err.detail || `HTTP ${r.status}`);
  }
  return r.json();
}
```

Then export typed functions:
```typescript
export const fetchStats = () => adminFetch("/api/admin/stats");
export const fetchUsers = () => adminFetch("/api/admin/users");
// ... etc
```

### Backend Endpoints

The admin API should provide these endpoint groups:

| Group | Endpoints |
|-------|----------|
| Stats | `GET /admin/stats` |
| Users | `GET /admin/users`, `PATCH /admin/users/:id`, `DELETE /admin/users/:id` |
| Pods | `GET /admin/pods?status=`, `DELETE /admin/pods/:id` |
| Models | `GET/POST /admin/models`, `PATCH/DELETE /admin/models/:id` |
| Invites | `GET/POST /admin/invites`, `DELETE /admin/invites/:id` |
| Metrics | `GET /admin/metrics`, `GET /admin/metrics/history?hours=` |
| Logs | `GET /admin/logs/:component?lines=&search=` |
| Alerts | `GET/PUT /admin/alerts/rules/:id`, `GET /admin/alerts/history`, `POST /admin/alerts/test-webhook` |
| Cloud | `GET /admin/cloud/resources` |
| Cost | `GET /admin/cost/summary`, `GET /admin/cost/trend?days=`, `GET /admin/cost/users` |
| Infra | `GET /admin/infra/ecs`, `GET /admin/infra/pods` |
| Audit | `GET /admin/audit?page=&size=&user=&method=&path=&status=&start=&end=` |
| System | `GET /admin/system/health` |

### Middleware

Two middleware components are needed for full SRE observability:

1. **Metrics middleware**: Ring buffer collecting request latency, status codes, endpoint counts. Periodic flush to database for history.
2. **Audit middleware**: Fire-and-forget (asyncio.create_task) logging of every API request — method, path, status, user, IP, duration.

## File Structure

```
web/src/app/sre/
├── layout.tsx          # Auth gate + sidebar + header
├── _table-styles.ts    # Shared table styles (from huawei-cloud-style)
├── page.tsx            # Overview dashboard
├── users/page.tsx
├── pods/page.tsx
├── models/page.tsx
├── invites/page.tsx
├── metrics/page.tsx
├── logs/page.tsx
├── alerts/page.tsx
├── cloud/page.tsx
├── cost/page.tsx
├── infra/page.tsx
├── audit/page.tsx
└── system/page.tsx
```

## Common Page Patterns

Every page should follow this structure:
```tsx
"use client";
import { useState, useEffect, useCallback } from "react";
import { fetchXxx } from "@/lib/sre-client";
import { thStyle, tdStyle, tableWrapperStyle, tableStyle } from "../_table-styles";

export default function SreXxx() {
  const [data, setData] = useState<Data[]>([]);
  const [loading, setLoading] = useState(true);

  const load = useCallback(async () => {
    setLoading(true);
    try {
      const result = await fetchXxx().catch(() => []);
      setData(Array.isArray(result) ? result : []);
    } finally {
      setLoading(false);
    }
  }, []);

  useEffect(() => { load(); }, [load]);

  return (
    <div>
      <h2 style={{ fontSize: 18, fontWeight: 600, margin: "0 0 20px 0", color: "#191919" }}>
        Page Title
      </h2>
      {loading ? (
        <div style={{ padding: 40, textAlign: "center", color: "#8a8e99" }}>加载中...</div>
      ) : (
        <div style={tableWrapperStyle}>
          <table style={tableStyle}>...</table>
        </div>
      )}
    </div>
  );
}
```

Key principles:
- Always handle loading state
- Catch API errors gracefully (`.catch(() => []` or `.catch(() => null)`)
- Use null-safe rendering (`?.toFixed()`, `?? "-"`)
- Format dates consistently with `toLocaleString("zh-CN")`
- Row hover with state tracking

## Detail Page Pattern

For resources that need a dedicated detail view (e.g., tenant detail):

```tsx
// /sre/tenants/[tenantId]/page.tsx
"use client";
import { useParams } from "next/navigation";

export default function TenantDetailPage() {
  const { tenantId } = useParams<{ tenantId: string }>();
  // Fetch detail, show key-value card layout
  // Back button: router.push("/sre/tenants")
  // Action buttons: Suspend/Activate, Make Admin/Remove Admin
}
```

For inline detail without navigation, use the **Detail Side Panel** pattern (see `huawei-cloud-style` skill).

## Trace Viewer Pattern

For distributed tracing or pipeline debugging:

**Components:**
- Space selector dropdown (choose which space/resource to trace)
- Start/Stop trace button with status indicator
- Buffer size display
- Expandable trace rows showing spans
- Span visualization: status dot + name + horizontal duration bar + duration in ms
- Span metadata as formatted JSON

**Key patterns:**
- Polling interval for active traces (e.g., every 3 seconds)
- Expandable rows with click-to-toggle
- Duration bars proportional to max span duration

## Request/Response Capture Middleware

For the log detail panel to show request/response data, the backend middleware must:

1. **Capture request body**: `await request.body()` before `call_next`
2. **Capture response body**: Consume `response.body_iterator`, collect chunks, rebuild Response
3. **Filter sensitive headers**: Skip `authorization`, `cookie`, `x-api-key`
4. **Truncate large bodies**: 10KB max with truncation notice
5. **Fire-and-forget write**: `asyncio.create_task(_write_log(...))` to avoid blocking response

```python
# Sensitive header filtering
skip = {"authorization", "cookie", "x-api-key"}
headers = {k: v for k, v in request.headers.items() if k.lower() not in skip}

# Body truncation
MAX_BODY = 10_240
text = data.decode("utf-8")
if len(text) > MAX_BODY:
    text = text[:MAX_BODY] + f"\n... (truncated, total {len(text)} chars)"
```

## Database Migration Pattern

When adding new columns to existing tables, use safe migration in the DB init:

```python
# Add columns if they don't exist (idempotent)
for col in ("request_headers", "request_body", "response_headers", "response_body"):
    await conn.execute(text(
        f"ALTER TABLE usage_logs ADD COLUMN IF NOT EXISTS {col} TEXT"
    ))
```

This runs on every startup but is idempotent thanks to `IF NOT EXISTS`.
