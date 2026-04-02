---
title: "Enumeration"
date: 2026-04-03T01:28:30+02:00
draft: false
---

## Scans de base

### Scan rapide (Top 100 ports)
`nmap -F <IP>`
* `-F` : Fast mode (scanne les 100 ports les plus communs au lieu de 1000).

### Scan standard (Top 1000 ports)
`nmap <IP>`

### Scan complet de tous les ports (65535)
`nmap -p- <IP>`
* `-p-` : Scanne tous les ports de 1 à 65535.

---

## Énumération Agressive

`nmap -sV -sC -p- -oA nmap_full <IP>`

**Explication des options :**
* `-sV` : **Version Detection**. Tente de déterminer la version du service (ex: Apache 2.4.41).
* `-sC` : **Default Scripts**. Lance les scripts de base du moteur NSE (Nmap Scripting Engine) pour détecter des vulnérabilités connues.
* `-oA nmap_full` : **Output All**. Sauvegarde le résultat dans les 3 formats (Nmap, Grepable, XML)

---

## ⚡ Optimisation et Vitesse

### Réglage de la rapidité (Timing)
Nmap propose 6 niveaux de rapidité (`-T0` à `-T5`).
* `-T4` : Recommandé pour les CTF (rapide et assez fiable).
* `-T5` : Très agressif, peut rater des ports si le réseau est instable.

### Ignorer le Ping
Si une machine ne répond pas au scan de base (pare-feu), forcez le scan :
`nmap -Pn <IP>`
* `-Pn` : Considère que l'hôte est actif (ne vérifie pas s'il répond au ping).

---

## Scans Spécifiques

### Scan UDP
Les services comme DNS, SNMP ou DHCP tournent en UDP.
`sudo nmap -sU --top-ports 100 <IP>`
* `-sS` : Scan TCP SYN (discret et rapide, nécessite `sudo`).
* `-sU` : Scan UDP.

---

## Astuces

| Option | Description |
| :--- | :--- |
| `-v` | **Verbose**. Affiche les ports au fur et à mesure qu'ils sont trouvés. |
| `--script vuln` | Vérifie si les services trouvés ont des failles critiques connues. |
| `-A` | Mode "Agressif" (équivaut à `-sV -sC -O --traceroute`). |
| `-O` | **OS Detection**. Tente de deviner si c'est du Linux ou du Windows. |