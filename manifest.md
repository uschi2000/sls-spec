_Back to [specification.md](specification.md)_

# SLS Manifest Specification

The manifest file declares immutable information about a package. It is located in the artifact at
`deployment/manifest.yml`. It is permissible to include additional keys in the manifest document
for deployment tooling to consume.

## Version 1.0 (in development)

```yaml
manifest-version: 1.0               # (required) Manifest specification version
product-group: com.palantir.baz     # (required) Maven group for the product
product-name: foobar                # (required) Maven artifact ID for product
product-version: 1.2.3              # (required) Maven version
product-type: service.v1            # (optional) Product layout type with spec version. Default to 'service.v1' if omitted.
                                    #            Type is one of service.v1, daemon.v1, asset.v1. See layout.md for details.
```

## Version 1.0.0-alpha

```yaml
manifestVersion: 1.0.0-alpha    # (required) Manifest specification version
productGroup: com.palantir.baz  # (optional) Maven group for the product
productName: foobar             # (required) Maven artifact ID
productVersion: 0.35.0          # (required) Maven version
daemon: false                   # (optional) Set to `true` if deployment tools should treat this as a control-plane daemon
```
