---
title: "Brutforce"
date: 2026-04-03T04:42:29+02:00
draft: true
showToc: true
TocOpen: true
---

#### john
```bash
john
```
#### hydra
```bash
hydra -l jan -P ../lists/rockyou.txt ssh://10.10.8.84
```

#### Hashcat
```bash
hashcat -m 1410 hashes.txt lists/rockyou.txt
```

| Hash Type                       | Hashcat Mode ( -m ) | Description                           |
| ------------------------------- | ------------------- | ------------------------------------- |
| MD5                             | 0                   | Hash MD5 standard                     |
| SHA-1                           | 100                 | Hash SHA-1                            |
| SHA-256                         | 1400                | Hash SHA-256                          |
| SHA-512                         | 1700                | Hash SHA-512                          |
| SHA-3 (Keccak)                  | 17400               | Hash SHA-3                            |
| bcrypt (`$2a$`, `$2b$`, `$2y$`) | 3200                | bcrypt Blowfish Unix crypt            |
| NTLM                            | 1000                | Hash Windows NT LM                    |
| LM                              | 3000                | Hash Windows LAN Manager (LM)         |
| MySQL                           | 300                 | MySQL 4.1/MySQL 5 hash                |
| PostgreSQL                      | 13100               | PostgreSQL MD5-based hash             |
| MSSQL (2000)                    | 131                 | Hash MSSQL 2000                       |
| MSSQL (2005)                    | 132                 | Hash MSSQL 2005                       |
| MSSQL (2012, 2014)              | 1731                | Hash MSSQL 2012/2014                  |
| Oracle 11g                      | 112                 | Oracle 11g/12c H: Type (Salted SHA-1) |
| Oracle 7-10g                    | 3100                | Oracle 7-10g (DES-based)              |
| WPA/WPA2                        | 2500                | WPA/WPA2 security hash                |
| Unix Crypt SHA-256              | 1800                | SHA-256 crypt hash (Linux)            |
| Unix Crypt SHA-512              | 1800                | SHA-512 crypt hash (Linux)            |
| RAR3-hp                         | 12500               | RAR3 Hashes (hybrid password)         |
| PDF                             | 10400               | PDF 1.4 - 1.6 (Acrobat 5 - 8)         |
| ZIP                             | 13600               | ZIP archive hashes                    |
##### Commande de base pour le crack de hash
```bash
hashcat -m [mode] -a 0 [hashfile] [path to wordlist]
```
- `-m [mode]`: spécifie le mode Hashcat basé sur le type de hash.
- `-a 0`: méthode d'attaque par dictionnaire.
- `[hashfile]`: fichier contenant le ou les hashes à casser.
- `[path to wordlist]`: chemin vers la liste de mots utilisée pour le crack.

##### Utiliser une attaque combinée
```bash
hashcat -m [mode] -a 1 [hashfile] [path to wordlist1] [path to wordlist2]
```

##### Utiliser une attaque de masque (brute-force avec des motifs)
```bash
hashcat -m [mode] -a 3 [hashfile] ?l?u?d?s
```
- `?l`: minuscules, `?u`: majuscules, `?d`: chiffres, `?s`: caractères spéciaux.
