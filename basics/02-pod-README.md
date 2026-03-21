## STEP-BY-STEP LEARNING (Your File)

| Section | Your Code | What It Does |
|---------|-----------|--------------|
| `apiVersion: v1` | ✓ | Uses stable Kubernetes API |
| `kind: Pod` | ✓ | Tells Kubernetes: "Create Pod" |
| `name: nginx` | ✓ | Pod called "nginx" |
| `namespace: roboshop` | ✓ | Isolated in your project |
| `labels` | ✓ | Services find this pod |
| `image: nginx` | ✓ | Downloads nginx Docker image |
| `containerPort: 80` | ✓ | Nginx listens on port 80 |

# TEST YOUR POD (5 Commands)


# 1. Deploy
kubectl apply -f 02-pod.yaml

# 2. Check status  
kubectl get pods -n roboshop
# Expected: nginx   1/1   Running   0   10s

# 3. Detailed view
kubectl describe pod nginx -n roboshop
# Events → Image pulled → Container started

# 4. Test inside pod
kubectl exec -it nginx -n roboshop -- curl localhost
# Expected: Nginx welcome page HTML


# WHAT TO OBSERVE (Key Learning)

# kubectl get pods -n roboshop
# NAME    READY   STATUS    RESTARTS   AGE
# nginx   1/1     Running   0          30s  ← SUCCESS!

# If STATUS = ContainerCreating → Wait 30s
# If ImagePullBackOff → Wrong image name
# If CrashLoopBackOff → No command in image
