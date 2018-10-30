
|

|

CI/CD
=====

The CI/CD are the pillars of DevOps which improve collaboration between operation and development terms and enable the delivery of high quality software for continued success. A Docker registry is there in gitlab and it can be used for pipelines, kubernetes and many more automation process. 
	
With CI, developers frequently integrate their code into a common repository. Rather than building features in isolation and submitting each of them at the end of the cycle, they continuously build software work products several times on any given day. Every time the code is inputted, the system starts the compilation process, runs unit tests and other quality-related checks as needed. CI relies heavily on test suites and an automated test execution. When done correctly, it enables developers to perform frequent and iterative builds, and deal with bugs early in the lifecycle.
	
CD executes a progressive set of test suites against every build and alerts the development team in case of a failure, which then rectifies it. In situations where there are no issues, CD conducts tests in a sequential manner. The end result is a build that is deployable and verifiable in an actual production environment. CD executes a progressive set of test suites against every build and alerts the development team in case of a failure, which then rectifies it. In situations where there are no issues, CD conducts tests in a sequential manner. The end result is a build that is deployable and verifiable in an actual production environment.


.. figure:: JD.*
   :width: 100 %
   :height: 350 px
   :scale: 100 %
   :align: center
   :alt: JD.*

JENKINS
-------

.. figure:: Jenkins.*
          :height: 100 px
          :width: 100 px
          :align: center


Jenkins master is running in Docker Swarm on the CI/CD server. Jenkins uses a Master-Slave architecture to manage distributed builds. In this architecture, Master and Slave communicate through TCP/IP protocol.

JENKINS MASTER
^^^^^^^^^^^^^^

- The Master's job is to handle:
	
	- Scheduling build jobs.
	- Dispatching builds to the slaves for the actual execution.
	- Monitor the slaves (possibly taking them online and offline as required).
	- Recording and presenting the build results.
	- A Master instance of Jenkins can also execute build jobs directly.

JENKINS SLAVE
^^^^^^^^^^^^^

- A Slave is a Java executable that runs on a remote machine. Following are the characteristics of Jenkins Slaves:

	- It hears requests from the Jenkins Master instance.
	- The job of a Slave is to do as they are told to, which involves executing build jobs dispatched by the Master.
	- It is possible to configure a job to always run on a particular Slave machine, or a particular type of Slave machine, or simply let Jenkins pick the next available Slave.


It offers simple way to set up a continuous integration or continuous delivery working environment for almost any kind of coding languages and source code repositories using pipelines as well as other development tools. It can be easily setup and and configure via its web interface. Jenkins integrates almost every tool in CI/CD integration toolchain. It is also possible that jenkins can distribute the work across multiple machine easily. It also helps to build drives, test and deployment across multiple platforms faster : e.g. Docker image build, pipeline for deployment.

GITLAB AND DOCKER REGISTRY
--------------------------
.. figure:: Gitlab.*
          :height: 100 px
          :width: 100 px
          :align: center

Gitlab is running in a container in Docker Swarm. Gitlab enables teams and team member to collaborate and work on various projects instead of managing multiple threads across disparate tools. It provides single data store, one user interface and one permission model by allowing teams to collaborate with the  flexible working environment to focus on software building quickly.

With the help of gitlab it is easy to deploy cloud native applications such as kubernetes. The integration of both Kubernetes and Gitlab makes easy to create and deploy application on the top of Kubernetes.

Gitlab is mainly used to host git repositories. There is also an integrated Docker registry. Users can connect/push/pull images in the Gitlab Docker registry using their Gitlab credentials.

.. figure:: GD.*
   :width: 100 %
   :height: 350 px
   :scale: 100 %
   :align: center
   :alt: GD.*
