📊 Déploiement d’une Infrastructure de Supervision Zabbix sur AWS










📖 Présentation du Projet

Ce projet consiste en la mise en place d’une infrastructure de supervision centralisée basée sur Zabbix, déployée sur le cloud Amazon Web Services (AWS) et entièrement conteneurisée à l’aide de Docker.

L’objectif principal est de surveiller de manière proactive les performances, la disponibilité et l’état de santé d’un environnement hétérogène, composé de serveurs Linux (Ubuntu) et Windows Server, tout en exploitant une architecture cloud sécurisée, scalable et facilement reproductible.

Ce dépôt accompagne un projet académique de supervision réseau et cloud, et fournit l’ensemble des fichiers de configuration nécessaires pour reproduire l’infrastructure, comprendre son architecture et analyser les résultats de supervision.

🎯 Objectifs du Projet

Mettre en place une supervision centralisée des infrastructures Linux et Windows

Déployer Zabbix sous forme de conteneurs Docker pour plus de portabilité

Exploiter les services AWS (EC2, VPC, Security Groups)

Automatiser le déploiement de la stack de monitoring

Visualiser en temps réel les métriques système (CPU, RAM, disque, réseau)

📑 Table des Matières

Architecture Réseau & Flux

Mise en place de l’Infrastructure AWS

Installation du Serveur Zabbix (Docker)

Installation et Configuration des Agents

Supervision & Résultats

Contenu du Dépôt

🏗 Architecture Réseau & Flux

L’architecture repose sur un VPC AWS dédié, garantissant l’isolation et la sécurité des ressources.
Le serveur Zabbix est hébergé sur une instance EC2 Ubuntu et exécuté via Docker Compose.

Schéma Logique
graph TD
    User((Administrateur)) -- HTTP:80 --> Web[Zabbix Web]
    AgentLin((Client Linux)) -- TCP:10051 --> Server[Zabbix Server]
    AgentWin((Client Windows)) -- TCP:10051 --> Server
    
    subgraph "AWS Cloud - VPC Zabbix"
        subgraph "Docker Host (EC2)"
            Web -- Port 10051 --> Server
            Web -- Port 3306 --> DB[(MySQL DB)]
            Server -- Port 3306 --> DB
        end
    end

Composants Principaux

Zabbix Server : Cœur de la supervision et collecte des métriques

Base de données MySQL 8.0 : Stockage des données de monitoring

Zabbix Web (Nginx) : Interface web de visualisation

Agents Zabbix : Installés sur les hôtes Linux et Windows

☁️ Mise en place de l’Infrastructure AWS
1. Choix de la Région

Sélection d’une région AWS (ex : us-east-1) afin d’optimiser la latence et la disponibilité.

2. Configuration Réseau (VPC)

VPC : 10.0.0.0/16

Subnet public pour l’accès Internet

Internet Gateway pour la connectivité externe

3. Sécurité (Security Groups)

Ports autorisés :

22 (SSH) : Administration du serveur

80 (HTTP) : Accès à l’interface Zabbix

10051 (Zabbix) : Communication agents ↔ serveur

4. Instances EC2
Rôle	Système	Type	Description
Zabbix Server	Ubuntu 22.04	t3.large	Docker + Stack Zabbix
Client Linux	Ubuntu 22.04	t3.medium	Machine supervisée
Client Windows	Windows Server 2022	t3.large	Machine supervisée
🚀 Installation du Serveur Zabbix (Docker)
Prérequis

Accès SSH à l’instance EC2

Droits sudo

Installation de Docker
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker ubuntu

Déploiement de la Stack Zabbix
docker-compose up -d

Accès à l’Interface Web

URL : http://<IP_PUBLIQUE>

Identifiants par défaut : Admin / zabbix

🔧 Installation et Configuration des Agents
Agent Linux (Ubuntu)
sudo apt install -y zabbix-agent
sudo nano /etc/zabbix/zabbix_agentd.conf
# Server=<IP_SERVEUR_ZABBIX>
sudo systemctl restart zabbix-agent

Agent Windows

Installation via package MSI Zabbix Agent

Configuration de l’adresse IP du serveur Zabbix

Ouverture du port 10051 dans le pare-feu Windows

📈 Supervision & Résultats

Une fois les agents configurés :

Les métriques système sont remontées vers le serveur Zabbix

Le statut ZBX apparaît en vert dans l’interface

Les graphiques sont générés automatiquement pour l’analyse des performances

📂 Contenu du Dépôt

docker-compose.yml : Définition complète de la stack Zabbix (Server, Web, Database MySQL)

architecture_zabbix_aws.drawio : Schéma détaillé de l’architecture réseau et cloud

img/ : Captures d’écran de l’interface Zabbix et des résultats

.env : Variables d’environnement Docker (sans informations sensibles)

README.md : Documentation complète du projet

👤 Auteur

Hamada Faris
Étudiant ingénieur en génie informatique & intelligence artificielle 👨🏻‍💻
Projet académique : Supervision Réseau & Cloud sur AWS
Technologies : Zabbix | Docker | AWS | Linux | Windows Server
