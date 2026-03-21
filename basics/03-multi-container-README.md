# basics/03-multi-container.yaml

# You wrote a sidecar pattern - nginx web server + almalinux helper! Great concept!

# ✅ WHAT YOU GOT RIGHT

# ✓ Perfect 2-container structure
# ✓ nginx image (web server)  
# ✓ almalinux:9 + sleep 1000 (keeps alive)
# ✓ Good labels
# ✓ command: ["sleep", "1000"] (sidecar alive)


## Multi-Container Pod (basics/03-multi-container.yaml)

**Purpose:** 2 containers in 1 pod = **Sidecar pattern**
- nginx = Main web server
- almalinux = Helper container (share localhost)

### What You Created

1 Pod ← Contains 2 containers
├── nginx (port 80)
└── almalinux (sleep forever)
↑ Both talk via localhost


### Deploy & Test
```bash
kubectl apply -f basics/03-multi-container.yaml
kubectl get pods -n roboshop
# Expected: multicontainer   2/2   Running   0   10s


Key Tests


# 1. Both containers healthy?
kubectl get pods -n roboshop  
# READY: 2/2 = SUCCESS!

# 2. Access nginx FROM almalinux
kubectl exec -it multicontainer -n roboshop -c almalinux -- bash
curl localhost                    # Nginx HTML page!
exit

# 3. Logs from BOTH containers
kubectl logs multicontainer -n roboshop -c nginx
kubectl logs multicontainer -n roboshop -c almalinux


### What You Learned Table

| Test | Expected Result | What It Means |
|------|-----------------|---------------|
| `2/2 Running` | SUCCESS | Both nginx + almalinux healthy |
| `curl localhost` | Nginx HTML page | Containers share same network |
| `sleep 1000` | Keeps almalinux alive | Sidecar needs command to run |


Status: ⏳ Deploy → Test curl localhost → ✅ Live


## **TEST STEP-BY-STEP (Do This Exactly)**

```bash

# 1. Deploy
kubectl apply -f basics/03-multi-container.yaml

# 2. **MOST IMPORTANT TEST**
kubectl get pods -n roboshop
# Expected: multicontainer   2/2   Running   0   10s
#              ↑↑    ↑
#           READY STATUS

# 3. PROOF containers communicate
kubectl exec -it multicontainer -n roboshop -c almalinux -- curl localhost
# Nginx welcome page = Containers share localhost!

# 4. Update README + Git
git add basics/03-multi-container.yaml README.md
git commit -m "03-multi-container.yaml: nginx + almalinux sidecar"
git push


WHAT MAKES THIS SPECIAL

# SINGLE POD = SHARED:
# ✅ Same IP address
# ✅ localhost communication  
# ✅ Shared volumes (later)
# ❌ Different pods = separate networks
