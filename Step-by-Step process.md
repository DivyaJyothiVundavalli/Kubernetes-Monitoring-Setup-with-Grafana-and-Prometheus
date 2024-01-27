**1. Install a Virtual Machine Manager:** such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation

**2.** [**Install Minikube:**](https://bit.ly/38bLcJy)

**3. ** [**Install Kubectl:**](https://bit.ly/32bSI2Z)

**4. ** [**Install Helm:**](https://github.com/helm/helm/releases)

**5. Set Environment Variables:** Add above four paths in env variables and restart the terminal

**6. Install prometheus using Helm:** 

**Add helm repo:** Before installing Prometheus using Helm, you need to add the Prometheus Helm chart repository.

`helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

**Update helm repo:** Make sure to update the Helm repository to get the latest information about available charts.

`helm repo update`

**Install Prometheus:**Now, use Helm to install Prometheus with the following command

`helm install prometheus prometheus-community/prometheus`

This command installs the Prometheus chart from the Prometheus Community Helm repository. The release name is set to "prometheus," but you can choose a different name if needed.

**Expose Prometheus Service**
This is required to access prometheus-server using your browser.

`kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext`

This command creates a NodePort service named "prometheus-server-ext" that exposes the Prometheus server on a specific port.

**Access Prometheus Web UI**

After exposing the service, you can access the Prometheus web UI using the NodePort.

i. Get the NodePort assigned to the Prometheus service:

`kubectl get svc prometheus-server-ext`
Snap:

ii. Get the minikube ip address using

`minikube ip`

Snap:

Access Prometheus in your web browser using the NodePort. For example:

`http://<Minikube_IP>:<NodePort>`

Snap:


**7. Install Grafana Using Helm:** processing is same as prometheus installation

**Add Helm Repository**

`helm repo add grafana https://grafana.github.io/helm-charts`

**Update Helm Repository**

`helm repo update`

**Install Grafana**
`helm install grafana grafana/grafana`

**Expose Grafana Service**
`kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext`
snap:

**Access Grafana Web UI**


****

