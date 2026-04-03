---
title: "Enumeration"
date: 2026-04-03T01:28:30+02:00
draft: false
showToc: true
TocOpen: true
---

## Nmap

### TCP Scan
Scan complet des services avec scripts par défaut.
```bash
nmap -sC -sV -p- <IP> -v
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
Plus rapide et flexible que gobuster.

fuzzing de répertoire
```bash
ffuf -u <URL>/FUZZ -w ~/Documents/lists/KaliLists/dirbuster/directory-list-2.3-medium.txt
```

fuzzing de sous-domaines / vhost
```bash
ffuf -u [http://SITE.com](http://SITE.com) -H "Host: FUZZ.SITE.com" -w ~/Documents/lists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

| Option | Fonction | Utilité |
| :--- | :--- | :--- |
| `-u` | URL | URL cible contenant le mot `FUZZ` |
| `-w` | Wordlist | Chemin vers le dictionnaire |
| `-fc` | Filter Code | Cache certains codes HTTP (ex: -fc 404,403) |
| `-fs` | Filter Size | Cache les réponses d'une taille précise (utile pour ignorer les fausses pages) |
| `-recursion` | Récursif | Scanne automatiquement les sous-dossiers trouvés |
| `-v` | Verbose | Affiche l'URL complète pour chaque résultat |

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

