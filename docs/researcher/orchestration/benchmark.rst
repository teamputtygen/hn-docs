.. _benchmark:

Benchmark
=========

Nuvla provides a benchmarking infrastructure, which can be used by any
authenticated user.

The benchmark entries themselves comply with an open schema where
users can publish benchmarking data in a consistent and flexible
way. The published benchmark record requires an associated Service
Offer attribute, where all the instance's resources are described, and
a Credentials attribute, associated with the user running/publishing
the benchmark. The remaining message attributes are optional, letting
users publish any sort of metrics as long as they belong to a
predefined namespace (as described in the official `SlipStream API
Documentation`_).

The benchmarks can then be used to select the best performing clouds
and service offers over time, continuously.

To illustrate this feature and build our own knowledge base, we
publish benchmarks resulting from our continuous monitoring
system. The following clouds are currently covered, and we will expand
this coverage to more clouds and service offers already configured on
the Nuvla service:

 * Exoscale Dietikon
 * Exoscale Geneva
  
The published benchmarks are obtained by running the *`Unixbench`_*
suite to measure CPU performance through fast synthetic benchmarks
like *Whetstone* and *Dhrystone*, every hours.

As an example, the following image shows the CPU performances on both
Exoscale data centers over a period of 12 hours. In the image it is
possible to evaluate the consistency of the CPU performance by
plotting the average benchmark scores plus two edge (low and high)
percentiles, which provide a general view of the CPU MIPS range. The
shorter the range, the more consistent the CPU performance is.

.. image:: ../images/benchmark-exoscale.png

For more details on the benchmark resource, including usage examples,
refer to the `benchmark API documentation`_.

.. _`benchmark API documentation`: http://ssapi.sixsq.com/#benchmark
.. _`SlipStream API Documentation`: http://ssapi.sixsq.com/#service-attribute-(cimi)
.. _`Unixbench`: https://github.com/kdlucas/byte-unixbench
