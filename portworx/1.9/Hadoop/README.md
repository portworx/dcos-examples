
The following instruction will install Hadoop service on DC/OS cluster backed by PX volumes for persistent storage.


# Prerequisites

- A running DC/OS v1.9 cluster with at least 3 private agents with Portworx running on all three
- Portworx works best when installed on all nodes in a DC/OS cluster.  If Portworx is to be installed on a subset of the cluster, then:
  * the agent-nodes must include attributes indicating the participate in the Portworx cluster.
  * services that depend on Portworx volumes must specify "constraints" to ensure they are launched on nodes that can access Portworx volumes.
- A node in the cluster with a working DC/OS CLI.

Please review the main [Portworx on DCOS](https://docs.portworx.com/scheduler/mesosphere-dcos/) documentation.

# Install Hadoop
## Adding repository to DC/OS cluster
Login to a node which has the DC/OS CLI installed and is authenticated to the DC/OS cluster
Run the following command to add the repository to the DC/OS cluster
```
$ dcos package repo add --index=0 hadoop-px https://px-dcos.s3.amazonaws.com/v1/hadoop-px/hadoop-px.zip
```
Now Hadoop-PX package should be available under Universe->Packages
![Hadoop Package List](img/Hadoop-install-01.png)
## Default Install
If you want to use the defaults, you can now run the dcos command to install the service
```
 $ dcos package install --yes hadoop-px
```
You can also click on the  “Install” button on the WebUI next to the service and then click “Install Package”.
It will create 3 Journal Nodes, 2 Name Nodes, 2 Nodes for the Zookeeper Failover Controller, 3 Data Nodes and 3 Yarn Nodes. 
The Data and Yarn nodes will be co-located on the same physical host.

## Advanced Install
If you want to modify the defaults, click on the “Install” button next to the package on the DC/OS UI and then click on
“Advanced Installation”
![Hadoop Install Options](img/Hadoop-install-02.png)
This provides an option to change the service name, volume name, volume size, and provide any additional options that needs to be passed to the docker volume driver.
You can also configure other Hadoop related parameters on this page including the number of Data and Yarn nodes for the Hadoop clsuter.
![Hadoop Install Options](img/Hadoop-install-03.png)
![Hadoop Portworx Options](img/Hadoop-install-04.png)
Click on “Review and Install” and then “Install” to start the installation of the service.
## Install Status
Click on the Services page to monitor the status of the installation.
![Hadoop Service Status](img/Hadoop-service-01.png)
There will be one service for the scheduler and one each for the Journal, Name, Zookeeper, Data and Yarn nodes. are in running state.
![Hadoop Install Complete](img/Hadoop-service-02.png)
Checking the Portworx's cluster will list multiple volumes that were automatically created using the options provided during install.
On the Portworx cluster, you should see multiple volumes that were automatically created using the options provided during install, one for each of the Journal, Name and Data nodes.
![Hadoop Portworx Volume](img/Hadoop-volume-01.png)

Install Hadoop CLI using the following command on DC/OS client
```
$ dcos package install hadoop-px --cli
Installing CLI subcommand for package [hadoop-px] version [stub-universe]
New command available: dcos hadoop-px
```
# Further resource

For more detailed description on using Portworx through DCOS please visit  [Portworx on DCOS framework homepage](https://docs.portworx.com/scheduler/mesosphere-dcos)

