# 04. Labels Pod 🎯

**Your Instructor's EXACT YAML** - nginx pod with **4 powerful labels** for organization

## 🎬 Visual Structure


frontend Pod ← Tagged with 4 Labels (like Instagram hashtags)

├── project: roboshop (Which app?)

├── component: frontend (Which part?)

├── environment: dev (Dev/Prod?)

└── version: v1 (v1/v2?)


## 📋 EXACT YAML Created (Instructor's Code)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend
  namespace: roboshop
  labels:
    project: roboshop          # ← PROJECT TAG
    component: frontend        # ← APP PART TAG  
    environment: dev           # ← ENVIRONMENT TAG
    version: v1               # ← VERSION TAG
spec:
  containers:
  - name: frontend
    image: nginx              # Frontend web server ✅


🚀 Deploy & Test (Copy-Paste Exactly)

# 1. Deploy
kubectl apply -f 04-labels.yaml

# 2. See ALL labels attached
kubectl get pods -n roboshop --show-labels

# 3. Test EACH label filter (MAGIC!)
kubectl get pods -n roboshop -l project=roboshop
kubectl get pods -n roboshop -l component=frontend  
kubectl get pods -n roboshop -l environment=dev
kubectl get pods -n roboshop -l version=v1


✅ Learning Tests Table

| Test Command                   | Expected                                         | What You Learned     |
| ------------------------------ | ------------------------------------------------ | -------------------- |
| kubectl get pods --show-labels | frontend ... project=roboshop,component=frontend | Labels visible ✅     |
| -l project=roboshop            | frontend                                         | Filter roboshop pods |
| -l component=frontend          | frontend                                         | Frontend only        |
| -l environment=dev             | frontend                                         | Dev environment only |
| -l version=v1                  | frontend                                         | v1 version only      |


💡 YOUR DOUBTS ANSWERED (Complete!)


| Your Question       | Clear Answer                                             |
| ------------------- | -------------------------------------------------------- |
| "nginx = frontend?" | ✅ YES! Nginx serves web pages = Frontend component       |
| component: frontend | Labels this nginx pod as "frontend part" of roboshop app |
| environment: dev    | Development env. Later: staging, prod namespaces         |
| version: v1         | Best Practice! Deploy v2 later side-by-side              |
| Why repeat labels?  | Each pod needs OWN labels for filtering                  |


🎯 Label Meanings (Production Ready)


| Label               | Purpose                  | Real Example                         |
| ------------------- | ------------------------ | ------------------------------------ |
| project: roboshop   | Groups ALL roboshop pods | kubectl get pods -l project=roboshop |
| component: frontend | Identifies app layer     | Frontend/backend/database            |
| environment: dev    | Env separation           | dev/staging/prod                     |
| version: v1         | Version control          | v1 → v2 upgrade                      |


🌍 Real-World Power (100+ Pods)

PROD Cluster (chaos without labels):
❓ 150 pods → Which is frontend? Which is roboshop?

✅ WITH LABELS:
kubectl get pods -l project=roboshop,component=frontend,environment=prod
→ frontend-v1   1/1 Running  # INSTANT!


✅ Success Criteria (All Tests)
✅ kubectl get pods --show-labels = 4 labels visible

✅ -l project=roboshop finds frontend pod

✅ -l component=frontend finds frontend pod

✅ -l environment=dev finds frontend pod

✅ -l version=v1 finds frontend pod

✅ Pod 1/1 Running (nginx healthy)

Status: ⏳ Deploy → Test 5 filters → ✅ LIVE


Labels = Kubernetes Superpower! 🚀

