
|

|

HOW TO ACCESS THE ENVIRONMENT
=============================

To access the environment just execute the command as shown in below with
correct IP of the environment, SSH key of the environment. But before accessing the
environment make sure that you have file which has SSH key saved in it. As soon
as you save the key you are ready to roll. Now you can access your environment by
following command.

.. code-block:: bash
   
   $ ssh -i cloud.key user@KUBERNETES_IP_address

So now you have full access to your environment which has kubernetes installed in
it. If you would like check whether pods and nodes are working fine then just execute
given command.

To check the pods status :

.. code-block:: bash  

   $ kubectl get pods --all-namespaces -o wide

To check the nodes status: 

.. code-block:: bash 

   $ kubectl get nodes

Please don't touch or don't try to modify pods in kube-system. If you do then service
will not work according to expectations. As soon as you know that all the nodes as
well as pods are running then just move towards understanding the
basic aspects of container and cloud architecture.
