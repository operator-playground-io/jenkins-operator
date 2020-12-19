# jenkins operator
### Overview
The Jenkins Operator is a Kubernetes Native Operator which manages operations for Jenkins on Kubernetes. It has been built with Immutability and declarative Configuration as Code in mind. The Jenkins Operator is easy to install with just a few manifest and allows users to configure and manage Jenkins on Kubernetes.

Out of the box it provides:

- Integration with Kubernetes
- Pipelines as Code
- Extensibility via Groovy Scripts
- Secure Defaults and Hardening


**jenkins-operator:**

With the standard Jenkins deployment we face lot of problems . We want to make Jenkins more robust, suitable for dynamic and multi-tenant environments where Jenkins Operator helps.

Below are problems which will get solve with Jenkins Operator

- Installing plugins with incompatible versions or security vulnerabilities
- Better configuration as code
- Security and hardening out of the box
- Make errors more visible for end users
- Backup and restore for jobs history
- Orphaned jobs with no JNLP connection
- Handle graceful shutdown properly
- Proper end to end tests for Jenkins lifecycle

**Architecture and design:**
Jenkins Operator fundamentals

The Jenkins Operator design incorporates the following concepts:

- Watches any changes of manifests and maintain the desired state according to deployed custom resource manifest
- Implements the main reconciliation loop which consists of two smaller reconciliation loops - base and user

![](_images/Architecture.png)

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

![](_images/Architecture1.png)





