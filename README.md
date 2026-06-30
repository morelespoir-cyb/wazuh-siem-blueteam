# 🛡️ Projet SIEM BlueTeam — Wazuh

> Infrastructure SIEM complète basée sur **Wazuh**, déployée en posture analyste SOC : collecte de logs, détection, corrélation d'événements et analyse de scénarios d'attaque — Projet ESGI Mastère Cybersécurité, 2025-2026.

![Status](https://img.shields.io/badge/Status-En_cours-orange?style=flat-square)
![Wazuh](https://img.shields.io/badge/Wazuh-4.13.0-blue?style=flat-square&logo=wazuh)
![Docker](https://img.shields.io/badge/Docker-Single--Node-2496ED?style=flat-square&logo=docker)
![ESGI](https://img.shields.io/badge/ESGI-Mastère_Cybersécurité-blue?style=flat-square)

---

## 🎯 Contexte

Mise en situation d'un analyste SOC chargé de déployer une solution de supervision de sécurité. Le projet va au-delà d'une simple configuration technique : il s'agit de construire une **logique d'analyse SIEM réaliste**, capable de relier des événements entre eux et de produire des analyses exploitables.

## 🧭 Périmètre du projet

- ✅ Déploiement Wazuh Server (Docker single-node)
- ✅ Déploiement d'un agent Linux
- ✅ Exploitation des logs `syslog` et `auditd`
- ✅ 5 use cases améliorés (au-delà des TPs existants)
- ✅ 2 corrélations d'événements (enchaînements logiques)
- ✅ 1 règle avancée à conditions multiples
- ✅ 1 règle basée sur la fréquence
- ✅ Whitelisting justifié pour réduction des faux positifs
- ✅ 3 dashboards exploitables
- ✅ Analyse complète d'un scénario d'attaque
- 🎯 Bonus : tagging structuré + mapping MITRE ATT&CK

## 🏗️ Architecture

┌────────────────────────────────────────────────────────────┐
│   Hyperviseur VirtualBox (Hôte Windows)                    │
│                                                             │
│  ┌──────────────────────────┐   ┌──────────────────────┐   │
│  │  VM Wazuh-Server-Morel   │   │  VM Wazuh-Agent-Morel│   │
│  │  Ubuntu 22.04.5 LTS      │   │  Ubuntu 22.04.5 LTS  │   │
│  │  6 Go RAM / 2 vCPU       │   │  2 Go RAM / 1 vCPU   │   │
│  │  192.168.56.10/24        │◄──┤  192.168.56.20/24    │   │
│  │                          │   │                       │   │
│  │  Docker single-node:     │   │  Wazuh Agent 4.13.0  │   │
│  │   - Wazuh Indexer        │   │   + auditd           │   │
│  │   - Wazuh Manager        │   │   + rsyslog          │   │
│  │   - Wazuh Dashboard      │   │                       │   │
│  └──────────────────────────┘   └──────────────────────┘   │
│         Réseau interne VirtualBox `wazuh-net`              │
└────────────────────────────────────────────────────────────┘

Documentation complète dans `infrastructure/architecture.md`.

## 📂 Structure du dépôt

```
.
├── infrastructure/        # Docker compose, scripts, doc archi
├── rules/                 # Règles Wazuh custom (local_rules.xml)
├── audit-rules/           # Configuration auditd
├── correlations/          # Documentation des corrélations
├── dashboards/            # Exports + captures dashboards
├── scenarios/             # Scénarios d'attaque + analyses
├── docs/                  # Documentation transverse + captures
├── wazuh-docker/          # Code officiel Wazuh-Docker (single-node)
└── rapport/               # Rapport final PDF
```

## 🚀 Suivi d'avancement

- [x] J1 — Création du dépôt et structure
- [x] J1-J2 — Déploiement infrastructure Wazuh ✅
- [ ] J3-J5 — Configuration logs et 5 use cases améliorés
- [ ] J6-J7 — Corrélations, règles avancées et whitelisting
- [ ] J8 — Dashboards et scénario d'attaque
- [ ] J9-J10 — Rapport final

## 🛠️ Stack technique

`Wazuh 4.13.0` `Docker` `Ubuntu 22.04` `auditd` `rsyslog` `MITRE ATT&CK` `VirtualBox`

## ⚖️ Cadre

Projet pédagogique réalisé dans le cadre du Mastère Cybersécurité ESGI Paris. Les scénarios d'attaque sont exécutés en environnement isolé (réseau interne VirtualBox), aucune cible externe n'est concernée.

## 👥 Équipe

- **Morel CAPO-CHICHI** — pilote technique, dépôt GitHub  
  📧 morelespoir03@gmail.com · 🔗 [github.com/morelespoir-cyb](https://github.com/morelespoir-cyb)

- **KONE Oumar** — binôme  
  🔗 [github.com/oumar953](https://github.com/oumar953)

Projet réalisé dans le cadre du Mastère Cybersécurité ESGI Paris, 2025-2026 — Encadrement : **Léo Perochon**.
