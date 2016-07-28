.. _specs-layout:

Service Layout Specification
============================

**Version: 2.0.0**

The Service Layout Specification Version 2 (SLSv2) describes how a product should be arranged
and packaged so that Skylab can manage service instances of it. It includes a **required**
service layout and package layout, along with **optional** extensions that enable additional
Skylab functionality.

This section includes:

* :ref:`specs-goals`
* :ref:`specs-required`

It also includes documentation for each optional extension of SLSv2. These are:

* :ref:`specs-mond`

The Skylab functionality enabled for a service that adheres to each component of SLSv2 is:

.. list-table::
  :widths: 25 75
  :header-rows: 1

  * - SLSv2 Component
    - Skylab Functionality
  * - Required Components
    - * Adding packages to Houston
      * Installing services
      * Generating service config files
      * Upgrading service binaries
      * Managing a service's lifecycle
  * - mond Extension
    - * Integrate Nagios checks

.. toctree::
   :titlesonly:
   :hidden:
   :maxdepth: 1

   pages/goals
   pages/required
   pages/mond
