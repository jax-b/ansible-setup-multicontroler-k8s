# Simple Ansible MultiController K8s
This ansible playbook is written to attempt to initialize a cluster if one does
not already exists. Then join each of the worker nodes to that cluster.
Finally, it will install the latest version of the Weave CNI.

I run by the motto
> If the playbook cannot handle a existing installation and update it
> then why use ansible
This playbook Will not overwrite a existing installation if you have one,
it will join new nodes to the cluster though.

I made this from my home lab. if you want to use it in a production
environment, I recommend you
 - double check this playbook
 - add package pinning for you cluster
 - Use your own CA for cluster certificates vs Self-Signed

If you run this playbook with all features enabled you will have a fully
functional K8s cluster without persistent storage. I leave it up to use to
choose your storage controller as its highly dependent as to what you available.
My lab is hyper-converged using proxmox and the CEPH storage system, so im using
the ceph-cni helm chart as my persistent storage.

The Kube and Traefik dashboards can be setup on your own terms to be exposed but remain at the default configuration of needing kubectl proxy to access them

### Configuration
There is a small amount of configurable to this see table
  - Custom Pod Network CIDR via the variable `kube_network`
  - Custom Service Network CIDR via the variable `kube_service_network`
  - Skip Install of the Calico CNI via `--skip-tags calico_cni`
  - Skip Install of the K8s Dashboard via `--skip-tags k8s_dashboard`
  - Skip Install of the MetalLB Network LB via `--skip-tags metallb`
  - Skip Install of the Traefik Ingest Controller via `--skip-tags traefik_ingest`
  - Skip All Pod deployments via `--skip-tags pod_deploy`

#### Required Host Groups
This playbook requires 3 different host groups to be set up:
 -  k8proxy
 -  kube_control_plane (1-7)
 -  kube_node (0-X)

#### My Setup
I am personally running:
|   type  | qty |
| ------- |-----|
| k8proxy | 1   |
| kube_control_plane  | 3   |
| kube_node  | 4   |
