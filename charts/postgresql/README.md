# postgresql

![Chart Version 0.0.1](https://img.shields.io/badge/version-0.0.1-blue)
![Alpha Maturity - Not Production Ready](https://img.shields.io/badge/maturity-alpha-red)
![Tested on AMD64 and ARM64 Architectures](https://img.shields.io/badge/arch-amd64%20%7C%20arm64-lightgrey)
![Security Scanned with Trivy](https://img.shields.io/badge/security-trivy-green)
![Kubernetes Manifests Validated with Kubeconform](https://img.shields.io/badge/validation-kubeconform-green)
![Tested with Helm Tests](https://img.shields.io/badge/testing-helm--test-green)

A simple Helm chart for PostgreSQL

## Overview

This chart deploys PostgreSQL using the official [Docker Library PostgreSQL image](https://hub.docker.com/_/postgres). It provides a simple, secure PostgreSQL deployment with persistent storage. Currently it does not support read replicas but it may do in the future.

## Installation

This chart is available as an OCI package from GitHub Container Registry.

### Install from OCI Registry

```bash
# Install the chart directly from OCI
helm install my-postgres oci://ghcr.io/simplehelm/postgresql --set auth.rootPassword=mypassword
```

### Install with existing secret

```bash
# Create secret first
kubectl create secret generic postgresql-secret \
  --from-literal=postgresql-root-password=mypassword

# Install with existing secret
helm install my-postgres oci://ghcr.io/simplehelm/postgresql --set auth.existingSecret=postgresql-secret
```

## Requirements

**Authentication is required**: You must provide either `auth.rootPassword` or `auth.existingSecret`. The chart will fail to install without proper authentication configuration.

If using `auth.existingSecret`, the secret must contain:
- `postgres-password`: The postgres user password
- `password`: The password for an additional user (only required if `auth.username` is set)

## Migration from bitnami/postgresql

First, take a backup! Use `pg_dump`, take a `VolumeSnapshot` or transcribe the data onto a stone tablet. I don't mind how you do it, but please do!

Now you've done that, as PersistentVolumeClaims are not deleted along with a StatefulSet. If you have deployed the Bitnami PostgreSQL Helm chart with reasonably standard settings, it should be possible to uninstall your Helm release and then re-deploy this chart with the same name (but a different values file) without losing any data.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity for pod assignment |
| auth | object | See values.yaml | PostgreSQL authentication configuration |
| auth.database | string | `""` | Additional database to create |
| auth.existingSecret | string | `""` | Name of existing secret to use for credentials (must contain 'postgres-password' key, and 'password' key if username is set) |
| auth.password | string | `""` | Password for additional user |
| auth.postgresPassword | string | `""` | postgres (superuser) user password (required if existingSecret is not set) |
| auth.username | string | `""` | Additional user to create with all privileges on the additional database |
| env | list | `[]` | Additional environment variables |
| fullnameOverride | string | `""` | Override the full name |
| image | object | `{"pullPolicy":"IfNotPresent","repository":"docker.io/library/postgres","tag":""}` | Container image configuration |
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.repository | string | `"docker.io/library/postgres"` | PostgreSQL container image repository |
| image.tag | string | `""` | Overrides the image tag which by default uses majorVersion |
| imagePullSecrets | list | `[]` | Secrets for pulling images from private registries |
| livenessProbe | object | `{"exec":{"command":["pg_isready","-h","127.0.0.1","-U","postgres"]},"initialDelaySeconds":30,"periodSeconds":10,"timeoutSeconds":5}` | Liveness probe |
| majorVersion | string | `"18"` | Major version of PostgreSQL to use as default tag and in resource labels |
| nameOverride | string | `""` | Override the chart name |
| nodeSelector | object | `{}` | Node selector for pod assignment |
| persistence | object | See values.yaml | Persistence configuration |
| persistence.size | string | `"8Gi"` | Size of persistent volume |
| persistence.storageClass | string | `""` | Storage class for persistent volume |
| podAnnotations | object | `{}` | Pod annotations |
| podLabels | object | `{}` | Pod labels |
| podSecurityContext | object | `{"fsGroup":10001,"fsGroupChangePolicy":"OnRootMismatch"}` | Pod security context |
| priorityClassName | string | `""` | Priority class name for pod scheduling |
| readinessProbe | object | `{"exec":{"command":["pg_isready","-h","127.0.0.1","-U","postgres"]},"initialDelaySeconds":5,"periodSeconds":2,"timeoutSeconds":1}` | Readiness probe |
| resources | object | `{}` | Resource limits and requests |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":true,"runAsGroup":10001,"runAsNonRoot":true,"runAsUser":10001,"seccompProfile":{"type":"RuntimeDefault"}}` | Security context |
| tolerations | list | `[]` | Tolerations for pod assignment |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)

