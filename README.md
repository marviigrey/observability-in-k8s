This project demonstrates observability using the popular cloud native tool known as prometheus to help us monitor resources in our clusters and also monitor the cluster itself. In this project we will be using a range of tools such as:
AWS EKS: a managed kubernetes service by AWS.
- [x] Create an Amazon Elastic Kubernetes Service (EKS) cluster with 2 worker nodes in the us-east-1 region.
- [x] Helm installed.

        curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
        chmod 700 get_helm.sh
        ./get_helm.sh 
- [x] Deploy a our prometheus application to this cluster 
using Helm.
- [x] Implement monitoring and alerting using Prometheus, Grafana, Alertmanager, and Loki.

        helm repo add prometheus-community https://prometheus-community.github.io/helm-charts  

        helm repo update 

        helm install prometheus prometheus-community/kube-prometheus-stack

        
- [ ] Expose Prometheus with the port forwarding command. This is for temporary use case, but you can easily edit the helm file and change the service type of prometheus pod to a loadbalancer type or nodePort instead of using ClusterIp type.
        kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090

Deploy a sample appication using the api.yaml manifest file and expose it using the svc.yaml.

     kubectl apply -f api.yaml svc.yaml

To be able to scrape metrics from the pod we will need to create a servicemonitor . 

Create a service monitor which defines a set of target for prometheus to monitor and scrape. They allow you to avoid touching prometheus config directly and give you a syntax to define targets.

        kubectl create -f service-monitor.yaml

Note: When we run;

        kubectl get prometheus.monitoring.coreos.com -o yaml

Under the metadata we can see that the service monitor has a matchLabel field that helps us to find service monitors which we can use to monitor resources within our cluster. The Label will be the same as the one used in our service monitor.

If we go to our prometheus dshboard, we can see the metrics of our app by simply using the labels to make search.

{job = "node-api"}

or navigate to the menu bar and click on target you will see the pods there.

After creating service discovery by adding targets to our prometheus using service monitors, we can add rules using the CRD known as prometheusrules which handles registering new rules to a prometheus instance. File needed for creating the rule is called the rules.yaml. to create the rule SD run:
     kubectl create -f rules.yaml
This file contains all the details needed to create rules on our prometheus server, it holds details such as the label that points to our crd called prometheusrules.monitoring.coreos.com. 

Next is to set up Alert manager rules using alertmanagers.monitoring.coreos.com. This will help us add alert to target instance. 
