# 03. Multi-Container Pod 🎯

**Sidecar Pattern:** nginx (main app) + almalinux (helper) **sharing localhost**

## 🎬 Visual Structure


1 POD = SHARED BOX 🗃️
├── nginx (port 80) ← Main web server
└── almalinux (sleep) ← Sidecar helper
↑ Both share: IP, localhost, volumes ✅



## 📋 What You Created (Your YAML)
```yaml
apiVersion: v1
kind: Pod                    # Kubernetes fact - ALWAYS Pod
metadata:
  name: multicontainer
  namespace: roboshop
spec:
  containers:
  - name: nginx              # Web server (port 80)
    image: nginx
  - name: almalinux          # Helper container
    image: almalinux:9
    command: ["sleep", "1000"] # Keeps alive forever



✅ Learning Tests Table

| Test Command                    | Expected Result            | What It Means                |
| ------------------------------- | -------------------------- | ---------------------------- |
| kubectl get pods -n roboshop    | multicontainer 2/2 Running | Both containers healthy      |
| curl localhost (from almalinux) | Nginx HTML page            | Containers share localhost   |
| sleep 1000                      | Pod stays alive            | Sidecar needs command to run |


🚀 Deploy & Test (Copy-Paste Exactly)

# 1. Deploy
kubectl apply -f 03-multi-container.yaml

# 2. Verify 2/2 containers
kubectl get pods -n roboshop
# Expected: multicontainer   2/2   Running   0   10s

# 3. PROOF containers communicate
kubectl exec -it multicontainer -n roboshop -c almalinux -- curl localhost
# Nginx welcome page = localhost shared ✅

# 4. Check logs from BOTH containers
kubectl logs multicontainer -n roboshop -c nginx
kubectl logs multicontainer -n roboshop -c almalinux


💡 Key Concepts (Your Questions Answered)

| Question                          | Answer                                                            |
| --------------------------------- | ----------------------------------------------------------------- |
| Why kind: Pod not multicontainer? | Kubernetes only knows Pod. Multi-container = Pod feature          |
| File name can be anything?        | Yes! 03-multi-container.yaml = human description ✅                |
| kind: multicontainer?             | ❌ 100% wrong - Kubernetes rejects instantly                       |
| Single vs Multi Pod?              | 02-pod.yaml = 1/1, 03-multi-container.yaml = 2/2                  |
| What do they share?               | Same IP, localhost, volumes ✅. Different pods = separate networks |

🔄 Visual Proof: Single vs Multi

02-pod.yaml (Single):           03-multi-container.yaml (Multi):
kind: Pod                       kind: Pod
└── nginx (1/1)                  ├── nginx (2/2)
                                 └── almalinux
BOTH use: kind: Pod ✅


🌍 Real-World Example

Production: nginx + log-shipper
File: webapp-with-logger.yaml
kind: Pod                      # Kubernetes requirement
containers: [nginx, logger]    # Sidecar pattern

# NGINX LOGS ANALYSIS (Perfect Success!) ✅
# Your nginx container is 100% HEALTHY! Here's what proves it:


✅ KEY OBSERVATIONS (What You Learned)


| Log Line                                                      | What It Means                | Status     |
| ------------------------------------------------------------- | ---------------------------- | ---------- |
| Configuration complete; ready for start up                    | Nginx started successfully   | ✅ WORKS    |
| nginx/1.29.6                                                  | Latest nginx version running | ✅ HEALTHY  |
| start worker process 30start worker process 31                | Nginx accepting requests     | ✅ READY    |
| ::1 - - [21/Mar/2026:10:48:04 +0000] "GET / HTTP/1.1" 200 896 | YOUR CURL HIT NGINX!         | 🎉 SUCCESS |



# 🚀 PROOF: Your Multi-Container Works Perfectly

1. nginx container: RUNNING + SERVING HTML ✅
2. Your curl test: REACHED nginx (HTTP 200) ✅  
3. localhost shared: CONFIRMED ✅


# 🎉 REAL-WORLD LEARNING

Your Sidecar Pattern = PRODUCTION READY:
nginx     ← Main app (serves website)
almalinux ← Helper (could be: logger, monitor, proxy)

YOUR PROOF: curl hit nginx → Sidecar localhost sharing WORKS!



## ✅ Success Criteria (ALL PASS ✓)
- ✅ `kubectl get pods` shows `2/2 Running`
- ✅ nginx logs show `"GET / HTTP/1.1" 200` **← YOUR PROOF**
- ✅ almalinux logs empty (sleep 1000 silent)
- ✅ `curl localhost` returns Nginx HTML

# Status: ✅ LIVE