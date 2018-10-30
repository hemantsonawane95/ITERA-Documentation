
|

|

KUBERNETES AND CONTAINER CLOUD ARCHITECTURE
===========================================

OVERVIEW
^^^^^^^^

Main goal of this architecture is to provide flexible infrastructure for container
services. This goal is achieved by using Kubernetes as cornerstone. OpenStack
Cinder for providing persistent storage. And Project Calico as robust networking
solution.

This architecture also includes CI/CD build around Jenkins tooling. Supported by
GitLab and a Docker registry as storages of code and containers.
Also this architecture is created to be portable and presents opportunities for
re-using it's components buy higher level applications.

- The Container Cloud is composed of 5 nodes

	- Kubernetes cluster is installed on 3 of these nodes.	
	
	- The 2 other nodes host CI/CD services:
		
		- Gitlab : Docker registry and Git repositories hosting
		
		- Jenkins: Pipelines for CI/CD

.. figure:: KDB.*
   :width: 100 %
   :height: 350 px
   :scale: 100 %
   :align: center
   :alt: KDB.*

OPENSTACK CLOUD PROVIDER FOR KUBERNETES
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Kubernetes provides automated container orchestration -- management of your machines and services for you -- improving your reliability and reducing the time and resources. In above architecture there aretwo components of OpenStack which interacts with Kubernetes: Octavia and Cinder which provides loadbalancing and PVC respectively over which virtual machines are created to deploy kubernetes on it. As soon as you have kubernetes you can start deploying applications on it.
Cinder is a persistent block level storage which can be used with openstack compute instances. It manages the creation, attachment and detachment of block devices to the cloud servers. Cinder provides an API to attach/detach volume to Nova instances. With OpenStack as a cloud provider for Kubernetes, volumes are created directly by Kubernetes and automatically attached to the virtual machines of the Kubernetes cluster.

Octavia is an open source, operator-scale load balancing solution designed to work with OpenStack. Octavia accomplishes its delivery of load balancing services by managing a fleet of virtual machines, containers, or bare metal servers  which it spins up on demand. Thank to Octavia, Kubernetes can expose services to the outside world using OpenStack floating IPs.

.. figure:: Architecture.*
   :scale: 100 %
   :width: 100 %
   :align: center
   :alt: Architecture.*

LOGGING, MONITORING AND ALERTING
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: GK.*
   	  :height: 100 px
   	  :width: 200 px
	  :align: center


The Logging, Monitoring, and Alerting toolchain i.e (LMA), is the operational health and response monitoring solution of ITERA for the Kubernetes cluster.

- The LMA stacks includes 2 dashboards:

	- Grafana allows to monitor your Kubernetes cluster performance. It includes 4 dashboards, Cluster, Node, Pod/Container and Deployment.The metrics collected are high-level cluster and node stats a          s well as lower level pod and container stats. Use the high-level metrics to alert on and the low-level metrics to troubleshoot.

	- Kibana which is the UI companion of Elasticsearch, simplifying visualization and querying. Kibana also has its own dashboard similar like grafana. Kibana core ships with the classics: histogram,          line graphs, pie charts, sunbursts, and more. Plus, you can use Vega grammar to design your own visualizations. Kibana developer tools offer powerful ways to help developers interact with the El          astic Stack. With Console, you can bypass using curl from the terminal and tinker with your Elasticsearch data directly.

.. figure:: GDB.*
   :width: 100 %
   :height: 300 px
   :scale: 100 %
   :align: center
   :alt: GDB.*
