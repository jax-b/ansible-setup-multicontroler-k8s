# Simple Ansible MultiController K8s
This ansible playbook is written to attempt to initialize a cluster if one does
not already exists. Then join each of the worker nodes to that cluster. 
Finally, it will install the latest version of the Weave CNI. 

I run by the motto
> If the playbook cannot handle a existing installation and update it
> then why use ansible
This playbook Will not overwrite a existing installation if you have one, 
it will join new nodes to the cluster though

I made this from my home lab. if you want to use it in a production
environment, I recommend you 
 - double check this playbook
 - add package pinning for you cluster
 - Use your own CA for cluster certificates vs Self-Signed

This playbook requires 3 different host groups to be set up:
 -  k8proxy
 -  k8cdrs (2-7)
 -  k8wkrs (0-X)

I am personally running:
|   type  | qty |
| ------- |-----|
| k8proxy | 1   |
| k8cdrs  | 3   |
| k8wkrs  | 3   |

There is a small amount of configurable to this see table
  - Custom Pod Network CIDR via the variable `kube_network_real`
  - Skip Install of the Weave CNI via `--skip-tags weave_cni`
  - Skip Install of the K8s Dashboard via `--skip-tags k8s_dashboard`
  - Skip All Pod deployments via `--skip-tags pod_deploy`