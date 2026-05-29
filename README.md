# 🚀 MySQL StatefulSet with ConfigMap and Secret in Kubernetes

This project demonstrates how to deploy MySQL using a StatefulSet in Kubernetes with:

- Namespace
- Headless Service
- StatefulSet
- Persistent Storage
- ConfigMap
- Secret
- MySQL Database

---

# 📂 Project Structure

```bash
.
├── namespace.yml
├── configmap.yml
├── secret.yml
├── service.yml
├── statefulset.yml
└── README.md
```

---

# 📌 What is StatefulSet?

A StatefulSet is used for stateful applications that require:

- Stable Pod Names
- Persistent Storage
- Ordered Deployment
- Ordered Scaling

Perfect for:
- MySQL
- MongoDB
- Kafka
- Redis

Example Pods:

```bash
mysql-statefulset-0
mysql-statefulset-1
mysql-statefulset-2
```

---

# 📌 What is ConfigMap?

ConfigMap stores non-sensitive configuration data.

Example:
- Database Name
- App Mode
- Port Number

In this project:

```yaml
MYSQL_DATABASE: devops
```

---

# 📌 What is Secret?

Secret stores sensitive data securely.

Example:
- Passwords
- API Keys
- Tokens

In this project:

```yaml
MYSQL_ROOT_PASSWORD: cm9vdA==
```

(Base64 encoded value of `root`)

---

# ⚙️ Step 1: Create Namespace

Apply namespace:

```bash
kubectl apply -f namespace.yml
```

Check namespace:

```bash
kubectl get ns
```

---

# 🌐 Step 2: Create ConfigMap

Apply ConfigMap:

```bash
kubectl apply -f configmap.yml
```

Check ConfigMap:

```bash
kubectl get cm -n mysql
```

Describe ConfigMap:

```bash
kubectl describe cm mysql-config-map -n mysql
```

---

# 🔐 Step 3: Create Secret

Apply Secret:

```bash
kubectl apply -f secret.yml
```

Check Secret:

```bash
kubectl get secret -n mysql
```

Describe Secret:

```bash
kubectl describe secret mysql-secret -n mysql
```

---

# 🌍 Step 4: Create Headless Service

Apply Service:

```bash
kubectl apply -f service.yml
```

Check Service:

```bash
kubectl get svc -n mysql
```

This Headless Service provides stable DNS for StatefulSet Pods.

---

# 🗄️ Step 5: Deploy StatefulSet

Apply StatefulSet:

```bash
kubectl apply -f statefulset.yml
```

Check StatefulSet:

```bash
kubectl get sts -n mysql
```

---

# 📦 Step 6: Check Pods

```bash
kubectl get pods -n mysql
```

Expected Output:

```bash
NAME                    READY   STATUS    RESTARTS   AGE
mysql-statefulset-0     1/1     Running   0          2m
mysql-statefulset-1     1/1     Running   0          2m
mysql-statefulset-2     1/1     Running   0          2m
```

---

# 💾 Step 7: Check Persistent Volume Claims

```bash
kubectl get pvc -n mysql
```

Expected Output:

```bash
NAME                                 STATUS   VOLUME
mysql-data-mysql-statefulset-0       Bound    pvc-xxxx
mysql-data-mysql-statefulset-1       Bound    pvc-yyyy
mysql-data-mysql-statefulset-2       Bound    pvc-zzzz
```

Each Pod gets its own dedicated storage.

---

# 🐚 Step 8: Enter Inside MySQL Pod

```bash
kubectl exec -it mysql-statefulset-0 -n mysql -- bash
```

---

# 🔑 Step 9: Login to MySQL

Inside the container:

```bash
mysql -u root -p
```

Enter Password:

```bash
root
```

OR directly:

```bash
mysql -u root -proot
```

---

# 🧪 Step 10: Verify Database

```sql
SHOW DATABASES;
```

Expected Output:

```sql
+--------------------+
| Database           |
+--------------------+
| devops             |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

---

# 🌍 StatefulSet DNS

Each Pod gets stable DNS.

Examples:

```bash
mysql-statefulset-0.mysql-service.mysql.svc.cluster.local
mysql-statefulset-1.mysql-service.mysql.svc.cluster.local
mysql-statefulset-2.mysql-service.mysql.svc.cluster.local
```

---

# 🔄 How ConfigMap Works

ConfigMap injects the database name into the container.

Inside StatefulSet:

```yaml
- name: MYSQL_DATABASE
  valueFrom:
    configMapKeyRef:
      name: mysql-config-map
      key: MYSQL_DATABASE
```

---

# 🔐 How Secret Works

Secret injects the root password securely.

Inside StatefulSet:

```yaml
- name: MYSQL_ROOT_PASSWORD
  valueFrom:
    secretKeyRef:
      name: mysql-secret
      key: MYSQL_ROOT_PASSWORD
```

---

# 🔑 Encode Secret Password

Convert password into Base64:

```bash
echo -n "root" | base64
```

Output:

```bash
cm9vdA==
```

---

# 🛠️ Useful Commands

## Check All Resources

```bash
kubectl get all -n mysql
```

---

## Describe StatefulSet

```bash
kubectl describe sts mysql-statefulset -n mysql
```

---

## Check Logs

```bash
kubectl logs mysql-statefulset-0 -n mysql
```

---

## Scale StatefulSet

```bash
kubectl scale sts mysql-statefulset --replicas=5 -n mysql
```

---

## Delete StatefulSet

```bash
kubectl delete -f statefulset.yml
```

---

## Delete Service

```bash
kubectl delete -f service.yml
```

---

## Delete ConfigMap

```bash
kubectl delete -f configmap.yml
```

---

## Delete Secret

```bash
kubectl delete -f secret.yml
```

---

## Delete Namespace

```bash
kubectl delete -f namespace.yml
```

---

# 🎯 Learning Outcome

By completing this project you will understand:

- Kubernetes StatefulSet
- Headless Service
- Persistent Storage
- Persistent Volume Claim (PVC)
- ConfigMap
- Secret
- Stable Pod Identity
- MySQL Deployment in Kubernetes

---

# 😂 Fun Fact

Deployment Pods:
> “Who am I today?”

StatefulSet Pods:
> “I am mysql-statefulset-0 and I have responsibilities.” 😎

ConfigMap:
> “Here are your settings.”

Secret:
> “And here’s the password... don’t tell anyone.” 🤫

---

# 👨‍💻 Author

Built with Kubernetes + MySQL + StatefulSet 🚀
