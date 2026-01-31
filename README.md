# 🚀 Kubernetes Deployment & Health Checks (Liveness & Readiness)

This repository documents my hands-on learning of **Kubernetes Deployment**, **rollouts**, **rollback**, and **health checks** using **livenessProbe** and **readinessProbe**.

All commands below are taken directly from my practice session and organized step-by-step for easy understanding and recruiter review.

---

## 🧹 Cleanup Existing Resources

```bash
kubectl delete all --all
kubectl get all

---

## 📝 Create Deployment YAML

```bash
vim deploy.yml
```

### Example deploy.yml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeploy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: mycontainer
        image: nginx
        ports:
        - containerPort: 80
```

## ▶️ Apply Deployment

```bash
kubectl apply -f deploy.yml
kubectl get all
kubectl get pods
```

📸 Screenshot:

```
(images/kubectl-get-pods.png)
```

---

## 🔍 Describe Pod

```bash
kubectl describe pod mydeploy
```

📸 Screenshot:

```
(images/describe-pod.png)
```

---

## 🔄 Rollout & Rollback

### Rollout Undo

```bash
kubectl rollout undo deployment mydeploy
```

### Rollout History

```bash
kubectl rollout history deployment mydeploy

---

## 🔁 Update Image (Rolling Update)

```bash
kubectl set image deployment mydeploy mycontainer=httpd
kubectl rollout status deployment mydeploy --watch
```

📸 Screenshot:

```
(images/rolling-update.png)
```

---

## ❤️ Health Checks (Liveness & Readiness)

```bash
vim healthcheck.yml
```

### healthcheck.yml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: mycontainer
        image: nginx
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
```

Apply Health Check:

```bash
kubectl apply -f healthcheck.yml
kubectl get pods

---

## 🐚 Exec Into Pod

```bash
kubectl exec -it mydeploy-XXXXX -- /bin/bash
```

📸 Screenshot:

```
(images/kubectl-exec.png)
```

---

## 📚 What I Learned

* Kubernetes Deployment creation
* Rolling updates and rollbacks
* Deployment history tracking
* Liveness Probe for container health
* Readiness Probe for traffic readiness
* Debugging pods using kubectl exec

---

## 💼 Recruiter Notes

This project demonstrates real-world Kubernetes operational skills including:

✅ Zero-downtime deployments
✅ Health monitoring using probes
✅ Rollback strategies
✅ Pod-level troubleshooting

---

## 🔗 Author

**Rajiv Nakhawa**
Aspiring DevOps & Cloud Engineer | Kubernetes | Docker | AWS

---


