# Monitoring Extension

This extension of the specification enables monitoring via the deployment infrastructure.
To use mond, a service must conform to both [SLS](/required.md) and this extension.


# Monitoring Check Script(s)

One or more scripts exist at the path ``<SERVICE_HOME>/service/monitoring/bin/``. The scripts can accept arguments
and must abide by the following contract:

* Must exit with one of four possible return values

  * 0 = OK
  * 1 = WARNING
  * 2 = CRITICAL
  * 3 = UNKNOWN

* Must return at least one line of text output to STDOUT
