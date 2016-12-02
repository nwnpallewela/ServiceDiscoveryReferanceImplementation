# ServiceDiscoveryReferanceImplementation
This repository contain vagrent file which will create two nodes with consul agents and demonstrate service discovery use case.

Reference implementation of service discovery will include a distributed setup with two nodes and each of these nodes will contain a consul agent, registrator instance and multiple services as shown in the following diagram.

![alt tag](https://github.com/nwnpallewela/ServiceDiscoveryReferanceImplementation/blob/master/SDReferenceImplementation.png)

Vagrant will be used to demonstrate two nodes in the setup at 172.20.20.10 and 172.20.20.11. These two instances will act as separate machines in a private network. Each node will have instance of consul server and they will elect a leader among them in the cluster. The status of the services registered in both consul agents can be seen in the provided UI, from host machine of the vagrant nodes at http://172.20.20.10:8500/ui. 

Registrator instances in each node will update the services available in the node for the consul agent in the respective node when it discovers any container exposed a port. Any newly created service instance or destroyed instance will be notified to consul agent through registrator and information on available services in the cluster can be found from any node through consul.


