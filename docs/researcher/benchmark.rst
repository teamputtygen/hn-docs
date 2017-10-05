.. _benchmark:

Benchmark
=========

Nuvla provides a benchmarking infrastructure, which can be used by any authenticated user.

The benchmark entries themselves complies with an open scheme where users can publish benchmark data
in a consistent and flexible way. This includes defining their own namespace, such that all benchmarks can be managed
together, in a coordinated way.

The benchmark can then be used to select best clouds and service offers.

To illustrate this feature and build our own knowledge base, we publish benchmarks resulting from our continuous
monitoring system. The following clouds are currently covered, and we will expand this coverage to more clouds already
configured on the Nuvla service:

 * Open Telekom Cloud (OTC)
 * Exoscale Dietikon
 * Exo Geneva
  
The published benchmark is the result of running the *Unixbench* suites to measure CPU performance through *Whetstone* and *Dhrystone*, every few hours.

The following image is a screen capture from Kibana of the latest 12 hours benchmark for the Exoscale clouds,

.. image:: images/benchmark-exoscale.png

showing expected results for this type of benchmark.

For more details on the benchmark resource, including usage examples, refer to the `benchmark API documentation`_.

.. _`benchmark API documentation`: http://ssapi.sixsq.com/#benchmark
