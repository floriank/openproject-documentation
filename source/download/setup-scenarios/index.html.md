# Setup Scenarios

Depending on volume and security requirements, OpenProject can be run in three different scenarios:

1. _Standalone_: OpenProject and the database management system (DBMS) are run on the same machine.
2. _Default Setup_: OpenProject and the DBMS are run on separate machines. The DBMS may or may not be shared with other applications. This is the recommended setup for small to medium organizations.
3. _Cluster Setup_: OpenProject is installed and run redundantly on multiple machines to which load is distributed using a load balancer. The DBMS is run on a separate machine and may or may not be clustered by itself. This is the recommended setup for larger organizations.

OpenProject itself comprises a number of components/processes:

1. Unicorn – this component hosts the actual application. Depending on the settings chosen, there are at least two unicorn processes running in parallel on the app server machine.
2. Apache 2 – this component provides the external interface, handles SSL termination (if SSL is used) and distributes/forwards web requests to the unicorn processes.
3. Ruby/Rake – Worker job for handling asynchronous tasks.

In either scenario, it is recommended to dedicate app server machines to OpenProject. While it is possible to install and run other applications in parallel or to use Apache for additional purposes, this increases the risk of experiencing unexpected effects when installing, updating, or reconfiguring OpenProject.

