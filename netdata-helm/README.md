# NetData Helm Chart for Rancher

This Helm chart deploys NetData monitoring agents across your Kubernetes cluster with automatic node registration to NetData Cloud. It's designed for Rancher GUI compatibility and ensures exactly one monitoring agent per node.

## Prerequisites

- Kubernetes cluster with Rancher
- NetData Cloud account
- Claiming token and room ID from NetData Cloud

## Getting NetData Claiming Credentials

1. Go to [NetData Cloud](https://app.netdata.cloud)
2. Sign in to your account
3. Navigate to your Space settings
4. Go to "Nodes" tab
5. Click "Add Nodes"
6. Copy the claiming token and room ID

## Installation via Rancher GUI

1. In Rancher, go to Apps & Marketplace
2. Click "Charts"
3. Upload this chart or add from repository
4. Click "Install"
5. Fill in the NetData configuration:
   - **NetData Claiming Token**: Paste your claiming token
   - **NetData Room ID**: Paste your room ID
   - **Child Node Claiming Token**: Usually same as parent token
   - **Child Node Room ID**: Usually same as parent room ID

## Installation via Helm CLI

```bash
# Install the chart
helm install netdata ./netdata-helm \
  --set parent.claiming.token="YOUR_CLAIMING_TOKEN" \
  --set parent.claiming.rooms="YOUR_ROOM_ID" \
  --set child.claiming.token="YOUR_CLAIMING_TOKEN" \
  --set child.claiming.rooms="YOUR_ROOM_ID"
```

## Configuration

Key configuration options:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.tag` | NetData image tag | `edge` |
| `restarter.enabled` | Enable container restarter | `true` |
| `parent.claiming.enabled` | Enable parent node claiming | `true` |
| `parent.claiming.token` | Claiming token for parent | `""` |
| `parent.claiming.rooms` | Room ID for parent | `""` |
| `child.claiming.enabled` | Enable child node claiming | `true` |
| `child.claiming.token` | Claiming token for children | `""` |
| `child.claiming.rooms` | Room ID for children | `""` |

## How it Works

- **Parent Node**: Deploys one NetData parent instance that serves as the central dashboard
- **Child Nodes**: Deploys NetData agents on every cluster node via DaemonSet
- **Auto-Registration**: All nodes automatically register with NetData Cloud using the provided tokens
- **Monitoring**: Provides comprehensive cluster monitoring with node-level detail

## Accessing NetData

After installation:
- Access the parent dashboard via LoadBalancer service (default port 19999)
- View all nodes in NetData Cloud dashboard
- Individual node metrics are streamed to the parent node

## Troubleshooting

1. **Nodes not appearing in NetData Cloud**: Check claiming token and room ID are correct
2. **Permission errors**: Ensure cluster has adequate RBAC permissions
3. **Resource issues**: Adjust resource requests/limits in values.yaml

## Support

For issues with this chart, check the NetData documentation at https://learn.netdata.cloud/docs