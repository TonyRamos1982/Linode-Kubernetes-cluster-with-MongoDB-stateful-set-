####  Install a stateful service (MongoDB) on Kubernetes using Helm
**Technologies Used:**  
Kubernetes, Helm, MongoDB, Mongo Express, Linode LKE, Linux

**Overview**:
- Create a managed K8s cluster with Linode Kubernetes Engine

<img width="1325" height="722" alt="Mod10-LKE1" src="https://github.com/user-attachments/assets/884cae1f-5681-4341-8b6f-8e817748bda1" />




- Deploy replicated MongoDB service in LKE cluster using a Helm chart
- Configure data persistence for MongoDB with Linode’s cloud storage
- Deploy UI client Mongo Express for MongoDB
- Deploy and configure nginx ingress to access the UI application from browser
