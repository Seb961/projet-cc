# 🧩 Déploiement Kubernetes — Example Voting App

Bienvenue dans ce projet de mise en œuvre de l’application open-source [Example Voting App](https://github.com/dockersamples/example-voting-app) sur Kubernetes. Ce guide vous accompagne pas à pas, avec des captures d’écran pour visualiser chaque étape clé du déploiement.

---

## 🎯 Objectif du Projet

Déployer une application composée de plusieurs microservices, comprendre son architecture, et tester son fonctionnement en temps réel dans un cluster Kubernetes.

---

## ⚙️ Prérequis

- Cluster Kubernetes opérationnel
- `kubectl` installé et configuré
- Git

---

## 🧭 Architecture de l’Application

L’application est structurée en microservices :

- Une interface utilisateur permet à l’utilisateur de voter
- Redis stocke les votes de manière temporaire
- Un **worker** traite ces votes et les enregistre dans PostgreSQL
- Une interface de résultats affiche l’évolution des votes

![Architecture de l'application](https://github.com/user-attachments/assets/10bd340a-14c0-4ef8-ac70-34845b521a85)

---

## 🚀 Clonage du Dépôt

```bash
git clone https://github.com/Seb961/projet-cc.git
cd example-voting-app/k8s-specifications
```

---

## 📦 Déploiement Kubernetes

1. **Création du namespace**

```bash
kubectl create namespace voting-app
```

2. **Déploiement de Redis et PostgreSQL**

```bash
kubectl apply -f redis-deployment.yaml -n voting-app
kubectl apply -f redis-service.yaml -n voting-app
kubectl apply -f db-deployment.yaml -n voting-app
kubectl apply -f db-service.yaml -n voting-app
```

3. **Déploiement du worker**

```bash
kubectl apply -f worker-deployment.yaml -n voting-app
```

4. **Déploiement des interfaces utilisateur**

```bash
kubectl apply -f vote-deployment.yaml -n voting-app
kubectl apply -f vote-service.yaml -n voting-app
kubectl apply -f result-deployment.yaml -n voting-app
kubectl apply -f result-service.yaml -n voting-app
```

---

## ✅ Vérification du Déploiement

### 📌 Vérifier les pods

```bash
kubectl get pod -n voting-app
```
![Pods](https://github.com/user-attachments/assets/63f1c379-00d1-4874-9615-2c8f2ba16b1a)

---

### 📌 Vérifier les déploiements

```bash
kubectl get deployment -n voting-app
```
![Deployments](https://github.com/user-attachments/assets/27f6f257-4f66-4ad3-9741-004c92d76938)

---

### 📌 Vérifier les ReplicaSets

```bash
kubectl get rs -n voting-app
```
![ReplicaSets](https://github.com/user-attachments/assets/0671bba9-8730-4e18-998b-27c328f12746)

---

### 📌 Vérifier les services

```bash
kubectl get svc -n voting-app
```
![Services](https://github.com/user-attachments/assets/e13393d4-cb4a-4cac-b678-ce6528274eaa)

---

## 🧾 Vue d’ensemble des ressources

```bash
kubectl get all -n voting-app
```
![Toutes les ressources](https://github.com/user-attachments/assets/9a286488-1a79-4ca1-b6fb-4cb8fcc5690e)

---

## 🌐 Accéder à l'application

### Interface de vote

Accédez à l’interface de vote via le navigateur à l’adresse suivante :  
http://172.180.0.7:31000  
![Interface de vote](https://github.com/user-attachments/assets/4bb23010-69de-4cea-bc53-a98033051122)

---

### Interface de résultats

Consultez les résultats en temps réel :  
http://172.180.0.29:31001  
![Résultats](https://github.com/user-attachments/assets/38202de4-048a-4865-a573-b3a730f67367)

---

## ✅ Conclusion

Votre application de vote est maintenant pleinement fonctionnelle et visible via les interfaces déployées. Ce projet illustre l’orchestration de microservices avec Kubernetes et peut être réutilisé comme base pour des applications plus complexes.
