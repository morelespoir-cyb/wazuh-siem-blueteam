# Règles auditd — Projet SIEM Wazuh BlueTeam

## Vue d'ensemble

Ce fichier documente les règles `auditd` déployées sur la VM `Wazuh-Agent-Morel`
pour alimenter les 5 use cases améliorés du Sprint 2.

## Localisation sur l'agent
## Structure des règles

Les règles sont regroupées par use case pour faciliter la traçabilité :

| Groupe | Fichiers/binaires surveillés | Tag (key) | Use case cible |
|--------|------------------------------|-----------|----------------|
| Authentification | `/var/log/auth.log`, `/var/log/faillog`, `/var/log/lastlog` | `authentication`, `faillog`, `lastlog` | UC1, UC2 |
| Identité utilisateurs | `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/gshadow` | `identity_*` | UC3 |
| Privilèges sudoers | `/etc/sudoers`, `/etc/sudoers.d/` | `privilege_escalation_sudoers` | UC3, UC4 |
| Gestion utilisateurs | `useradd`, `userdel`, `usermod`, `groupadd`, `passwd` | `user_management`, `password_change` | UC3 |
| Permissions | `chmod`, `chown` | `perm_change` | UC4 |
| Élévation privilèges | `sudo`, `su` | `sudo_usage`, `su_usage` | UC1, UC2 |
| Téléchargement | `wget`, `curl` | `download_tools` | UC5 |
| Activité /tmp | `/tmp`, `/var/tmp` | `tmp_activity` | UC5 |
| Persistence | `/etc/crontab`, `/etc/cron.*` | `cron_change` | Bonus |
| Config SSH | `/etc/ssh/sshd_config`, `/root/.ssh/` | `ssh_config_change`, `ssh_keys_change` | Bonus |

## Application des règles

```bash
sudo augenrules --load
sudo systemctl restart auditd
```

## Vérification

```bash
sudo auditctl -l | wc -l   # nombre de règles chargées
sudo auditctl -s           # état du daemon
```

## Format des événements auditd

Chaque événement génère des logs dans `/var/log/audit/audit.log` du type :
Le champ `key` correspond au tag défini dans les règles et permet le filtrage
côté Wazuh Manager pour déclencher les règles custom du Sprint 2.

## Auteurs

Morel CAPO-CHICHI · KONE Oumar — ESGI Mastère Cybersécurité 2025-2026
