Cluster Planning
================

Clarification
-------------

* Multi-Zone: single Kubernetes clusters on single cloud provider on multi availability zones.
* Multi-Datacenter: multiple Kubernetes clusters on different cloud/on-premise data centers.

This is a lightweight version of a broader Cluster Federation feature (previously referred to by the affectionate nickname “Ubernetes”). Full Cluster Federation allows combining separate Kubernetes clusters running in different regions or cloud providers (or on-premise data centers). However, many users simply want to run a more available Kubernetes cluster in multiple zones of their single cloud provider, and this is what the multizone support in 1.2 allows (this previously went by the nickname “Ubernetes Lite”).

.. note::
   A single Kubernetes cluster is not intended to span multiple availability zones. Instead, we recommend building a higher-level layer to replicate complete deployments of highly available applications across multiple zones (see the multi-cluster doc and cluster federation proposal for more details).

Scope of a single cluster
-------------------------

