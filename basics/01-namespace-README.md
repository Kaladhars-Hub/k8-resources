# 01. Namespace (basics/01-namespace.yaml)

**Purpose:** Creates isolated project space for roboshop (like VPC for Kubernetes)

## What You Created
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: roboshop
  labels:
    environment: development
    project: roboshop

# What You Learned Table

| Test Command                                  | Expected Result               | What It Means               |
| --------------------------------------------- | ----------------------------- | --------------------------- |
| kubectl get ns roboshop                       | roboshop Active 10s           | Namespace created + healthy |
| kubectl get ns                                | roboshop in 5 namespaces list | Added to cluster            |
| kubectl get pods -n roboshop                  | No resources found            | Empty workspace ready       |
| kubectl get ns roboshop -o yaml \| grep labels | environment: development      | Your labels preserved       |


# Deploy & Test Commands

# Deploy
kubectl apply -f basics/01-namespace.yaml

# Verify
kubectl get ns roboshop                    # Basic status
kubectl get ns -o wide                     # All namespaces
kubectl get pods -n roboshop               # Empty = Ready!
kubectl describe ns roboshop               # Detailed info


# Key Learning Points

| Concept         | What You Learned                     |
| --------------- | ------------------------------------ |
| Namespace       | Virtual cluster isolation (like VPC) |
| kind: Namespace | Creates project workspace            |
| Labels          | Tags for organization + discovery    |
| kubectl apply   | Declarative deployment (GitOps)      |
| -n roboshop     | Target specific namespace            |

# Real-World Use

#Dev team → roboshop-dev namespace
#Prod team → roboshop-prod namespace
#Finance app → finance namespace
#All isolated, no naming conflicts


# Success Criteria

 kubectl get ns shows roboshop Active

 Labels visible: environment: development

 kubectl get pods -n roboshop empty

 No errors in kubectl describe ns roboshop

Status: ✅ LIVE