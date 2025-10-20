# mysql

![Chart Version 0.0.1](https://img.shields.io/badge/version-0.0.1-blue)
![Alpha Maturity - Not Production Ready](https://img.shields.io/badge/maturity-alpha-red)
![Tested on AMD64 and ARM64 Architectures](https://img.shields.io/badge/arch-amd64%20%7C%20arm64-lightgrey)
![Security Scanned with Trivy](https://img.shields.io/badge/security-trivy-green)
![Kubernetes Manifests Validated with Kubeconform](https://img.shields.io/badge/validation-kubeconform-green)
![Tested with Helm Tests](https://img.shields.io/badge/testing-helm--test-green)

A simple Helm chart for MySQL

## Overview

This chart deploys MySQL using the official [Docker Library MySQL image](https://hub.docker.com/_/mysql). It provides a simple, secure MySQL deployment with persistent storage. Currently it does not support read replicas but it may do in the future.

## Installation

This chart is available as an OCI package from GitHub Container Registry.

### Install from OCI Registry

```bash
# Install the chart directly from OCI
helm install my-mysql oci://ghcr.io/simplehelm/mysql --set auth.rootPassword=mypassword
```

### Install with existing secret

```bash
# Create secret first
kubectl create secret generic mysql-secret \
  --from-literal=mysql-root-password=mypassword

# Install with existing secret
helm install my-mysql oci://ghcr.io/simplehelm/mysql --set auth.existingSecret=mysql-secret
```

## Requirements

**Authentication is required**: You must provide either `auth.rootPassword` or `auth.existingSecret`. The chart will fail to install without proper authentication configuration.

If using `auth.existingSecret`, the secret must contain:
- `mysql-root-password`: The MySQL root password
- `mysql-password`: The MySQL user password (only required if `auth.username` is set)

## Migration from bitnami/mysql

First, take a backup! Use `mysqldump`, take a `VolumeSnapshot` or transcribe the data onto a stone tablet. I don't mind how you do it, but please do!

Now you've done that, as PersistentVolumeClaims are not deleted along with a StatefulSet. If you have deployed the Bitnami MySQL Helm chart with reasonably standard settings, it should be possible to uninstall your Helm release and then re-deploy this chart with the same name (but a different values file) without losing any data.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity for pod assignment |
| auth | object | See values.yaml | MySQL authentication configuration |
| auth.database | string | `""` | MySQL database to create |
| auth.existingSecret | string | `""` | Name of existing secret to use for credentials (must contain 'mysql-root-password' key, and 'mysql-password' key if username is set) |
| auth.password | string | `""` | MySQL user password |
| auth.rootPassword | string | `""` | MySQL root password (required if existingSecret is not set) |
| auth.username | string | `""` | MySQL user to create |
| env | list | `[]` | Additional environment variables |
| fullnameOverride | string | `""` | Override the full name |
| global | object | `{"majorVersion":"8.4"}` | Global configuration |
| global.majorVersion | string | `"8.4"` | Major version of MySQL to use as default tag and in resource labels |
| image | object | `{"pullPolicy":"IfNotPresent","repository":"docker.io/library/mysql","tag":""}` | Container image configuration |
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.repository | string | `"docker.io/library/mysql"` | MySQL container image repository |
| image.tag | string | `""` | Overrides the image tag which by default uses global.majorVersion |
| imagePullSecrets | list | `[]` | Secrets for pulling images from private registries |
| livenessProbe | object | `{"exec":{"command":["mysqladmin","ping","-h","127.0.0.1","-u","root","-p$MYSQL_ROOT_PASSWORD"]},"initialDelaySeconds":30,"periodSeconds":10,"timeoutSeconds":5}` | Liveness probe |
| nameOverride | string | `""` | Override the chart name |
| nodeSelector | object | `{}` | Node selector for pod assignment |
| persistence | object | See values.yaml | Persistence configuration |
| persistence.size | string | `"8Gi"` | Size of persistent volume |
| persistence.storageClass | string | `""` | Storage class for persistent volume |
| podAnnotations | object | `{}` | Pod annotations |
| podLabels | object | `{}` | Pod labels |
| podSecurityContext | object | `{"fsGroup":10001,"fsGroupChangePolicy":"OnRootMismatch"}` | Pod security context |
| readinessProbe | object | `{"exec":{"command":["mysqladmin","ping","-h","127.0.0.1","-u","root","-p$MYSQL_ROOT_PASSWORD"]},"initialDelaySeconds":5,"periodSeconds":2,"timeoutSeconds":1}` | Readiness probe |
| resources | object | `{}` | Resource limits and requests |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":true,"runAsGroup":10001,"runAsNonRoot":true,"runAsUser":10001,"seccompProfile":{"type":"RuntimeDefault"}}` | Security context |
| tolerations | list | `[]` | Tolerations for pod assignment |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)

