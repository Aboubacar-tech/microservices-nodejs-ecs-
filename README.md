#  Décomposition d’une application Node.js monolithique en microservices

Ce projet est le résultat d’un travail de refactorisation complet d’une application de forum, passant d’une architecture monolithique traditionnelle à des **microservices conteneurisés**, orchestrés sur **AWS ECS**.

> 🧠 *Ce projet représente une montée en compétences concrète sur :*  
> *Docker, Amazon ECR, Amazon ECS, routage ALB, découpage d’API REST, et scalabilité indépendante.*

---

## 🧱 Architecture du projet

![Architecture](/architecture.png)

> *(schéma généré ou extrait du livrable — montre la séparation entre Users, Threads, Posts, et l’équilibreur de charge)*

L’application initiale (monolithe Node.js) a été :
1. Conteneurisée avec Docker
2. Déployée sur Amazon ECS (mode EC2)
3. Refactorisée en **3 microservices indépendants**

---

## 📦 Microservices créés

| Microservice | Endpoints REST associés |
|--------------|--------------------------|
| **users**    | `/api/users/*`           |
| **threads**  | `/api/threads/*`         |
| **posts**    | `/api/posts/*`           |

Chaque service :
- Possède son propre dépôt **Amazon ECR**
- Est déployé via une **task definition ECS dédiée**
- Expose son API sur le port `3000` en interne

---

##  Ce que j’ai appris et mis en pratique

### ✅ Conteneurisation
- Écrire un `Dockerfile` optimisé pour Node.js
- Supprimer le clustering inutile (`cluster` → mono-processus)
- Construire et taguer des images Docker

### ✅ AWS ECR & ECS
- Créer des dépôts privés sur Amazon ECR
- Pousser / pull des images Docker vers AWS
- Définir des **task definitions** (CPU, mémoire, mapping ports)
- Déployer des **services ECS** avec haute disponibilité

###  Réseau & équilibrage de charge
- Utiliser un **Application Load Balancer (ALB)**
- Créer des **target groups** par microservice
- Configurer le **routage basé sur le chemin** (`/api/users*`, etc.)

###  Refactorisation d’API REST
- Passer d’un seul `server.js` de 80+ lignes à **3 fichiers spécialisés**
- Maintenir la compatibilité API sans casser les clients

---
