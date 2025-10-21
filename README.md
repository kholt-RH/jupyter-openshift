# Jupyter Notebook Helm Chart for OpenShift

This Helm chart deploys a single Jupyter Notebook instance on OpenShift, including:
- A Deployment (using `quay.io/jupyter/minimal-notebook:latest`)
- A Service for internal access
- An optional OpenShift Route for external HTTPS access
- Optional persistent storage for notebook files

---

## 🧩 Prerequisites

- OpenShift cluster with Helm 3 installed
- Access to a project/namespace where you can deploy resources
- (Optional) A default StorageClass for dynamic PVC provisioning

---

## 🚀 Installation

1. Clone or copy this chart:
   ```bash
   git clone https://github.com/<your-org>/jupyter-openshift.git
   cd jupyter-openshift
   ```

2. Install the chart:
   ```bash
   helm install my-jupyter . -n jupyter --create-namespace
   ```

3. Wait for the pod to start:
   ```bash
   oc get pods -n jupyter
   ```

4. Get the route URL:
   ```bash
   oc get route -n jupyter
   ```

5. Open the route URL in your browser.  
   The default login token is `openshift` (set in `values.yaml`).

---

## ⚙️ Configuration

You can override any default value using `--set` flags or a custom `values.yaml`.

### Common Overrides

| Setting | Description | Example |
|----------|--------------|----------|
| `env.jupyterToken` | Token to access the notebook | `--set env.jupyterToken=mysecret` |
| `persistence.enabled` | Enable persistent storage | `--set persistence.enabled=true` |
| `persistence.size` | PVC size | `--set persistence.size=10Gi` |
| `route.host` | Custom route hostname | `--set route.host=jupyter.apps.mycluster.local` |
| `route.tls.termination` | TLS type (`edge`, `reencrypt`, `passthrough`) | `--set route.tls.termination=reencrypt` |

---

## 🧹 Uninstall

To remove all deployed resources:

```bash
helm uninstall my-jupyter -n jupyter
```

---

## 🧠 Notes

- Default image: [quay.io/jupyter/minimal-notebook](https://quay.io/repository/jupyter/minimal-notebook)
- Default port: `8888`
- All resources will be created with the release name as a prefix (e.g., `my-jupyter`).

---

## 🧪 Example: Persistent Notebook with Custom Route

```bash
helm upgrade --install my-jupyter ./jupyter-notebook \
  -n jupyter \
  --set persistence.enabled=true \
  --set route.host=jupyter.apps.cluster.example.com \
  --set env.jupyterToken=mytoken
```
