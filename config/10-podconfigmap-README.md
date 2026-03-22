This is the "Magic Moment" in Kubernetes. You have the Dictionary (ConfigMap) and now you are going to create the Reader (The Pod).

Instead of typing every single variable inside the Pod, we use a single command called envFrom. This tells the Pod: "Go to that ConfigMap, take everything inside it, and turn it into environment variables for me."

🛠️ The "Reader" Pod (config/10-pod-configmap.yaml)
Copy this exactly into your config folder. Notice how clean it looks because the data is stored elsewhere!

YAML
apiVersion: v1
kind: Pod
metadata:
  name: pod-config-reader
spec:
  containers:
  - name: nginx
    image: nginx
    envFrom:                 # ← THE MAGIC INSTRUCTION
    - configMapRef:          # ← "Look at a ConfigMap"
        name: pod-config     # ← The name of the Dictionary we made in 09
10. Pod with ConfigMap - Simple README 🎯
Save as config/10-pod-configmap-README.md


# 10. Pod with ConfigMap (The Reader) 📖➡️📦

**Purpose:** Learn how to "Inject" a whole dictionary of settings into a Pod using `envFrom`.

## 🎬 What We Created

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-config-reader
spec:
  containers:
  - name: nginx
    image: nginx
    envFrom:                 # ← THE KEY!
    - configMapRef:          # ← Reference the Dictionary
        name: pod-config     # ← From Lab 09
🤔 What is envFrom? (SIMPLE)
Think of envFrom = BULK IMPORT!

- Lab 08 (Env): You had to manually type every single variable. (Slow)
- Lab 10 (envFrom): You just point to the ConfigMap, and K8s 
  automatically loads all 3 variables (course, duration, trainer). (Fast)


#🧪 Test Commands 

Bash
# 1. Deploy the Pod
kubectl apply -f config/10-pod-configmap.yaml -n roboshop

# 2. Wait for it to be 'Running'
kubectl get pods -n roboshop

# 3. PROOF: Check if the variables from the ConfigMap are inside!
kubectl exec -it pod-config-reader -n roboshop -- env | grep -E "course|trainer|duration"


#📊 Expected Results

# Output from the 'env' command:
course=DevSecOps with AWS
duration=120hrs
trainer=Elonmusk

## 📊 Comparison: Why we use `envFrom`

| Scenario | Old Way (`env`) | New Way (`envFrom`) |
| :--- | :--- | :--- |
| **Adding 50 variables** | Type 100+ lines of YAML. | Type **3 lines** of YAML. |
| **Changing a Setting** | Edit every single Pod file. | Edit **ONE** ConfigMap file. |
| **Multiple Apps** | Copy-paste variables everywhere. | All apps point to one central source. |

> **DevOps Pro-Tip:** In a production environment like **RoboShop**, your 'User', 'Cart', and 'Shipping' services might all need the same `MONGO_URL`. Instead of typing it 3 times, you create one ConfigMap and use `envFrom` in all three services!

DEVOPS TRUTH: 
Using ConfigMaps with envFrom makes your code 
DRY (Don't Repeat Yourself). It makes managing 
100 microservices as easy as managing one!


#🚀 Complete Workflow

Bash
# 1. Ensure you are in the 'config' folder
# 2. Deploy
kubectl apply -f config/10-pod-configmap.yaml -n roboshop

# 3. Verify
kubectl exec -it pod-config-reader -n roboshop -- env | grep trainer

# 4. Git it
git add config/10-pod-configmap.*
git commit -m "10-pod-config: successfully injected bulk config via envFrom"
git push origin main
Status: ⏳ Pod Created → 🔗 Linked to ConfigMap → ✅ Proof in Env → Git! 🚀


---

### 🔍 **Important Troubleshooting Tip:**
If you try to create this Pod **before** you create the ConfigMap, the Pod will fail with a **`CreateContainerConfigError`**. 

**Why?** Because the Pod is looking for its "Instruction Manual" (ConfigMap) and can't find it. Always create your ConfigMaps/Secrets **BEFORE** the Pods that need them.
