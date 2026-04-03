---
title: "Protocoles"
date: 2026-04-03T03:33:13+02:00
draft: false
showToc: true
TocOpen: true
---

Cette cheatsheet regroupe les vecteurs d'attaque et commandes d'énumération pour les services les plus courants rencontrés en CTF et en environnement réel.

---

## FTP (21) - File Transfer Protocol
Protocole de transfert de fichiers non chiffré. Très courant pour l'exfiltration ou la récupération de configurations.

### Actions Prioritaires
* **Connexion Anonyme** : Tester si l'utilisateur `anonymous` avec un mot de passe vide (ou `anonymous`) est accepté.
* **Exploration** : Lister récursivement tous les fichiers accessibles.

### Commandes Utiles
```bash
# Vérification via Nmap
nmap --script ftp-anon,ftp-syst -p 21 <IP>

# Connexion manuelle
ftp <IP>
# Login: anonymous | Password: (vide)

# Lister tout récursivement (une fois connecté)
ls -R
```

---

## SSH (22) - Secure Shell
Accès sécurisé en ligne de commande. Difficile à attaquer directement sauf si des versions obsolètes ou des clés privées sont trouvées.

### Actions Prioritaires
* **Banner Grabbing** : Identifier la version exacte (peut révéler l'OS, ex: `Ubuntu-4ubuntu0.3`).
* **Bruteforce** : À tenter seulement si aucune autre piste n'est disponible.

### Commandes Utiles
```bash
# Identifier la version
nc -vn <IP> 22

# Audit de sécurité des algos (SSH-Audit)
ssh-audit <IP>

# Bruteforce via Hydra
hydra -L users.txt -P passwords.txt ssh://<IP>
```

---

## DNS (53) - Domain Name System
Traduit les noms de domaine en adresses IP. Une mauvaise configuration peut révéler toute l'infrastructure interne.

### Actions Prioritaires
* **Transfert de Zone (AXFR)** : Tenter de récupérer toute la base de données du domaine.
* **Énumération de sous-domaines** : Si l'AXFR échoue.

### Commandes Utiles
```bash
# Tentative de transfert de zone
dig axfr @<DNS_IP> <DOMAIN>

# Énumération simple
host -t ns <DOMAIN>  # Chercher les serveurs de noms
host -t mx <DOMAIN>  # Chercher les serveurs mails
```


---

## SMB (445) - Server Message Block
Partage de fichiers et d'imprimantes Windows. Le vecteur de compromission n°1 en Active Directory.

### Actions Prioritaires
* **Null Session** : Tester l'accès sans identifiant.
* **Partages ouverts** : Chercher des dossiers `Backup`, `Confidential`, ou `Users`.

### Commandes Utiles
```bash
# Énumération complète (Linux)
enum4linux-ng -A <IP>

# Lister les partages (Null Session)
smbclient -L //<IP>/ -N

# Se connecter à un partage spécifique
smbclient //<IP>/<SHARE_NAME> -N

# Énumération avancée via CrackMapExec
crackmapexec smb <IP> --shares -u '' -p ''
```

---

## SNMP (161)

---

## LDAP (389) - Lightweight Directory Access Protocol
Annuaire centralisant les utilisateurs et objets d'un réseau (souvent Active Directory).

### Actions Prioritaires
* **Null Bind** : Énumérer sans authentification.
* **Extraire les utilisateurs** : Récupérer la liste des comptes pour un futur bruteforce.

### Commandes Utiles
```bash
# Énumération du domaine et des objets
ldapsearch -x -h <IP> -s base namingcontexts

# Extraire tous les utilisateurs (si autorisé)
ldapsearch -x -h <IP> -b "dc=EXAMPLE,dc=COM" "(objectClass=user)" sAMAccountName
```

---

## NFS (2049) - Network File System
Partage de fichiers Unix. Souvent moins sécurisé que SMB.

### Actions Prioritaires
* **Showmount** : Lister les répertoires exportés.
* **Mount** : Monter le répertoire localement pour explorer les fichiers.

### Commandes Utiles
```bash
# Lister les partages
showmount -e <IP>

# Monter un partage localement
mount -t nfs <IP>:/<REMOTE_DIR> /mnt/nfs_temp -nolock
```


---

## SMTP (25) - Simple Mail Transfer Protocol
Envoi de mails. Utile pour confirmer l'existence d'utilisateurs sur le système.

### Actions Prioritaires
* **Énumération d'utilisateurs** : Utiliser les commandes `VRFY` ou `RCPT TO`.

### Commandes Utiles
```bash
# Via telnet/netcat
nc -vn <IP> 25
VRFY root
VRFY admin

# Via Nmap
nmap --script smtp-enum-users -p 25 <IP>
```

---

## Bases de Données (MySQL 3306 / MSSQL 1433)
Cibles privilégiées pour l'exfiltration de données ou l'exécution de commandes (xp_cmdshell).

### Actions Prioritaires
* **Identifiants par défaut** : `root:root`, `sa:password`, `postgres:postgres`.
* **Accès distant** : Vérifier si la base accepte les connexions extérieures.

### Commandes Utiles
```bash
# MySQL : Connexion distante
mysql -u root -h <IP> -p

# MSSQL : Énumération via Nmap
nmap --script ms-sql-info,ms-sql-empty-password -p 1433 <IP>
```
