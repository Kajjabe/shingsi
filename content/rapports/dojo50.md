---
title: "Dojo50"
date: 2026-04-03T05:25:48+02:00
draft: true
showToc: true
TocOpen: true
---
{{< notice type="info" >}}
Les DOJO sont les challenges mensuels de YesWeHack.
{{< /notice >}}

## Description
A path traversal vulnerability exists in the Bucket Vault challenge due to an inconsistency between the input validation and the filename sanitization process. The application blocks the `..` sequence but performs sanitization after this check, allowing an attacker to bypass the filter using a CRLF injection (`%0A`).

## Exploitation
1. The sign action checks if the filename contains `..` to prevent directory traversal.
2. By injecting a newline character `.` + `\n` + `.` (`.%0A.`), the filter `str_contains` is bypassed.
3. The sanitizeFilename function then removes the control character, re-joining the two dots and creating a valid path traversal sequence: `public/../super_secret.txt`.
4. The server generates a valid HMAC signature for this forbidden path.

## PoC
1. Request a signature for the forged path:
`GET /?action=sign&filename=public/.%0A./super_secret.txt`
2. Use the returned expires and signature to download the file:
`GET /?action=download&filename=public/../super_secret.txt&expires=[TS]&signature=[SIG]`
3. The flag is retrieved: `D0n7_l3t_M3_c0n7tr0l_F1l3n4m3!!`

## Risk
An attacker can read any file stored on the server's filesystem that the PHP process has access to, leading to a full leak of sensitive data.

## Remediation
Sanitize the user input before performing security checks.