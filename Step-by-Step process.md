# Step by Step Process

**Install a Virtual Machine Manager:** 
---
>such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation

[**Install Minikube**](https://bit.ly/38bLcJy)
---
[**Install Kubectl**](https://bit.ly/32bSI2Z)
---
[**Install Helm**](https://github.com/helm/helm/releases)
---
**Set Environment Variables:** 
---
>Add above four paths in env variables and restart the terminal


**Install prometheus using Helm:** 
---
 **Add helm repo:** Before installing Prometheus using Helm, you need to add the Prometheus Helm chart repository.

 `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

 **Update helm repo:** Make sure to update the Helm repository to get the latest information about available charts.
  
  `helm repo update`
  
  **Install Prometheus:** Now, use Helm to install Prometheus with the following command
  
  `helm install prometheus prometheus-community/prometheus`
  
  >This command installs the Prometheus chart from the Prometheus Community Helm repository. The release name is set to "prometheus," but you can choose a different name if needed.
  
  **Expose Prometheus Service**
  This is required to access prometheus-server using your browser.
  
  `kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext`
  
  >This command creates a NodePort service named "prometheus-server-ext" that exposes the Prometheus server on a specific port.
  
  **Access Prometheus Web UI**
  ---
  
  After exposing the service, you can access the Prometheus web UI using the NodePort.
  
  i. Get the NodePort assigned to the Prometheus service:
  
  `kubectl get svc prometheus-server-ext`
  
 ![prometheus ext](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/prometheus%20external.PNG)
  
  ii. Get the minikube ip address using
  
  `minikube ip`

   ![ip address](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/minikube%20ip%20address.PNG)
  
  Access Prometheus in your web browser using the NodePort. For example:
  
  `http://<Minikube_IP>:<NodePort>`
  ![promethues web Access](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/prometheus%20UI.PNG)

  Querying using Prometheus
  ---
  ![query](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/prometheus%20query.PNG)


**Install Grafana Using Helm:** processing is same as prometheus installation
---

**Add Helm Repository**

`helm repo add grafana https://grafana.github.io/helm-charts`

**Update Helm Repository**

`helm repo update`

**Install Grafana**

`helm install grafana grafana/grafana`

**Expose Grafana Service**

`kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext`

 ![grafana expose](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/grafana%20external%20service.PNG)

**Access Grafana Web UI**

![grafana login](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/garafana%20login.PNG)

****
# Logging in to Grafana
---

Run the following command to retrieve the Grafana admin password from the Kubernetes secret:

`kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo`

>This command decodes the base64-encoded admin password from the secret and prints it to the terminal.

In the Grafana login page, use the following credentials:

Username: admin

Password: Paste the copied admin password.

Click on the "Log In" button.

>After logging in, you may want to change the password for security reasons.

**Add Prometheus Datasource**
---
i. Click on the "Add your first data source" or the "+" button to add a new datasource.

ii. Choose "Prometheus" from the list of available data sources.

iii. Configure Prometheus Datasource

>**Name:** Give a name to your Prometheus datasource (e.g., Prometheus).

>**HTTP:** Set the URL for your Prometheus server. 

iv. Click the "Save & Test" button to save the Prometheus datasource configuration and test the connection.

**Access Dashboards:**
---
i. In the Grafana sidebar, click on the "+" icon to open the "Create" menu.

ii.Select "Dashboard" from the menu.

**Import Dashboard:**
---

i.Click on the "Import" button.

ii. Provide the Dashboard ID (e.g., 3662) in the "Grafana.com Dashboard" field.

iii. Click on the "Load" button to fetch the details of the dashboard.

![dashboard](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/garafana%20dashoard.PNG)

To view Metric in web:
--
Expose the prometheus-kube-state-metrics service and access it in Web

![Metrics](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/metrics.PNG)

>Remember that exposing services via NodePort might not be suitable for production environments. In production, you might consider using LoadBalancer services or an Ingress controller for more advanced routing and load balancing.


To add your application metrics to Prometheus static configurations
---
you need to ensure that your application exposes metrics in a format that Prometheus can scrape. The common format is the Prometheus exposition format, and your application should expose a /metrics endpoint by default.

To add application-related monitoring in a Prometheus YAML file:
---
We have to update our application data in the promethes yaml file in configmaps

![configmap](https://github.com/DivyaJyothiVundavalli/Kubernetes-Monitoring-Setup-with-Grafana-and-Prometheus/blob/main/Snaps%20folder/configmap.PNG)

Sample:
```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'your-application'
    static_configs:
      - targets: ['your-application-service:your-application-port']
    metrics_path: '/metrics'
```







