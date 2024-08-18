# TimescaleDB for RoadRunner Project

This repository contains the necessary configurations and instructions for deploying TimescaleDB in the RoadRunner project using Helm charts.

## Repository Structure

```
timescaledb/
├── charts/
│   └── timescaledb-single/
├── values/
│   ├── dev-values.yaml
│   └── prod-values.yaml
├── ingress/
│   └── timescaledb-ingress.yaml
└── README.md
```

## Prerequisites

- Kubernetes cluster running (dev-cluster or prod-cluster)
- Helm 3.x installed
- `kubectl` configured to communicate with your cluster

## Deployment

We use the official TimescaleDB Helm chart for deployment. The chart is not included in this repository but is fetched during the installation process.

To deploy TimescaleDB:

1. Add the TimescaleDB Helm repository:
   ```
   helm repo add timescale https://charts.timescale.com/
   helm repo update
   ```

2. Install TimescaleDB using the appropriate values file:

   For dev environment:
   ```
   helm install timescaledb-dev timescale/timescaledb-single -f values/dev-values.yaml -n road-runner
   ```

   For prod environment:
   ```
   helm install timescaledb-prod timescale/timescaledb-single -f values/prod-values.yaml -n road-runner
   ```

3. Apply the Ingress configuration:
   ```
   kubectl apply -f ingress/timescaledb-ingress.yaml -n road-runner
   ```

## Configuration

The `values/` directory contains environment-specific configuration files:

- `dev-values.yaml`: Configuration for the development environment
- `prod-values.yaml`: Configuration for the production environment

Modify these files to adjust the TimescaleDB deployment according to your needs.

## Ingress

The `ingress/timescaledb-ingress.yaml` file contains the Ingress configuration for exposing TimescaleDB services using Traefik. Ensure that Traefik is properly set up in your cluster before applying this configuration.

## Maintenance

For backup, restore, and other maintenance tasks, refer to the official TimescaleDB documentation and the Helm chart documentation.

## Troubleshooting

If you encounter issues with the deployment, check the following:

1. Ensure all prerequisites are met
2. Verify that the values in the configuration files are correct
3. Check the pod status and logs using `kubectl`

For more detailed troubleshooting, consult the TimescaleDB and Kubernetes documentation.