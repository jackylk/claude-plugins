---
name: hcloud
description: "Operate Huawei Cloud services using hcloud CLI (KooCLI). Use when the user wants to manage cloud resources like ECS, CCE, VPC, ELB, RDS, OBS, IAM, SWR, DNS, NAT, EIP, CES, AS, FunctionGraph, APIG, or any other Huawei Cloud service. Triggers on: '华为云', 'hcloud', '云资源', '云服务', cloud infrastructure management, or when querying/modifying Huawei Cloud resources."
---

# hcloud — Huawei Cloud CLI

## Command Pattern

```
hcloud <Service> <Operation> --param1=value1 --param2=value2 [global-options]
```

Exception: OBS uses `hcloud obs <command>` (see OBS section below).

## Before Constructing Any Command

1. Look up available operations: `hcloud <Service> --help`
2. Look up exact params: `hcloud <Service> <Operation> --help`
3. Generate param skeleton: `hcloud <Service> <Operation> --skeleton`

Never guess parameter names. Always verify via `--help` or `--skeleton` first.

## Global Options

| Option | Purpose |
|--------|---------|
| `--cli-region=cn-north-4` | Target region (default from profile) |
| `--cli-profile=hwstaff` | Select config profile |
| `--cli-output=json` | Output format: `json`, `table`, `tsv` |
| `--cli-query='$.items[*].id'` | JMESPath filter on response |
| `--cli-waiter` | Poll until field reaches target value |
| `--dryrun` | Validate request without executing |
| `--skeleton` | Generate JSON param skeleton |

## Safety Rules

1. **Destructive operations** (Delete*, Stop*, Remove*, Batch*): always run with `--dryrun` first, show the user what will happen, then confirm before executing
2. **Batch operations**: extra caution — list affected resources first, confirm count with user
3. **Never auto-execute** create/delete/stop/reboot without user confirmation
4. **Cost-aware**: warn when creating billable resources (ECS, ELB, NAT, EIP, RDS)

## Profile Management

```bash
hcloud configure list          # List all profiles
hcloud configure show          # Show active profile details
hcloud configure set --cli-profile=<name> --cli-access-key=<AK> --cli-secret-key=<SK> --cli-region=<region>
```

Active profile: `hwstaff`, region: `cn-north-4`

## OBS (Object Storage)

OBS uses a special subcommand syntax (wraps obsutil):

```bash
hcloud obs ls [obs://bucket/prefix]     # List buckets or objects
hcloud obs cp <src> <dst>               # Upload/download/copy
hcloud obs sync <src> <dst>             # Sync directories
hcloud obs mb obs://bucket-name         # Create bucket
hcloud obs rm obs://bucket/key          # Delete object
hcloud obs cat obs://bucket/key         # View object content
hcloud obs stat obs://bucket/key        # Object metadata
hcloud obs sign obs://bucket/key        # Generate signed URL
```

Use `hcloud obs help` for full list, `hcloud obs help <cmd>` for details.

## Quick Examples

```bash
# List ECS instances
hcloud ECS NovaListServers --cli-region=cn-north-4

# List CCE clusters
hcloud CCE ListClusters

# Query with JMESPath filter
hcloud ECS NovaListServers --cli-query='$.servers[*].{name:name,id:id,status:status}'

# Wait for async operation
hcloud ECS ShowServer --server_id=xxx --cli-waiter='status==ACTIVE'

# List VPCs
hcloud VPC ListVpcs

# List EIPs
hcloud EIP ListPublicips
```

## Workflow: Investigating an Issue

1. Identify the service and resource type
2. List resources: `hcloud <Service> List* --cli-query=...`
3. Get details: `hcloud <Service> Show*/Get* --<id_param>=<id>`
4. Check related resources (security groups, subnets, etc.)
5. Present findings before taking action

## Workflow: Creating Resources

1. Check params: `hcloud <Service> Create* --skeleton`
2. Show planned configuration to user
3. Run with `--dryrun` if supported
4. Confirm with user, then execute
5. Verify: `hcloud <Service> Show* --<id_param>=<id>`

## Error Handling

- Auth errors (401/403): check `hcloud configure show`, verify AK/SK and permissions
- Region errors: ensure `--cli-region` matches the resource's region
- Param errors: re-check with `--help`, params are case-sensitive
- Rate limiting: wait and retry, don't loop aggressively

## Service Reference

For detailed service-specific operations, see [references/services.md](references/services.md).
