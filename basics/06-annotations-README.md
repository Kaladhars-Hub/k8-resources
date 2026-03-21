## **06. Annotations Demo - Simple README** 🎯

**Save as `basics/06-annotations-demo-README.md`**

```markdown
# 06. Annotations Demo 📝

**Purpose:** Learn **metadata tags** (like sticky notes on pods)

## 🎬 What We Created

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: annotations-demo
  annotations:                    # ← STICKY NOTES!
    jenkins.url: "http://jenkins.joindevops.com/build/job/5"
    imageregistry: "https://hub.docker.com/"
spec:
  containers:
  - name: nginx
    image: nginx
```

## 🤔 **What Are Annotations? (SIMPLE)**

```
Think of annotations = POD STICKY NOTES!

Labels = "What type of pod?" (project=roboshop)
Annotations = "Extra info about this pod" (build URL, registry)

Example sticky notes on your pod:
- "Built by Jenkins job #5"
- "Image from Docker Hub"
```

## 🧪 **Test Commands (Copy-Paste)**

```bash
# 1. Deploy
kubectl apply -f basics/06-annotations-demo.yaml -n roboshop

# 2. See pod + annotations
kubectl get pods -n roboshop

# 3. Show ALL metadata (labels + annotations)
kubectl get pod annotations-demo -n roboshop -o yaml | grep -A 10 annotations

# 4. Annotations ONLY
kubectl get pod annotations-demo -n roboshop --show-labels
```

## 📊 **Expected Results**

```
# kubectl get pods
NAME                READY   STATUS    AGE
annotations-demo    1/1     Running   10s

# kubectl describe pod annotations-demo -n roboshop
Annotations:
  imageregistry:  https://hub.docker.com/
  jenkins.url:    http://jenkins.joindevops.com/build/job/5
```

## 💡 **Why Jenkins URL + Docker Hub? (REAL USE)**

| **Annotation** | **Why Add It?** | **Who Uses It?** |
|----------------|-----------------|--------------------|
| `jenkins.url` | **"This pod built by Jenkins job #5"** | DevOps: "Click → see build logs!" |
| `imageregistry` | **"Image came from Docker Hub"** | Support: "Check image history" |

```
PRODUCTION EXAMPLE:
Pod fails → Support team sees:
"jenkins.url: http://jenkins/job/prod-123" 
→ Click → "Oh! Bad commit in #123 → Rollback!"
```

## 🔍 **Labels vs Annotations (Simple Table)**

| **Labels** | **Annotations** |
|------------|-----------------|
| `project=roboshop` | `jenkins.url=...` |
| **Find pods** (`kubectl get pods -l project=roboshop`) | **Extra info** (URLs, build info) |
| **Short + searchable** | **Long text + non-searchable** |
| **10 chars max** | **Any length** |

## 🚀 **Complete Workflow**

```bash
# 1. Deploy
kubectl apply -f basics/06-annotations-demo.yaml -n roboshop

# 2. Verify running
kubectl get pods -n roboshop  # 1/1 Running ✓

# 3. See annotations
kubectl describe pod annotations-demo -n roboshop | grep -A 5 Annotations

# 4. Git it
git add basics/06-annotations-demo.*
git commit -m "06-annotations: metadata sticky notes demo"
git push origin main
```

## ✅ **Success Criteria**
- ✅ Pod `1/1 Running`
- ✅ `kubectl describe` shows **2 annotations**
- ✅ Understand **labels vs annotations**
- ✅ Know **why add build URLs**

## 🎯 **Real-World Use (You Get It Now!)**
```
CI/CD Pipeline → Creates pod → Adds annotation:
"jenkins.url: http://jenkins/job/prod-v2.3.1"

Alert: "Pod crashed!" → 
DevOps clicks URL → Sees bad commit → Fixes instantly!
```

**Status: ⏳ Deploy → Observe annotations → Git → Done!** 🚀
```

## **Quick Deploy + Test**
```bash
# Create YAML
cat > basics/06-annotations-demo.yaml << 'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: annotations-demo
  annotations:
    jenkins.url: "http://jenkins.joindevops.com/build/job/5"
    imageregistry: "https://hub.docker.com/"
spec:
  containers:
  - name: nginx
    image: nginx
EOF

# Deploy + test
kubectl apply -f basics/06-annotations-demo.yaml -n roboshop
kubectl get pods -n roboshop
kubectl describe pod annotations-demo -n roboshop | grep -A 5 Annotations
```

**Annotations = Sticky notes with build info! Deploy → `kubectl describe` → See jenkins URL → Git!** 🎉