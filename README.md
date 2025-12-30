# ArgoCD MCP Server

<!-- mcp-name: io.github.asklokesh/argocd-mcp-server -->

<div align="center">

# Argocd Mcp Server

[![GitHub stars](https://img.shields.io/github/stars/LokiMCPUniverse/argocd-mcp-server?style=social)](https://github.com/LokiMCPUniverse/argocd-mcp-server/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/LokiMCPUniverse/argocd-mcp-server?style=social)](https://github.com/LokiMCPUniverse/argocd-mcp-server/network)
[![GitHub watchers](https://img.shields.io/github/watchers/LokiMCPUniverse/argocd-mcp-server?style=social)](https://github.com/LokiMCPUniverse/argocd-mcp-server/watchers)

[![License](https://img.shields.io/github/license/LokiMCPUniverse/argocd-mcp-server?style=for-the-badge)](https://github.com/LokiMCPUniverse/argocd-mcp-server/blob/main/LICENSE)
[![Issues](https://img.shields.io/github/issues/LokiMCPUniverse/argocd-mcp-server?style=for-the-badge)](https://github.com/LokiMCPUniverse/argocd-mcp-server/issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/LokiMCPUniverse/argocd-mcp-server?style=for-the-badge)](https://github.com/LokiMCPUniverse/argocd-mcp-server/pulls)
[![Last Commit](https://img.shields.io/github/last-commit/LokiMCPUniverse/argocd-mcp-server?style=for-the-badge)](https://github.com/LokiMCPUniverse/argocd-mcp-server/commits)

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![MCP](https://img.shields.io/badge/Model_Context_Protocol-DC143C?style=for-the-badge)](https://modelcontextprotocol.io)

[![Commit Activity](https://img.shields.io/github/commit-activity/m/LokiMCPUniverse/argocd-mcp-server?style=flat-square)](https://github.com/LokiMCPUniverse/argocd-mcp-server/pulse)
[![Code Size](https://img.shields.io/github/languages/code-size/LokiMCPUniverse/argocd-mcp-server?style=flat-square)](https://github.com/LokiMCPUniverse/argocd-mcp-server)
[![Contributors](https://img.shields.io/github/contributors/LokiMCPUniverse/argocd-mcp-server?style=flat-square)](https://github.com/LokiMCPUniverse/argocd-mcp-server/graphs/contributors)

</div>

A Model Context Protocol (MCP) server for integrating ArgoCD GitOps continuous delivery tool with GenAI applications, enabling intelligent Kubernetes deployment management.

## Features

- **Comprehensive ArgoCD API Coverage**:
  - Application Management: Create, sync, delete, rollback applications
  - Project Management: Configure projects, roles, and policies
  - Repository Management: Add/remove Git repositories, Helm charts
  - Cluster Management: Register and manage Kubernetes clusters
  - Sync Operations: Manual/auto sync, sync windows, hooks
  - Health Monitoring: Application health, sync status, resource tree
  - RBAC Management: Users, roles, policies, JWT tokens
  - GitOps Workflows: Automated deployments from Git commits
  - Multi-tenancy: Namespace isolation, project restrictions
  - Application Sets: Template-based multi-cluster deployments

- **Authentication Methods**:
  - JWT Token authentication
  - API Token authentication
  - OIDC/OAuth2 integration
  - LDAP authentication support

- **Enterprise Features**:
  - Multi-cluster support
  - Disaster recovery workflows
  - Progressive delivery (Canary, Blue-Green)
  - Automated rollback on failures
  - Audit logging and compliance
  - Resource optimization
  - Secret management integration

## Installation

```bash
pip install argocd-mcp-server
```

Or install from source:

```bash
git clone https://github.com/asklokesh/argocd-mcp-server.git
cd argocd-mcp-server
pip install -e .
```

## Configuration

Create a `.env` file or set environment variables:

```env
# ArgoCD Connection
ARGOCD_SERVER=argocd.example.com
ARGOCD_AUTH_TOKEN=your_auth_token
ARGOCD_INSECURE=false

# Optional Settings
ARGOCD_GRPC_WEB=true
ARGOCD_TIMEOUT=30
ARGOCD_CLIENT_CERT=/path/to/cert.pem
ARGOCD_CLIENT_KEY=/path/to/key.pem

# Multi-Cluster Support
ARGOCD_PROD_SERVER=argocd-prod.example.com
ARGOCD_PROD_TOKEN=prod_token

ARGOCD_DEV_SERVER=argocd-dev.example.com
ARGOCD_DEV_TOKEN=dev_token
```

## Quick Start

### Basic Usage

```python
from argocd_mcp import ArgoCDMCPServer

# Initialize the server
server = ArgoCDMCPServer()

# Start the server
server.start()
```

### Claude Desktop Configuration

Add to your Claude Desktop config:

```json
{
  "mcpServers": {
    "argocd": {
      "command": "python",
      "args": ["-m", "argocd_mcp.server"],
      "env": {
        "ARGOCD_SERVER": "argocd.example.com",
        "ARGOCD_AUTH_TOKEN": "your_auth_token"
      }
    }
  }
}
```

## Available Tools

### Application Management

#### List Applications
```python
{
  "tool": "argocd_list_applications",
  "arguments": {
    "project": "default",
    "namespace": "argocd"
  }
}
```

#### Create Application
```python
{
  "tool": "argocd_create_application",
  "arguments": {
    "name": "my-app",
    "project": "default",
    "source": {
      "repoURL": "https://github.com/org/repo.git",
      "path": "manifests",
      "targetRevision": "main"
    },
    "destination": {
      "server": "https://kubernetes.default.svc",
      "namespace": "production"
    },
    "syncPolicy": {
      "automated": {
        "prune": true,
        "selfHeal": true
      }
    }
  }
}
```

#### Sync Application
```python
{
  "tool": "argocd_sync_application",
  "arguments": {
    "name": "my-app",
    "prune": true,
    "force": false,
    "strategy": {
      "hook": {
        "force": true
      }
    }
  }
}
```

#### Get Application Status
```python
{
  "tool": "argocd_get_application_status",
  "arguments": {
    "name": "my-app",
    "refresh": true
  }
}
```

#### Rollback Application
```python
{
  "tool": "argocd_rollback_application",
  "arguments": {
    "name": "my-app",
    "revision": "abc123"
  }
}
```

### Project Management

#### Create Project
```python
{
  "tool": "argocd_create_project",
  "arguments": {
    "name": "production",
    "description": "Production applications",
    "sourceRepos": ["https://github.com/org/*"],
    "destinations": [
      {
        "server": "https://kubernetes.default.svc",
        "namespace": "prod-*"
      }
    ],
    "clusterResourceWhitelist": [
      {"group": "*", "kind": "*"}
    ]
  }
}
```

### Repository Management

#### Add Repository
```python
{
  "tool": "argocd_add_repository",
  "arguments": {
    "url": "https://github.com/org/repo.git",
    "name": "my-repo",
    "type": "git",
    "username": "git-user",
    "password": "git-token",
    "insecure": false
  }
}
```

#### Add Helm Repository
```python
{
  "tool": "argocd_add_helm_repository",
  "arguments": {
    "url": "https://charts.example.com",
    "name": "my-charts",
    "type": "helm",
    "username": "helm-user",
    "password": "helm-password"
  }
}
```

### Cluster Management

#### Add Cluster
```python
{
  "tool": "argocd_add_cluster",
  "arguments": {
    "name": "production-cluster",
    "server": "https://k8s-prod.example.com",
    "config": {
      "bearerToken": "cluster-token",
      "tlsClientConfig": {
        "insecure": false,
        "caData": "base64-encoded-ca"
      }
    }
  }
}
```

### Application Sets

#### Create ApplicationSet
```python
{
  "tool": "argocd_create_applicationset",
  "arguments": {
    "name": "multi-cluster-app",
    "generators": [
      {
        "clusters": {},
        "selector": {
          "matchLabels": {
            "environment": "production"
          }
        }
      }
    ],
    "template": {
      "metadata": {
        "name": "{{cluster}}-app"
      },
      "spec": {
        "project": "default",
        "source": {
          "repoURL": "https://github.com/org/repo.git",
          "path": "{{cluster}}"
        },
        "destination": {
          "server": "{{server}}",
          "namespace": "default"
        }
      }
    }
  }
}
```

### Health and Monitoring

#### Get Application Health
```python
{
  "tool": "argocd_get_application_health",
  "arguments": {
    "name": "my-app"
  }
}
```

#### Get Resource Tree
```python
{
  "tool": "argocd_get_resource_tree",
  "arguments": {
    "name": "my-app"
  }
}
```

## Advanced Configuration

### Multi-Cluster GitOps

```python
from argocd_mcp import ArgoCDMCPServer, ClusterConfig

# Configure multiple ArgoCD instances
clusters = {
    "production": ClusterConfig(
        server="argocd-prod.example.com",
        auth_token="prod_token",
        default_namespace="argocd"
    ),
    "staging": ClusterConfig(
        server="argocd-staging.example.com",
        auth_token="staging_token",
        default_namespace="argocd-staging"
    ),
    "development": ClusterConfig(
        server="argocd-dev.example.com",
        auth_token="dev_token",
        default_namespace="argocd-dev"
    )
}

server = ArgoCDMCPServer(clusters=clusters, default_cluster="production")
```

### Progressive Delivery

```python
# Configure progressive delivery strategies
progressive_config = {
    "canary": {
        "steps": [
            {"setWeight": 10},
            {"pause": {"duration": "5m"}},
            {"setWeight": 30},
            {"pause": {"duration": "5m"}},
            {"setWeight": 50},
            {"pause": {"duration": "5m"}},
            {"setWeight": 100}
        ]
    },
    "blueGreen": {
        "activeService": "app-active",
        "previewService": "app-preview",
        "autoPromotionEnabled": false
    }
}

server = ArgoCDMCPServer(progressive_config=progressive_config)
```

### Disaster Recovery

```python
from argocd_mcp import ArgoCDMCPServer, DisasterRecoveryConfig

dr_config = DisasterRecoveryConfig(
    backup_enabled=True,
    backup_schedule="0 2 * * *",  # Daily at 2 AM
    backup_location="s3://backups/argocd",
    restore_priority=["production", "staging", "development"]
)

server = ArgoCDMCPServer(dr_config=dr_config)
```

## Integration Examples

See the `examples/` directory for complete integration examples:

- `basic_gitops.py` - Basic GitOps workflows
- `multi_cluster_deployment.py` - Multi-cluster application deployment
- `progressive_delivery.py` - Canary and blue-green deployments
- `disaster_recovery.py` - Backup and restore workflows
- `genai_gitops.py` - AI-powered GitOps automation
- `security_scanning.py` - Integration with security tools

## Security Best Practices

1. **Use service accounts** with minimal permissions
2. **Enable RBAC** with proper policies
3. **Implement network policies** for cluster isolation
4. **Use sealed secrets** or external secret operators
5. **Enable audit logging** for compliance
6. **Implement admission controllers** for policy enforcement
7. **Regular security scanning** of images and manifests

## Error Handling

The server provides detailed error information:

```python
try:
    result = server.execute_tool("argocd_sync_application", {
        "name": "my-app"
    })
except ArgoCDError as e:
    print(f"ArgoCD error: {e.error_code} - {e.message}")
    if e.error_code == "OutOfSync":
        print("Application is out of sync")
    elif e.error_code == "ComparisonError":
        print("Error comparing application state")
```

## Performance Optimization

1. **Use application sets** for similar apps
2. **Enable resource caching** in ArgoCD
3. **Implement sync windows** to control deployment times
4. **Use webhook notifications** instead of polling
5. **Optimize manifest generation** with Helm/Kustomize

## Troubleshooting

### Common Issues

1. **Sync failures**
   - Check application logs
   - Verify repository access
   - Review resource quotas

2. **Authentication errors**
   - Verify token validity
   - Check RBAC policies
   - Review server connectivity

3. **Performance issues**
   - Check ArgoCD server resources
   - Review application count
   - Optimize manifest size

## Contributing

Contributions are welcome! Please read our contributing guidelines and submit pull requests.

## License

MIT License - see LICENSE file for details