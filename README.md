K8s Single Master Cluster
==================================

## Iniciar Infraestructura

```bash
vagrant up k8s-master
```

- Ver el estado de las máquinas:

```bash
vagrant status
```

```bash
Current machine states:

k8s-master                running (virtualbox)
k8s-node01                not created (virtualbox)
k8s-node02                not created (virtualbox)
```

## Single Master Environment

Modo Single Master Cluster para Kubernetes. Recomendado para pruebas y/o ambientes en redes privadas.

```bash
ansible-playbook -i k8s-inventory k8s.yml --limit 'develop'
```

- Verificar Proceso de Instalación:

```bash
vagrant ssh k8s-master
kubectl get pods --all-namespaces
```

### Dashboard

- Instalación Dashboard

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
# Verificar instalacion
kubectl -n kube-system get service kubernetes-dashboard
```

- Modificación Acceso al Dashboard: ClusterIP por NodePort

```bash
kubectl -n kube-system edit service kubernetes-dashboard
```

- Modificar nodePort por 31500

```bash
kubectl -n kube-system edit service kubernetes-dashboard
kubectl -n kube-system get service kubernetes-dashboard
```

- Configuración Firewall

```bash
firewall-cmd --permanent --add-port=31500/tcp
firewall-cmd --reload
```

- Credenciales Dashboard

```bash
kubectl -n kube-system get service kubernetes-dashboard

kubectl create serviceaccount cluster-admin-dashboard-sa

kubectl create clusterrolebinding cluster-admin-dashboard-sa \
--clusterrole=cluster-admin \
--serviceaccount=default:cluster-admin-dashboard-sa
```

```bash
kubectl get secret | grep cluster-admin-dashboard-sa
cluster-admin-dashboard-sa-token-kfcdz   kubernetes.io/service-account-token   3      26s
```

```bash 
kubectl describe secret cluster-admin-dashboard-sa-token-kfcdz
Name:         cluster-admin-dashboard-sa-token-kfcdz
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: cluster-admin-dashboard-sa
              kubernetes.io/service-account.uid: abba4b8e-0a16-11e9-bf5c-005056b74f91

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  7 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImNsdXN0ZXItYWRtaW4tZGFzaGJvYXJkLXNhLXRva2VuLWtmY2R6Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImNsdXN0ZXItYWRtaW4tZGFzaGJvYXJkLXNhIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiYWJiYTRiOGUtMGExNi0xMWU5LWJmNWMtMDA1MDU2Yjc0ZjkxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6Y2x1c3Rlci1hZG1pbi1kYXNoYm9hcmQtc2EifQ.LD_Mg69Km9CcseUwvoSplpcvG2cPc-dnZMxVKthO9baCZELvbp2Us0vyRlVGXZEIr4l-ChnO3nvsMFNep7YfAMdJYeyPxQExXOCFH26XixfWVCGtfcruYFEcL1pUVKWLL9zCbyJ6d-29hHsmWFUitKQxuwgxyJTygvoF7WNFahuOqTOnGFG6NO0u-ZHxcoLl2uqBPZtjA2zX1G6Gp7mALplSMwYnwA0YiS0sXSneYFKSi8uf4wztEjQL-ymjSt0NyBl5uveSXwGqlFs_KbFvvXk01sO4BfjLe5a9aXGtqNcyf3-VwZ459E4ZMnsQsDc8OWn1KR7eaTPuVrFjv-uesw
```

- Acceso a Dashboard

```bash 
https://172.0.1.2:30500/#!/login
```

# Referencias

- https://docs.giantswarm.io/guides/install-kubernetes-dashboard/
- https://medium.com/@wso2tech/multi-node-kubernetes-cluster-with-vagrant-virtualbox-and-kubeadm-9d3eaac28b98
