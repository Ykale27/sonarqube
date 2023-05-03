# canvass-sonarqube
Sonarqube setup for repository scanning

## Table of Contents
- Prerequisites
- Usage

## Prerequisites
Make sure that the below items are in place:

1. You have an existing Azure Subscription, if not create one.
2. Install Azure CLI if not already installed using the below documentation.
    [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
3. Open your command prompt or terminal and login to your Azure account by running the following command:
```bash
az login
```
4. Create a resource group where all the resources will be grouped.
```bash
az group create --name <resource-group-name> --location <location>
```
5. Create an AKS cluster (preferably with v1.25.5) by running the following command:
```bash
az aks create --resource-group <resource-group-name> --name <cluster-name> --node-count <node-count> --kubernetes-version 1.25.5 --generate-ssh-keys
```
6. Once the AKS cluster is created, you can connect to it by running the following command:
```bash
az aks get-credentials --resource-group <resource-group-name> --name <cluster-name>
```

7. Make sure kubectl is installed on your terminal/cmd prompt using the below documentation
    [kubectl](https://kubernetes.io/docs/tasks/tools/)
   
## Usage

1. Clone the repository to your local machine
2. Navigate to the project directory

3. Encode USERNAME and PASSWORD of Postgres using following commands:
--------
    echo -n "postgresadmin" | base64
    echo -n "admin123" | base64
    
4. Create the Secret using kubectl apply:
-------
    kubectl apply -f postgres-secrets.yml

5. Create PV and PVC for Postgres using yaml file:
-----
    kubectl apply -f postgres-storage.yaml

6. Deploying Postgres with kubectl apply:
-----------
    kubectl apply -f postgres-deployment.yaml
    kubectl apply -f postgres-service.yaml

7. Create PV and PVC for Sonarqube:
-------------
    kubectl apply -f sonar-pv-data.yml
    kubectl apply -f sonar-pv-extensions.yml
    kubectl apply -f sonar-pvc-data.yml
    kubectl apply -f sonar-pvc-extentions.yml

8. Create configmaps for URL which we use in Sonarqube:
-------
    kubectl apply -f sonar-configmap.yaml

9. Deploy Sonarqube:
-------------
    kubectl apply -f sonar-deployment.yml
    kubectl apply -f sonar-service.yml
    
10. Check pods:
-------
    kubectl get pods
    
11. Now Goto Loadbalancer and check whether service comes Inservice or not, If it comes Inservice copy DNS Name of Loadbalancer and check in web UI:
-------
    kubectl get svc
    
12. After that we will get similar dashboard:
![image](https://user-images.githubusercontent.com/109966978/235906796-b6890241-8f53-481f-a8ad-5480754ba57b.png)


13. Default Credentials for Sonarqube:
-------
    UserName: admin
    PassWord: admin
    

