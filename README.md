This project demonstrates observability using the popular cloud native tool known as prometheus to help us monitor resources in our clusters and also monitor the cluster itself. In this project we will be using a range of tools such as:
AWS EKS: a managed kubernetes service by AWS.
- [x] Create an Amazon Elastic Kubernetes Service (EKS) cluster with 2 worker nodes in the us-east-1 region. 
- [ ] Deploy a our prometheus application to this cluster using Helm.

        helm repo add prometheus-community https://prometheus-community.github.io/helm-charts  

        helm repo update 

        helm install prometheus prometheus-community/kube-prometheus-stack

        
- [ ] Expose Prometheus with the port forwarding command. This is for temporary use case, but you can easily edit the helm file and change the service type of prometheus pod to a loadbalancer type or nodePort instead of using ClusterIp type.
        kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090