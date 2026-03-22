# 08. Environment Variables 🌍

**Purpose:** Learn how to pass **Hardcoded Settings** into a container (The "Old Way").

## 🎬 What We Created

```yaml
kind: Pod                       # ← Type of object
apiVersion: v1                  # ← API Version
metadata:
  name: env-demo
spec:
  containers:
  - name: nginx
    image: nginx
    env:                        # ← THE SETTINGS START HERE
    - name: course              # Key
      value: kubernetes         # Value
    - name: trainer             # Key
      value: Elonmusk           # Value
🤔 What Are Environment Variables? (SIMPLE)
Think of Env Vars = APP INSTRUCTIONS!

When the container starts, it looks at these "Notes" to know how to behave.
Example: 
- "What database should I connect to?"
- "What is my trainer's name?"

In this lab, we are "Injecting" the words 'kubernetes' and 'Elonmusk' 
directly into the Nginx container's brain.
🧪 Test Commands (Copy-Paste)
Bash
# 1. Deploy
kubectl apply -f basics/08-env-demo.yaml -n roboshop

# 2. Check pod status
kubectl get pods -n roboshop

# 3. PROOF: Go inside the pod and check the variables
kubectl exec -it env-demo -n roboshop -- env

# 4. Search for specific variables
kubectl exec -it env-demo -n roboshop -- env | grep -E "course|trainer"
📊 Expected Results
# Output from the 'env' command:
course=kubernetes
trainer=Elonmusk
KUBERNETES_SERVICE_PORT=443
...
💡 Why use env? (REAL USE)
Setting	Value	Why?
DB_URL	db.roboshop.com	Tells the app where the database lives.
LOG_LEVEL	DEBUG	Tells the app to show more detailed errors.
APP_COLOR	blue	Used for "Blue-Green" deployment testing.
DEVOPS WARNING: 
This is the "Hardcoded" way. If the trainer changes from 'Elonmusk' 
to 'Kaladhar', you have to edit this YAML and re-deploy. 
(Later, we will use ConfigMaps to solve this!)
🔍 Order of Keys (The "Kind vs ApiVersion" Question)
YAML
# THIS IS VALID:
kind: Pod
apiVersion: v1

# THIS IS ALSO VALID:
apiVersion: v1
kind: Pod
Why? YAML is a "Map" (Key-Value pairs). Kubernetes reads the whole file into its memory first, then looks for the keys it needs. The order doesn't matter, but indentation (spaces) always does!

🚀 Complete Workflow
Bash
# 1. Create/Fix the YAML
# 2. Deploy
kubectl apply -f basics/08-env-demo.yaml -n roboshop

# 3. Verify variables are inside
kubectl exec -it env-demo -n roboshop -- env | grep trainer

# 4. Git it
git add config/08-environments config/08-environments-README.md
git commit -m "08-env: injecting hardcoded environment variables"
git push origin main
✅ Success Criteria
✅ Pod 1/1 Running

✅ kubectl exec shows course=kubernetes

✅ Understand that env variables are injected at startup

✅ Know that Order of kind/apiVersion does not matter

Status: ⏳ Deploy → Exec into Pod → Verify Env → Git! 🚀
