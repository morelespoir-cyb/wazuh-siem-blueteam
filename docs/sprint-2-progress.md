# Sprint 2 — Progression (au 1er juillet 2026)

## ✅ Blocs complétés

### Bloc A — Configuration auditd sur la VM Agent

- 28 règles auditd déployées dans `/etc/audit/rules.d/wazuh-agent-morel.rules`
- Surveillance : authentification, identité (`/etc/passwd`, `/etc/shadow`), sudoers, commandes sensibles, téléchargement, /tmp
- Tags (keys) organisés par use case pour traçabilité

### Bloc B — Configuration Agent Wazuh (ossec.conf)

- 3 sources de logs ajoutées : `/var/log/audit/audit.log`, `/var/log/auth.log`, `/var/log/syslog`
- User `wazuh` ajouté au groupe `adm` (lecture auth.log)
- Backup préalable de la config initiale

### Bloc C — Structure repo

- `audit-rules/` : règles auditd + doc
- `rules/` : local_rules.xml + doc use cases
- `docs/captures/sprint-2/` : préparé pour captures de tests

### Bloc D — 5 use cases améliorés (19 règles custom)

| # | Use case | IDs | MITRE | Règles |
|---|----------|-----|-------|--------|
| UC1 | Bruteforce SSH progressif | 100000-100003 | T1110.001 | 4 (préventif/confirmé/actif/succès post-BF) |
| UC2 | Connexion root anormale | 100010-100012 | T1078.003 | 3 (IP externe/horaires atypiques/su usage) |
| UC3 | Escalade privilèges rapide | 100020-100023 | T1136.001, T1548.003 | 4 (useradd/sudoers/CORRÉLATION/usermod) |
| UC4 | Modification sudoers illégitime | 100030-100032 | T1548.003 | 3 (outil non-visudo/sudoers.d/CORRÉLATION FIM) |
| UC5 | Chaîne wget→chmod→exec | 100040-100044 | T1105, T1059, T1222.002 | 5 (download/chmod/CORRÉLATION 2 étapes/FULL CHAIN/staging) |

**Total : 19 règles custom déployées.**

## 🔧 Difficultés techniques rencontrées

### Erreur 1 : `frequency="1"` refusé

Wazuh impose `frequency >= 2` pour la logique de fréquence.
Résolu en utilisant `<if_matched_sid>` + `<timeframe>` sans `frequency`
pour les corrélations mono-événement.

### Erreur 2 : `<same_agent />` deprecated

Balise obsolète depuis Wazuh 4.13. Retirée du fichier ; le contexte
`<if_matched_sid>` implique déjà l'agent source.

### Erreur 3 : `\S` refusé dans regex Wazuh

Le moteur regex de Wazuh Manager ne supporte pas `\S` en syntaxe standard.
Solution pragmatique : simplification de UC3.4 avec `<match>usermod</match>`
au lieu du regex complexe.

## 📅 Prochaine étape

- **Bloc E** : tests réels sur la VM Agent (Hydra pour UC1, sudo su pour UC2,
  useradd + visudo pour UC3, sed pour UC4, wget + chmod + exec pour UC5)
- **Bloc F** : captures des alertes remontées + commit final
