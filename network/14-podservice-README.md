# 📘 1. Service Demo Pod

---

````markdown
# 14. Service Demo Pod 🏷️

## 🎯 Purpose
Learn how to attach **labels** to a Pod so it can be selected by a Kubernetes Service.

---

## 🎬 What We Created

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: service-demo
  labels:
    project: roboshop
    component: frontend
    environment: dev
spec:
  containers:
  - name: nginx
    image: nginx
````

👉 This Pod includes labels used by Services for discovery.

---

## 🏷️ What Are Labels?

Labels are key-value pairs attached to Kubernetes objects.

### Example:

* project: roboshop
* component: frontend
* environment: dev

### Used For:

* Selecting Pods
* Grouping resources
* Connecting Services

---

## 📊 Label Breakdown

| Label Key   | Value    | Purpose                |
| ----------- | -------- | ---------------------- |
| project     | roboshop | Identifies application |
| component   | frontend | Specifies app layer    |
| environment | dev      | Defines environment    |

---

## 🤔 Why Labels Matter

| Scenario           | Without Labels ❌ | With Labels ✅  |
| ------------------ | ---------------- | -------------- |
| Service Discovery  | Not possible     | Works properly |
| Pod Identification | Difficult        | Easy           |
| Scaling & Grouping | Hard             | Simple         |

---

## ⚠️ Important: Clean Environment (Your Doubt)

Before testing Services, check existing pods:

```bash
kubectl get pods -n roboshop
```

If you see an old pod like:

```
nginx-pod
```

👉 It may interfere with your test if it has similar labels.

### ✅ 🛠️ Connectivity Test
⚠️ If test pod already exists:

```bash
kubectl delete pod nginx-pod -n roboshop
```

✔ Keeps environment clean
✔ Avoids confusion
✔ Ensures Service targets correct Pod

---

## 🔗 How Service Uses Labels

```yaml
selector:
  project: roboshop
  component: frontend
  environment: dev
```

👉 Service matches labels and routes traffic.

---

## 🧪 Test Commands

```bash
# Deploy Pod
kubectl apply -f network/14-podservice.yaml -n roboshop

# Verify labels
kubectl get pods -n roboshop --show-labels

# Test Label Selector
kubectl get pods -n roboshop -l component=frontend
```

---

## 📊 Expected Output

```
service-demo   1/1   Running   project=roboshop,component=frontend,environment=dev
```

---

## 🔍 Verification (MOST IMPORTANT)

```bash
#Get Pod IP
kubectl get pod service-demo -n roboshop -o wide
#Check Service Endpoints
kubectl get ep nginx -n roboshop
```

### ✔ Check:

* Pod IP
* Endpoint IP

👉 If both match → 
✔ Service is correctly routing traffic.
✔ Labels are working
✔ You understood Service Discovery

---

## 🛠️ Connectivity Test

```bash
kubectl run test-connection --rm -it \
--image=curlimages/curl -n roboshop \
-- curl http://nginx
```

👉 Output:

```
Welcome to nginx!
```

---

## 📦 Git Workflow

```bash
git add basics/07-service-demo.*
git commit -m "07-service-demo: pod ready for service discovery"
git push origin main
```

---

## 🧠 Core Concept

Pod = Application (nginx)
Labels = Identity (frontend)
Service = Traffic Router

Service → Matches labels → Sends traffic → Pod responds

---

## 🔍 Interview Questions

### Q1: Why are labels important?

Labels are used to organize and identify resources. Services use them to find and connect to Pods.

---

### Q2: Why not use Pod IP?

Pods are ephemeral and IPs change. Labels provide stable identification. This is called **loose coupling**.

---

### Q3: How to verify Service → Pod connection?

Compare Pod IP and Service endpoints using:

* `kubectl get pod -o wide`
* `kubectl get ep`


## 🧠 Interview Answer

**Q: How do you verify Service is connected to a Pod?**

Answer:

> I check the Pod IP using `kubectl get pod -o wide` and compare it with Service endpoints using `kubectl get ep`. If both match, the Service is correctly routing traffic.

---

## 🎯 Summary

* Labels are key-value pairs
* Used for grouping and identification
* Required for Service communication
* Enable dynamic service discovery

---

## 🚀 Status

🏷️ Labels Ready → 🔗 Service Connected → 🚀 Traffic Flow Verified

```

---

⚖️ get vs describe (Simple)

| Command            | Use Case           |
| ------------------ | ------------------ |
| `kubectl get`      | Quick overview     |
| `kubectl describe` | Detailed debugging |


✔ Use:

kubectl get pods

kubectl get ep