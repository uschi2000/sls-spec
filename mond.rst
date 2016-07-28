.. _specs-mond:

mond Extension
==============

This extension of the :ref:`specs-layout` enables monitoring checks via leveraging Houston's
built in Nagios and NRPE modules. To use mond, a service must conform to both SLSv2 itself
and this extension.

The mond extension of SLSv2 includes:

* :ref:`mond-check-script`

.. note:: See the :ref:`for-devs-mond` page for guidance on adding monitoring checks to
  your product.

.. _mond-check-script:

Monitoring Check Script(s)
--------------------------

Script(s) exist at the path ``<SERVICE_HOME>/service/monitoring/bin/``. When properly configured,
these scripts will be called by Nagios through the localhost's NRPE module. The scripts can accept arguments
and must abide by the following Nagios contract:

* Must exit with one of several possible return values

  * 0 = OK
  * 1 = WARNING
  * 2 = CRITICAL
  * 3 = UNKNOWN

* Must return at least one line of text output to STDOUT

For further information around Nagios Plugins please view the `Nagios Plugin API <https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/pluginapi.html>`_
