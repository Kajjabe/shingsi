---
title: "Adminsys"
date: 2026-07-11T05:46:55+02:00
draft: false
showToc: true
TocOpen: true
---

## 1. Méthodologie Générale de Réflexion (Troubleshooting)

Face à un problème en entretien (ex: "Le site web ne répond plus"), TOUJOURS suivre cette logique à voix haute :

1. Isoler le problème : Est-ce global ? Est-ce un seul utilisateur ? Est-ce que le ping répond ?
2. Vérifier les 4 piliers matériels : CPU, Mémoire, Stockage (Disque plein = 80% des pannes), Entrées/Sorties.
3. Vérifier les Services & Logs : Le processus tourne-t-il ? Que disent les fichiers de logs ?
4. Vérifier le Réseau : Le port est-il ouvert ? Le pare-feu bloque-t-il ? La résolution DNS fonctionne-t-elle ?

---

## 2. Analyse Système (CPU, RAM, Disque)

### Diagnostic de premier niveau
Commandes pour comprendre instantanément l'état de santé d'un serveur.

- ```htop``` : Vue globale interactive (Processus, CPU, RAM)
- ```df -h``` : Vérifier l'espace disque disponible (Le -h rend la lecture humaine : Go, Mo)
- ```free -m``` : Vérifier l'utilisation de la mémoire RAM en temps réel (en Mo)
- ```uptime``` : Voir la charge système (Load Average) et l'uptime du serveur

Question piège d'entretien : "Mon df -h dit qu'il reste de l'espace, mais je ne peux plus créer de fichiers. Pourquoi ?"
Réponse : Saturation des Inodes (la table d'index des fichiers). On vérifie avec df -i.

### Trouver ce qui sature le disque
Commande pour trouver les 10 plus gros dossiers/fichiers à partir de la racine :
```bash
du -ah / | sort -rh | head -n 10
```
---

## 3. Gestion des Services & Logs

### Statut et contrôle
-  Vérifier l'état d'un service (ex: nginx)
```bash
sudo systemctl status nginx
```
-  Redémarrer un service après modification de conf
```bash 
sudo systemctl restart nginx
```
### Inspection des logs (La clé de la vérité)
- Lire les logs d'un service géré par Systemd en temps réel
```bash
 sudo journalctl -u nginx -f
```
-  Regarder les erreurs systèmes générales en direct
```bash
sudo tail -f /var/log/syslog
```
---

## 4. Analyse Réseau & Ports

### Identification des blocages
- sudo ss -tulpn : Lister tous les ports en écoute avec les processus associés (Indispensable !)
- nc -zv IP PORT : Tester si un port spécifique est ouvert sur un serveur distant
- sudo iptables -L -n -v : Vérifier les règles de filtrage du pare-feu actif (ou utiliser 'sudo ufw status')

---

## 5. Tableau de Synthese : Commandes de Survie

| Commande | Utilité en Entretien / Diagnostic | Option clé |
| :--- | :--- | :--- |
| top / htop | Identifier un processus zombie ou un CPU à 100% | N/A |
| df | Vérifier si le disque est plein (/var/log souvent saturé) | -h (Human readable) |
| du | Trouver précisément quel fichier/dossier prend toute la place | -sh * (Résumé par dossier) |
| ss | Remplaçant moderne de netstat. Voit qui écoute sur quel port | -tulpn (TCP, UDP, Listen, Pid, Numeric) |
| lsof | Trouve quel processus utilise quel fichier ou port | -i :80 (Qui utilise le port 80) |
| ps | Lister les processus en cours | aux (Tous les détails) |
| kill | Arrêter un processus proprement ou de force | -9 PID (Force brute) |
| dig / nslookup | Diagnostiquer un problème de résolution de nom (DNS) | domaine |

---

## 6. Scénario Type d'Entretien : "Le serveur web est lent"

Si le recruteur te lance ce défi, déroule ce script :

1. Je me connecte en SSH et je lance uptime et htop pour checker le Load Average.
2. Si le CPU/RAM est au max : Je cherche le processus coupable avec 'ps aux --sort=-%cpu'.
3. Si rien côté CPU/RAM : Je fais un 'df -h' pour voir si les disques sont saturés (ce qui bloque les écritures de sessions ou de bases de données).
4. Si les disques sont OK : Je regarde les logs en direct avec 'tail -f /var/log/nginx/error.log' pour voir s'il y a une attaque ou des erreurs applicatives.
5. Si les logs sont calmes : Je vérifie les connexions réseau actives avec 'ss -s' pour voir s'il n'y a pas une saturation du nombre de connexions TCP.

