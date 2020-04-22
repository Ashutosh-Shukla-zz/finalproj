# Team 10
| Name | Email Address |
| --- | --- |
| Ashutosh Shukla | shukla.as@husky.neu.edu|
| Akash Balani |balani.a@husky.neu.edu |

## Introduction:
This is an attraction pass management e-commerce application. It has 2 entities Customer and Vendor. Vendors can add passes and customers can buy those passes. 

## Infrastructure:
The project is deployed on Kubernetes cluster and has the following components:
### Namepsaces:
	- Frontend:
		1. Frontend deployment
		2. Frontend Load balancer Service
	- Backend:
		1. Backend Customer Deployment
		2. Backend Vendor Deployment
		3. Backend Customer Node port Service 
		4. Backend Vendor Node port Service
	- DB:
		1. Mysql Database deployment
		2. Mysql Node port Service
	- Grafana:
		1. Prometheus-grafana operator using helm charts.

To deploy the kubernetes cluster checkout the git repo https://github.com/Ashutosh-Shukla/finalproj-infra
In your aws Route 53 create a private hosted zone and note down the ID
In your S3 create a bucket to store state of the kops cluster.

Run the following command to provision the cluster.

```sh
ansible-playbook main.yaml --extra-vars "command=start kops_state_store=s3://<Name of state store bucker> cluster_name=cluster.<domain name> dns_zone_id=<PrivateHostedZoneID> ssh_path=<Public Key path>"
```

Once the cluster is created check the cluster health with 

```sh
Kubectl get nodes
```


## Application:
To deploy the application along with all components

Run the deployment.sh script inside the finalproj-deployment directory inside infra repo

```sh
./finalproj-deployment/deployment.sh
```

To start the frontend run  the command and copy the external IP
```sh
kubectl get svc -n frontend
```

To open grafana cluster port forward the pod name to a local port by a similar command

```sh
kubectl port-forward <Grafana-Pod_name> 3000 -n grafana
```

## Delete Cluster:

To delete the cluster run the delete kops cluster command

```sh
ansible-playbook main.yaml --extra-vars "command=delete kops_state_store=s3://<Name of state store bucker> cluster_name=cluster.<domain name> dns_zone_id=<PrivateHostedZoneID> ssh_path=<Public Key path>"
```

Github Links:
[Infrastructure](https://github.com/Ashutosh-Shukla/finalproj-infra)
[Frontend](https://github.com/Ashutosh-Shukla/finalproj-frontend)
[Backend-Customer](https://github.com/Ashutosh-Shukla/finalproj-vendor)
[Backend-Vendor](https://github.com/Ashutosh-Shukla/finalproj-customer)

Docker Images:
ashutoshshukla/finalproj-frontend:v1
ashutoshshukla/finalproj-customer:v1
ashutoshshukla/finalproj-vendor:v1
mysql:5.6


