_Back to [specification.md](specification.md)_

# SLS Layout Specification

#### Version 1

This page documents the directory structure of an SLS artifact.

In [Manifest version 1.0](manifest.md), the `product-type` field specifies the type and version
of the artifact. The version refers to the specification version for this page. There
are currently three known types:

* `service`: products which run long-lived processes. This is the default if type is unset.
* `daemon`: like services, but run in the control plane
* `asset`: archives which can be downloaded by other services but do not "run" per se.

## Packaging Specification

The deployment package must be a gzip-compressed tar archive with the file name
`$PRODUCT_NAME-$PRODUCT_VERSION` and either the `.sls.tgz` or the `.sls.tar.gz` extension.

All SLS artifacts contain a single top-level directory with a user-readable name, required to be
`$PRODUCT_NAME-$PRODUCT_VERSION`. The tar filename headers can not begin with `./` or similar.

## `deployment` Directory

Within the top-level directory exists a `deployment` directory with a file named `manifest.yml`.
The manifest contains a set of descriptors about the artifact and is documented in
[manifest.md](manifest.md). Deployment tools may specify other files to be included in the
deployment directory.

Example: for a product `foobar` version `1.2.3`:

```
foobar-1.2.3
└── deployment
    └── manifest.yml
```

## Services and Daemons

Services and Daemons abide by the same layout specification. The following are **required**:

* A `service` directory which contains all immutable files that do not change between
  product installations. This includes binaries, libraries, and static resources.
  This can also be symlink to such a directory.

* An init script at the path `service/bin/init.sh` which supports the `start`,`stop`, and
  `status` commands. These commands should behave in accordance with the
  [Linux Standard Base](http://refspecs.linuxbase.org/LSB_3.1.1/LSB-Core-generic/LSB-Core-generic/iniscrptact.html)
  specification.

```
foobar-1.2.3
├── deployment
│   └── manifest.yml
└── service
    └── bin
        └── init.sh
```

If your product will mutate the filesystem, it should aim to do so within a `var` directory located
within the top-level directory (a sibling of `service`). `var` is not strictly required. A suggested
`var` layout:

```
foobar-1.2.3
├── deployment
│   └── manifest.yml
├── service
│   └── bin
│       └── init.sh
└── var
    ├── conf            # Application configuration, possibly managed by deployment tooling.
    ├── data            # On-disk state managed by the application
    │   └── tmp         # Application temp directory
    ├── log             # Application logs
    ├── run             # Application run files (e.g. pidfile)
    ├── security        # Contains keys, TLS certificates, and other security files, possibly managed by deployment tooling.
    └── tmp             # Reserved for deployment tooling temp directory

```

#### Installing, Running, and Upgrading an SLS Service

Because the `service` directory is immutable, it can be shared amongst several installations of the
same artifact on a host via symbolic links. Each installation should have its own `var` directory
which is a sibling of the symlink, not of the directory to which the symlink points.

When deployment tooling runs an SLS service, it should set the `SERVICE_HOME` environment variable
and the current working directory to the top-level directory of the service (parent of `var`) before
invoking `service/bin/init.sh`.

When the deployment tooling upgrades an SLS service, new files in `var` (which do not currently
exist in the installation) will be copied into the `var` directory, but existing files *shall not*
be overwritten by the upgrade. This is known as a "no-clobber" strategy.

## Assets

Assets contain an `asset` directory in addition to their `deployment` directory which can contain
freeform data and files:

```
foobar-1.2.3
├── asset
│   └── ...
└── deployment
    └── manifest.yml
```
