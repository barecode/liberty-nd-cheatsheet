# WAS traditional ND to Liberty ND

## Terminology Mapping
### traditional WebSphere to Liberty

Cell -> Collective
Deployment Manager (DMgr) -> Collective Controller
Federated Node -> Host
Federated Server -> Collective Member
Job Manager -> N/A (although you can use the Job Manager to manage Liberty)
Admin Agent -> N/A (no management of Liberty provided)
Session Replicaiton: Memory-to-Memory -> N/A
Session Replication: Database -> Supported! [tutorial]
Session Replication: WebSphere eXtreme Scale (WXS) -> Supported! [tutorial] 
Transactions -> ???
EJB failover -> ???
What else Mike?!

## Conceptual Differences

In traditional WebSphere, an organization of managed servers is called a 'cell'. The cell is controlled by the Deployment Manager (DMgr). The DMgr acts as the "master of the universe" in that it owns the master copy of the configuration for itself as well as all other servers that are part of the cell. The servers that are part of the cell are called 'federated servers'. A federated server gives up control of its life cycle and configuration to the Deployment Manager.

In Liberty, the organization of managed servers is called a 'collective'. The collective is managed by the Collective Controller. Unlike the cell construct, the individual servers in a collective do not give up control of their configuration to the Controller. Each server within a collective retains control of its configuration and allows the Controller to act upon the server's life cycle.

There is an inversion of control between the models. In the cell, the Deployment Manager has total control and exclusive ownership of the configuration. In the collective, the Collective Controller has optional control and no ownership of configuration (other than its own server configuration).

## Common Operations

### How do I...

### 1. List applications in a server?

Applications be deployed either through the dropins directory or configured via the server.xml. There is no command-line operation to query the set and status of deployed applications, but there are REST APIs and MBeans which you can use.

In the Admin Center you can view all deployed applications in the Explore tool. You can enable the Admin Center with the adminCenter-1.0 feature. Details on the full set-up are here: [https://www.ibm.com/support/knowledgecenter/en/SSEQTP_8.5.5/com.ibm.websphere.wlp.doc/ae/twlp_ui_setup.html]

The Admin Center works for both a standalone server as well as a server within a collective. Details on collectives here: [https://developer.ibm.com/wasdev/docs/article_introducingcollectives/]

For more on the configuraiton of applications in the server.xml check out the config:
[https://www.ibm.com/support/knowledgecenter/en/SSD28V_8.5.5/com.ibm.websphere.wlp.core.doc/autodita/rwlp_metatype_core.html#mtFile3]



### 2. How to get "server status" of all servers NOT just the defaultServer.

You can have a few options. Locally, you can run `wlp/bin/server status serverName`. That will tell you the started/stopped status of a given server. If you omit the serverName, it will show you the status of the defaultServer.

Through the Admin Center's Explore tool you can see all servers and their status when accessing the Admin Center on a collective controller. You can also get the backing data via the collective REST API. The best way to explore the Collective REST API is via the apiDiscovery-1.0 feature: [https://www.ibm.com/support/knowledgecenter/en/SS7K4U_liberty/com.ibm.websphere.wlp.zseries.doc/ae/twlp_api_discovery.html]


### 3. How to check if a cluster is running?

There are three primary ways: Admin Center, [ClusterManagerMBean](https://www.ibm.com/support/knowledgecenter/beta/SSHR6W/com.ibm.websphere.javadoc.liberty.doc/com.ibm.websphere.appserver.api.collectiveController_1.5-javadoc/com/ibm/websphere/collective/controller/class-use/ClusterManagerMBean.html) and Collective REST API.

You can access this information via the standard JMX means (native Java, jconsole) or using the JMX REST Connector feature [restConnector-1.0](https://www.ibm.com/support/knowledgecenter/en/SSAW57_8.5.5/com.ibm.websphere.wlp.nd.doc/ae/rwlp_feature_restConnector-1.0.html). 

General info on clusters: [https://developer.ibm.com/wasdev/docs/introducing-liberty-clusters/]

To see a sample of how to use the ClusterManagerMBean, see this sample script to get cluster status: [https://developer.ibm.com/wasdev/downloads/#asset/scripts-jython-Get_Cluster_Status]

### 4. How list a Cluster or Collective members?

As above (Admin Center, MBean and Collective REST API). 

This is a sample script to list cluster members: [https://developer.ibm.com/wasdev/downloads/#asset/scripts-jython-List_Cluster_Members]


