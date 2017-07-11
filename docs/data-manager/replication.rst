
Replicating Data
================

Onedata allows for full or partial replication of datasets between
storage resources managed by Oneprovider instances.  The replication
can be performed at the level of entire spaces, directories, files
or specific file blocks.

Onedata web interface provides visual information on the current
replication of each file among the storage providers supporting the
user space in which this file is located.  Sample replication
visualization is presented in the image below:

.. image:: images/replication-status-example.png

REST interface
~~~~~~~~~~~~~~

For full control over transfer and replication users can directly
invoke REST API of Oneprovider service. The documentation for this
API can be found in the `official documentation
<https://onedata.org/#/home/documentation/doc/using_onedata/replication_management.html>`_.
