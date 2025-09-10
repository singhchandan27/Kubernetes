# Dumbpad Deployment on KIND (Kubernetes in Docker)

This repository contains Kubernetes manifests to deploy **Dumbpad** on a **KIND cluster** with persistent storage and service exposure.

---

## 📂 Repository Structure
```
.
├── deployment.yml                  # Deployment manifest (3 replicas of Dumbpad app)
├── persistentvolume.yml            # PersistentVolume (PV) for local storage
├── persitancevolumeclam.yml        # PersistentVolumeClaim (PVC) bound to PV
└── service.yml                     # Service to expose Dumbpad pods
```

---

## ⚙️ Prerequisites

- [KIND](https://kind.sigs.k8s.io/) installed and running  
- [kubectl](https://kubernetes.io/docs/tasks/tools/) configured to access KIND cluster  
- Docker installed and running  

---

## 🚀 Deployment Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/singhchandan27/Kubernetes.git
   cd Kubernetes/dumbpad
   ```

2. **Create namespace** (Create NS as per your need)
   ```bash
   kubectl create namespace nginx
   ```

3. **Apply PersistentVolume (PV)**
   ```bash
   kubectl apply -f persistentvolume.yml
   ```

4. **Apply PersistentVolumeClaim (PVC)**
   ```bash
   kubectl apply -f persitancevolumeclam.yml
   ```

5. **Deploy Dumbpad app**
   ```bash
   kubectl apply -f deployment.yml
   ```

6. **Create Service**
   ```bash
   kubectl apply -f service.yml
   ```

---

## 📦 Components

### 1. Deployment (`deployment.yml`)
- Runs **3 replicas** of the Dumbpad app  
- Uses Docker image: `dumbwareio/dumbpad:latest`  
- Mounts persistent storage at `/app/data`  

### 2. PersistentVolume (`persistentvolume.yml`)
- **1Gi storage** on host path `/mnt/app`  
- Storage class: `local-storage`  
- Reclaim policy: `Retain`  

### 3. PersistentVolumeClaim (`persitancevolumeclam.yml`)
- Requests **1Gi storage**  
- Access mode: `ReadWriteOnce`  

### 4. Service (`service.yml`)
- Exposes Dumbpad pods on port **3000** inside the cluster  
- Service type: **ClusterIP**  

---

## 🔍 Verify Deployment

Check all resources:
```bash
kubectl get all -n nginx
```

Check PersistentVolumes:
```bash
kubectl get pv
kubectl get pvc -n nginx
```

Port-forward service for local access:
```bash
kubectl port-forward svc/dumbpad-service -n nginx 3000:3000
```

Now open [http://localhost:3000](http://localhost:3000) in your browser 🎉  

---

## 🧹 Cleanup

To remove all resources:
```bash
kubectl delete -f service.yml
kubectl delete -f deployment.yml
kubectl delete -f clamvoluem.yml
kubectl delete -f persistvolume.yml
kubectl delete namespace nginx
```

---

## 📌 Notes

- This deployment is configured for **KIND** (uses `hostPath` storage).  
- In cloud environments (AWS, GCP, Azure), replace PV and PVC with cloud-specific storage classes.  
- For external access, update the service type from `ClusterIP` to `NodePort` or `LoadBalancer`.  
