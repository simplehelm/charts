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

Unlike the Bitnami chart it does not have the facility to create extra databases or users during initialisation, due to the complexity and security tradeoffs required. If there is sufficient demand we can add the ability to mount external volumes to `/docker-entrypoint-initdb.d/`.

Please see the [documentation for the container image](https://github.com/docker-library/docs/blob/master/postgres/README.md) for additional environment variables you can provide to customise configuration and behaviour.

## Installation

This chart is available as an OCI package from GitHub Container Registry.

### Install from OCI Registry

```bash
# Install the chart directly from OCI
helm install my-postgres oci://ghcr.io/simplehelm/postgresql --set auth.postgresPassword=mypassword
```

### Install with existing secret

```bash
# Create secret first
kubectl create secret generic postgresql-secret \
  --from-literal=postgres-password=mypassword

# Install with existing secret
helm install my-postgres oci://ghcr.io/simplehelm/postgresql --set auth.existingSecret=postgresql-secret
```

## Requirements

**Authentication is required**: You must provide either `auth.postgresPassword` or `auth.existingSecret`. The chart will fail to install without proper authentication configuration.

If using `auth.existingSecret`, the secret must contain:
- `postgres-password`: The postgres user password

## Migration from bitnami/postgresql

The Bitnami chart and container image for PostgreSQL has some quite complex behaviour which makes it very difficult to provide a straight migration path. Unfortunately in this case, we recommend that you use `pg_dump` and `pg_restore` to migrate between charts.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity for pod assignment |
| auth | object | See values.yaml | PostgreSQL authentication configuration |
| auth.existingSecret | string | `""` | Name of existing secret to use for credentials (must contain 'postgres-password' key) |
| auth.postgresPassword | string | `""` | postgres (superuser) user password (required if existingSecret is not set) |
| env | list | `[{"name":"POSTGRES_INITDB_ARGS","value":"--auth-host=scram-sha-256"}]` | Additional environment variables |
| fullnameOverride | string | `""` | Override the full name |
| image | object | `{"pullPolicy":"IfNotPresent","repository":"docker.io/library/postgres","tag":""}` | Container image configuration |
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.repository | string | `"docker.io/library/postgres"` | PostgreSQL container image repository |
| image.tag | string | `""` | Overrides the image tag which by default uses majorVersion |
| imagePullSecrets | list | `[]` | Secrets for pulling images from private registries |
| livenessProbe | object | `{"exec":{"command":["pg_isready","-h","localhost","-U","postgres"]},"initialDelaySeconds":30,"periodSeconds":10,"timeoutSeconds":5}` | Liveness probe |
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
| readinessProbe | object | `{"exec":{"command":["pg_isready","-h","localhost","-U","postgres"]},"initialDelaySeconds":5,"periodSeconds":2,"timeoutSeconds":1}` | Readiness probe |
| resources | object | `{}` | Resource limits and requests |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":true,"runAsGroup":10001,"runAsNonRoot":true,"runAsUser":10001,"seccompProfile":{"type":"RuntimeDefault"}}` | Security context |
| tolerations | list | `[]` | Tolerations for pod assignment |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)

