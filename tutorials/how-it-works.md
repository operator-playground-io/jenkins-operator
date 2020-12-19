---
title: How Jenkins Operator works
description: This tutorial explains how Jenkins Operator works
---
### How it works

The jenkins-operator design incorporates the following concepts:

- observe all changes of the manifests and maintain the desired state according to the Jenkins Custom Resource manifest
- implement the main reconciliation loop which consists of two main phases — base and user

![](_images/operator-flow.PNG)

Base reconciliation phase is responsible for ensuring base Jenkins configuration, like Jenkins Master pod, plugins, hardening, etc.

User reconciliation phase is responsible for ensuring user-provided configuration, like custom Groovy scripts of Configuration as Code plugin files.

![](_images/Architecture.PNG)

Base reconciliation loop takes care of reconciling base Jenkins configuration, which consists of:

- Ensure Manifests - monitors any changes in manifests
- Ensure Jenkins Pod - creates and verifies the status of Jenkins master Pod
- Ensure Jenkins Configuration - configures Jenkins instance including hardening, initial configuration for plugins, etc.
- Ensure Jenkins API token - generates Jenkins API token and initialized Jenkins client

User reconciliation loop takes care of reconciling user provided configuration, which consists of:

- Ensure Restore Job - creates Restore job and ensures that restore has been successfully performed
- Ensure Seed Jobs - creates Seed Jobs and ensures that all of them have been successfully executed
- Ensure User Configuration - executed user provided configuration, like groovy scripts, configuration as code or plugins
- Ensure Backup Job - creates a Backup job and ensures that backup has been successfully performed

![](_images/Architecture1.PNG)

