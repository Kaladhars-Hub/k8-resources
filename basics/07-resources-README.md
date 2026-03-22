This is one of the most critical steps in becoming a DevOps engineer. In a real company, if you don't set these values, one "greedy" application could eat all the RAM on a server and crash the entire system.

Here is the simple breakdown of your instructor's code, followed by the README in your exact requested format.

---

### 💡 Simple Breakdown: What is happening here?

Think of your **Worker Node (EC2)** as a **Hotel**.

1.  **`requests` (The Reservation):**
    * This is the minimum your pod needs to "check-in." 
    * If you say `cpu: 100m`, Kubernetes looks for a hotel room (Node) that has at least 10% of a CPU core empty. 
    * **If no room is available, your Pod stays "Pending."**

2.  **`limits` (The Security Guard):**
    * This is the absolute maximum the pod is allowed to use.
    * **Memory Limit:** If your pod tries to use 101Mi but the limit is 100Mi, Kubernetes kills it instantly (**OOMKilled**).
    * **CPU Limit:** If your pod tries to use more than 120m, Kubernetes doesn't kill it; it just **throttles** (slows it down) like an internet speed cap.



---

## **05. Resource Management - Simple README** 🎯

**Save as `basics/05-resources-demo-README.md`**

```markdown
# 05. Resource Management 🏗️

**Purpose:** Learn **Resource Contracts** (Managing CPU and RAM usage)

## 🎬 What We Created

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resources-demo
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:             # ← THE RESERVATION (Min needed)
        memory: "68Mi"
        cpu: "100m"         # 1/10th of a CPU core
      limits:               # ← THE CEILING (Max allowed)
        memory: "100Mi"
        cpu: "120m"
```

## 🤔 **What Are Resources? (SIMPLE)**

```
Think of Resources = HOTEL RULES!

Requests = "I need a room with 2 beds guaranteed."
Limits = "If you try to bring 10 people into the room, security kicks you out."

Why use them?
- To stop "Bully" pods from stealing all the RAM.
- To help K8s find the best server (Node) to put your pod on.
```

## 🧪 **Test Commands (Copy-Paste)**

```bash
# 1. Deploy
kubectl apply -f basics/07-resources-demo.yaml -n roboshop

# 2. See pod status
kubectl get pods -n roboshop

# 3. Verify the "Contract" details
kubectl describe pod resources-demo -n roboshop | grep -A 5 Requests

# 4. Check real-time usage (If metrics server is installed)
kubectl top pod resources-demo -n roboshop
```

## 📊 **Expected Results**

```
# kubectl describe pod resources-demo -n roboshop
Requests:
  cpu:     100m
  memory:  68Mi
Limits:
  cpu:     120m
  memory:  100Mi
```

## 💡 **Why Requests vs Limits? (REAL USE)**

| **Type** | **Scenario** | **Result** |
|----------|--------------|------------|
| **Requests** | **Node is 95% full** | K8s won't put your pod there. It looks for a "bigger room." |
| **Limits (RAM)** | **Memory Leak** | Pod exceeds 100Mi → Status: **OOMKilled** (Out of Memory). |
| **Limits (CPU)** | **High Traffic** | Pod tries to use 200m → K8s **Throttles** (Slows it down). |

```
PRODUCTION EXAMPLE:
A developer writes bad code that uses infinite RAM. 
WITHOUT Limits: The whole EC2 crashes. Production is DOWN. 
WITH Limits: Only that one pod is killed. Everything else is SAFE!
```

## 🔍 **CPU vs Memory Units (Simple Table)**

| **Unit** | **Meaning** | **Analogy** |
|------------|-----------------|-----------------|
| `100m` | **100 millicores** | 10% of a single CPU slice |
| `1000m` | **1 Full Core** | 100% of one CPU |
| `68Mi` | **68 Mebibytes** | Roughly 68 MB of RAM |

## 🚀 **Complete Workflow**

```bash
# 1. Deploy
kubectl apply -f basics/07-resources-demo.yaml -n roboshop

# 2. Verify running
kubectl get pods -n roboshop  # 1/1 Running ✓

# 3. Check for "OOMKilled" errors (if you set limits too low)
kubectl get pods -n roboshop -w

# 4. Git it
git add basics/05-resources-demo.*
git commit -m "05-resources: managing cpu and ram limits"
git push origin main
```

## ✅ **Success Criteria**
- ✅ Pod `1/1 Running`
- ✅ `kubectl describe` shows **Correct Requests & Limits**
- ✅ Understand **CPU Throttling vs OOMKilled**
- ✅ Know that **Requests help with Scheduling**

## 🎯 **Real-World Use (You Get It Now!)**
```
Cloud Cost Optimization:
By setting requests correctly, we can pack more pods onto fewer EC2 instances. 
Less EC2s = Smaller AWS Bill! 💰
```

**Status: ⏳ Deploy → Check Describe → Monitor Usage → Git!** 🚀
```

---

### **Quick Deploy + Test**
```bash
# Create YAML
cat > basics/07-resources-demo.yaml << 'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: resources-demo
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        memory: "68Mi"
        cpu: "100m"
      limits:
        memory: "100Mi"
        cpu: "120m"
EOF

# Deploy + test
kubectl apply -f basics/07-resources-demo.yaml -n roboshop
kubectl describe pod resources-demo -n roboshop | grep -A 10 resources
```


## 🩺 Troubleshooting This File

### Common Error: OOMKilled
If you see `Status: OOMKilled`, it means the Nginx container tried to use more than **100Mi**.
1. **Check:** `kubectl describe pod resources-demo -n roboshop`
2. **Look for:** `Exit Code: 137`
3. **Fix:** Increase `limits.memory` to `128Mi` and re-apply.

### Common Error: Pending
If the pod stays `Pending`:
1. **Check:** `kubectl get nodes` to see if nodes are healthy.
2. **Check:** `kubectl describe pod` for `FailedScheduling`.
3. **Fix:** Your `requests.cpu: "100m"` might be too high for the remaining space on your nodes.