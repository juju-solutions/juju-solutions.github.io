---
layout: default
title: "Using Juju with Container Technologies like Docker and Kubernetes"
permalink: /containers.html
---

This page will outline how we expect to use Juju with Docker and Kubernetes.

## Docker

These charms provide docker and flannel as a subordinate charm for creating a mesh-overlay network for Docker Container hosts, which enables cross-host communication for distributed services, container farms, and HA configurations.

- [https://github.com/chuckbutler/flannel-docker-charm](https://github.com/chuckbutler/flannel-docker-charm)
- [https://github.com/chuckbutler/docker-charm](https://github.com/chuckbutler/docker-charm)

## Kubernetes 

This bundle can be used to deploy kubernetes onto any cloud it can be used directly in the juju gui or via the juju-quickstart or deployer cli

- [https://github.com/kapilt/bundle-kubernetes](https://github.com/kapilt/bundle-kubernetes)
