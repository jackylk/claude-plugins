# Huawei Cloud Service Quick Reference

Common services and their frequently used operations. Always run `hcloud <Service> --help` for the full list.

## Compute

### ECS (Elastic Cloud Server)
```bash
hcloud ECS NovaListServers                          # List all servers
hcloud ECS NovaShowServer --server_id=<id>          # Server details
hcloud ECS NovaCreateServers --skeleton             # Create server skeleton
hcloud ECS BatchStartServers --skeleton             # Start servers
hcloud ECS BatchStopServers --skeleton              # Stop servers
hcloud ECS BatchRebootServers --skeleton            # Reboot servers
```

### AS (Auto Scaling)
```bash
hcloud AS ListScalingGroups                         # List scaling groups
hcloud AS ListScalingInstances --scaling_group_id=<id>
```

## Containers

### CCE (Cloud Container Engine)
```bash
hcloud CCE ListClusters                             # List clusters
hcloud CCE ShowCluster --cluster_id=<id>            # Cluster details
hcloud CCE ListNodes --cluster_id=<id>              # List nodes
hcloud CCE ListNodePools --cluster_id=<id>          # List node pools
hcloud CCE CreateNodePool --skeleton                # Create node pool
hcloud CCE UpdateNodePool --skeleton                # Update node pool
```

### SWR (Software Repository for Container)
```bash
hcloud SWR ListReposDetails --namespace=<ns>        # List repos
hcloud SWR ListRepositoryTags --namespace=<ns> --repository=<repo>
```

## Networking

### VPC
```bash
hcloud VPC ListVpcs                                 # List VPCs
hcloud VPC ListSubnets                              # List subnets
hcloud VPC ListSecurityGroups                       # List security groups
hcloud VPC ListSecurityGroupRules --security_group_id=<id>
```

### ELB (Elastic Load Balance)
```bash
hcloud ELB ListLoadBalancers                        # List load balancers
hcloud ELB ListListeners                            # List listeners
hcloud ELB ListPools                                # List backend pools
```

### EIP (Elastic IP)
```bash
hcloud EIP ListPublicips                            # List EIPs
hcloud EIP CreatePublicip --skeleton                # Allocate EIP
```

### NAT Gateway
```bash
hcloud NAT ListNatGateways                          # List NAT gateways
hcloud NAT ListSnatRules --nat_gateway_id=<id>      # List SNAT rules
```

### DNS
```bash
hcloud DNS ListPublicZones                          # List DNS zones
hcloud DNS ListRecordSetsByZone --zone_id=<id>      # List records
```

## Database

### RDS
```bash
hcloud RDS ListInstances                            # List RDS instances
hcloud RDS ShowInstance --instance_id=<id>           # Instance details
hcloud RDS ListSlowLogs --instance_id=<id>          # Slow query logs
```

## Storage

### OBS (via obs subcommand)
See SKILL.md OBS section.

### EVS (Elastic Volume Service)
```bash
hcloud EVS ListVolumes                              # List volumes
hcloud EVS ShowVolume --volume_id=<id>              # Volume details
```

## Security & Identity

### IAM
```bash
hcloud IAM KeystoneListUsers                        # List users
hcloud IAM KeystoneListProjects                     # List projects
hcloud IAM KeystoneListRegions                      # List regions
```

## Monitoring

### CES (Cloud Eye)
```bash
hcloud CES ListMetrics --namespace=<ns>             # List metrics
hcloud CES ShowMetricData --namespace=<ns> --metric_name=<m> --dim.0=<d>
hcloud CES ListAlarmRules                           # List alarm rules
```

## Serverless

### FunctionGraph
```bash
hcloud FunctionGraph ListFunctions                  # List functions
hcloud FunctionGraph ShowFunctionConfig --function_urn=<urn>
```

### APIG (API Gateway)
```bash
hcloud APIG ListApisV2 --instance_id=<id>          # List APIs
hcloud APIG ListEnvironmentsV2 --instance_id=<id>  # List environments
```

## Tips

- Use `--cli-query` with JMESPath to filter large result sets
- Use `--cli-output=table` for human-readable output
- Combine `--cli-waiter` with async operations to wait for completion
- Most List operations support pagination via `--limit` and `--offset`/`--marker`
