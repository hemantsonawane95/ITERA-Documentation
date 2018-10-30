
|

|

HOW TO DEPLOY APPLICATIONS ON CONTAINER CLOUD
==============================================


USING KUBECTL AND KUBERNETES API DIRECTLY
-----------------------------------------

Before deploying or running the application on container cloud you need to have kubernetes cluster deployed on the cloud environment. It is also necessary to configure kubectl command-line tool to communicate with kubernetes cluster. But you will already have kubernetes deployed on your cloud environment. So you don't need to be worry for that. As soon as you have your environment ready you can start application deployment process. There are two ways to deploy application on kubernetes cluster:

USING YAML FILE 
^^^^^^^^^^^^^^^

You can run an application by creating a Kubernetes Deployment object, and you can describe a Deployment in a YAML file. For example, this YAML file describes a Deployment that runs the nginx:1.7.9.

.. code-block:: yaml

   { apiVersion: apps/v1 
	kind: Deployment
	metadata:
	  name: nginx-deployment
	spec:
	  selector:
	    matchLabels:
	      app: nginx
	  replicas: 2 # tells deployment to run 2 pods matching the template
	  template:
	    metadata:
	      labels:
		app: nginx
	    spec:
	      containers:
	      - name: nginx
		image: nginx:1.7.9
		ports:
		- containerPort: 80 }

Now to create deployment based on YAML file execute following commands in your terminal.

.. code-block:: bash

   $ kubectl apply -f https://k8s.io/examples/application/deployment.yaml

To get the information about deployed application execute following command

.. code-block:: bash
   
   $ kubectl describe deployment nginx-deployment

By executing the above command you will get similar output as given below. The describe command allows you to interrogate different kubernetes resources such as pods, deployments, and services at a deeperlevel.

.. code-block:: yaml

   Name:     nginx-deployment
   Namespace:    default
   CreationTimestamp:  Tue, 30 Aug 2016 18:11:37 -0700
   Labels:     app=nginx
   Annotations:    deployment.kubernetes.io/revision=1
   Selector:   app=nginx
   Replicas:   2 desired | 2 updated | 2 total | 2 available | 0 unavailable
   StrategyType:   RollingUpdate
   MinReadySeconds:  0
   RollingUpdateStrategy:  1 max unavailable, 1 max surge
   Pod Template:
     Labels:       app=nginx
     Containers:
      nginx:
       Image:              nginx:1.7.9
       Port:               80/TCP
       Environment:        <none>
       Mounts:             <none>
      Volumes:             <none>
   Conditions:
      Type          Status  Reason
      ----          ------  ------
      Available     True    MinimumReplicasAvailable
      Progressing   True    NewReplicaSetAvailable
   OldReplicaSets:   <none>
   NewReplicaSet:    nginx-deployment-1771418926 (2/2 replicas created)
   No events.

To get the status of deployment pods and to delete the deployment from kubernetes cluster execute following commands,

.. code-block:: bash

   $ kubectl get pods -l app=nginx
   
   $ kubectl delete deployment nginx-deployment  


RUN THE APPLICATION DIRECTLY
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The second method of deploying application is very simple and a deployment is a logical reference to pods and their configurations.

Create deployment by using kubectl create command.

.. code-block:: bash

   $ kubectl create deployment nginx --image=nginx

And that's it you deployed an application on kubernetes cluster. To describe and check the status of deployment use commands from previous method 5.1. Make the deployment accessible via internet.
	
.. code-block:: bash   

   $ kubectl create service nodeport nginx --tcp=80:80

This creates a public facing service on the host for the deployment. Because this is a NodePort service, Kubernetes will assign this service a port on the host machine in the 30000-32767 port range.
It is also necessary to expose the deployment on the internet. Execute following command to expose the deployment.

.. code-block:: bash 

   $ kubectl expose deployment/my-nginx

So you are pretty much done with the deployment. Now, it's time to see on which port the service is running. To get the current services,

.. code-block:: bash 

   $ kubectl get svc

You  will get the following result by executing the svc command  and will get on which port the service is running.

.. code-block:: bash

   $ nginx        NodePort    10.98.24.29   <none>        80:32555/TCP   52s

To see whether the deployment is running on the port just go to the IP of your cloud environment and use exposed port after the IP and there you go you will be redirected to your deployment page. There isanother way to see whether the deployment is exposed or not just by using curl command.

.. code-block:: bash

   $ curl ip:32555  # ip - ip of cloud environment 


USING HELM
----------

Helm helps you manage Kubernetes applications. Helm Charts helps you define, install, and upgrade even the most complex Kubernetes application.
Charts are easy to create, version, share, and publish. 
Use Helm to:

	- Find and use popular software packaged as Helm charts to run in Kubernetes
	- Share your own applications as Helm charts
	- Create reproducible builds of your Kubernetes applications
	- Intelligently manage your Kubernetes manifest files
	- Manage releases of Helm packages

The example is given below about how to install application on kubernetes using helm:

EXAMPLE : INSTALL GOGS USING HELM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gogs is a painless self-hosted Git service : https://gogs.io/

An helm chart to install gogs exists and will be used to install Gogs in the Kubernetes cluster. The helm chart is not in the stable repository, it is instead located in the incubator.

1. Enable the incubator repository 

.. code-block:: bash
 
   $ helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com/
     
    "incubator" has been added to your repositories
	
2. Create a file gogs.yaml containing the following content :

.. code-block:: yaml

   { persistence:
       storageClass: 'general'
       size: 50Gi
     postgresql:
      persistence:
        storageClass: 'general'
        size: 10Gi }

The storageClass general corresponds to Ceph RBD. The allocated volume for Gogs will be of 50 Gi.

3. Install Gogs with helm

.. code-block:: bash

   $ helm install --name gogs-signals -f gogs.yaml incubator/gogs --namespace=testing

4. Check the result

.. code-block:: bash

   $ kubectl get svc -n testing | grep gogs
    
   gogs-signals-gogs        NodePort   10.96.196.198 <none> 80:31465/TCP,22:32448/TCP
   gogs-signals-postgresql  ClusterIP  10.96.118.4   <none>  5432/TC

Gogs was correctly installed on the top of the Kubernetes cluster. It is now possible to access the webui with the port 31465. 
 

PERSISTENT VOLUMES FOR KUBERNETES
---------------------------------


The storage class standard was created in the Kubernetes cluster. It will create volumes from Cinder for Kubernetes. The volumes will be automatically created and attached to the virtual machines on the top of which Kubernetes is installed.


Example of PVC for Kubernetes using the storage class standard:

.. code-block:: yaml

   { kind: PersistentVolumeClaim  
     apiVersion: v1  
     metadata:  
       name: task-pv-claim  
     spec:  
       storageClassName: standard  
       accessModes:  
         - ReadWriteOnce  
       resources:  
         requests:  
           storage: 3Gi }
