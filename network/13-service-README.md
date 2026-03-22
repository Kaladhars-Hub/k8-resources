A Service is the "Permanent Phone Number" for your Pods. Since Pods die and get new IP addresses all the time, the Service stays the same so users can always find the app.

# 13. Service (The Permanent Address) 📞🌐

**Purpose:** Learn how to create a **Service** to give your Pods a stable IP and Load Balancer.

## 🎬 1. What We Created (The YAML)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx              # ← The "Phone Book" name
spec:
  selector:                # ← THE MATCHMAKER
    project: roboshop      # Looks for Pods with these
    component: frontend    # exact labels to send
    environment: dev       # traffic to them.
  ports:
    - protocol: TCP
      port: 80             # Service Port (The "Outside" door)
      targetPort: 80       # Container Port (The "Inside" door)
🤔 2. Why do we need a Service? (SIMPLE)
Plaintext
| Fact                   | Problem                               | Solution: Service 📞 |
| :--------------------- | :------------------------------------ | :------------------ |
| **Pods are Mortal** | They die and get new IPs constantly.  | Stays alive forever. |
| **Pods are many** | Which one of the 10 Pods do I call?   | Acts as a Load Balancer. |
| **Communication** | Hard to track 10.0.0.5, 10.0.0.12...  | Use the name 'nginx'. |
🧪 3. Test Commands (Copy-Paste)
Bash
# 1. Create a Pod with matching labels FIRST (Important!)
kubectl run nginx-pod --image=nginx -l project=roboshop,component=frontend,environment=dev -n roboshop

# 2. Deploy the Service
kubectl apply -f network/13-service.yaml -n roboshop

# 3. List Services
kubectl get svc -n roboshop

# 4. Describe the Service (Check the "Endpoints" section)
kubectl describe svc nginx -n roboshop
📊 4. The "Port" Comparison Table
Plaintext
| Port Type       | Description                                      |
| :-------------- | :----------------------------------------------- |
| **port** | The port number exposed by the Service itself.   |
| **targetPort** | The port the application is listening on (Pod).  |
| **nodePort** | (Optional) A port on the physical EC2 instance. |
🚀 5. Complete Workflow
Bash
# 1. Ensure the 'network' folder exists
mkdir -p network

# 2. Deploy
kubectl apply -f network/13-service.yaml -n roboshop

# 3. Verify Endpoints (This proves the Service found the Pod)
kubectl get ep nginx -n roboshop

# 4. Git it
git pull origin main --rebase
git add network/13-service.*
git commit -m "13-service: added stable networking for frontend"
git push origin main
🔍 6. Interview Power Answer
Plaintext
Question: "How does a Service know which Pods to send traffic to?"

Your Answer: "It uses 'Selectors' and 'Labels'. The Service looks for any Pod 
in the namespace that has labels matching its selector. If a Pod matches, 
its IP is added to the Service's 'Endpoints' list automatically."
Status: ⏳ Service Created → 🔗 Selector Matched → ✅ Traffic Ready → Git! 🚀


---

### **Important Note for your Lab:**
If you run `kubectl describe svc nginx` and the **Endpoints** section is **empty (none)**, it means your Service is "lonely." It couldn't find any Pods with the labels `project: roboshop`, `component: frontend`, and `environment: dev`. 

**Always make sure your Pod Labels and Service Selectors match exactly!**