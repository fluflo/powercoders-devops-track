---
title: Introduction to Nexus
---

## Agenda

- What is Nexus?
- Supported Artifact Types
- Usage

---

## What is Nexus?

- Platform for storing Build Artifacts
- Open Source and Paid Version
- On Premise

---

## Why?

- Speed
- Bandwidth
- Security
- Internal and Public Repositories

---

## Artifact Types

- Docker
- Go
- PyPI
- Raw
- ...

[help.sonatype.com/repomanager3/nexus-repository-administration/formats](https://help.sonatype.com/repomanager3/nexus-repository-administration/formats)

---

## Usage

- Create Repository
- Depends on your kind of Artifact

---

## Container Example

```bash
podman login <hostname>:<port>
podman pull <hostname>:<port>/gogreeter:v0.1.0
podman build -t <hostname>:<port>/gogreeter:v0.1.1 .
podman push <hostname>:<port>/gogreeter:v0.1.1
```
