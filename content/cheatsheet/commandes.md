---
title: "Commandes & raccourcis"
date: 2026-04-03T04:45:20+02:00
draft: false
showToc: true
TocOpen: true

---

## Raccourcis Linux
### Déplacement du curseur

- **[CTRL] + [A]** : Déplacer le curseur au **début** de la ligne actuelle.
- **[CTRL] + [E]** : Déplacer le curseur à la **fin** de la ligne actuelle.
- **[CTRL] + [←] / [→]** : Sauter au début du mot actuel ou précédent.
- **[ALT] + [B] / [F]** : Reculer ou avancer d'un mot complet.

---

### Effacer du texte
- **[CTRL] + [U]** : Effacer tout depuis la position actuelle du curseur jusqu'au **début** de la ligne.
- **[CTRL] + [K]** : Effacer tout depuis la position actuelle du curseur jusqu'à la **fin** de la ligne.
- **[CTRL] + [W]** : Effacer le **mot** précédant la position du curseur.

---

### Gestion du contenu & Processus
- **[CTRL] + [Y]** : Colle le texte ou le mot précédemment effacé avec les raccourcis ci-dessus.
- **[CTRL] + [C]** : Arrête la tâche ou le processus en cours (envoie le signal **SIGINT**). Indispensable pour stopper un scan Nmap ou un outil de brute-force.
- **[CTRL] + [D]** : Ferme le flux **STDIN** (connu sous le nom de *End-of-File* ou EOF). Permet souvent de quitter un shell ou de valider une saisie.
- **[CTRL] + [Z]** : Suspend le processus actuel et le met en arrière-plan (envoie le signal **SIGTSTP**).

---

### Navigation & Historique
- **[CTRL] + [L]** : Nettoie l'écran du terminal. C'est l'équivalent rapide de la commande `clear`.
- **[CTRL] + [R]** : Recherche inversée dans l'historique des commandes. Tape un mot-clé pour retrouver une commande complexe tapée précédemment.
- **[↑] / [↓]** : Naviguer entre la commande précédente ou suivante dans l'historique.

---

## Commandes Linux

| Commande | Description |
| :--- | :--- |
| **man <outil>** | Ouvre les pages de manuel de l'outil spécifié. |
| **<outil> -h** | Affiche la page d'aide de l'outil. |
| **apropos <mot-clé>** | Cherche le mot-clé dans les descriptions des pages de manuel. |
| **cat** | Concatène et affiche le contenu des fichiers. |
| **whoami** | Affiche le nom d'utilisateur actuel. |
| **id** | Retourne l'identité de l'utilisateur (UID, GID). |
| **hostname** | Affiche ou définit le nom d'hôte du système. |
| **uname** | Affiche le nom et les détails du système d'exploitation. |
| **pwd** | Retourne le chemin du répertoire de travail actuel. |
| **ifconfig** | Utilitaire pour configurer ou afficher les interfaces réseau. |
| **ip** | Utilitaire moderne pour manipuler le routage, les interfaces et tunnels. |
| **netstat** | Affiche les statistiques et l'état du réseau. |
| **ss** | Utilitaire pour investiguer les sockets (plus rapide que netstat). |
| **ps** | Affiche l'état des processus en cours. |
| **who** | Affiche qui est actuellement connecté au système. |
| **env** | Affiche les variables d'environnement. |
| **lsblk** | Liste les périphériques de bloc (disques, partitions). |
| **lsusb** | Liste les périphériques USB. |
| **lsof** | Liste les fichiers ouverts par le système. |
| **lspci** | Liste les périphériques PCI. |
| **sudo** | Exécute une commande en tant qu'un autre utilisateur (souvent root). |
| **su** | Change d'utilisateur (par défaut vers le superutilisateur). |
| **useradd** | Crée un nouvel utilisateur. |
| **userdel** | Supprime un compte utilisateur et ses fichiers liés. |
| **usermod** | Modifie un compte utilisateur existant. |
| **addgroup** | Ajoute un groupe au système. |
| **delgroup** | Supprime un groupe du système. |
| **passwd** | Change le mot de passe d'un utilisateur. |
| **dpkg** | Installe, supprime et configure des paquets Debian (.deb). |
| **apt** | Gestionnaire de paquets de haut niveau pour Debian/Ubuntu. |
| **aptitude** | Alternative textuelle avancée à `apt`. |
| **snap** | Installe et configure des paquets isolés (snaps). |
| **gem** | Gestionnaire de paquets standard pour Ruby. |
| **pip** | Gestionnaire de paquets standard pour Python. |
| **git** | Utilitaire de ligne de commande pour le contrôle de version. |
| **systemctl** | Contrôle et gère les services et le gestionnaire système `systemd`. |
| **journalctl** | Interroge et affiche les journaux (logs) de `systemd`. |
| **kill** | Envoie un signal à un processus (souvent pour l'arrêter). |
| **bg** | Met un processus suspendu en arrière-plan. |
| **jobs** | Liste les processus tournant en arrière-plan. |
| **fg** | Ramène un processus d'arrière-plan au premier plan. |
| **curl** | Transfère des données depuis ou vers un serveur. |
| **wget** | Télécharge des fichiers depuis des serveurs HTTP(s) ou FTP. |
| **python3 -m http.server** | Lance un serveur web rapide sur le port TCP 8000. |
| **ls** | Liste le contenu d'un répertoire. |
| **cd** | Change de répertoire. |
| **clear** | Nettoie l'écran du terminal. |
| **touch** | Crée un fichier vide ou met à jour la date d'accès. |
| **mkdir** | Crée un nouveau répertoire. |
| **tree** | Affiche le contenu d'un répertoire récursivement sous forme d'arbre. |
| **mv** | Déplace ou renomme des fichiers/répertoires. |
| **cp** | Copie des fichiers ou répertoires. |
| **nano** | Éditeur de texte simple en ligne de commande. |
| **which** | Retourne le chemin d'accès d'un exécutable. |
| **find** | Cherche des fichiers dans une hiérarchie de répertoires. |
| **updatedb** | Met à jour la base de données locale pour la commande `locate`. |
| **locate** | Trouve des fichiers rapidement via la base de données locale. |
| **more** | Affiche le contenu d'un fichier page par page. |
| **less** | Alternative à `more` avec plus de fonctionnalités de navigation. |
| **head** | Affiche les dix premières lignes d'un fichier. |
| **tail** | Affiche les dix dernières lignes d'un fichier. |
| **sort** | Trie les lignes d'un texte ou d'un fichier. |
| **grep** | Recherche des motifs précis dans un texte ou fichier. |
| **cut** | Supprime ou extrait des sections de chaque ligne d'un fichier. |
| **tr** | Remplace ou supprime des caractères spécifiques. |
| **column** | Formate l'entrée en plusieurs colonnes. |
| **awk** | Langage de traitement et d'analyse de motifs textuels. |
| **sed** | Éditeur de flux pour filtrer et transformer du texte. |
| **wc** | Compte les lignes, les mots et les octets d'un fichier. |
| **chmod** | Modifie les permissions d'un fichier ou répertoire. |
| **chown** | Modifie le propriétaire et le groupe d'un fichier ou répertoire. |



## Raccourcis Windows

- **[Win] + [R]** : Ouvre la boîte de dialogue "Exécuter" (pratique pour lancer `cmd`, `powershell` ou `control`).
- **[Win] + [E]** : Ouvre l'Explorateur de fichiers.
- **[Win] + [X]** : Ouvre le menu d'accès rapide (accès direct au Gestionnaire de périphériques, Terminal Admin, etc.).
- **[Win] + [L]** : Verrouille la session.
- **[Win] + [Pause/Attn]** : Ouvre les informations système (version Windows, RAM, nom du PC).
- **[CTRL] + [SHIFT] + [ESC]** : Ouvre directement le Gestionnaire des tâches.
- **[ALT] + [D]** : Sélectionne la barre d'adresse dans l'explorateur (pour taper un chemin réseau par exemple).

---

## Commandes Windows
### CMD

| Commande | Équivalent Linux | Description |
| :--- | :--- | :--- |
| **dir** | `ls` | Liste les fichiers et répertoires. |
| **cd** | `cd` | Change de répertoire. |
| **type <file>** | `cat` | Affiche le contenu d'un fichier texte. |
| **cls** | `clear` | Nettoie l'écran du terminal. |
| **systeminfo** | `uname -a` | Affiche les détails complets du système et des patchs (Hotfixes). |
| **whoami /priv** | `id` | Affiche l'utilisateur actuel et ses **privilèges** (Crucial en PrivEsc). |
| **hostname** | `hostname` | Affiche le nom de la machine. |
| **ipconfig /all** | `ifconfig` | Affiche la configuration réseau détaillée. |
| **netstat -ano** | `netstat` | Affiche les connexions réseau et les ports ouverts avec les PID. |
| **findstr** | `grep` | Cherche une chaîne de caractères dans un texte ou une sortie. |
| **tasklist** | `ps` | Liste les processus en cours d'exécution. |
| **taskkill /PID <ID> /F**| `kill -9` | Force l'arrêt d'un processus. |
| **net user** | `cat /etc/passwd` | Liste les utilisateurs locaux. |
| **net localgroup** | `groups` | Liste les groupes locaux (ex: `net localgroup Administrators`). |
| **net share** | - | Affiche les partages SMB actifs sur la machine. |
| **attrib** | `ls -l` | Affiche ou modifie les attributs de fichiers (caché, système, etc.). |
| **where** | `which` | Trouve le chemin d'un exécutable. |
| **icacls <file>** | `ls -l` | Affiche ou modifie les permissions (ACL) d'un fichier. |

#### Gestion de session
- **`query user`** : Voir qui est connecté sur la machine.
- **`logoff <ID>`** : Déconnecter un utilisateur.
- **`shutdown /r /t 0`** : Redémarrer la machine immédiatement.
- **`runas /user:administrator cmd`** : Exécuter un prompt en tant qu'un autre utilisateur.

---

### PowerShell

- **`Get-Service`** : Liste tous les services (cherche ceux arrêtés ou avec des droits faibles).
- **`Get-Process`** : Liste les processus détaillés.
- **`Get-Content <file>`** : Équivalent de `cat`.
- **`Invoke-WebRequest -Uri <URL> -OutFile <File>`** : Équivalent de `wget` ou `curl` pour télécharger un outil (ex: `winPEAS.exe`).
- **`Test-NetConnection -ComputerName <IP> -Port <Port>`** : Scanner de port basique intégré.
- **`Get-HotFix`** : Liste les mises à jour de sécurité installées (pour trouver des exploits de noyau).
- **`$PSVersionTable`** : Affiche la version de PowerShell (important pour savoir quels scripts on peut lancer).

