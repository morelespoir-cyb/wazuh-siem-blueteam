# Architecture du projet SIEM Wazuh

## Vue d'ensemble

Infrastructure de type **single-node** déployée via **Docker** sur Ubuntu 22.04 LTS, hébergée sous VirtualBox.

## Schéma fonctionnel

L'architecture se compose de deux VMs Ubuntu Server 22.04 LTS hébergées sous VirtualBox, reliées par un réseau interne `wazuh-net` :

- **VM Wazuh-Server-Morel** (192.168.56.10/24) : héberge la stack Docker single-node de Wazuh
- **VM Wazuh-Agent-Morel** (192.168.56.20/24) : machine cible des scénarios d'attaque, équipée de l'agent Wazuh, d'auditd et de rsyslog

Voir le schéma complet dans le README à la racine du dépôt.

## Composants déployés

### VM Wazuh-Server-Morel

| Composant | Version | Port | Rôle |
|-----------|---------|------|------|
| Wazuh Indexer | 4.13.0 | 9200 | Stockage/recherche des événements (OpenSearch) |
| Wazuh Manager | 4.13.0 | 1514, 1515, 514, 55000 | Réception des logs, règles, corrélation |
| Wazuh Dashboard | 4.13.0 | 5601 → 443 | Interface web de supervision |

### VM Wazuh-Agent-Morel *(à déployer Sprint 1 final)*

| Composant | Rôle |
|-----------|------|
| Wazuh Agent 4.13.0 | Collecte des logs et envoi au Manager |
| auditd | Audit des appels système et commandes sensibles |
| rsyslog | Centralisation des logs système |

## Redirections de ports VirtualBox (NAT depuis l'hôte Windows)

| Service | Port hôte | Port invité | Usage |
|---------|-----------|-------------|-------|
| SSH | 2222 | 22 | Administration SSH depuis Windows Terminal |
| Wazuh Dashboard | 8443 | 443 | Accès à l'interface web depuis le navigateur Windows |

## Choix techniques

- **Single-node** (et non multi-node) : adapté à un environnement de TP avec 6 Go de RAM alloués à la VM Server
- **Docker** plutôt qu'installation native : déploiement reproductible et isolation
- **Certificats SSL auto-signés** générés via `wazuh-certs-generator` pour chiffrer les communications inter-composants
- **vm.max_map_count=262144** : prérequis OpenSearch pour la gestion mémoire mappée
- **Ubuntu Server 22.04 LTS** : version officiellement supportée par Wazuh 4.13.x

## Sources de logs prévues (VM Agent)

- `/var/log/auth.log` (authentification SSH)
- `/var/log/syslog` (logs système)
- `auditd` (commandes sensibles, modifications de `/etc/passwd`, `/etc/sudoers`)

## Auteurs

- **Morel CAPO-CHICHI** *(pilote technique, dépôt GitHub)*
- **KONE Oumar** *(binôme)* — [github.com/oumar953](https://github.com/oumar953)

Projet réalisé dans le cadre du Mastère Cybersécurité ESGI Paris, 2025-2026 — Encadrement : **Léo Perochon**.
