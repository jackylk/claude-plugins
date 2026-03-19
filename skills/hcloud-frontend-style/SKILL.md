---
name: hcloud-frontend-style
description: Create web pages and consoles using Huawei Cloud console UI style. Use this skill whenever the user mentions "华为云风格", "Huawei Cloud style", wants a console/admin panel with a professional enterprise look, or is building any management dashboard or operations console. Also trigger when creating table-heavy pages, sidebar navigation layouts, or enterprise-style admin UIs. This skill defines the complete design system — layout, colors, typography, navigation, tables, cards, buttons, forms, and login pages.
---

# Huawei Cloud Console Style Guide

This skill defines a complete design system inspired by the Huawei Cloud console. Use it to build professional, enterprise-grade web management consoles with a clean, spacious, and highly readable UI.

The style works particularly well for: admin panels, SRE consoles, cloud resource management, operations dashboards, and any data-heavy management interface.

## Technology Stack

- **Framework**: Next.js (App Router) with React, "use client" components
- **Styling**: Inline React `CSSProperties` objects (no CSS-in-JS libraries needed)
- **Font**: System font stack for maximum compatibility

## Color Palette

```
Primary Blue:    #0073e6   — links, active states, primary buttons
Success Green:   #52c41a   — healthy status, success badges
Warning Orange:  #e37318   — warnings, PATCH method
Danger Red:      #e6393d   — errors, delete buttons, brand accent
Purple:          #722ed1   — PUT method badge

Text Primary:    #191919   — headings, primary text
Text Secondary:  #575d6c   — labels, descriptions
Text Muted:      #8a8e99   — timestamps, placeholders, disabled

Border Primary:  #e5e5e5   — table headers, card borders
Border Light:    #f0f0f0   — table row dividers
Background:      #f5f7fa   — page background
Background Card: #fff      — card/table background
Background Head: #fafafa   — table header background

Badge Blue BG:   #e6f4ff   — info badge background
Badge Green BG:  #f6ffed   — success badge background
Badge Red BG:    #fff2f0   — error badge background
Badge Orange BG: #fff7e6   — warning badge background
```

## Typography

```typescript
const FONT_FAMILY = "-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif";
```

| Element | Size | Weight | Color |
|---------|------|--------|-------|
| Page title (h2) | 18px | 600 | #191919 |
| Section title | 15px | 600 | #191919 |
| Nav group header | 15px | 700 | #191919 |
| Nav item | 14px | 400 (600 active) | #575d6c (#0073e6 active) |
| Table header (th) | 12px | 500 | #575d6c |
| Table cell (td) | 14px | 400 | #191919 |
| Monospace data | 13px | 400 | #191919 or #575d6c |
| Timestamp | 12-13px | 400 | #8a8e99 |
| Badge | 12px | 400-600 | varies |
| Button | 14px | 400-600 | varies |

## Overall Layout

The standard layout is a three-zone structure:

```
┌─────────────────────────────────────────────┐
│  Top Header (48px, black #000)              │
├────────────┬────────────────────────────────┤
│  Sidebar   │  Main Content                  │
│  (220px)   │  (flex: 1, padding: 24px)      │
│  white bg  │  #f5f7fa background            │
│            │                                │
│            │                                │
└────────────┴────────────────────────────────┘
```

### Top Header

- Height: 48px, background: `#000`
- Left: brand name (colored, e.g. red #e6393d, 16px bold) + separator `|` + page title (white, 14px)
- Right: username (gray #ccc, 13px) + logout button (bordered, gray)
- Mobile: hamburger button (hidden by default, shown at `max-width: 768px`)

### Sidebar

- Width: 220px, white background, right border `1px solid #e5e5e5`
- Collapsible with smooth transition (`transition: width 0.2s, min-width 0.2s`)
- Navigation items: height 44px, paddingLeft 24px
- Active item: left border `3px solid #0073e6`, background `#f5f7fa`, color `#0073e6`, fontWeight 600
- Hover: same as active visually
- Grouped navigation with dividers between groups

### Sidebar Group Structure

```typescript
const GROUP_HEADER_STYLE: React.CSSProperties = {
  fontSize: 15,
  color: "#191919",
  fontWeight: 700,
  padding: "20px 20px 8px",
  lineHeight: 1.4,
};

const GROUP_DIVIDER_STYLE: React.CSSProperties = {
  height: 1,
  background: "#e5e5e5",
  margin: "8px 20px",
};
```

Group headers are LARGER and BOLDER than menu items (15px/700 vs 14px/400). Each group is separated by a horizontal divider line. The first group (typically "Overview") has no header or divider.

## Tables (Critical — Huawei Cloud Signature Style)

Tables are the most important component. They must feel spacious and airy — this is the defining characteristic of Huawei Cloud's table design.

### Shared Table Styles

Always extract table styles into a shared file (e.g., `_table-styles.ts`) to avoid duplication:

```typescript
import type React from "react";

export const thStyle: React.CSSProperties = {
  padding: "14px 16px",
  textAlign: "left",
  fontSize: 12,
  color: "#575d6c",
  fontWeight: 500,
  borderBottom: "1px solid #e5e5e5",
  background: "#fafafa",
  whiteSpace: "nowrap",
};

export const tdStyle: React.CSSProperties = {
  padding: "16px 16px",
  fontSize: 14,
  color: "#191919",
  borderBottom: "1px solid #f0f0f0",
};

export const tableWrapperStyle: React.CSSProperties = {
  background: "#fff",
  border: "1px solid #e5e5e5",
  borderRadius: 4,
  overflow: "hidden",
};

export const tableStyle: React.CSSProperties = {
  width: "100%",
  borderCollapse: "collapse",
};

export const linkBtnStyle: React.CSSProperties = {
  background: "none",
  border: "none",
  color: "#0073e6",
  fontSize: 13,
  cursor: "pointer",
  padding: "2px 8px",
};

export const dangerBtnStyle: React.CSSProperties = {
  background: "none",
  border: "none",
  color: "#e6393d",
  fontSize: 13,
  cursor: "pointer",
  padding: "2px 8px",
};

export const ROW_HOVER_BG = "#f5f7fa";
```

### Key Table Principles

1. **Spacious padding** — 14-16px vertical, 16px horizontal. Rows should feel tall and airy
2. **Subtle borders** — Header border `#e5e5e5`, row dividers `#f0f0f0` (even lighter)
3. **No outer heavy borders** — Wrapper has `border: 1px solid #e5e5e5`, borderRadius 4
4. **Row hover** — `#f5f7fa` background on hover with `transition: background 0.15s`
5. **Header style** — Small (12px), gray (#575d6c), medium weight (500), light background (#fafafa)
6. **Operation links** — Blue for normal actions, red for destructive, no underline, no background

### Table with Section Title

When a table has a title inside its card:
```tsx
<div style={tableWrapperStyle}>
  <div style={{ padding: "16px 20px 12px", fontWeight: 600, fontSize: 15, color: "#191919" }}>
    Section Title
  </div>
  <table style={tableStyle}>...</table>
</div>
```

### Empty State

```tsx
<tr>
  <td colSpan={N} style={{ ...tdStyle, textAlign: "center", color: "#8a8e99", padding: 40 }}>
    暂无数据
  </td>
</tr>
```

### Pagination

Bottom bar style:
```tsx
<div style={{ display: "flex", alignItems: "center", gap: 12 }}>
  <button disabled={page <= 1} style={{ height: 32, padding: "0 16px", border: "1px solid #c2c6cc", ... }}>
    上一页
  </button>
  <span style={{ fontSize: 13, color: "#575d6c" }}>第 {page} 页，共 {total} 条</span>
  <button disabled={!hasNext} style={{ ... }}>下一页</button>
</div>
```

## Cards

```typescript
{
  background: "#fff",
  border: "1px solid #e5e5e5",
  borderRadius: 4,    // NOT 6 or 8 — Huawei uses subtle rounding
  padding: 20,
}
```

### Metric Cards

```tsx
<div style={{ background: "#fff", border: "1px solid #e5e5e5", borderRadius: 4, padding: 20 }}>
  <div style={{ fontSize: 13, color: "#575d6c", marginBottom: 8 }}>Label</div>
  <div style={{ fontSize: 32, fontWeight: 600, color: "#0073e6" }}>42</div>
</div>
```

## Buttons

| Type | Background | Color | Border |
|------|-----------|-------|--------|
| Primary | #0073e6 | #fff | none |
| Primary (brand) | #e6393d | #fff | none |
| Default | #fff | #575d6c | 1px solid #c2c6cc |
| Text link | none | #0073e6 | none |
| Danger text | none | #e6393d | none |

Standard button: `height: 32px, padding: "0 16px", borderRadius: 2, fontSize: 14`

Hover states: Primary darkens slightly, default border/text turns blue (#0073e6).

## Status Indicators

Dot + label pattern:
```tsx
<span style={{ display: "inline-flex", alignItems: "center", gap: 6 }}>
  <span style={{ width: 8, height: 8, borderRadius: "50%", background: "#52c41a" }} />
  Running
</span>
```

Badge pattern:
```tsx
<span style={{ padding: "2px 8px", borderRadius: 2, fontSize: 12, color: "#52c41a", background: "#f6ffed" }}>
  Active
</span>
```

## Login Page

Dark themed, centered card:
- Page background: `#1a1a2e`
- Card: `width: 380px, background: "#16213e", borderRadius: 8, padding: 40`
- Input: `background: "#0f3460", border: "1px solid #2a4a7f", borderRadius: 4, color: "#fff"`
- Brand name in center, colored (e.g., red)
- Error messages: red text on `rgba(230,57,61,0.1)` background

## Auth Pattern

Use localStorage token with a `checking` state to prevent login page flash:
```typescript
const [authed, setAuthed] = useState(false);
const [checking, setChecking] = useState(true);

useEffect(() => {
  const token = localStorage.getItem("my_admin_token");
  if (token) setAuthed(true);
  setChecking(false);
}, []);

if (checking) return <div style={{ minHeight: "100vh", background: "#1a1a2e" }} />;
if (!authed) return <LoginForm />;
return <ConsoleLayout>{children}</ConsoleLayout>;
```

## Forms & Inputs

```typescript
const inputStyle: React.CSSProperties = {
  border: "1px solid #d9d9d9",
  borderRadius: 4,
  padding: "6px 10px",
  fontSize: 13,
  outline: "none",
  height: 32,
  boxSizing: "border-box",
};
```

## Filter/Search Bars

Horizontal row of inputs + selects + search button above tables:
```tsx
<div style={{ display: "flex", flexWrap: "wrap", gap: 12, marginBottom: 20, alignItems: "center" }}>
  <input style={inputStyle} placeholder="Filter..." />
  <select style={inputStyle}>...</select>
  <button style={{ height: 32, padding: "0 16px", background: "#0073e6", color: "#fff", border: "none", borderRadius: 4 }}>
    Search
  </button>
</div>
```

## Progress Bars

```tsx
function ProgressBar({ value, label }: { value: number; label: string }) {
  const color = value >= 90 ? "#e6393d" : value >= 70 ? "#e37318" : "#52c41a";
  return (
    <div style={{ marginBottom: 16 }}>
      <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 6 }}>
        <span style={{ fontSize: 13, color: "#575d6c" }}>{label}</span>
        <span style={{ fontSize: 13, color: "#191919", fontWeight: 500 }}>{value.toFixed(1)}%</span>
      </div>
      <div style={{ height: 12, background: "#f0f0f0", borderRadius: 6, overflow: "hidden" }}>
        <div style={{ height: "100%", width: `${Math.min(value, 100)}%`, background: color, borderRadius: 6 }} />
      </div>
    </div>
  );
}
```

## API Client Pattern

Centralize API calls in a shared client file with:
- Base URL constant
- Auth header injection from localStorage
- Automatic redirect on 401/403
- Typed fetch functions per resource

## Detail Side Panel

A common pattern for viewing record details without leaving the list page. The panel slides in from the right over a backdrop.

```tsx
{/* Backdrop */}
<div
  onClick={onClose}
  style={{
    position: "fixed", inset: 0,
    background: "rgba(0,0,0,0.15)",
    zIndex: 999,
  }}
/>
{/* Panel */}
<div style={{
  position: "fixed", top: 0, right: 0, bottom: 0,
  width: 480,
  background: "#fff",
  boxShadow: "-4px 0 16px rgba(0,0,0,0.08)",
  zIndex: 1000,
  display: "flex", flexDirection: "column",
  fontFamily: FONT_FAMILY,
}}>
  {/* Header: title + close button */}
  <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", padding: "20px 24px 0" }}>
    <div style={{ fontSize: 16, fontWeight: 600, color: "#191919" }}>Title</div>
    <button onClick={onClose} style={{ background: "none", border: "none", fontSize: 20, color: "#8a8e99", cursor: "pointer" }}>×</button>
  </div>
  {/* Optional tabs */}
  {/* Scrollable content */}
  <div style={{ padding: "16px 24px", flex: 1, overflowY: "auto" }}>
    {/* Key-value rows, code blocks, etc. */}
  </div>
</div>
```

### Tabs in Side Panel

```tsx
const tabBase: React.CSSProperties = {
  padding: "10px 20px", fontSize: 14, cursor: "pointer",
  background: "none", border: "none",
  borderBottom: "2px solid transparent",
  color: "#575d6c", fontFamily: FONT_FAMILY,
};
const tabActive: React.CSSProperties = {
  ...tabBase, color: "#0073e6", borderBottomColor: "#0073e6", fontWeight: 500,
};
```

### Key-Value Rows

```tsx
const rowStyle: React.CSSProperties = {
  display: "flex", gap: 16, padding: "10px 0",
  borderBottom: "1px solid #f0f0f0",
};
const labelStyle: React.CSSProperties = { fontSize: 13, color: "#575d6c", minWidth: 80, flexShrink: 0 };
const valueStyle: React.CSSProperties = { fontSize: 13, color: "#191919", wordBreak: "break-all" };
```

### Code Blocks (for JSON/headers/body)

```tsx
const sectionTitle: React.CSSProperties = {
  fontSize: 13, fontWeight: 600, color: "#191919", marginBottom: 8,
};

const codeBlock: React.CSSProperties = {
  background: "#f7f8fa",
  border: "1px solid #e5e5e5",
  borderRadius: 4,
  padding: "12px 16px",
  fontSize: 12,
  fontFamily: "monospace",
  whiteSpace: "pre-wrap",
  wordBreak: "break-all",
  maxHeight: 300,
  overflowY: "auto",
  margin: 0,
  color: "#333",
};

// Format JSON strings for display
function formatJson(raw: string): string {
  try { return JSON.stringify(JSON.parse(raw), null, 2); }
  catch { return raw; }
}
```

## Collapsible Sidebar

The sidebar supports collapse/expand with smooth transition:

```tsx
// Collapsed: 60px width, show only icons or abbreviated labels
// Expanded: 220px width, full labels
const sidebarStyle: React.CSSProperties = {
  width: collapsed ? 60 : 220,
  minWidth: collapsed ? 60 : 220,
  transition: "width 0.2s, min-width 0.2s",
  background: "#fff",
  borderRight: "1px solid #e5e5e5",
  overflowY: "auto",
};
```

## Responsive / Mobile

- Sidebar: hidden on mobile (`max-width: 768px`), replaced by a hamburger toggle button in the header
- Tables: wrapped in `overflow-x: auto` container
- Grid layouts: use `auto-fit, minmax(200px, 1fr)` for metric cards

## Null Safety

Always use optional chaining when rendering API data:
- `value?.toFixed(2) ?? "-"` instead of `value.toFixed(2)`
- `item.name || item.fallback_field || "-"` for fields that may vary
- `new Date(timestamp).toLocaleString("zh-CN")` for dates
