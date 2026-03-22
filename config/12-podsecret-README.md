# 12. Secrets in Pod (Using Environment Variables) 🔐

**Purpose:** Learn how to use a Kubernetes Secret inside a Pod as environment variables.

---

## 🎬 What We Created

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secrets-demo
spec:
  containers:
  - name: nginx
    image: nginx
    envFrom:
    - secretRef:
        name: pod-secret
````

👉 This Pod reads all values from a Secret (`pod-secret`) and injects them as environment variables.

---

## 🔑 Example Secret (username & password)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: pod-secret
type: Opaque
data:
  username: YWRtaW4=        # 'admin'
  password: cGFzc3dvcmQ=    # 'password'
```

---

## 🤔 What Happens Here?

* Kubernetes reads the Secret
* Converts Base64 → original values
* Injects them into the container as environment variables

So inside the container:

```bash
echo $username
echo $password
```

---

## 📊 How It Works (Simple Table)

| Step              | What Happens                        |
| ----------------- | ----------------------------------- |
| Secret Created    | Stores encoded username & password  |
| Pod Starts        | Reads Secret using `envFrom`        |
| Container Running | Values available as env variables   |
| App Uses Them     | Access via `$username`, `$password` |

---

## 🧪 Test Commands (Copy-Paste)

```bash
# 1. Create Secret
kubectl apply -f config/11-secrets.yaml -n roboshop

# 2. Deploy Pod
kubectl apply -f config/12-secrets-pod.yaml -n roboshop

# 3. Check Pod
kubectl get pods -n roboshop

# 4. Login into Pod
kubectl exec -it secrets-demo -n roboshop -- bash

# 5. Check environment variables
echo $username
echo $password
```

---

## 📌 Key Difference (env vs envFrom)

| Method    | Usage                          | When to Use           |
| --------- | ------------------------------ | --------------------- |
| `env`     | One variable at a time         | Few secrets           |
| `envFrom` | Load all variables from Secret | Many secrets (easier) |

---

## 🚀 Complete Workflow

```bash
# Apply Secret
kubectl apply -f config/11-secrets.yaml -n roboshop

# Apply Pod
kubectl apply -f config/12-secrets-pod.yaml -n roboshop

# Verify
kubectl get pod secrets-demo -n roboshop
```

---

## 🔍 Interview Question

**Q: How does a Pod use Secrets in Kubernetes?**

**Answer:**
A Pod can use Secrets by mounting them as environment variables or volumes. In this example, we used `envFrom` to load all secret values as environment variables inside the container.

---

## ⚠️ Important Note

* Base64 is **NOT encryption** ❌
* It is just encoding ✔
* For real security, use tools like:

  * AWS KMS
  * HashiCorp Vault

---

## ✅ Summary

* Secrets store sensitive data
* Pod uses `envFrom` to access them
* Values become environment variables
* Easy and commonly used in real projects

---

Status: 🔐 Secret Created → 📦 Injected into Pod → 🚀 Ready to Use

