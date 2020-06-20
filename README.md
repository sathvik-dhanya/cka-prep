# cka-prep

Some resources to prepare for the CKA exam.

#### CKA Curriculum

https://github.com/cncf/curriculum/blob/master/CKA_Curriculum_V1.18.pdf

- [Scheduling 5%](scheduling)
- [Logging/monitoring 5%](logging_monitoring)
- [Application lifecycle management 8%](application_lifecycle_mgmt)
- [Cluster maintenance 11%](cluster_maint)
- [Security 12%](security)
- [Storage 7%](storage)
- [Troubleshooting 10%](troubleshooting)
- [Core concepts 19%](core_concepts)
- [Networking 11%](networking)
- [Installation, configuration & validation 12%](installation_configuration_validation)

(There are changes to curriculum, and you'll be using v1.19 after Sept 2020)

#### Get familiar with
Kubectl Cheatsheet - https://kubernetes.io/docs/reference/kubectl/cheatsheet/

Kubectl explain - https://blog.heptio.com/kubectl-explain-heptioprotip-ee883992a243

Be fast with Kubectl 1.18 CKA - https://medium.com/faun/be-fast-with-kubectl-1-18-ckad-cka-31be00acc443

```bash
# The `-o yaml` in conjunction with `--dry-run=client` allows you to create a manifest 
# template from an imperative spec, combined with `--edit`, it allows you to modify 
# the object before creation

kubectl create service clusterip my-svc -o yaml --dry-run=client > svc.yaml
kubectl create --edit -f svc.yaml
```

### Extra resources
[CKA practice environment](https://github.com/arush-sal/cka-practice-environment)

[Kubernetes learning resources list](https://docs.google.com/spreadsheets/d/10NltoF_6y3mBwUzQ4bcQLQfCE1BWSgUDcJXy-Qp2JEU/edit#gid=0)

[CKA sample challenges](https://levelup.gitconnected.com/kubernetes-cka-example-questions-practical-challenge-86318d85b4d)
