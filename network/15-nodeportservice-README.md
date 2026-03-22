You’ve already done great work—this just needed **clean structure + small fixes + consistency**. I reorganized it so it reads like a **professional DevOps README**.

---

# ✅ Clean & Organized Version

````markdown
# 15. NodePort Service (External Access) 🌍

## 🎯 Purpose
Learn how to expose a Kubernetes Service **outside the cluster** using NodePort.

---

## 🎬 What We Created

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-np
spec:
  type: NodePort
  selector:
    project: roboshop
    component: frontend
    environment: dev
    purpose: node-port
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
````

👉 This Service exposes the application to the outside world using a **NodePort**.

---

## 🧠 Core Concept

```text
Pod = Application (nginx)
Service (ClusterIP) = Internal access
Service (NodePort) = External access

User → Node IP:NodePort → Service → Pod
```

---

## ⚠️ Important Requirement

Your Pod must have matching labels:

```yaml
labels:
  project: roboshop
  component: frontend
  environment: dev
  purpose: node-port
```

👉 If labels don’t match → Service will NOT work

---

## 🧪 Deployment & Initial Verification

```bash
# Apply Service
kubectl apply -f network/15-nodeport.yaml -n roboshop

# Verify Service
kubectl get svc -n roboshop
```

👉 Observe:

* TYPE → NodePort
* PORT → 80:3xxxx

---

## 📊 Expected Output

```text
NAME        TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
nginx-np    NodePort   10.x.x.x       <none>        80:3xxxx/TCP   10s
```

Example:

```text
80:30007/TCP
```

* 80 → Service port
* 30007 → NodePort

---

## 🔍 Detailed Verification Steps

### 1️⃣ Check Service

```bash
kubectl get svc nginx-np -n roboshop
```

---

### 2️⃣ Check Endpoints

```bash
kubectl get ep nginx-np -n roboshop
```

👉 Must NOT be: `<none>` ❌
👉 Should be: `192.xxx.xxx.xxx:80` ✅

---

### 3️⃣ Check Pod Labels

```bash
kubectl get pods --show-labels -n roboshop
```

👉 Ensure:

```text
purpose=node-port
```

✔ If missing → Service won’t connect

---

## 🌐 How to Access

### Step 1 — Get Node IP

```bash
curl ifconfig.me
```

Example:

```text
34.xxx.xxx.xxx
```

---

### Step 2 — Access in Browser

```text
http://<EC2-Public-IP>:<NodePort>
```

Example:

```text
http://34.xxx.xxx.xxx:30007
```

👉 You should see:
**Welcome to nginx!**

---

## ⚠️ Common Issues

| Issue              | Reason                 | Fix                  |
| ------------------ | ---------------------- | -------------------- |
| Page not loading   | Security Group blocked | Open NodePort in AWS |
| No endpoints       | Label mismatch         | Fix labels           |
| Connection refused | Pod not running        | Check pod status     |
| Wrong port         | Using service port     | Use NodePort         |

---

## 🔐 AWS Important Step

👉 Go to Security Group → Inbound Rules → Add:

```text
Type: Custom TCP
Port Range: 30000-32767
Source: 0.0.0.0/0
```

👉 Without this → NodePort will NOT work

---

## 📦 Git Workflow

```bash
git add network/15-nodeport.*
git commit -m "15-nodeport: exposed service externally using NodePort"
git push origin main
```

---

## 🔍 Interview Questions

### Q1: What is NodePort?

NodePort exposes a Service on a static port on each Node, allowing external access.

---

### Q2: What is NodePort range?

Default range: **30000–32767**

---

### Q3: Difference between ClusterIP and NodePort?

| Type      | Access        |
| --------- | ------------- |
| ClusterIP | Internal only |
| NodePort  | External      |

---

### Q4: Why not use NodePort in production?

NodePort is not scalable and not secure. In production, we use:

* LoadBalancer
* Ingress

---

## 🎯 Summary

* NodePort exposes Service externally
* Access using NodeIP + NodePort
* Requires matching labels
* Needs firewall/security group configuration

---

## 🚀 Status

🌐 Service Exposed → 🔗 NodePort Open → 🚀 Accessible from Browser

```
