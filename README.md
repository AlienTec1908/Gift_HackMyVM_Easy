# Gift - HackMyVM (Easy) - Writeup

![Gift Icon](Gift.png)

Dieses Repository enthält einen zusammengefassten Bericht über die Kompromittierung der HackMyVM-Maschine "Gift" (Schwierigkeitsgrad: Easy).

## Metadaten

*   **Maschine:** Gift (HackMyVM - Easy)
*   **Link zur VM:** [https://hackmyvm.eu/machines/machine.php?vm=Gift](https://hackmyvm.eu/machines/machine.php?vm=Gift)
*   **Autor des Writeups:** DarkSpirit
*   **Datum:** 2. November 2022
*   **Original Writeup:** [https://alientec1908.github.io/Gift_HackMyVM_Easy/](https://alientec1908.github.io/Gift_HackMyVM_Easy/)

## Zusammenfassung des Angriffspfads

Die initiale Erkundung mittels `nmap` offenbarte einen SSH-Dienst (Port 22, OpenSSH 8.3) und einen Nginx-Webserver (Port 80). Die anschließende Web-Enumeration mit `gobuster`, `nikto` und `wfuzz` (Subdomain-Fuzzing) lieferte keine verwertbaren Angriffspunkte außer der Standard-`index.html`.

Der Fokus verlagerte sich auf den SSH-Dienst. Ein Brute-Force-Angriff mit `hydra` wurde **direkt gegen den `root`-Benutzer** unter Verwendung der `rockyou.txt`-Passwortliste durchgeführt. Dieser Angriff war erfolgreich und enthüllte das extrem schwache Passwort `simple` für den `root`-Account.

Da der Root-Login über SSH erlaubt war, ermöglichte dies den direkten Login als `root` mit dem gefundenen Passwort. Nach dem erfolgreichen SSH-Login als `root` wurden sowohl die User- als auch die Root-Flag im selben Verzeichnis (vermutlich `/root`) gefunden und ausgelesen.

## Verwendete Tools (Auswahl)

*   `arp-scan`
*   `nmap`
*   `gobuster`
*   `nikto`
*   `wfuzz`
*   `hydra`
*   `ssh`
*   `cat`

## Angriffsschritte (Übersicht)

1.  **Reconnaissance:** Ziel-IP (`arp-scan`), Portscan (`nmap`) -> Port 22 (SSH), Port 80 (HTTP/Nginx).
2.  **Web Enumeration:** `gobuster`, `nikto`, `wfuzz` (Subdomains) -> Keine verwertbaren Ergebnisse.
3.  **SSH Brute-Force:** `hydra` Angriff auf Benutzer `root` mit `rockyou.txt` -> Passwort `simple` gefunden.
4.  **Initial Access & Root:** Login via `ssh root@gift.hmv` mit Passwort `simple` -> Direkte Root-Shell.
5.  **Flags:** `cat root.txt; cat user.txt` im (vermutlich) `/root`-Verzeichnis ausführen.

## Flags

*   **User Flag (`/root/user.txt` - Vermutet):** `HMV665sXzDS`
*   **Root Flag (`/root/root.txt` - Vermutet):** `HMVtyr543FG`

---

## Disclaimer

Die hier dargestellten Informationen und Techniken dienen ausschließlich Bildungs- und Forschungszwecken im Bereich der Cybersicherheit. Die beschriebenen Methoden dürfen nur auf Systemen angewendet werden, für die eine ausdrückliche Genehmigung vorliegt (z.B. in CTF-Umgebungen wie HackMyVM, Penetrationstests mit schriftlicher Erlaubnis). Das unbefugte Eindringen in fremde Computersysteme ist illegal und strafbar. Die Autoren übernehmen keine Haftung für missbräuchliche Verwendung der bereitgestellten Informationen. Handeln Sie stets legal und ethisch verantwortlich.

---
