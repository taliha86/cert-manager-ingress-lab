# 🚀 cert-manager + Ingress Lab (Kubernetes Playground)

## 📌 Overview

This project demonstrates how to configure HTTPS in Kubernetes using:

* NGINX Ingress Controller
* cert-manager
* Self-signed TLS certificates

The setup is designed to work in a Kubernetes playground environment without LoadBalancer or real DNS.

---

## 🧱 Architecture

Client → NodePort → Ingress Controller → Service → Pod

TLS is terminated at the Ingress using cert-manager generated certificates.

---

## ⚙️ Components Used

* Kubernetes
* NGINX Ingress Controller
* cert-manager (self-signed issuer)
* Sample application (http-echo)

---

## 🚀 Steps Performed

### 1. Install NGINX Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/baremetal/deploy.yaml
```

---

### 2. Deploy Sample Application

* Created Deployment using `hashicorp/http-echo`
* Exposed via ClusterIP Service

---

### 3. Create Ingress Resource

* Configured host-based routing (`hello.local`)
* Linked service to ingress

---

### 4. Install cert-manager

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml
```

---

### 5. Create Self-Signed Issuer

```yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: selfsigned-issuer
spec:
  selfSigned: {}
```

---

### 6. Enable TLS in Ingress

* Added annotations:

  * cert-manager.io/issuer
* Configured TLS block with secret

---

### 7. Certificate Generation

cert-manager automatically:

* Created Certificate resource
* Generated TLS Secret
* Attached certificate to Ingress

---

### 8. Testing

Since no DNS is available, used:

```bash
curl -k -H "Host: hello.local" https://<NODE-IP>:<NODEPORT>
```

---

## ✅ Outcome

* Successfully configured HTTPS in Kubernetes playground
* Automated TLS using cert-manager
* Verified secure access via curl

---

## ⚠️ Limitations

* Self-signed certificates (not trusted)
* No external DNS
* NodePort used instead of LoadBalancer

---

## 🧠 Key Learnings

* Ingress requires a controller to function
* cert-manager automates certificate lifecycle
* Host-based routing is critical for Ingress
* Playground environments require workarounds

---

## 👨‍💻 Author
Taliha 
