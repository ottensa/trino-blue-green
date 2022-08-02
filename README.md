# Starburst/Trino example for a GitOps based Blue/Green Deployment
## Overview

In the following I will walk through how one can setup Starburst for zero-downtime maintenance, think about version upgrades, adding new data sources, etc. I will apply GitOps and Blue/Green Deployment principles. It is intended to give you an idea how to approach this and not to give you a production ready out of the box solution.

The technologies I will be using are:

- Starburst/Trino of course 🙂
- Lyft Presto Gateway
- ArgoCD
- Github
- Kubernetes

The idea is that we have a Starburst or Trino cluster happily running and processing data. Once we apply a configuration change and commit it to our git repository the following shall happen:

1. Deploy a new cluster with the new configuration
2. Register the new cluster with the Presto Gateway
3. Move new queries to the new cluster
4. Wait for queries on the old cluster to be finished
5. Decommission old cluster