# Service Layout Specification

The Service Layout Specification Version 2 (SLSv2) describes how a product should be arranged
and packaged so that deployment infrastructure can use it to create and manage service instances. It includes a **required**
service layout and package layout, along with **optional** extensions that enable additional
deployment infrastructure functionality.

For each component of SLSv2, the functionality it enables is enumerated below.

* [Required Components](/required.md/)
  * Add and index the product in the package repository
  * Install services
  * Generate service configuration files
  * Upgrade services
  * Manage the lifecycle of services
* [Monitoring Extension](/monitoring.md/)
  * Integrate monitoring checks for services into the deployment infrastructure

## Goals of SLSv2

### Service Layout

The SLSv2 service layout aims to provide a consistent way to manage services.

* Simple, atomic upgrade and rollback: Changing which binaries are used to run a service is achieved by changing the ``service`` symlink.
* Straightforward backup and restore: Backing up or restoring a stopped service can be performed by copying the ``var`` directory of the service.

### Package Layout

The SLSv2 package layout is designed to make deployment packages compatible with
both deployment tools and a manual workflow.

* An engineer without access to deployment tools can still deploy the same packages manually.
* Product teams do not have to produce multiple distribution packages aimed at different tools
  or manual workflows.

