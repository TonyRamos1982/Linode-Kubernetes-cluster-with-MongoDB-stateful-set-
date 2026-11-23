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
- check the components created in the cluster with the use of the helm chart:
  
| componets created |
| :--- |
| <ul><li>pods</li><li>headless service</li><li>secret</li><li>stateful set</li></ul> |

- NOTE: a headless service was created instead of ClusterIP because this provides stable, unique DNS records for each individual MongoDB pod so that replica set members can find, communicate, and elect a primary among themselves
``` console
$ kubectl get pod
$ kubectl get all
$ kubectl get secret
```
<img width="906" height="248" alt="image" src="https://github.com/user-attachments/assets/cc0728f8-5dd4-4cb5-953f-59fdbb7adb7f" />

- Persistent volumes were created in Linode cluster automatically and attached to each pod
<img width="825" height="322" alt="image" src="https://github.com/user-attachments/assets/3d4218f9-bff3-424e-979f-2a913df193db" />

- Deploy UI client Mongo Express for MongoDB using a kubernetes yaml since its just a single pod with its service, no helm chart needed.
``` console
$ kubectl apply -f helm-mongo-express.yaml 
```
<img width="827" height="313" alt="image" src="https://github.com/user-attachments/assets/632045b2-ba99-447b-889d-d935adfeea66" />

- Deploy and configure nginx ingress to access the UI application from browser
``` console
# 1. Add the ingress-nginx helm chart repository
helm repo add ingress-nginx https://github.io

# 2. Refresh your local repository cache
helm repo update

# 3. Deploy ingress-nginx into your current namespace as a LoadBalancer
helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx \
  --set controller.service.type=LoadBalancer
```
- Check the components installed by helm:
  | componets created |
| :--- |
| <ul><li>ingress-nginx controller pod</li><li>ingress-nginx deployment</li><li>secret</li><li>ingress-nginx controller service of type: load balancer</li></ul> |
<img width="1013" height="137" alt="image" src="https://github.com/user-attachments/assets/e4251daa-2590-416c-8460-24c118b8e5de" />

