# Projet Kubernetes - Déploiement de l'application Example Voting App
 
## Objectif
 
Mettre en place l’application open-source [Example Voting App](https://github.com/dockersamples/example-voting-app) dans un cluster Kubernetes, comprendre son architecture et tester le cycle de vote en temps réel.
 
---
 
## Prérequis
 
- Un cluster fonctionnel
- `kubectl` installé et configuré
- Git
 
---

# Infrastructure mise en place

![architecture](https://github.com/user-attachments/assets/10bd340a-14c0-4ef8-ac70-34845b521a85)
 
🧭 Architecture de l’application

L’architecture de l’application repose sur une approche distribuée composée de plusieurs microservices interconnectés. L’utilisateur interagit d’abord avec l’interface web de vote, qui lui permet de choisir entre deux options. Lorsqu’un vote est soumis, il est transmis à Redis, qui agit comme une file d’attente pour assurer un traitement rapide et asynchrone.

Un service worker interroge Redis en continu pour récupérer les nouveaux votes, puis les enregistre dans une base de données PostgreSQL afin de garantir la persistance des données, même en cas de redémarrage des services. Enfin, une seconde interface web interroge cette base de données pour afficher les résultats du vote en temps réel, permettant à l’utilisateur de visualiser l’évolution des votes de façon dynamique et continue.
  
## Clonage du dépôt
 
```bash
git clone https://github.com/Seb961/projet-cc.git
cd example-voting-app/k8s-specifications
```
# Déploiement des ressources Kubernetes

```bash
## Création d'un namespace pour isoler nos ressources
kubectl create namespace voting-app  
  
## Déploiement des services de données
kubectl apply -f redis-deployment.yaml -n voting-app  
kubectl apply -f redis-service.yaml -n voting-app  
kubectl apply -f db-deployment.yaml -n voting-app  
kubectl apply -f db-service.yaml -n voting-app  
 
## Déploiement du worker
kubectl apply -f worker-deployment.yaml -n voting-app
 
## Déploiement des interfaces utilisateur (vote et résultats)
kubectl apply -f vote-deployment.yaml -n voting-app  
kubectl apply -f vote-service.yaml -n voting-app  
kubectl apply -f result-deployment.yaml -n voting-app  
kubectl apply -f result-service.yaml -n voting-app  
```
## Vérification

```bash
kubectl get pod -n voting-app  
```
![get-pod](https://github.com/user-attachments/assets/63f1c379-00d1-4874-9615-2c8f2ba16b1a)

On retrouve bien tous nos pods qui sont en état running.  

```bash
kubectl get deployment -n voting-app  
```
![get-deployment](https://github.com/user-attachments/assets/27f6f257-4f66-4ad3-9741-004c92d76938)
  
On retrouve bien tous nos deployments comme on peut le voir dans la colonne READY.  
  
```bash
kubectl get rs -n voting-app
```
![get-rs](https://github.com/user-attachments/assets/0671bba9-8730-4e18-998b-27c328f12746)

On retrouve bien tous nos replicaSet comme on peut le voir dans la colonne DESIRED. 
  
```bash
kubectl get svc -n voting-app  
```
![get-svc](https://github.com/user-attachments/assets/e13393d4-cb4a-4cac-b678-ce6528274eaa)

On retrouve bien tous nos services avec les ports sur lesquels ont peut les joindre.
  
## Vue d'ensemble des ressources
```bash
kubectl get all -n voting-app  
```
![get-all](https://github.com/user-attachments/assets/9a286488-1a79-4ca1-b6fb-4cb8fcc5690e)

On retrouve bien toutes nos ressources créées et utilisées par notre application !  
  
## Accéder et tester l'aplication
On peut vérifier l'adresse ip du node avec la commande :  
```bash
kubectl describe node  
```  
Et ensuite accéder à l'interface de vote depuis le navigateur : http://http://172.180.0.7:31000
![site](https://github.com/user-attachments/assets/4bb23010-69de-4cea-bc53-a98033051122)
  
  
Accéder à l'interface de resultat depuis le navigateur : http://172.180.0.29:31001  
![resultat](https://github.com/user-attachments/assets/38202de4-048a-4865-a573-b3a730f67367)
  
L'application est fonctionnelle !
