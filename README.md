# Fork of Hipster Shop: Cloud-Native Microservices Demo Application

This is a folk of a GCP demo add set to run on ICP.

### Option 1: Running on IBM Cloud Private (ICP)

1.  Install tools specified in the previous section (Docker, kubectl, skaffold)

1.  Connect to an ICP Cluster

1.  Create a namespace with correct permissions

    ```
    kubectl create namespace microservice-demo
    ```   

1.  Switch to it

    ```
    kubens microservice-demo
    
    ```

1.  Apply a role bindings to the namespace to ensure pods run with non-root access

    ```
    kubectl apply -f icp/cluster-role.yaml
    ```
    
    Not applying permissions will results in: 
    ```
    Error: container has runAsNonRoot and image will run as root
    ```
    
    as ICP is restrictive about how containers can run by default as explained in [this article](https://medium.com/@zhimin.wen/running-wild-container-image-on-icp-3-1-1-security-and-enforcement-19bf9e26a3d8).

1. Deploy the application

    ```
    kubectl apply -f ./release/kubernetes-manifests.yaml
    ```

1.  Run `kubectl get pods` to see pods are in a Ready state.

1.  frontend-external was set as a LoadBalancer. A LoadBalancer is not currently configured for ICP so change the type from LoadBalancer to NodePort in [release/kubernetes-manifests.yaml](release/kubernetes-manifests.yaml).

1. Find the assigned your application, then visit the application using the IP address of a worker node and with that port in the browser.

    ```
    kubectl get service/frontend-external
    ```

    ```
    kubectl get nodes
    ``` 
    e.g. [http://172.16.50.68:31913/](http://172.16.50.68:31913/)
