# 🚀 Spring Boot App — CI/CD complet vers AWS EC2

Application Java Spring Boot containerisée avec Docker et déployée 
automatiquement sur AWS EC2 via un pipeline CI/CD GitHub Actions.

## 📐 Architecture

```
GitHub Push (main)
│
▼
┌─────────────────────┐
│  Job 1 : CI         │
│  - Build Maven      │
│  - Build image Docker│
│  - Push → AWS ECR   │
└─────────────────────┘
│
▼ (si succès)
┌─────────────────────┐
│  Job 2 : CD         │
│  - SSH → EC2        │
│  - docker compose   │
│    pull + up        │
└─────────────────────┘
│
▼
App live sur EC2
```
## 🛠️ Stack technique

| Technologie | Rôle |
|---|---|
| **Java 21 / Spring Boot** | Framework backend |
| **Maven** | Build et gestion des dépendances |
| **Docker** | Containerisation (multi-stage build) |
| **Docker Compose** | Orchestration sur EC2 |
| **GitHub Actions** | Pipeline CI/CD |
| **Amazon ECR** | Registre d'images Docker |
| **AWS EC2** | Hébergement de l'application |

## 🐳 Lancer en local avec Docker

```bash
# Cloner le repo
git clone https://github.com/TheGeraud/Spring-boot-docker.git
cd Spring-boot-docker

# Build et lancer
docker compose up --build

# L'app est accessible sur http://localhost:80
```

## ⚙️ Lancer sans Docker

```bash
# Compiler et lancer avec Maven
./mvnw spring-boot:run
```

## 🔄 Pipeline CI/CD

Le pipeline se déclenche automatiquement à chaque push sur `main` 
et s'exécute en deux jobs distincts :

**Job 1 — Build & Push (CI)**
1. Compilation du projet avec Maven
2. Build de l'image Docker (multi-stage : Maven → JRE)
3. Push de l'image sur Amazon ECR

**Job 2 — Deploy (CD)**
- Se déclenche uniquement si le Job 1 réussit
- Connexion SSH à l'instance EC2
- Pull de la nouvelle image depuis ECR
- Redémarrage du container avec `docker compose up --force-recreate`

> Les credentials AWS et la clé SSH sont stockés dans les 
**GitHub Secrets** — aucune donnée sensible dans le code.

## 📁 Structure du projet
```
Spring-boot-docker/
├── .github/
│   └── workflows/        # Pipeline CI/CD GitHub Actions
├── src/                  # Code source Java Spring Boot
│   └── main/java/        # Controllers, Services, Models
├── Dockerfile            # Build multi-stage (Maven → JRE)
├── docker-compose.yml    # Orchestration
└── pom.xml               # Dépendances Maven
```

## 💡 Ce que j'ai appris

- Mettre en place un pipeline **CI/CD complet** avec deux jobs séparés
- Automatiser le **déploiement SSH sur EC2** via GitHub Actions
- Utiliser les **environnements GitHub** pour isoler les secrets
- Configurer un **build multi-stage Java** avec Maven et JRE minimal
- Comprendre la différence entre **CI** (build/push) et **CD** (déploiement)

## 🔧 Améliorations futures

- [ ] Supprimer l'étape de debug SSH des logs
- [ ] Ajouter des tests unitaires dans le pipeline
- [ ] Mettre en place un healthcheck après déploiement
- [ ] Ajouter des notifications Slack en cas d'échec

## 📄 Licence

MIT License — © 2026 TheGeraud
