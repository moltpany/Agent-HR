# Feishu App Required Permissions

When configuring your Feishu app's Permission Management, import the following permissions based on what the agent needs to do.

## Core Permissions (Required for All Bots)

| Permission | API Scope | Why |
|-----------|-----------|-----|
| Read messages in group chats | `im:message:readonly` | Receive group messages |
| Send messages | `im:message` | Reply to users |
| Get user info | `contact:user.base:readonly` | Identify who's talking |

## Common Optional Permissions

| Category | Permission | API Scope |
|----------|-----------|----------|
| **Calendar** | Read calendar events | `calendar:calendar:readonly` |
| **Calendar** | Create/edit events | `calendar:calendar` |
| **Docs** | Read documents | `docx:document:readonly` |
| **Docs** | Edit documents | `docx:document` |
| **Sheets** | Read spreadsheets | `sheets:spreadsheet:readonly` |
| **Sheets** | Edit spreadsheets | `sheets:spreadsheet` |
| **Bitable** | Read databases | `bitable:app:readonly` |
| **Bitable** | Edit databases | `bitable:app` |
| **Tasks** | Manage tasks | `task:task` |
| **DM** | Send direct messages | `im:message` (with dmPolicy set) |

## How to Add Permissions

1. In your app's dashboard, go to **Permission Management**
2. Search for the permission by name or API scope
3. Click **Apply** next to each permission you need
4. After adding all permissions, go to **Version Management & Release** → create a new version and publish

⚠️ Permission changes only take effect after publishing a new app version.

## Minimal Set for a General-Purpose Chat Bot

```
im:message
im:message:readonly
contact:user.base:readonly
```
