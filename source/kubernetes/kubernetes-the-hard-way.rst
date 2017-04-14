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

Starting Services
-----------------

.. code-block:: bash

   /usr/bin/kubelet --kubeconfig=/etc/kubernetes/kubelet.conf --require-kubeconfig=true --pod-manifest-path=/etc/kubernetes/manifests --allow-privileged=true --network-plugin=cni --cni-conf-dir=/etc/cni/net.d --cni-bin-dir=/opt/cni/bin --cluster-dns=10.96.0.10 --cluster-domain=cluster.local

.. code-block:: bash

   kube-apiserver --insecure-bind-address=127.0.0.1 --admission-control=NamespaceLifecycle,LimitRanger,ServiceAccount,PersistentVolumeLabel,DefaultStorageClass,ResourceQuota --service-cluster-ip-range=10.96.0.0/12 --service-account-key-file=/etc/kubernetes/pki/apiserver-key.pem --client-ca-file=/etc/kubernetes/pki/ca.pem --tls-cert-file=/etc/kubernetes/pki/apiserver.pem --tls-private-key-file=/etc/kubernetes/pki/apiserver-key.pem --token-auth-file=/etc/kubernetes/pki/tokens.csv --secure-port=6443 --allow-privileged --advertise-address=122.116.172.49 --kubelet-preferred-address-types=ExternalIP,Hostname --anonymous-auth=false --etcd-servers=http://127.0.0.1:2379




