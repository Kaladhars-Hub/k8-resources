# 05. Error Test Pod 🚨

**Purpose:** Learn **3 Kubernetes failure modes** + **debug commands**

## 🎬 What We Did (Step-by-Step)

### **Step 1: Create Fake Image Failure**
```yaml
# basics/05-error-test.yaml
apiVersion: v1
kind: Pod
metadata:
  name: error-test
spec:
  containers:
  - name: nginx
    image: ngindfdfadsfsaf  # ← FAKE IMAGE (DELIBERATE!)


# Deploy:

kubectl apply -f basics/05-error-test.yaml -n roboshop
kubectl get pods -n roboshop

Result: error-test 0/1 ImagePullBackOff


# Step 2: Why Delete + Recreate Pod?

Kubernetes Rule: "Pods can't change clothes while wearing them!"

OLD POD (nginx name):

name: nginx
image: ngindfdfadsfsaf

NEW POD (almalinux name):

name: almalinux
image: almalinux:9


**PROBLEM:** Name changed = New person! Kubernetes says:❌ "I can't change nginx to almalinux. DELETE old pod first!"



**SOLUTION:**
```bash
kubectl delete pod error-test -n roboshop  # Remove old clothes
kubectl apply -f basics/05-error-test.yaml  # Wear new clothes



# Step 3: Test Valid Image (CrashLoopBackOff)

# Change to:
- name: almalinux
  image: almalinux:9  # ← VALID IMAGE

kubectl delete pod error-test -n roboshop
kubectl apply -f basics/05-error-test.yaml -n roboshop

# Result: error-test 0/1 CrashLoopBackOff


📊 Your Results Table

| Test        | Status               | What Happened                | Debug Command      |
| ----------- | -------------------- | ---------------------------- | ------------------ |
| Fake Image  | 0/1 ImagePullBackOff | ngindfdfadsfsaf not found    | kubectl describe   |
| Valid Image | 0/1 CrashLoopBackOff | almalinux exits (no command) | kubectl logs       |
| Events      | Shows timeline       | Pull attempts + failures     | kubectl get events |


🔍 Debug Commands (Production Ready!)

# 1. Status overview
kubectl get pods -n roboshop

# 2. Detailed error
kubectl describe pod error-test -n roboshop
# Events → "Failed to pull image", "ErrImagePull"

# 3. Container logs
kubectl logs error-test -n roboshop -c almalinux
# Empty = no command given

# 4. Full timeline
kubectl get events -n roboshop --sort-by='.lastTimestamp'


💡 Key Concepts Explained

| Status           | Meaning                     | Real-World Fix                      |
| ---------------- | --------------------------- | ----------------------------------- |
| ImagePullBackOff | Fake/broken image           | image: nginx or myregistry/app:v1.2 |
| CrashLoopBackOff | Container exits immediately | Add command: ["sleep", "1000"]      |
| 0/1              | No containers ready         | Pod broken                          |
| kubectl describe | Debug superpower            | Shows exact error reason            |


🚀 Complete Workflow (Copy-Paste)

# 1. FAKE IMAGE TEST
cat > basics/05-error-test.yaml << 'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: error-test
spec:
  containers:
  - name: nginx
    image: ngindfdfadsfsaf
EOF
kubectl apply -f basics/05-error-test.yaml -n roboshop
kubectl get pods -n roboshop  # 0/1 ImagePullBackOff ✓

# 2. DELETE + VALID IMAGE (CrashLoopBackOff)

kubectl delete pod error-test -n roboshop
kubectl apply -f basics/05-error-test.yaml -n roboshop
kubectl get pods -n roboshop  # 0/1 CrashLoopBackOff ✓

# 3. Debug
kubectl describe pod error-test -n roboshop
kubectl logs error-test -n roboshop -c almalinux  # Empty ✓


✅ Success Criteria (ALL PASS ✓)
✅ ImagePullBackOff with fake image

✅ CrashLoopBackOff with valid image

✅ kubectl describe shows exact errors

✅ Understand why delete + recreate

✅ Empty logs (no command given)

Status: ✅ 2 FAILURE MODES + DEBUG MASTERED


# Pod Lifecycle: Create → Fail → Debug → Fix → Repeat! 🚀

🎉 PRODUCTION DEBUG FLOW YOU MASTERED


🚨 Pod 0/1? 
  ↓
kubectl describe → Exact error
  ↓  
kubectl logs → Container output  
  ↓
kubectl events → Timeline
  ↓
Fix YAML → delete + apply
  ↓
1/1 Running ✓
