####  Install a stateful service (MongoDB) on Kubernetes using Helm
**Technologies Used:**  
Kubernetes, Helm, MongoDB, Mongo Express, Linode LKE, Linux

**Overview**:
- Create a managed K8s cluster with Linode Kubernetes Engine, configure worker nodes only. Linode will take care of the control plane for your cluster. 

<img width="825" height="422" alt="Mod10-LKE1" src="https://github.com/user-attachments/assets/884cae1f-5681-4341-8b6f-8e817748bda1" />

- Downloand test-kubeconfig.yaml and configure enviromental variable in ur local machine to connect to LKE cluster and use kubectl then set file permission to allow only user account access to file
- Export KUBECONFIG=test-config.yaml (In my case, I'm using a ubuntu WSL vm in my windows 10 machine)
<img width="689" height="137" alt="image" src="https://github.com/user-attachments/assets/7bf8aa89-3946-4094-a847-7e75fe367ebe" />

- Deploy replicated MongoDB service in LKE cluster using a Helm chart, by adding the MongoDB Bitnami Repo to your local machine and pull the MongoDB helm chart.
- Configure values.yaml file to override the desired MongoDB parameters: architecture:replicaset, storageClass: "linode-block-storage", rootPassword: secret-root-pwd
- Configure data persistence for MongoDB with Linode’s cloud storage
  
``` console
$ helm install mongodb --values helm-mongodb bitnami/mongodb
```
- check the components created in the cluster with the use of the helm chart: pods, headless service and stateful sets were created
``` console
$ kubectl get pod
$ kubectl get all
$ kubectl get secret
```
- Persistent volumes were created in Linode cluster automatically and attached to each pod
<img width="825" height="422" alt="image" src="https://github.com/user-attachments/assets/3d4218f9-bff3-424e-979f-2a913df193db" />


  

- Deploy UI client Mongo Express for MongoDB
- Deploy and configure nginx ingress to access the UI application from browser
