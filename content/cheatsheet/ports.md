---
title: "Ports"
date: 2026-04-03T04:33:34+02:00
draft: false
---

| Port     | Service      | Description                                    | Action Prioritaire (Pentest)            |
| :------- | :----------- | :--------------------------------------------- | :-------------------------------------- |
| **21**   | **FTP**      | Transfert de fichiers (souvent non chiffré).   | Tester `anonymous` login / Sniffing.    |
| **22**   | **SSH**      | Accès sécurisé à distance (Ligne de commande). | Bruteforce / Chercher clés privées.     |
| **23**   | **Telnet**   | Ancêtre du SSH, tout passe en clair.           | Sniffer les identifiants sur le réseau. |
| **25**   | **SMTP**     | Envoi de courriers électroniques.              | Énumérer utilisateurs (`VRFY`).         |
| **53**   | **DNS**      | Résolution de noms de domaine en IP.           | Tester le transfert de zone `AXFR`.     |
| **80**   | **HTTP**     | Protocole web de base (non chiffré).           | Fuzzing répertoires / Check versions.   |
| **88**   | **Kerberos** | Authentification réseau (Active Directory).    | Attaques `AS-REP` & `Kerberoasting`.    |
| **110**  | **POP3**     | Récupération de mails (non chiffré).           | Bruteforce d'identifiants.              |
| **139**  | **NetBIOS**  | Session de service pour partage de fichiers.   | Souvent lié au port 445 (SMB).          |
| **161**  | **SNMP**     | Gestion et monitoring d'équipements.           | Bruteforce `Community String`.          |
| **389**  | **LDAP**     | Annuaire d'utilisateurs et d'objets.           | Énumération d'utilisateurs/groupes.     |
| **443**  | **HTTPS**    | Web sécurisé via SSL/TLS.                      | Analyse Certificat (Subdomain leak).    |
| **445**  | **SMB**      | Partage de fichiers/imprimantes (Windows).     | `Null Session` / Partages ouverts.      |
| **1433** | **MSSQL**    | Base de données Microsoft SQL Server.          | Tester `sa` / Injections SQL.           |
| **2049** | **NFS**      | Partage de fichiers (Unix/Linux).              | Lister les dossiers partagés.           |
| **3306** | **MySQL**    | Base de données Open Source (Web).             | Accès distant sans MDP / SQLi.          |
| **3389** | **RDP**      | Bureau à distance Windows (GUI).               | Tester `BlueKeep` / Bruteforce.         |
| **5432** | **Postgre**  | Base de données SQL avancée.                   | Tester `postgres:postgres`.             |
| **5900** | **VNC**      | Contrôle de bureau à distance.                 | Tester l'absence de mot de passe.       |
| **8080** | **HTTP-Alt** | Port alternatif (Serveurs Web/Admin).          | Chercher Tomcat, Jenkins, APIs.         |