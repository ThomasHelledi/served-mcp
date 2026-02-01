# Infrastructure Tools

Tools for managing infrastructure resources, health monitoring, and cluster operations.

---

## Connection Management

### GetInfrastructureConnections

List all infrastructure connections.

#### Response

```json
{
  "connections": [
    {
      "id": 1,
      "name": "Eden Cluster",
      "type": "proxmox",
      "host": "10.10.10.5",
      "status": "connected",
      "resourceCount": 15
    },
    {
      "id": 2,
      "name": "Production K8s",
      "type": "kubernetes",
      "host": "10.10.10.20",
      "status": "connected",
      "resourceCount": 42
    }
  ]
}
```

#### Example Usage

```
User: "What infrastructure do we have?"
AI: Calls GetInfrastructureConnections
AI: "2 infrastructure connections:

     1. Eden Cluster (Proxmox) - 10.10.10.5
        Status: Connected
        Resources: 15 VMs/containers

     2. Production K8s (Kubernetes) - 10.10.10.20
        Status: Connected
        Resources: 42 deployments"
```

---

### TestInfrastructureConnection

Test connectivity to infrastructure.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |

#### Response

```json
{
  "success": true,
  "latencyMs": 12,
  "message": "Connection successful"
}
```

---

## Resource Management

### GetInfrastructureResources

List resources from a connection.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `type` | string | No | vm, container, deployment, pod |

#### Response

```json
{
  "resources": [
    {
      "id": "vm-100",
      "name": "k8s-control",
      "type": "vm",
      "status": "running",
      "ip": "10.10.10.20",
      "cpu": { "cores": 4, "usage": 25 },
      "memory": { "total": 16384, "used": 8192 },
      "uptime": "15d 3h"
    }
  ]
}
```

---

### GetInfrastructureResourceDetails

Get detailed resource information.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |

#### Response

```json
{
  "resource": {
    "id": "vm-100",
    "name": "k8s-control",
    "type": "vm",
    "status": "running",
    "config": {
      "cores": 4,
      "memory": 16384,
      "disk": 100,
      "network": "vmbr0"
    },
    "metrics": {
      "cpuUsage": 25,
      "memoryUsage": 50,
      "diskUsage": 35,
      "networkIn": 1024,
      "networkOut": 512
    },
    "tags": ["kubernetes", "control-plane"],
    "linkedProject": { "id": 1, "name": "Infrastructure" }
  }
}
```

---

### StartInfrastructureResource

Start a stopped resource.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |

---

### StopInfrastructureResource

Stop a running resource.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |
| `force` | bool | No | Force stop |

---

### RebootInfrastructureResource

Reboot a resource.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |

---

## Health Monitoring

### GetClusterHealth

Get overall cluster health.

#### Response

```json
{
  "health": {
    "status": "healthy",
    "score": 95,
    "checks": [
      { "name": "kubernetes", "status": "healthy", "message": "All nodes ready" },
      { "name": "database", "status": "healthy", "message": "MySQL responding" },
      { "name": "cache", "status": "warning", "message": "Redis memory 85%" },
      { "name": "storage", "status": "healthy", "message": "Sufficient space" }
    ]
  }
}
```

#### Example Usage

```
User: "Check cluster health"
AI: Calls GetClusterHealth
AI: "Cluster Health: 95% (Healthy)

     Services:
     - Kubernetes: Healthy - All nodes ready
     - Database: Healthy - MySQL responding
     - Cache: Warning - Redis memory at 85%
     - Storage: Healthy - Sufficient space

     Action recommended: Monitor Redis memory usage"
```

---

### GetKubernetesHealth

Get Kubernetes cluster health details.

#### Response

```json
{
  "kubernetes": {
    "status": "healthy",
    "nodes": {
      "ready": 3,
      "total": 3
    },
    "pods": {
      "running": 42,
      "pending": 0,
      "failed": 0
    },
    "namespaces": [
      { "name": "served", "pods": 12, "status": "active" },
      { "name": "monitoring", "pods": 8, "status": "active" }
    ]
  }
}
```

---

### GetDatabaseHealth

Get database health status.

#### Response

```json
{
  "database": {
    "status": "healthy",
    "type": "mysql",
    "version": "8.0.35",
    "connections": {
      "active": 45,
      "max": 500
    },
    "replication": {
      "status": "healthy",
      "lag": "0s"
    },
    "size": {
      "total": "25GB",
      "used": "18GB"
    }
  }
}
```

---

### GetProxmoxHealth

Get Proxmox host health.

#### Response

```json
{
  "proxmox": {
    "status": "healthy",
    "version": "8.1.3",
    "nodes": [
      {
        "name": "eden",
        "status": "online",
        "cpu": 25,
        "memory": 65,
        "vms": 8,
        "containers": 7
      }
    ],
    "storage": [
      { "name": "local-zfs", "used": "450GB", "total": "1TB" }
    ]
  }
}
```

---

### GetServiceHealth

Get application service health.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `service` | string | No | Specific service name |

#### Response

```json
{
  "services": [
    {
      "name": "served-api",
      "status": "healthy",
      "replicas": { "ready": 2, "desired": 2 },
      "latency": { "p50": 45, "p95": 120, "p99": 250 },
      "errorRate": 0.01
    }
  ]
}
```

---

## Metrics & Monitoring

### GetInfrastructureResourceMetrics

Get resource metrics over time.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |
| `period` | string | No | 1h, 24h, 7d (default: 1h) |

#### Response

```json
{
  "metrics": {
    "cpu": [
      { "time": "10:00", "value": 25 },
      { "time": "10:05", "value": 30 },
      { "time": "10:10", "value": 28 }
    ],
    "memory": [...],
    "disk": [...],
    "network": [...]
  }
}
```

---

### GetInfrastructureSummary

Get infrastructure overview.

#### Response

```json
{
  "summary": {
    "totalResources": 57,
    "byType": {
      "vm": 8,
      "container": 7,
      "deployment": 42
    },
    "health": {
      "healthy": 54,
      "warning": 2,
      "critical": 1
    },
    "utilization": {
      "cpu": 35,
      "memory": 60,
      "storage": 45
    }
  }
}
```

---

## Snapshots

### CreateInfrastructureSnapshot

Create a resource snapshot.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |
| `name` | string | Yes | Snapshot name |
| `description` | string | No | Description |

---

### GetInfrastructureSnapshots

List snapshots for a resource.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |

---

### RestoreInfrastructureSnapshot

Restore from a snapshot.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |
| `snapshotId` | string | Yes | Snapshot to restore |

---

## Resource Linking

### LinkResourceToProject

Link infrastructure to a project.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |
| `projectId` | int | Yes | Project ID |

---

### LinkResourceToCustomer

Link infrastructure to a customer.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connectionId` | int | Yes | Connection ID |
| `resourceId` | string | Yes | Resource ID |
| `customerId` | int | Yes | Customer ID |

---

## Best Practices

1. **Monitor health regularly** - Set up alerts for degraded services
2. **Create snapshots** - Before major changes, always snapshot
3. **Link resources** - Connect infrastructure to projects for cost tracking
4. **Check utilization** - Monitor CPU/memory to right-size resources
5. **Test connections** - Verify connectivity after network changes
