# sls-spec

This repository contains the current version of the Service Layout Specification.

The Service Layout Specification (SLS) describes a set of conventions for how a service should be arranged and packaged.
It is designed to separate configuration and other mutable state from service binaries and artifacts, which are treated
as immutable. Making services adhere to this specification provides a consistent experience and enables tooling to be
built around the specification.

For more information on the specification, see [specification.md](specification.md).

## Proposing changes

  1.  Open a ticket in this repository with a description of your proposal. Feel
      free to open a pull request as well if the change is sufficiently straightforward.
  2.  Tag the spec maintainers: currently @markelliot and @gdearment.
  3.  Receive approval from at least two of the maintainers. If there is any dissent,
      iterate on the proposal until all parties are satisfied.
  4.  Open a pull request with the appropriate changes (if not already present). This
      will be merged by one of the maintainers. If the changes are not backwards compatible,
      the version of the specification must be incremented.
  5.  Open tickets or pull requests on projects that implement the specification to support
      the new changes. Currently these projects are:
      * [gradle-java-distribution](https://github.com/palantir/gradle-java-distribution)

## License
This project is made available under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
