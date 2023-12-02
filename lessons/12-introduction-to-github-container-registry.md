---
title: Introduction to GitHub Container Registry
---

## Agenda

- What is GHCR?
- Supported Artifact Types
- Usage

---

## What is GHCR?

- Solution of GitHub to store Artifacts
- SaaS or On Premise

---

## Artifact Types

- Docker
- RubyGems
- npm
- Apache Maven
- Gradle
- NuGet

---

## Usage

- Depends on your kind of Artifact

---

## Container Example

```bash
podman login ghcr.io
podman pull ghcr.io/zhews/pygreeter:v0.1.0
podman build -t ghcr.io/zhews/pygreeter:v0.1.1 .
podman push ghcr.io/zhews/pygreeter:v0.1.1
```
