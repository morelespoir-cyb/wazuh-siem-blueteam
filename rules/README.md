# Règles Wazuh custom — Sprint 2 BlueTeam

## Vue d'ensemble

Ce dossier contient les **5 use cases améliorés** implémentés en règles Wazuh
custom, plus les corrélations et règles avancées bonus.

## Fichier principal

- `local_rules.xml` — Toutes les règles custom du projet

## Plan des 5 use cases

| # | Use case | Rule IDs | MITRE ATT&CK | Sévérité |
|---|----------|----------|--------------|----------|
| UC1 | Bruteforce SSH progressif | 100000-100009 | T1110.001 | 5→10→15 |
| UC2 | Connexion root anormale | 100010-100019 | T1078.003 | 12 |
| UC3 | Escalade de privilèges rapide | 100020-100029 | T1548.003 | 15 |
| UC4 | Modification sudoers illégitime | 100030-100039 | T1548.003 | 13 |
| UC5 | Chaîne wget/curl → chmod → exec | 100040-100049 | T1105 + T1059 | 14 |

## Déploiement sur le Manager

Les règles sont montées dans le container Manager via volume Docker :

```bash
docker cp rules/local_rules.xml single-node-wazuh.manager-1:/var/ossec/etc/rules/local_rules.xml
docker exec single-node-wazuh.manager-1 /var/ossec/bin/wazuh-control restart
```

## Vérification

```bash
docker exec single-node-wazuh.manager-1 /var/ossec/bin/wazuh-logtest
# Puis coller un log de test pour valider le match de règle
```

## Auteurs

Morel CAPO-CHICHI · KONE Oumar — ESGI Mastère Cybersécurité 2025-2026
