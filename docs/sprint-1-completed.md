# Sprint 1 — Infrastructure ✅

**Date de fin :** 1er juillet 2026

## 🎯 Réalisations

- ✅ VM `Wazuh-Server-Morel` : Ubuntu 22.04.5 LTS, 6 Go RAM, IP fixe `192.168.56.10`
- ✅ VM `Wazuh-Agent-Morel` : Ubuntu 22.04.5 LTS, 2 Go RAM, IP fixe `192.168.56.20`
- ✅ Réseau interne `wazuh-net` avec communication bidirectionnelle validée
- ✅ Docker single-node : Indexer + Manager + Dashboard v4.13.0
- ✅ Certificats SSL auto-signés générés via `wazuh-certs-generator`
- ✅ Agent Wazuh 4.13.0 installé et enrollé au Manager (version pinnée)
- ✅ Dashboard accessible sur `https://127.0.0.1:8443`
- ✅ Premiers événements SIEM ingérés (**222 alertes en 24h**)

## 🖼️ Preuves visuelles

Les captures d'écran sont dans `docs/captures/sprint-1/` :

- `01-dashboard-overview.png` — Vue d'ensemble Dashboard : 1 agent Actif et 222 alertes réparties (108 sévérité moyenne, 114 sévérité faible)
- `02-events-timeline.png` — Chasse à la menace : timeline des événements sur 24h avec pic d'activité lors de l'enrollment de l'agent
- `03-events-details.png` — Table détaillée des événements CIS SCA remontés par l'agent (règles 19004, 19007, 19008, 19009)

## ⚠️ Point d'attention technique — Incompatibilité version agent/manager

L'agent Wazuh a d'abord été installé en dernière version disponible du repo,
provoquant l'erreur suivante côté Manager :
Résolu par un pinning explicite de version :

```bash
sudo apt install wazuh-agent=4.13.0-1
sudo apt-mark hold wazuh-agent
```

Cette contrainte de compatibilité (agent ≤ manager) est un contrôle natif de
`wazuh-authd` pour éviter que des agents plus récents n'envoient des données
incompatibles avec le Manager. Le `apt-mark hold` verrouille la version pour
éviter tout upgrade non maîtrisé lors des futurs `apt upgrade`.

## 🧪 Test de validation en direct

Après enrollment, une capture de la vue "Chasse à la menace" (voir
`02-events-timeline.png`) montre **222 événements réels** en 24h, ingérés
sans intervention manuelle :

- Règles CIS Ubuntu 22.04 LTS Benchmark v2.0.0 (Security Configuration Assessment)
- Niveaux de sévérité 3 et 7 (règles 19004, 19007, 19008, 19009)
- Agent `Wazuh-agent-Morel` confirmé comme source

Le pipeline SIEM est validé : **Agent → Manager → Indexer → Dashboard**.

## 📅 Prochaine étape

Sprint 2 (J3-J5) : Configuration des sources de logs `auditd`, développement
des 5 use cases améliorés et rédaction des règles Wazuh custom.
