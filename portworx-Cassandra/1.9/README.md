
The following instruction will install Cassandra service on DC/OS cluster backed by PX volumes for persistent storage.


# Prerequisites

- A running DC/OS v1.9 cluster with at least 3 private agents with Portworx running on all three
- Portworx works best when installed on all nodes in a DC/OS cluster.  If Portworx is to be installed on a subset of the cluster, then:
  * the agent-nodes must include attributes indicating the participate in the Portworx cluster.
  * services that depend on Portworx volumes must specify "constraints" to ensure they are launched on nodes that can access Portworx volumes.
- A node in the cluster with a working DC/OS CLI.

Please review the main [Portworx on DCOS](https://docs.portworx.com/scheduler/mesosphere-dcos/) documentation.

# Install Cassandra
## Adding repository to DC/OS cluster
Login to a node which has the DC/OS CLI installed and is authenticated to the DC/OS cluster
Run the following command to add the repository to the DC/OS cluster
```
 $ dcos package repo add --index=0 cassandra https://px-dcos.s3.amazonaws.com/v2/cassandra/cassandra.json
```
Now Cassandra-PX package should be available under Universe->Packages
![Cassandra Package List](img/Cassandra-install-01.png)
## Default Install
If you want to use the defaults, you can now run the dcos command to install the service
```
 $ dcos package install --yes cassandra-px
```
You can also click on the  “Install” button on the WebUI next to the service and then click “Install Package”.
This will install all the prerequisites and start a 3 node Cassandra cluster.

## Advanced Install
If you want to modify the defaults, click on the “Install” button next to the package on the DC/OS UI and then click on
“Advanced Installation”
![Cassandra Install Options](img/Cassandra-install-02.png)
This provides an option to change the service name, volume name, volume size, and provide any additional options that needs to be passed to the docker volume driver.
Cassandra related parameters can also be modified for example number of Cassandra nodes.
![Cassandra Install Options](img/cassandra-install-03.png)
![Cassandra Portworx Options](img/Cassandra-install-04.png)
Click on “Review and Install” and then “Install” to start the installation of the service.
## Install Status
Click on the Services page to monitor the status of the installation.
![Cassandra Service Status](img/Cassandra-service-01.png)
Cassandra cluster is ready to use when the schedulre service and all the cassandra services are in running state.
![Cassandra Install Complete](img/Cassandra-service-02.png)
Checking the Portworx's cluster will list multiple volumes that were automatically created using the options provided during install.
There will be one volume for each cassandra server
![Cassandra Portworx Volume](img/Cassandra-volume-01.png)

Install Cassandra CLI using the following command on DC/OS client
```
 $ dcos package install cassandra-px --cli
Installing CLI subcommand for package [cassandra-px] version [stub-universe]
New command available: dcos cassandra-px
```
# Further resource

For more detailed description on using Portworx through DCOS please visit  [Portworx on DCOS framework homepage](https://docs.portworx.com/scheduler/mesosphere-dcos)

