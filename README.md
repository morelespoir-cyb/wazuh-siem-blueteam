# 🛡️ Projet SIEM BlueTeam — Wazuh

> Infrastructure SIEM complète basée sur **Wazuh**, déployée en posture analyste SOC : collecte de logs, détection, corrélation d'événements et analyse de scénarios d'attaque — Projet ESGI Mastère Cybersécurité , 2025-2026.

![Status](https://img.shields.io/badge/Status-En_cours-orange?style=flat-square)
![Wazuh](https://img.shields.io/badge/Wazuh-4.x-blue?style=flat-square&logo=wazuh)
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
