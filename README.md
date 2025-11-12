# Simple Helm Charts

![Status](https://img.shields.io/badge/status-under%20development-orange)
![Helm v3](https://img.shields.io/badge/helm-v3-blue)
![Helm v4](https://img.shields.io/badge/helm-v4-blue)
![License](https://img.shields.io/badge/license-MIT-green)

This project aims to provide simple, but secure and well-maintained Helm charts for commonly used open source projects.

Since Bitnami has [chosen to paywall their container images](https://github.com/bitnami/containers/issues/83267), the community has no clear way to deploy many common applications. For many use cases a simple chart with sensible defaults is an ideal solution to this. However despite the existence of freely available and well maintained container images for many of these applications, no clear alternative to Bitnami charts exists. This project aims to change that.

Initial targets include MySQL and PostgreSQL. Previously Valkey was on the list but the project has [already made their own chart that embraces the same values](https://github.com/valkey-io/valkey-helm). With proper tooling and community support, we plan to expand this list sustainably.

> Simplify, then add lightness
>
> -- [<cite>Colin Chapman, founder of Lotus</cite>](https://en.wikipedia.org/wiki/Colin_Chapman)

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
