# 09. ConfigMap (The Dictionary) 📖

**Purpose:** Learn how to store settings **outside** the Pod for easier management.

## 🎬 What We Created

```yaml
apiVersion: v1
kind: ConfigMap                 # ← A new object type!
metadata:
  name: pod-config              # ← The name Pods will use to find this
  namespace: roboshop
data:                           # ← THE DICTIONARY
  course: "DevSecOps with AWS"
  duration: "120hrs"
  trainer: "Elonmusk"


🤔 Why use a ConfigMap? (SIMPLE)

Think of ConfigMap = A GLOBAL SETTINGS FILE.

In Lab 08 (Env), the settings were "Tattooed" on the Pod. To change them, 
you had to kill the Pod.

In Lab 09 (ConfigMap), the settings are in a "Shared Notebook." 
Multiple Pods can read from this same notebook. If you update the notebook, 
you don't have to rewrite the Pod code!


🧪 Test Commands 

Bash
# 1. Create the ConfigMap
kubectl apply -f config/02-configmap.yaml

# 2. List all ConfigMaps in the namespace
kubectl get cm -n roboshop

# 3. See the "Dictionary" inside
kubectl describe cm pod-config -n roboshop

# 4. Alternative view (YAML format)
kubectl get cm pod-config -n roboshop -o yaml
📊 Expected Results
# kubectl get cm -n roboshop
NAME          DATA   AGE
pod-config    3      10s

# kubectl describe cm pod-config
Data
====
course:   DevSecOps with AWS
duration: 120hrs
trainer:  Elonmusk


🚀 The Complete Workflow (Moving to Config & Security)


Bash
# 1. Create the directory if it doesn't exist
mkdir -p config

# 2. Deploy the ConfigMap
kubectl apply -f config/02-configmap.yaml

# 3. Verify it exists
kubectl get cm pod-config -n roboshop

# 4. Git it (Notice the new folder!)
git add config/02-configmap.*
git commit -m "09-configmap: creating central dictionary for app settings"
git push origin main
✅ Success Criteria
✅ kubectl get cm shows pod-config.

✅ kubectl describe shows all 3 key-value pairs.

✅ Understand that ConfigMap is an independent object (It doesn't have a container!).

Status: ⏳ Create ConfigMap → Verify Dictionary → Ready for Pod Injection! 🚀

🤔 Why kind: ConfigMap? (SIMPLE)
Think of ConfigMap = A GLOBAL SETTINGS FILE.

- It doesn't run code.
- It doesn't use CPU or RAM.
- It just sits in the cluster holding information for Pods to read.

---

### **Verification for your Portfolio:**
**Question:** *"Can a ConfigMap exist without a Pod?"*
**Your Answer:** "Yes. A ConfigMap is an independent resource. It is like a library book—it stays on the shelf (the cluster) until a Pod (the reader) comes along to use it."