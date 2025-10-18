# Simple Helm Charts

![Status](https://img.shields.io/badge/status-under%20development-orange)
![Helm](https://img.shields.io/badge/helm-v3-blue)
![License](https://img.shields.io/badge/license-MIT-green)

The project aims to provide simple, but secure and well-maintained Helm charts for commonly used open source projects.

Since Bitnami has [chosen to paywall their container images](https://github.com/bitnami/containers/issues/83267), there is now an open question within the community of how to deploy a lot of these common applications.

For many use cases a simple chart with sensible defaults is an ideal solution to this. However despite the existence of freely available and well maintained container images for many of these applications, no obvious source of non-Bitnami charts has been created. This project aims to change that.

Initially the targets are MySQL, PostgreSQL and Valkey. However with a library chart, good automation and the support of others I hope this can be sustainably expanded.

## Project Principles

* Keep it simple. We do not aim to cover every use case.
* Use well-maintained images such as those in the Docker Hub Library.
* Always use secure defaults.
* Use SemVer and don't break things in minor releases!
* Avoid corporate control.

## Project goals
* Automated chart updates for container image tags.
* Test everything!

## Project Status
Very much in the initial stages at the moment. After the initial structure and tooling is built we will be open to additional contributors.
