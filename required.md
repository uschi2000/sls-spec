# Standard Layout Specification Required Components

This page describes the two **required** components of the specification, service layout and package layout.

## Standard Layout Specification Details

These components are **required** by SLS. :

* A ``service`` directory which contains all immutable files that do not change between
  product installations. This includes binaries, libraries, and static resources.
  This can also be symlink to such a directory.

* A ``deployment`` directory which contains a file named ``manifest.yml` This file contains a set of descriptors about the artifact:

   * `manifestVersion`: the version of this file
   * `productName`: the name of the product contained in this artifact
   * (optional) `productGroup`: the Maven-style group for this product
   * `productVersion`: the version of the product

  As an example:
  ```yaml
  manifestVersion: 1.0.0-alpha
  productName: foobar
  productGroup: com.palantir.baz
  productVersion: 0.35.0
  ```
  
  Generally, artifacts are published to a Maven repo, and `productGroup:productName:productVersion` should provide
  a precise address for how to retreive this artifact from that Maven repo.

  The ``deployment`` directory is a sibling of the ``service`` and ``var``
  directories. A product may depend on any resources in the ``deployment`` directory.

* An init script at the path ``service/bin/init.sh`` which supports the ``start``,
  ``stop``, and ``status`` commands. These commands should behave in accordance with the
  [Linux Standard Base](http://refspecs.linuxbase.org/LSB_3.1.1/LSB-Core-generic/LSB-Core-generic/iniscrptact.html)
  specification. When the deployment infrastructure calls the init script, it sets the current directory and the ``SERVICE_HOME``
  environment variable to the top-level directory of your service, i.e., the parent directory of both the
  ``service`` and ``deployment`` directories.

The components below are **suggested** by the SLS service layout. If you include these
directories in your product's layout, then your product won't need additional configuration
to integrate with deployment tooling that relies on them:

* A ``var`` directory which contains all service-specific
  files. This includes configuration files, PID files, and log files.
  This directory is a sibling of the ``service`` directory. If ``service``
  is a symlink to the directory with your product's immutable files, then ``var`` is
  a sibling of the symlink, not of the directory to which the symlink points. When the deployment infrastructure
  upgrades an SLS service, new files in ``var`` (which did not exist in the old version
  of the service) will be copied into the service's ``var`` directory, but existing
  files *will not* be overwritten by the upgrade.

* ``var/log``: directory which contains log files
* ``var/conf``: directory which contains configuration files
* ``var/data``: directory which contains files that a service instantiating your product will edit
* ``var/tmp``: directory can be used for any temporary files that the product may use

The components below will me **managed and modified** by the SLS service layout. If you include these
directories in your product's layout, recognize that their contents might be changed in the process of deployment.

* ``var/security``: directory which contains certificates and security stores
* ``var/conf``: directory which contains configuration files

Below is the directory structure that results from including both the required and suggested
components of the service layout:

    service/
      bin/
        init.sh
    var/
      log/
      conf/
      data/
      security/
    deployment/
      manifest.yml

Note that the service's binaries are *not* included in this directory tree. Instead, they are
stored in a separate ``.binaries`` directory, the default path of which is
``<deployhome>/.binaries``. This allows multiple services to share the same binaries.

## Standard Layout Service Package Layout

The deployment package must be a gzip-compressed tar archive with the file name
``<PRODUCT_NAME>-<PRODUCT_VERSION>`` and either the ``.tgz`` or the ``.tar.gz`` extension.

The package must contain a single top-level directory which *should* have a user-readable name,
such as ``<PRODUCT_NAME>-<PRODUCT_VERSION>``. This directory must contain the ``service`` and
``deployment`` directories and may also contains the optional ``var`` directory.

Below are the contents of a deployment package that includes all three directories::

    <PRODUCT_NAME>-<PRODUCT_VERSION>/
      service/
      var/
      deployment/

The archive *must not* have a top-level ``.`` directory:

    $ tar -tf very-bad-1.0.0.tgz
      ./
      ./very-bad-1.0.0/
      ./very-bad-1.0.0/service/
      #...
    $ tar -tf very-good-1.0.0.tgz
      very-good-1.0.0/
      very-good-1.0.0/service/
      #...
