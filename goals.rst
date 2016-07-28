.. _specs-goals:

Goals of SLSv2
==============

This page describes the motivation behind the two required components of SLSv2.

Service Layout
--------------

The SLSv2 service layout is designed to manage services in a consistent way, and to separate
service-specific resources from shared resources. The latter goal achieves these sub-goals:

* Upgrades are simple: Changing which binaries are used to run a service
  is as simple as changing the target of the ``service`` symlink.
* Backups and restores are simple: Backing up or restoring a stopped
  service can be done by backing up or restoring the ``var`` directory of
  the service.
* Duplication of resources is reduced: Multiple installations of the same version of a product
  can share a single copy of their binaries and resources by creating a symlink to the same
  ``service`` directory.

Package Layout
--------------

The SLSv2 package layout is designed to make deployment packages compatible with
both deployment tools and a manual workflow. This achieves these sub-goals:

* An FDE without access to the deployment tools can still deploy the same packages manually.
* Product teams do not have to produce multiple distribution packages aimed at different tools
  or manual workflows.

