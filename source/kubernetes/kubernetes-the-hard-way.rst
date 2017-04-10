Kubernetes The Hard Way
=======================

This is Ian's iteration on learning Kubernetes the hard way.

At the time of writing(Apr 10th, 2017), the stable version of Kubernetes is v1.6.1

Binaries
--------

Kubernetes will need the following binaries

Running outside of container
****************************

These can be found in :code:`kubernetes/server/bin`

* docker
* kubelet
* kube-proxy


Running as a container
**********************

The following are recommended to run from Hyperkube

* kube-apiserver
* kube-controller-manager
* kube-scheduler

For etcd, check the Makefile inside :code:`kubernetes/cluster/images/etcd/Makefile`
For k8s v.1.6.1, the recommended etcd version is v3.0.17

* etcd

Authentication
--------------

HTTPS certs
***********

Generate the necessary certs with :code:`./generate_certs.sh`

User Token
**********

Generate using

.. code-block:: bash

   dd if=/dev/urandom bs=128 count=1 2>/dev/null | base64 | tr -d "=+/" | dd bs=32 count=1 2>/dev/null

Now let's generate kubeconfig files

.. code-block:: yaml

   apiVersion: v1
   kind: Config
   users:
   - name: kubelet
     user:
       token: ${KUBELET_TOKEN}
   clusters:
   - name: local
     cluster:
       certificate-authority: ${CA_CERT_BUNDLE_PATH}
   contexts:
   - context:
       cluster: local
       user: kubelet
     name: service-account-context
   current-context: service-account-context


