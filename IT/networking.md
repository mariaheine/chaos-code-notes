# status

## todo

- [x] wget
- [ ] curl
- [ ] cookies, what are they actually, what stuff you can stuff into them, how can you retrieve them, how can you see the cookies per website 
- [ ] booklist: https://www.reddit.com/r/Cisco/comments/119usm9/recommended_cisco_books/
	- [ ] Network warrior (after cisco one) https://www.amazon.com/Network-Warrior-Everything-Need-Wasnt/dp/1449387861/
- [ ] ssh
- [ ] dmz
- [ ] so http request always stamps a random port on the client request so that the server know where to send the data back?
- [ ] tls vs. sssl
- [ ] https://en.wikipedia.org/wiki/Network_socket
- [ ] ### **Dein Arch ↔ Ubuntu SSH**
```
OSI-Schicht | Aktion                     | Protokoll/Tool
------------|----------------------------|----------------
7. Anwendung| `ssh arch-pc`              | SSH-Client
8. Darstellung| Schlüsselaustausch       | Diffie-Hellman
9. Sitzung  | SSH-Session aufbauen       | SSH-Protokoll
10. Transport| TCP zu Port 22             | `netstat -tln`
11. Netzwerk | IP: 192.168.1.50 → 192.168.1.100 | `ip route`
12. Datenlink| MAC-Adresse finden         | `arp -a`
13. Physikalisch| Ethernet/Wi-Fi Signale  | `ethtool eth0`
```
- **Beispiel: Web-Browsing**
```
OSI-Schicht | Was passiert                | TCP/IP-Schicht
------------|-----------------------------|---------------
7. Applikation | Browser sendet HTTP GET  | Anwendung
8. Darstellung | TLS-Verschlüsselung      | Anwendung  
9. Sitzung     | Cookie-Management        | Anwendung
10. Transport   | TCP-Verbindung Port 443  | Transport
11. Netzwerk    | IP-Paket zu Server-IP    | Internet
12. Datenlink   | Ethernet-Frame zu Router | Netzwerkzugang
13. Physikalisch| Elektrische Signale      | Netzwerkzugang
```

- troubleshooting by osi
```
"Ping funktioniert, aber Webseite nicht?" 
→ Schichten 1-3 OK (ping = ICMP = Schicht 3)
→ Problem in Schicht 4 (TCP) oder 7 (HTTP)

"Kann andere PCs im Netzwerk nicht sehen?"
→ Problem in Schicht 2 (ARP, MAC) oder 3 (IP-Subnetz)
```
- [ ] what is meant with a PCI device, like ones shown by lspci
- [ ] learning concepts from having a server:
```
## **Learning Path:**

1. **Local first** - Write servers on your computer
    
2. **Expose with ngrok** - Learn about tunnels
    
3. **DigitalOcean** - Deploy for real
    
4. **Add domain** - Learn DNS
    
5. **Add HTTPS** - Learn SSL/TLS
    
6. **Add firewall** - Learn security

### **Networking Concepts You Can Practice:**

1. **Ports & Protocols** - TCP vs UDP, well-known ports
    
2. **Firewalls** - iptables, ufw, security groups
    
3. **Load Balancing** - Nginx, HAProxy
    
4. **Reverse Proxies** - exposing multiple services
    
5. **SSL/TLS** - Let's Encrypt certificates
    
6. **WebSockets** - real-time bidirectional
    
7. **SSH Tunnels** - port forwarding
    
8. **VPN** - WireGuard, OpenVPN
    
9. **DNS** - bind9, custom domains
    
10. **Monitoring** - `netstat`, `tcpdump`, `wireshark`
```

## cleared

- Fragen: [[#basic internet knowledge]]
- Allgemein
	- [ ] [[#OSI-Modell]]
- Die Hardware
	- [ ] NCI
	- [ ] Router
	- [ ] Switch
	- [ ] Basic Computerteile
- Besondere Konzepte
	- [ ] [[#NAT]]
	- [x] [[#Firewall]]
- OSI
	- 2. Schicht
		- [x] [[#MAC]]
		- [x] [[#ARP]]
	- 7. Schicht
		- [ ] [[#DNS]]
			- [ ] Domain
			- [ ] DNS-Resolver

# theory



## OSI-Modell

### Overview

> Das OSI-Modell (*Open Systems Interconnection*) ist ein ***theoretisches Referenzmodell***, das die Netzwerkkommunikation in ***sieben logisch getrennten Schichten*** beschreibt.

> Was ist der **_Faktor_**, nach dem wir OSI-Schichten trennen?
> 
> ***Welche Art von Information ausgewertet (?) wird, um Daten weiterzuleiten.*** 

> 🏕️ Understanding layers helps you know when something breaks.

***1. Physical*** - ***Bits**, Kabel, WLAN als Übertragungsmedium, Funk-Signale*

- In der Physical-Schicht werden als *elektrische, optische oder Funk-Signale* gesendet.
- Hier gibt es *keine Datenpakete*, ***nur rohe Bits***.
- Die Physical-Schicht ***überträgt nur zum nächsten Gerät*** (***der Hop***).

***2. Data Link*** - ***Frames, Mac-Adressen**, Ethernet, WLAN als Protokoll*

- Aufgaben
	- *Frames*: Die Data-Link-Schicht ***unterteilt die Daten in Frames***.
	- *Fehlererkennung*: Sorgt für eine ***fehlerarme*** ***Übertragung*** zwischen zwei direkt verbundenden Gäreten.
	- *MAC-Addressen*: Sie verwenden ***MAC-Adressen***, um Gärete im lokalen Netzwerk eindeutig zu identifizieren.
- ***IEEE-802-Protokollfamilie***
	- *802.3 und 802.11 sind gleichrangige Standards innerhalb IEEE-802-Familie*. Sie sind Geschwister, keine Eltern-Kind-Beziechung.
	- WLAN
		- ***IEEE 802.11 Protokoll*** - Data-Link Protokoll
		- Markiere bitte, dass WLAN auf beide 1. und 2. Schichten legt.
			- Hier bedeutet ein Protokoll (IEEE 802.11).
			- In der Schicht 1 bedeutet es Funk-Signal, elektromagnetische Wellen.
	- Ethernet
		- Ethernet - *Name der Technologie*
		- ***IEEE 802.3 Protokoll*** - Data-Link Protokoll, *offizieller Ethernet-Standard*
	- Beide:
		- nutzen *MAC-Adressen*
		- haben *Frames*
		- nutzen *Fehlererkennung (CRC/FCS)*
		- aber Details sind unterschidelich und 
- *Ein Switch* aber nicht Router! (Mac-Identifizierung)


***3. Network*** - *[[#IP]] / ICMP*

- Die Network-Schicht *bestimmt den Übertragungsweg* (***Routing***) für Datenpakete.
- Sie verwendet dazu *IP-Adressen*.
- Sie entscheidet immer nur den *Next Hop* - *zu welchem nächsten Router* ein Paket *weitegeleitet* wird.
	- [ ] todo: definition of a Hop
- Jeder nächsten Router *trifft* (make!) diese Entscheidung *erneut*.
	- Jeder Router:
		- schaut auf die *Ziel-IP*
		- benutzt *seine eigene Routing-Tabelle*
		- entscheidet *neu*, wohin das Paket *als Nächstes* geht
- ! Ports gehören hier nicht, sondern zur Transport-Schicht.
- **?** ICMP
	- [ ] if icmp is here then why we say it is the data link that works on the fehlerarme ubertragung
- *Ein Router* aber nicht ein Switch! (IP-Identifizierung)

***4. Transport*** - *TCP, UDP, Portnummern*

- Die Transport-Schicht sorgt für die *Ende-zu-Ende-Kommunikation* zwischen Anwendungen.
	- [ ] what is the meaning of end-to-end
- Sie verwendet ***Portnummern***, um Daten *dem richtigen Programm zuzuordnen*.
	- [ ] who actually *uses* the ports? TCP?

***5. Session*** - *Sitzungslogik, Session-ID*

- Die Sitzung-Schicht beschreibt wie Sitzungen zwischen zwei Kommunikationspartnern *verwaltet, startet und beendet* werden.
	- [ ] musst es immer nur *zwei* sein?
- Sie sorgt dafür, dass *mehrere gleichzeitige* Verbindungen *korrekt voneinander* getrennt bleiben.
- Aber!
	- *Eine Session-ID ist kein Muss* und wird oft **nicht explizit** auf OSI-Ebene umgesetzt.
		- [ ] was bedeutet hier kein Muss?
		- [ ] what does not explicitly implemented (umsetzen) mean here? as far as i understand nothing is actually implemented in osi, it is only a theoretical reference model, right?
	- In der Praxis *werden* viele Session-Funktionen ***von** TCP oder der Application-Schicht **übernommen***.
		- [ ] how TCP? does it have means of marking session-ids?
		- [ ] and how in application layer?
		- [ ] actually if neither of these two when who else? someone has to do it, right?

***6. Presentation*** - *TLS / Verschlüssung*

- Die Presentation-Schicht ist zuständig für: 
	- ***Verschlüssung und Entschlüssung***
	- ***Kompression***
	- ***Datenformatierung*** (z. B. ***Zeichencodierung** - character encoding mir ASCII / ISO / UTF*)
- Verschlüsselung *findet nur statt*, wenn *entsprechende Protokolle* verwendet werden (z. B. *TLS bei HTTPS*).

***7. Application*** - *HTTP, HTTPS, FTP, SMTP, DNS, [[#SSH]]

- Die Application-Schicht *ermöglicht Anwendungen*, Programmen wie Browser oder E-Mail-Client, Daten über das Netzwerk zu senden und zu empfangen.
	- [ ] is Schicht ermoglicht the right wording here? isnt it more like that this level is *composed of protocols* that describe or actually *ermoglichen* applications and programs to send and receive the stuff over the internet. or is it actually a figure of speaking that we use for all the levels? like same when we said that Transport layer *uses* ports to identify receiving applications
- Der Client sendet z. B. *HTTP-Anfragen (GET /index.html, POST)* an einen Server.
- 🔎 ***Die Daten sind hier menschenlesbar*** (*Strings, Texte, Befehle*).
	- [ ] is it the physical schicht that does the final translation into bits?
- [ ] Clarification about DNS being here
	- Die Application-Schicht weiß nur, dass sie eine DNS-Anfrage gestellt hat. 
	- Sie weiß nur die ***logische Namen*** wie *google.com*.
	- IP-Adresse werden normalerweise durch Betribssystem oder DNS-Resolver aufgelöst

### Terms & Common Questions

#### Routing

- ***Was ist ein Router?***
	- Ein Router verbindet *verschiedene Netzwerke miteinander* und *leitet Datenpakete weiter*, z. B. vom Heimnetz oder locales Netzwerk ins Internet.

- ***Router vs. Switch***
	- *Router:* verbindet *verschiedene Netzwerke*.
		- Leitet Datenpakete (***entscheidet wohin***) *anhand der **IP-Adressen** weiter*.
		- OSI 3: Network
	- *Switch:* verbindet Geräte *innerhalb desselben Netzwerks*.
		- Leitet Daten *anhand der* ***MAC-Adressen*** *weiter*.
		- OSI 2: Data Link

-  ***Was ist Routing?***
	- Routing *festlegt* den *Übertragungsweg*, den die Datenpakete nehmen sollen. 
	- Es bedeutet nicht, die Daten selbst zu senden.
#### Session

***Was ist eine Session?***

- Eine *temporäre Verbindung* zwischen Client und Server.
	- [ ] is then session not part of the tcp definition, what is its relation of session to the statefulness of tcp and it being connection-based
- *Speichert Informationen während der Sitzung*
	- z. B. Login oder Warenkorb
- Aufgaben:
	- sitzung *aufbauen, verwalten und beenden*
		- [ ] is that not a cyclic definition that session builds and manages a session?
	- mehrere Verbindungen zwischen ***denselben Endpunkten unterscheiden***
- Endet, wenn der Benutzer sich abmeldet oder die Sitzung *abläuft*.
	- [ ] Oder die Verbindung wird verloren? Auch eine TCP Frage.
- OSI:
	- *Session-Schicht (Layer 5)* im OSI-Modell kümmert sich um Sitzungen zwischen zwei Kommunikationspartnern.

***Session vs. Cookies***
- Cookies speichern Daten langfristig, Sessions temporär während einer Verbindung.

## Internetprotokolfamilie oder TCP/IP

> *TCP/IP ist ein SUBSET von OSI*. Deshalb werden die Notizen ***bei*** OSI-Schichten organisiert.

```
OSI-Modell (7 Schichten)           TCP/IP-Modell (4 Schichten)
7. Anwendung                        | 
8. Darstellung                      |→ 4. Anwendung
9. Sitzung                          |
10. Transport       ← Übereinstimmung → 3. Transport
11. Netzwerk        ← Übereinstimmung → 2. Internet
12. Datenlink                        |→ 1. Netzwerkzugang
13. Physikalisch                     |
```
### Basic Concepts

***Was ist TCP/IP-Modell?***

- TCP/IP ist das *Protokoll-Modell*, auf dem das Internet basiert.
- *Es beschreibt, **wie Daten paketweise** über Netzwerke **übertragen werden**.*
- Es besteht aus 4 Schichten.

***Paketvermittlung (Grundidee des Internets)***
- Daten werden ***in Pakete zerlegt***.
- Pakete können *unterschiedliche Wege* nehmen.
- *Zusammensetzung* beim Empfänger.

***4 Schichten des TCP/IP-Modell***

- Das TCP/IP-Modell beschreibt *wie das Internet tatsächlich funktioniert*.
- Entwickelt aus dem *ARPANET* (Vorläufer des Internets)
	1. *Network Access*  
		- Physische *Übertragung* + *lokale* Adressierung
		- Ethernet, WLAN, MAC-Adressen
		- OSI 1, 2: Physical + Data Link
	2. *Internet*  
		- IP-Addressierung & Routing
		- IP, ICMP
		- OSI 3: Network
	3. *Transport*  
		- ? Ende-zu-Ende-Kommunikation
		- Ports
		- TCP (zuverlässig) vs. UDP (schnell)
		- OSI 4: Transport
	4. *Application*  
		- Protokolle für *Anwendungen*
		- (HTTP, FTP, DNS, SMTP)
		- OSI 4-7: Application

***Was ist die Beziehung zwischen TCP/IP und OSI?***

- OSI-Modell = *theoretisches Referenzmodell* (7 Schichten)
- TCP/IP-Modell = *praktisches Protokoll-Modell* (4 Schichten)
- TCP/IP-Protokolle *lassen sich* den OSI-Schichten *zuordnen*.
- ***OSI erklärt, TCP/IP funktioniert***.


# ---
# 🍰 7. Anwendung / Application

## HTTP & HTTPS

> HTTP, kurz für ***Hypertext Transfer Protocol***, ist eines von *mehreren* Protokollen, die werden für Kommunikation mit Servern *verwendet*. 
> 
> *Webbrowser* nutzen ausschließlich HTTP/HTTPS - das ist ihr enziges Protokol.

- *HTTP läuft über TCP,* normalerweise *Port 80*
- *HTTPS ist die **verschlüsselte** Version von HTTP und läuft über TCP + TLS*, normalerweise *Port 443*
- Eine HTTP Anfrage besteht aus:
	- Methode (z. B. GET, POST)
	- Host (z. B. myserver.duckdns.com)
	- Pfad (z. B. /search)
	- Protokollversion (z. B. HTTP/1.1)
	- Headern (metadaten wie `User-Agent`, `Content-Type`)
	- Request-Body (nur bei POST/PUT/PATCH Methoden)
- Es gibt 7 Methoden:
```c
GET     – Daten abrufen
POST    – Daten senden/erstellen und oft Daten in Response-Body Daten erhalten
PUT     – Daten senden und vollständig ersetzen
DELETE  – Daten löschen
PATCH   – Daten teilweise aktualisieren
HEAD    – Nur Header abrufen (ohne Inhalt)
OPTIONS – Verfügbare Methoden erfragen
```
- Jede Methode bekommt eine Antwort, Unterschied ist nur ***was*** in der Antwort kommt.
	- Manchmal es ist nur ein Statuscode-Antwort (wie z. B. 200 OK oder 404 nichts gefunden)
```
┌─────────────────────────────────────────────────────────┐
│  STATUS  │  Meaning           │  What to do             │
├──────────┼────────────────────┼─────────────────────────┤
│  200     │  OK               │  🎉 Success!            │
│  201     │  Created          │  ✅ Made new thing      │
│  301     │  Moved            │  🔁 Update URL          │
│  400     │  Bad Request      │  📝 Check your data     │
│  401     │  Unauthorized     │  🔑 Need login          │
│  403     │  Forbidden        │  🚫 Can't access        │
│  404     │  Not Found        │  🔍 Wrong URL           │
│  429     │  Too Many         │  😴 Slow down           │
│  500     │  Server Error     │  ⏳ Try again later     │
└─────────────────────────────────────────────────────────┘
```

***Http GET vs. POST***

| Merkmal          | GET                | POST                        |
| ---------------- | ------------------ | --------------------------- |
| Datenübertragung | in URL (sichtbar)  | im Nachrichtenteil (Body)   |
| Verwendung       | Daten *abrufen*    | Daten *senden / ändern*     |
| Sicherheit       | weniger sicher     | sicherer für sensible Daten |
| Längenbegrenzung | URL-Länge begrenzt | praktisch unbegrenzt        |

***Was ist ein Statuscode (200, 404, 500)?***
- HTTP-Statuscodes sagen **den Erfolg oder Fehler einer Anfrage**.
- **Beispiele:**
	- `200 OK` → Alles gut, Daten geliefert
	- `404 Not Found` → Seite nicht gefunden
	- `500 Internal Server Error` → Serverfehler

### tools

#### wget

> An ancient spell from 1996, derives its name from *World Wide Web* and *get*, a HTTP request method.
> 
> Developed to have a single, reliable tool to use both HTTP and FTP to download files.
> 
> Reliable meaning it could deal with network failures and continue downloading while the connection was reestablished.
> 
> It could handle recursive downloads and downloading webpages for offline view.
##### basics

- ***by default `wget` downloads to the current working directory!***
	- this is neat!
	- you can override this with the flag:
	- `-P ./local_folder` 
		- save to a specific local directory
- `--spider` 
	- dry run to see what would be downloaded!
- `-r` 
	- recursive download of a directory
- `wget -r -nH --user username --password pass ftp://host:port`
	- basic recursive download structure
	- ***don't forget to specify protocol before your host address***
	- ftp or http
- `-nH`
	- by default wget will create a host-named directory where downloads will be placed, like *192.168.0.83/your/files*
	- `-nH`
		- will cut out the *192.168.0.83* part
- `--cut-dirs=N`
	- you can use it to skip N levels of directory structure on the host

```
# Without -nH and without --cut-dirs=1:
./192.168.1.105/projects/file.txt

# With -nH and --cut-dirs=1:
./file.txt  # Much cleaner!
```
##### advanced

- *authentication & cookies*
	- `wget --user=user --password=pass http://...`
	- `wget --load-cookies cookies.txt http://...`
	- `wget --save-cookies cookies.txt http://...`
- *limiting damage*
	- `wget -Q 100M -r ftp://...     `      # Quit after 100MB total
	- `wget -l 2 -r ftp://...   `           # Only 2 levels deep
	- `wget -np -r ftp://...     `          # Don't go to parent directories ??
- *resume & retry*
	- `wget -c ftp://...   `                 # Continue partial download
	- `wget -t 5 -w 10 ftp://... `           # 5 retries, wait 10s between
	- `wget --retry-connrefused ftp://...`   # Retry even on connection refused
- *bandwidth control*
	- `wget --limit-rate=500k ftp://... `    # Max 500KB/s
	- `wget --limit-rate=1m ftp://...   `    # Max 1MB/s
- *timeout control*
	- [ ] to research further, i can't think of an application
	- `wget --timeout=30 ftp://...   `       # 30 second timeout
	- `wget --dns-timeout=10 ftp://...   `   # DNS timeout
	- `wget --connect-timeout=15 ftp://... ` # Connection timeout
- *pattern matching*
	- `wget -r -A "*.jpg,*.png" ftp://... `  # Only JPG and PNG
	- `wget -r -R "*.tmp,*.bak" ftp://...`   # Exclude temp/backup files
	- `wget -r --accept-regex ".*2024.*\.jpg" ftp://... ` # Regex pattern\
- *directory mirroring*
	- [ ] research what does that even mean
	- `wget -mk ftp://...`                    # -m = mirror mode (-r -N -l inf)
	- `wget -mk -w 2 ftp://...`               # Mirror with 2s wait between
	- `wget -mk --no-if-modified-since ftp://...`  # Ignore timestamps
- *timestamping*
	- `wget -r --timestamping ftp://...`      # Only download if newer
	- `wget -r -N ftp://... `                 # Same as --timestamping
- *session handling*
	- `wget --keep-session-cookies http://...`  # Save session cookies
	- `wget --no-cookies http://...`            # Don't use/save cookies
- 🔥 *headers and user agents*
	- `wget --header="Accept: application/json" http://...`
	- `wget --user-agent="Mozilla/5.0" http://...  # Fake browser`

## Cookies
## DNS

- [ ] records: cname, a, mx etc.
- [ ] DNS Poisoning
- [ ] Lastverteiling per DNS - DNS Load Balancing
- [ ] DNS Provider
- [ ] **ICANN/IANA**
- [ ] **DNSSEC
	- DNSSEC *schützt* DNS *vor Manipulation*, indem DNS-Antworten kryptografisch signiert werden.

***Was ist eine Domain?***

- Eine Domain ist eine eindeutige Adresse wie `google.com`, die ***auf*** Webserver und Online-Dienste ***verweist***. 
- Domains werden benutzt, weil es für uns viel enfacher ist, *menschenlesbare* Namen *zu **merken** als* IP-Adressen. 
- Eine Domain besteht aus einer ***TLD*** (*Top-Level-Domain*) wie `.com` und einer ***SLD*** (*Second-Level-Domain*) wie `google`.
- Eine Domain wird durch DNS in öffentliche IP-Adresse ***aufgelöst***.

***Was macht DNS?***

- DNS, das Domain Name System, übersetzt *Domainnamen* wie google.com *in IP-Adressen*.
- Damit Computer mit Servern kommunizieren *können*.
- Wir benutzen DNS, weil Menschen *sich* IP-Adressen nicht gut *merken* können.
- Es ist auf 7. OSI-Application-Layer
	- [ ] Warum? Weil es geht über menschenlesenbaren Daten?

***DoH / DoT – was ist das?***

- *DNS over HTTPS* oder *DNS over TLS*
- Sie *verschlüsseln DNS-Anfragen*, sodass Dritte nicht *mitlesen* können, welche Domains abgefragt werden.

***Was ist ein DNS-Resolver?***

- *Was?* 
	- Ein DNS-Resolver ist *ein Dienst oder ein Server*, der ***Namensauflösung*** (DNS Resolution) *durchführt*.
- *Hauptaufgabe?* 
	- Seine *Hauptaufgabe* ist die ***rekursive Abfrage***, ***bei der er*** eine *angefragte* Domain in die *zugehörige* IP-Adresse übersetzt.
- *Wo?* 
	- Er kann selbst gehostet sein, aber meistens benutzen wir entweder den von unserem ISP oder öffentlichen Resolver wie den von Cloudflare (`1.1.1.1`).

***Wie wird eine DNS-Abfrage aufgelöst?***

```
### 🔁 Schritte vereinfacht:

1. Clientanwendung → DNS-Resolver
2. Resolver → Root-Server
3. Root-Server → TLD-Server (.org)
4. TLD → autoritativer Nameserver (duckdns.org)
5. IP-Adresse zurück zum Client
```

- Lass uns das Beispiel von `mydomain.duckdns.org` benutzen.
- Ihre *Client-Anwendung (z. B. ein Browser)* sendet eine Anfrage an *einen konfigurierten DNS-Resolver*.
	- ISP-Resolver oder öffentlicher Resolver, aber man kann auch einen selbst gehosteten Resolver benutzen.
- Wenn *der Eintrag* *nicht* im Cache *vorliegt*, der Resolver *initiiert* eine ***rekursive Abfrage***.
- Zuerst konsultiert er einen ***Root-Nameserver***. 
	- Dieser *verweist ihn auf* die *Nameserver* für die ***Top-Level-Domain (TLD)*** `.org`.
- Diese ***TLD-Nameserver*** verweist ihn weiter auf den ***autoritativen Nameserver für die Zone*** `duckdns.org`.
	- Er gehört in unserem Fall zu *DuckDNS* und er ***enthaltet den finale A-Record*** und *antwortet mit IP-Adresse*, die der `mydomain` *zugeordnet ist*.
- Der Resolver sendet diese IP-Adresse zurück.
	- Und speichert sie *typischerweise* ***für die Dauer der TTL (Time to Live)*** in seinem Cache.

```
# TTL in DNS-Antwort sehen
dig mydomain.duckdns.org

;; ANSWER SECTION:
mydomain.duckdns.org. 60 IN A 92.123.45.67
#                     ↑
#                  TTL = 60 Sekunden
```


### ⚗️ tools

#### resolvectl
  
 >Resolve domain names, IPv4 and IPv6 addresses, DNS resource records, and services.  
 >
 >Introspect and reconfigure the DNS resolver.  
 >
 >More information: https://www.freedesktop.org/software/systemd/man/latest/resolvectl.html.  
  
 - `resolvectl status`  
	 - dns settings
- `resolvectl statistics`
	- [ ] transations cache and some other stuff
- `resolvectl show-cache`
	- will print the cache
 - `resolvectl query domain1 domain2 ...` 
	 - eesolve the IPv4 and IPv6 addresses for one or more domains:  
	 - *Data from: network* says that it was not cached
	- If it was cached it would say *cache network* which would mean it was earlier obtained from network

```
λ resolvectl query mail.proton.me    
mail.proton.me: 185.70.42.37                   -- link: wlp3s0  
  
-- Information acquired via protocol DNS in 100.8ms.  
-- Data is authenticated: no; Data was acquired via local or encrypted transport: no  
-- Data from: network
```
  
 - Retrieve the domain of a specified IP address:  
   `resolvectl query ip_address`  
  
 - Flush all local DNS caches:  
   `resolvectl flush-caches`  
  
 - Retrieve an MX record of a domain:  
   `resolvectl --legend no --type MX query domain`  
  
 - Retrieve a TLS key:  
   `resolvectl tlsa tcp domain:443`

#### dig

> DNS lookup utility.  
> 
> More information: https://manned.org/dig.  

 - `dig +short example.com`  
	 -  glookup the IP(s) associated with a hostname (A records)
 - `dig +noall +answer example.com`  
	 - get a detailed answer for a given domain (A records)
- `dig +short example.com A|MX|TXT|CNAME|NS`
	- query a specific dns record type
- `dig +tls @1.1.1.1 example.com`
	- name an alternate dns server to query
	- and optionally use DoT with `+tls`
- `dig -x 8.8.8.8`
	- reverse dns lookup 
	- [ ] PTR record
- `dig +nssearch example.com`
	- find authoritative name servers for the zone and display SOA records
	- [ ] records what?
- `dig +trace example.com`
	- perform iterative queries and display the entire trace path to resolve a domain name:  
	- [ ] this is a really interesting command, look into it, how to understand what it returns
- `dig +tcp -p port @dns_server_ip example.com`
	- query a DNS server over a non-standard port using the TCP protocol
	- [ ] complicated, no idea what is that for

#### host

> Lookup domain name server.

- `host domainname`
	- Lookup A, AAAA, and MX records of a domain
	- [ ] what do those mean
- `host -t <field> domainname`
	- lookup other fields
- `host ip_adress`
	- reverse IP address to domain lookup
- `host domain 8.8.8.8`
	- Specify an alternate DNS server to query:
	- [ ] i don't understand, i kno this ip is google dns 

#### duckdns

> Wenn du einen öffentlichen Server *von zu Hause aus betreiben* möchtest, du wirst wahrscheinlich Verbindungsprobleme haben, weil du keine *statische IP-Adresse* ***besitzt***.    
> 
> Eine der Lösungen bei einer dynamischen IP-Adresse ist die Nutzung von DuckDNS.

- when you login to `duckdns.org` and create a domain it will automatically grab your current IP
	- but you need to keep it updated, you can use [[linux fundamentals#🗻 systemd]] for that
	- see the setup process explained at [[linux fundamentals#custom service (duckdns.org)]]


## SSH

### Overview

***Wird benutzt, um***:

- sich auf Server *anzumelden*
- remote Befehle *auszuführen*
- Daten sicher zu *übertragen*

***Wichtige Eigenschaften***:

- Verschlüsselt die gesamte erbindung
- Authentifiziert Client und Server
	- [ ] wie funktioniert das?
- Schützt vor Abhören und Manipulation
- ***Es benutzt nur/ausschließlich TCP***
	- Es benötigt die zuverlässige, verbindungsorientierte Übertragung, die TCP bietet.

### openssh

![[Pasted image 20260203190625.png]]

> All the below commands are part of `openssh`.
> 
> https://en.wikipedia.org/wiki/OpenSSH

#### key exchange

> ***Because you should never use password authentication for public ssh servers!***

***Setting up***:

- `ssh-keygen`
	- lets you generate a key interactively
	- this will generate *two files*!
		- *id_ed25519* and *id_ed25519.pub*
		- ***always share ONLY the .pub one***
		- the other one is your private key!
	- by default it will be placed in `~/.ssh`
	- it also generates a pratty random image based on generated key:
```
+--[ED25519 256]--+  
|+o .             |  
|.o. .            |  
|. .. .           |  
|+.  . . . .      |  
|=.   ..oS+ . .   |  
|.. .+.=.= o o    |  
|. .=o= X . o     |  
| o+ *+o + o      |  
|.*B=o+E. o.      |  
+----[SHA256]-----+
```
- then at client you have something like:
```
λ cat ~/.ssh/id_ed25519.pub    
ssh-ed25519 AAAA[..key] merryangel@lain
```

***Copying key to your server***:

- *notice it is always the client that generates the key and sends it to the server*
- `ssh-copy-id -i path/to/certificate.pub -p port username@remote_host`
	- ***make sure you point `-i` at `.pub` certificate***
	- *-p port* 
		- is not always required
		- but it should be, you shouldn't use default port
		- change it in config
	- *remotehost* will likely be the ip
	- *received key* will be most likely stored here:
		- also in `~/.ssh/` directory, in the `authorized_keys` file
		- see with `cat ~/.ssh/authorized_keys`

#### sshd config

***Server Daemon***:

- ***sshd***
	- Arch: *sshd.service* as part of `openssh`
		- arch installs both server and client through one packege
		- `systemctl status sshd`
	- Ubuntu: *ssh.service* as part of `openssh-server`
		- ubuntu installs client `ssh-clien` and server `ssh-server` separately
		- `systemctl status ssh`
- `sudo sshd -T`
	- this will showg the server's active settings

***Config***:

> System-wide config is located in `/etc/ssh/sshd_config`.
> 
> For more info see `man sshd_config`

- ***REMEMBER*** to restart `sshd` service after making changes to the config
	- `sudo systemctl restart sshd`
- *allow root login*
	- ***ESSENTIAL CHANGE*** definitely disable it because scanners will be definitely targettin `root` user
	- set it to `no`
	- this does NOT prevent your specified, allowed user (see below) from executing `sudo` commands
- *port number*
	- ***ESSENTIAL CHANGE*** for internet-facing servers, the core reason is the *brute-force* automation, attackers continously scan entire IP ranges for the default ssh port 22.
		- Changing this port if a fundamental layer of "*security through obscurity*"
		- 99% of malicious SSH traffic targets port 22
		- You should be using some port from the *Registered* range, see [[#Port]]
- *key authentication*
	- ***ESSENTIAL CHANGE*** disable password authentication and enable public key authentication.
```
PubkeyAuthentication yes
PasswordAuthentication no
```
- *specify AllowedUsers*
	- *good to change*
	- it is checked first
	- if the user is not on the list, the authentication will be immediately dropped
```
AllowUsers meowria bobthepiglet
```
- *use vpn*
	- [ ] wireguard
- [ ] port knocking
- *2FA*
	- `sudo pacman -S libpam-google-authenticator`
	- `google-authenticator` 
		- will give you a QR code for the autenticator
		- and generate backup codes, save them
			- [ ] save them where?
	- [ ] Configure SSH to use it: Edit the PAM configuration for SSH (`/etc/pam.d/sshd`) and the SSH daemon config (`/etc/ssh/sshd_config`) to enable "authentication methods" and challenge-response.
	- [ ] Crucial Testing: You will then need to provide both your SSH key **and** the time-based code when logging in.
	- [ ] A "Dummy-Friendly" Warning: Setting this up requires editing system files. The biggest risk is misconfiguring PAM and locking yourself out. Do not attempt this until you are very comfortable with your key-based access and have a backup console (like physical access or a hosting provider's web console).
	- [ ] *A safer intermediate step before 2FA is to fully implement `AllowUsers` and a firewall. This gives you massive security gains with less risk of lockout.*

#### connection via ssh & scp

***Connect***:

- `ssh -p portnumber username@local_ip_OR_domain_name`
	- will connect to an ssh server on port `portnumber`
- `exit`
	- will exit ssh connection
	- emergency escape when session is frozen:
		- `Enter`, then type `~.`
- On first connection you will se something like:
```bash
λ ssh -p 39335 meowria@doomhamster.duckdns.org    
The authenticity of host '[doomhamster.duckdns.org]:39335 ([95.88.154.218]:39335)' can't be established.  
ED25519 key fingerprint is SHA256:1HO61o2gysI1yt2p7jJgINKaXIxs/I/FwpHiKy/ZUFg.  
This host key is known by the following other names/addresses:  
   ~/.ssh/known_hosts:1: [hashed name]  
   ~/.ssh/known_hosts:4: [hashed name]  
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes  
Warning: Permanently added '[doomhamster.duckdns.org]:39335' (ED25519) to the list of known hosts.
```
- It means:
	- SSH's *host key verification*.
	- This is SSH's way of saying: *"I've never seen this server before. Are you SURE you want to connect?"*
	- ED25519 key fingerprint is SHA256:1HO61o2gysI1yt2p7jJgINKaXIxs/I/FwpHiKy/ZUFg.
		- [ ] *How is this unique identity established? How do we make sure it is unque*? This is your server's *unique identity*. Think of it like a fingerprint for your server. When you said "yes", SSH saved this fingerprint to `~/.ssh/known_hosts` on your local machine.
	- ***IMPORTANT*** Next time you connect SSH will check:
		- "Does the server's fingerprint match what I saved?"
		- If it matches → *silent success*
		- If it's different → *LOUD WARNING* (possible man-in-the-middle attack!)
```
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
    ~/.ssh/known_hosts:4: [hashed name]
```
- This is SSH being *extra helpful*. It's saying: 
	- "I've seen this same fingerprint before, but under different names/addresses." That's because you probably connected to the same server using its local IP (`192.168.0.28`) earlier.
  
***Copying files:***

- `scp -i path/to/the/key path/to/the/file user@address:/home/or/not/path`
	- you need to first create the target directory
	- if you want to copy the other way around then switch the paths places
- `-r`
	- lets you recursively copy directories
- `-P`
	- use a specific port
	- ***for some reason unlike in ssh here it is uppercase***

### Quest

#### Creating a Limited-Access User for Friends

```

Yes, this is a classic and excellent use case. Here's how to set it up for a friend to access a meme directory:

**Step-by-Step Guide:**

1. **Create the User and Meme Directory**:
    
    bash
    
    # Create the user 'memelord' (or any name) with no password login
    sudo useradd -m -s /bin/bash memelord
    # Create a directory for memes and give memelord ownership
    sudo mkdir /home/memelord/meme_vault
    sudo chown memelord:memelord /home/memelord/meme_vault
    
2. **Restrict SSH Access to a Specific Command (Key Security Step)**:  
    This is done in the `~/.ssh/authorized_keys` file on the server.
    
    - Your friend generates an SSH key pair and sends you the **public key**.
        
    - You add it to `/home/memelord/.ssh/authorized_keys`, but prefix it with a command restriction:
        
        bash
        
        # In /home/memelord/.ssh/authorized_keys
        command="/usr/bin/rbash",no-port-forwarding,no-x11-forwarding,no-agent-forwarding,no-pty ssh-ed25519 AAA...your-friends-public-key
        
        This forces any connection with that key to run `rbash` (restricted bash) and disables SSH features like port forwarding.
        
3. **Restrict the User's Filesystem Access (Jail)**:
    
    - Set up the user's shell as `rbash` (edit `/etc/passwd` or use `sudo usermod -s /usr/bin/rbash memelord`).
        
    - Create a restricted environment where they can only navigate within their `meme_vault`. This involves setting up a controlled `PATH` and preventing directory traversal. A common method is to create a special `.profile` for the user.
        
4. **Use `scp` or `sftp` for File Transfer**:  
    With this setup, your friend can use `scp` or `sftp` to upload/download files to the `meme_vault` directory, but they won't get a general shell prompt to explore the system.
    

**Simpler Alternative: Use a Dedicated Tool**  
For a more robust and easier solution, consider setting up a proper **sftp-only chroot jail** using the built-in `sftp` subsystem in OpenSSH. This is designed exactly for this purpose.

Would you like me to detail the simpler `sftp`-only jail method, or the steps for the restricted command approach?
```

## FTP

### tools

> Apart from tools listed here, `wget` can be used aswell.

#### ftp (🔥 unencrypted & outdated)

> 🔥 Passwords and data travel in plain text, ***use only on trusted networks***.
> 
> Can download *only single files*.

- `ftp`
	- just run this command to enter an **interactive mode**
- commands in the interactive mode:
	- `open 192.168.1.105 2121`
	- `ls`
	- `get`
		- you can only get single files with `ftp`
		- for downloading directories seek different paths
	- `cd`

#### lftp

> ***Sophisticated file transfer program.***

- `lftp -u username -p portnumber 192.168.0.83`
	- connect with a given username, port number and host
	- this opens an interactive terminal
- `mput /path/to/file`
	- uploads a file to the server
- `mget /path/to/files/*.png`
	- both mput and mget can be used with
- `ls/pwd/mkdir/rm`
	- use typical commands within the server
# 6. 🖼️ Darstellung / Presentation

# 5. 👯 Sitzung / Session

- [ ] NetBIOS, RPC, SI

# 4. 🚂 Transport

- [ ] multiplexing

## ***Unterschied TCP vs. UDP***

| TCP                                                      | UDP                                           |
| -------------------------------------------------------- | --------------------------------------------- |
|                                                          |                                               |
| verbindungsorientiert                                    | verbindungslos                                |
| zuverlässig                                              | schneller                                     |
| more ram and bandwith usage, a lot of header information | smaller packets, lesser bandwith requirements |
| *Paket-Reihenfolge garantiert* / geordnete Pakaten       | keine Garantie                                |
| z. B. *HTTP, HTTPS, SSH*                                 | z. B. *DNS, VoIP, Streaming*                  |

## ***TCP***
- Vorteile:
	- *Bestätigungen (Acknowledgements)*
		- TCP *fügt* den gesendeten Daten zusätzliche Kontrollinformationen *hinzu*.
		- Der Server sendet *Daten*, und der Client *bestätigt deren* Empfang.
	- *Zuverlässige Übertragung (Guaranteed Delivery)*
		- Wenn keine Bestätigung empfangen wird, *sendet* TCP die Daten *erneut*.
	- *Verbindungsorientierd (Connection-based)*
		- *Vor* der Datenübertragung wird eine *eindeutige Verbindung* zwischen Client und Server etabliert und auf beiden Seiten *verwaltet*.
	- *Staukontrolle (Congestion Control)* 
		- TCP *passt* die Übertragungs*rate* an, wenn das Netzwer *überlastet* ist, und *wartet* auf bessere *Bedingungen*.
	- *Geordnete Pakete*
		- Pakete können unterschiedliche Wege nehmen, *werden aber* von TCP *nummeriert* und beim Empfänger in der richtigen Reihenfolge *zusammengesetzt*.
- Nachteile:
	- *Größere Pakete*
		- TCP hat *einen relativ* großen *Header* mit vielen Kontrollinformationen - auch bei kleinen Daten*mengen*.
	- *Höherer Bandbreitenverbrauch*
		- *Durch* die zusätzlichen Header-Daten wird mehr Bandbreite *benötigt*.
	- *Langsamer als UDP*
		- ¹Bestätigungen, ²Paketnummerierung und ³Staukontrolle verursachen zusätzsliche *Verzögerungen*.
	- *Zustandsbehaftet (Stateful)*
		- Der Server speichert Verbindungsinformationen.
		- Wenn der Server *abstürtzt* oder neu startet, *geht* dieser Zustand *verloren* und *laufende* Verbindungen *brechen ab*.
	- *Braucht mehr Server-Resourcen*
		- Da jede Verbindung Speicher benötigt, ist die Anzahl gleichzeitiger Verbindungen *begrenzt*.
		- Diese *Eigenschaft* wird *bei* DDoS-Angriffen *ausgenutzt*.

## ***UDP***
- Pros:
	- *Kleinere Pakete*
		- UDP hat einen sehr kleinen Header.
	- *Geringer Bandbreitverbrauch*
	- *Schneller als TCP*
		- Keine Verbindungsaufbau-, Bestätigungs- oder Wiederholungs*mechanismen*.
	- *Zustandslos (Stateless)*
		- Der Server speichert keine Verbindungsinformation.
- Cons:
	- *Keine Bestätigung*
		- UDP sendet Daten ohne Empfangsbestätigung.
	- *Keine garantierte Zustellung*
		- Pakete können verloren gehen, und niemand *merkt* es.
	- *Verbindungslos (Connectionless)*
		- Es gibt keine feste Verbindung zwischen Client und Server aufgebaut.
	- *Keine Staukontrolle*
		- UDP reduziert die *Sendegeschwindigkeit* nicht *bei* hoher Netzwerk*last*.
	- *Keine Reihenfolge*
		- Pakete können in falcher Reihenfolge *ankommen*.
	- *Keine **integrierte** Sicherheit*
		- UDP ***selbst*** *bietet* keine Authentifizierung oder Verschlüsselung.
		- Sicherheit muss durch andere Protokolle *realisiert* werden.

## Port

- [ ] https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Well-known_ports

***Was ist ein Port?***

> See: [[#Port Forwarding]]

https://stackoverflow.com/questions/152457/what-is-the-difference-between-a-port-and-a-socket

- In der Alltagssprache wird "Port" oft als Synonym für die ***Portnummer*** verwendet.
	- Ein Port ist ein ***logischer Endpunkt für die Netzwerkkommunikation*** auf einem Gerät.
- Es ist durch eine ***Nummer zwischen 0 und 65535*** identifiziert.
- ***Der Zweck*** von Ports besteht darin, ***mehrere Endpunkte*** *auf einer bestimmten Netzwerkadresse* zu unterscheiden.
- Portnummern sind *unterteilt* in: 
	- Well-Known (0-1023)
	- Registered (1024-4151)
	- Dynamic/Private Ports (4156 - 65535).
- Portnummern sind ein Teil von einem Socket.

***IP vs. Port***

- IP-Adresse - *welches Gerät*
	- Schicht: 2.Internet
- Port - *welsches Programm auf dieses Gerät*
	- Schicht: 3.Transport

## Socket

***Was ist ein Socket?***

- Ein Socket ist die *eindeutige, **aktive** **Kombination*** *aus* ***IP-Adresse, Transportprotokoll (TCP/UDP) und Portnummer***. 
- Es beschreibt einen Endpunkt in der Netzwerkkommunikation.
- ***Bei TCP*** wird eine Verbindung *durch* *ein* ***Socket-Paar*** (Client + Server) *definiert*. 
	- Ein Server kann mit ***einem Listening-Socket*** auf einem Port viele *parallele* ***Verbindungs-Sockets*** für verschiedene Clients *verwalten*.
- ***Bei UDP*** ist die Kommunikation verbindungslos. 
	- *Ein Paket wird von einem Socket zu einem anderen gesendet*. 
	- Die *Antwortadresse* ist ***im Paket-Header enthalten***, *sodass* kein festes Socket-Paar notwendig ist.

### tools

#### ss

>  Utility to investigate sockets.

***Flags***:

- `-l`
	- *listening* sockets
	- those waiting for connections
	- without `-l` all connected sockets will be listed, not just listening
- `-t`
	- *tcp* sockets
- `-u`
	- *udp* sockets
- `-n`
	- *numeric* addresses and ports
	- instead of trying to resolve hostnames or servicenames
		- `22` instead of `ssh`
		- `0.0.0.0` instead of `localhost`
- `-p`
	- *process*, shows proces PID and name that owns a given socket

***Examples***:

- ***LISTENING***: `sudo ss -tlunp`
	- ***Listening Sockets***: Ports, die auf *eingehende* Verbindungen _warten_.
- ***ACTIVE***: `sudo ss -tunp`
	- ***Established Sockets***: Aktive Verbindungen, die _gerade_ Daten austauschen.

## VPN (SSL/TLS VPNs like OpenVPN)

> These create a secure ***tunnel over TCP or UDP*** (Layer 4 protocols), inside of which your network traffic flows.
# 3. 🗺️ Netzwerk

- [ ] IP, ICMP, BGP

## IP

***Was ist eine IP-Adresse?***
- Eine IP-Adresse ist eine *eindeutige Adresse*, die jeden Computer, *netzwerkverbundene* Gerät oder jeden Server in einem Netzwerk identifiziert.

***Was ist der Unterschied zwischen IPv4 und IPv6?***
- IPv4: 32-Bit-Adressee, nur
- IPv6: 128-Bit-Adresse

### ⚗️ tools

#### ip
  
 > Show/manipulate routing, devices, policy routing and tunnels.  
 > 
 > Some subcommands such as `address` have their own usage documentation. 
 > 
 > More information: https://manned.org/ip.8.  
 
 ***List interfaces with brief link layer info***:  
 - `ip -brief link`  
 - it will not tell you your local address, use `ip addr` for that
  
 ***List interfaces with detailed info***:  
 - https://manned.org/man/arch/ip-address.8
 - `ip address`  oder `ip addr`
 - kann auch `ip -brief addr`, good way to get you local ip
 - you can also specify which device with:
	 - `ip addr show wlp3s0`
  
***Display the routing table***:  
 - [ ] what does that mean
 - `ip route`  
  
 ***Show neighbors (ARP table)***: 
 - see: [[#ARP]]
 - `ip neighbour`  
  
***Make an interface up/down***: 
 - [ ] what does that mean
   `sudo ip link set ethX up|down`  
   - [ ] i guess the same can be done with nmcli?
  
  ***Add/Delete an IP address to an interface***:  
 - [ ] what does that mean
   `sudo ip address add|delete ip_address/mask dev ethX`  
- [ ] using this to bypass DHCP issues:
```
# First find your router's IP (usually 192.168.1.1 or 192.168.0.1)
# Set static IP on Ethernet
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 192.168.1.1

# Test connection to router
ping 192.168.1.1
```
  
 - Add a default route:  
 - [ ] what does that mean
  `sudo ip route add default via ip_address dev ethX`

## ICMP

***Was ist ICMP?***
- *Internet Control Message Protocol*
- Es ist ein **Protokoll der Internet-Schicht** im TCP/IP-Modell
	- OSI Layer 3
- Aufgabe: **Fehler- und Statusmeldungen im Netzwerk**
- Typische Funktionen
	- **Ping / Echo Request & Reply** → prüfen, ob ein Ziel erreichbar ist
	- **Fehlermeldungen** z. B.: „Host unreachable“, „Time exceeded“
	- **Diagnose und Netzwerkmanagement**, nicht zum Datentransfer von Anwendungen
- Merksatz
	- ICMP überträgt **Kontrollmeldungen**, nicht Nutzdaten. 
	- TCP/IP transportiert die Daten – ICMP sagt nur, ob alles klappt oder Probleme auftreten.

## Subnetz

> Ein Subnetz ist ***eine Aufteiling*** eines größeren IP-Netzwerks ***in kleinere, logische Segmente***.

- [ ] gr check
- ***Hauptsächlich*** ist das ein Thema von ***lokalen Netzwerken***.
- Typische Subnetz-Größen für lokale Netzwerke:
```
/24 255.255.255.0 256 Adressen, 254 nutzbar, Standard für zu Hause

/16 255.255.0.0 60k+ Adressen, Große Unternehmen

/8 255.0.0.0 16mln.+ Adressen, Riesen-Nezwerke
```
- Aber es gibt auch Globale Subnetze:
```
8.8.8.0/24 # google DNS server
1.1.1.0/24 # cloudflare DNS server

es gibt auch mehr davon, z. B. ISPs:
62.0.0.0/8 # Telekom
```
- Warum? Sie helfen, IP-Adressen zu organisieren und den Datenverkehr zu verwalten.
- [ ] Über DMZ lernen

## VPN (IPSec, Wireguard)

> These protocols ***encrypt and encapsulate IP packets themselves***. They work with any traffic (TCP, UDP, etc.) that uses IP, making them very versatile.

# 2. 💽 Datenlink

## MAC

> MAC-Adresse, ***kurz für Media Access Control***, ist ein 48-bit-*langer*, *global eindeutiger* ***Identifier***, der jeder Netzwerkkarte (NIC) ***zugewissen*** ist.

- Sie wird *üblicherweise* als 12-*stellige* *Hexadezimalzahl* ret45r5.
	- ***OUI (Organizational Unique Identifier):*** Die ersten 6 Hex-Stellen, die von der IEEE an einen Hersteller vergeben werden.
	- ***NIC-spezifischer Teil:*** Die *verbleibenden* 6 Hex-Stellen werden von dem Hersteller *individuell pro Gerät ausgewählt*.
- MAC-Adresse gehört *zur* 2. OSI-Schicht *und ist fundamental* für die Adressierung *innerhalb* eines lokalen Netzwerk.

***Additional***:
- *Es gibt keine Garantie **auf** absolute Einzigartikgeit*: Obwohl sie weltweit eindeutig sein sollte, kan sie duch ***¹Virtuallisiereung, ²Spoofing oder ³Herstellerfehler*** *dupliziert werden*.
- *ARP-Protokoll*: Die MAC-Adresse ist *über das ARP-Protokoll* mit der IP-Adresse in lokalen Netzwerken verbunden.

## ARP

> ARP (***Address Resolution Protocol***) wirkt ungefähr als *Telefonbuch deines lokalen Netzwerks*. 
> 
> Während ***DNS*** Domainnamen (wie `google.com`) in ***öffentliche IPs*** übersetzt, übersetzt ***ARP*** diese ***lokale IPs*** in die ***physikalischen MAC-Adressen*** *der Geräte* in deinem lokalen Netzwerk (Heim- oder Büronetzwerk).

***Der Weg:***
```
INTERNET:          DNS: myname.duckdns.org → 92.123.45.67 (öffentliche IP)

DEIN ROUTER:       NAT: 92.123.45.67:2222 → 192.168.1.20:22

HEIMNETZWERK:      ARP: 192.168.1.20 → MAC b8:27:eb:12:34:56 (dein Gerät)
```

```
1. Dein PC weiß: "Ich will zu 192.168.1.20"
2. PC ruft im Netzwerk: "HEY! Wer hat 192.168.1.20?"  ← ARP Request (Broadcast)
3. Dein SSH-Server antwortet: "Ich bin 192.168.1.20, meine MAC ist a4:bb:6d:cc:88:11" ← ARP Reply
4. Dein PC merkt sich: "192.168.1.20 = a4:bb:6d:cc:88:11"  ← ARP Cache
5. Jetzt kann dein PC Daten direkt an diese MAC-Adresse senden
```

> See [[#DHCP vs. ARP]].

> ***Adress Resolution Protocol***: [[#ARP]]
- For: Devices using ***static*** IPs (*manual configuration*)
	- [ ] how can i set local static IP
- Asks: ***Who has this IP? Tell me your MAC!***
- Checks: If a given IP is already in use on the network.
	- `arping -c 3 -D 192.168.0.28`
	- [ ] but just doing `ping` on that address kindof did the same job or no?
- OSI 2.: Link
### tools

#### ip

- `ip neighbour`
	- ARP-Tabelle anzeigen
- `ip neighbour flush all`
	- ARP-Cache leeren

#### arp

https://man7.org/linux/man-pages/man8/arp.8.html

#### arping

https://man7.org/linux/man-pages/man8/arping.8.html

#### arp-scan

- `sudo arp-scan --interface=wlp2s0 --localnet`
	- will tell you who is on your network right now
	- first find your network device with `ip -brief link`
## Ethernet
- Was ist ***Ethernet***? - Name dier Netzwerktechnologie
	- Eine **kabelgebundene Netzwerktechnologie** für LANs (Layer 2).
# 1. Physicalisch

## Funk Signal

- Was ist ***Funk-Signal***?
	- Ein drahtloses Signal, das Informationen mithilfe elektromagnetischer Wellen durch die Luft überträgt.
	- Die Informationen werden auf **Radiowellen** aufmoduliert.
	- Wird z. B. bei **WLAN, Bluetooth, Mobilfunk** verwendet.
	- Funksignale gehören zur **Physical-Schicht**
	- Sie übertragen **Bits (0 und 1)** als elektromagnetische Wellen

# Besondere Koncepte

## NAT

> 

***NAT ist ein "real-world hack" - Der praktische Workaround für IPv4-Knappheit***

```
# Ohne NAT (theoretisch sauber):
Jedes Gerät bekommt öffentliche IPv4
→ Benötigt 4+ Milliarden IPs
→ Gibt es nicht!

# Mit NAT (praktischer Workaround):
Ein Router: 1 öffentliche IP
├── PC: 192.168.1.100:54321 → Router:85.x.x.x:12345
├── Phone: 192.168.1.101:12345 → Router:85.x.x.x:54321
└→ Beide nutzen dieselbe öffentliche IP!
```

***An example process:***

- Client wants to make a GET request on some server.
- It starts it with its own local address. 
	- **But attaches the router's MAC Address to the sent Frames.**

***Übung***
```
# 1. Zeige deine private IP
ip addr show

# 2. Zeige deine öffentliche IP (dank NAT!)
curl ifconfig.me

# 3. Teste ob du hinter NAT bist
# Auf deinem Router müsstest du sehen:
# Private: 192.168.1.100:54321 → Public: 85.x.x.x:12345

# 4. Simuliere NAT-Probleme
# Blockiere ausgehende Ports (wie manche Firewalls)
sudo iptables -A OUTPUT -p tcp --dport 80 -j DROP
# → Webseiten laden nicht mehr
# → Das ist NAT/Firewall-Verhalten!
```
- [ ] DMZ and subnets
- [ ] upv6 vs. nat

`sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.254.47:8080`


### Private to Public IP translation

### Port Forwarding

***Was ist Port Forwarding?***

- Port Forwarding ist *eine Regel in deinem Router*, die *eingehende Verbindungen* von außen (Internet) *an einen bestimmten Computer* in deinem *lokalen Netzwerk (LAN) weiterleitet*.
- ***Ohne Port Forwarding*** *blockiert die Firewall deines Routers (NAT) alle eingehenden Verbindungen aus dem Internet* – das ist die Standardeinstellung aus Sicherheitsgründen.
- Mann musst beide `WAN` und `LAN` Ports konfigurieren.
	- `WAN` 
		- *ein externes Port*
		- it's the port number that people on the internet will use
	- `LAN` 
		- *ein privates Port*
		- it's the port your server is actually listening on
	- zuhause du wirst beide oft zu dem gleichen Port *einstellen*
```
Internet ──┬── [WAN Port: 39335] 
           │
        [YOUR ROUTER]
           │
           └── [LAN Port: 39335] ──► Server (192.168.0.28)
```

### L4 Load Balancing

## Firewall

- [ ] iptables
- [ ] ufw
- [ ] security groups

***Was ist ein Firewall?***
- Eine Firewall oder eine *Brandwand* ist ***ein Sicherungssystem***, das ein ***Computernetzwerk/Rechnernetz*** oder einen einzelnen Computer von *unerwünschten Netzwerkzugriffen* schützt.
- Es gibt beide software- und hardwarebasierte Firewalls.

### tools

#### ufw

>  ***Uncomplicated firewall***
>  
>  It should be noted that UFW can use either [iptables](https://archlinux.org/packages/?name=iptables) or [nftables](https://archlinux.org/packages/?name=nftables) as the back-end firewall. Users accustomed to calling UFW to manage rules do not need to take any actions to learn underlying calls to iptables or to nftables thanks to `nft` accepting iptables syntax, for example within `/etc/ufw/before.rules`.

- [ ] iptables / nftables

```bash
sudo pacman -S ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222  # Use your SSHD-Port config.
sudo ufw allow 8000   # z.B. Dein Python-Server
sudo ufw enable
```

- `sudo ufw status numbered verbose`
	- *this will display nothing unless ufw is enabled*
	- `numbered` 
		- useful because you can use those numbers do delete parts of the config
		- `sudo ufw delete 3`
			- delete rule 3
	- `verbose`
		- will show all underlying default config apart from what you set manually
- `sudo ufw enable`
	- start the ufw
	- note you will probably like to also set systemd to start it automatically
		- `sudo systemctl enable ufw`
- `sudo ufw default deny`
	- this will set default *incoming* connections to deny
	- *you can see how this is set when you run status verbose*
- `sudo ufw allow from 192.168.0.0/24`
	- allow any protocol from inside a 192.168.0.1-225 LAN
- `sudo ufw limit 39335/tcp`
	- will allow tcp on 39335 with limitation of subsequent connections to help against brute force attempts
	- ***this is basically how you allow an ssh connection through a specified port since it only uses tcp and you shouldn't use default port number**
	- if you don't specify `/tcp` this will allow both tcp and udp
		- [ ] anything else?
	- you can verify this with [[#nmap]] from another local pc with:
		- `-sSU` will explicitly test for both tcp and udp
```
λ sudo nmap -sSU -p 39335 192.168.0.28  
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-02-03 19:29 CET  
Nmap scan report for doomhamster (192.168.0.28)  
Host is up (0.0038s latency).  
  
PORT      STATE  SERVICE  
39335/tcp open   unknown  
39335/udp closed unknown  
MAC Address: 14:DA:E9:D1:1D:56 (ASUSTek Computer)  
  
Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
```
- `ufw allow Deluge`
	- [ ] wtf?
# Analysis

### netcat

> ***Swiss Army knife of networking tools.***
> 
> Basic syntax: `nc [options] host port`

- `nc host port`
	- by default will attempt to make a TCP connection to a specified host and port
- `nc -u host port`
	- UDP connection
- `nc -z host 21-2222`
	- scan for open ports
	- in specified range
	- can also check a single port
- `-v`
	- verbose

Resources:
- https://linuxize.com/post/netcat-nc-command-with-examples/

### nmap

***host discovery***
- `nmap 192.168.0.0/24`
	- most basic local scan
- `--packet-trace`
	- see every packet that nmap sends and receives
- you can also point it at *public addresses* like:
	- `nmap scanme.nmap.org`
	- `nmap somepublicip`
- `nmap -sn 192.168.0.0/24`
	- ping-scan (disable port scan)
		- [ ] it sends *ICMP ping packets* (layer 3) and waits for responses, which is slower
		- 
	- *first find the local network range with*:
		- `ip route`
		- sth like `192.168.0 or 1.0/24`
	- ***0/24 will scan from 1 - 254***
		- this is the common range for home networks, since it supports 255 devices which usually plenty enough
		- [ ] 0 and 255 are not usable
		- [ ] this is called CIDR notation
```
Most common local networks
nmap -sn 192.168.0.0/24    # Many routers default
nmap -sn 192.168.1.0/24    # Also common default
nmap -sn 10.0.0.0/24       # Google Wi-Fi, some ISPs
nmap -sn 10.0.1.0/24       # Apple routers
```
- If you want to find local adresess you might be better/faseter off with `arp-scan` or `ip neighbour`.


# Linux Network Manager

>  Remember you can also get info about your network adapters with `ip -brief link` .

## nmcli

- [ ] is it the one most commonly used on linux? what are the alternatives and why are some more used than others?
- [ ] why does arch linux image come with iwd but then after installation is complete there is no more iwd but nmcli

***nmcli connection***:

>  All network data is stored as connections.

- `nmcli connection`
	- alone will work as `show` 
- `nmcli connection show`
	- will show all connections
	- `nmcli connection show NAME`
		- will show very detailed information on that connection
- `nmcli connection delete NAME`

***nmcli device***:

>  Show and manage network interfaces.

- `nmcli device`
	- on its own will give an outlook on devices and their status
	- `ifname` below stands for device name that you can find here
- `nmcli device show ifname`
	- detailed info about a device
- `nmcli up/down ifname`
	- connect/disconnect a device
- `monitor`
- `nmcli device wifi`
	- ***important***
	- alone will list available wifi acces points
	- `nmcli device wifi connect wifiname/bssid`
- `nmclid set ifname`
	- `autoconnect yes/no`
	- `managed yes/no`

## iwd

>  If all you want is wifi. Simpler than Network Manager.
# Security Tools


### fail2ban

- get or update `fail2ban`
- create a local config file, don't use the default one because it might get overwritten in an update:
	- `cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`
- edit those settings:
	- for an ssh server:
```sql
[sshd]
enabled   = true

--- change to the port your server is using
--- THIS IS CRITICAL, otherwise fail2ban will not work and ban nothing on your ssh server
port      = 39335

-- Use the SSH pattern-matching rules to identify failed login attempts in my logs
--- This filter is here: cat etc/fail2ban/filter.d/sshd.conf
filter    = sshd

--- where logs go
logpath   = /var/log/fail2ban.log

--- Number of failed attempts before banning (stricter than default 5)
maxretry  = 3
bantime   = 86400

--- Time window to count failures (600 seconds = 10 minutes)
findtime  = 600
```
- check the `fail2ban` service status:
	- `systemctl status fail2ban`
	- enable it with:
		- `sudo systemctl enable fail2ban`
- ***IMPORTANT, config changes***
	- restart the service after you change config settings
	- `sudo systemctl restart fail2ban`
- ***IMPORTANT, check if jail is active***
	- `sudo fail2ban-client status`
	- below is a correct status if you run fail2ban on your ssh server
```bash
❯ sudo fail2ban-client status  
Status  
|- Number of jail:      1  
`- Jail list:   sshd
```
- watch the activity with:
	- `sudo tail -f /var/log/fail2ban.log`
### rkhunter

> Searches for rootkits and malware.

- `sudo rkhunter --check`
- `sudo rkhunter --update`

### lynis

> System and security auditing tool.

- `sudo lynis update info`
- `sudo lynis audit system`
	- run a system audit
- `sudo lynis audit dockerfile path/to/dockerfile`
	- run an audit of a dockerfile

# Processes

## Öffentlicher Server von zu Hause

> Below are the minimal steps to make the server rather safe and usable.
- 1. [[#openssh]] first see necessary config steps for the ssh server
- 2. [[#ufw]] set up software firewall
- 3. [[#fail2ban]] https://github.com/fail2ban/fail2ban benutzen, weil *innerhalb von nur wenigen Minuten* Angriffe werden anfagnegn.
	- Bots suchen ständig nach neuen öffentlichen IPs
	- SSH-Brute-Force beginnt innerhalb von 5-10 Minuten
	- Web-Crawler und Vulnerability-Scanner folgen schnell
	- Du kannst Statistiken hier finden:
```
# Nach 1 Stunde öffentlichem SSH (Port 22):
sudo grep "Failed password" /var/log/auth.log | wc -l
# Ergebnis: ~50-100 Versuche

# Nach 24 Stunden:
# ~1000-5000 SSH-Brute-Force-Versuche
```
- ***fail2ban*** configuration für SSH:
```
# Backup der originalen Konfiguration
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

# Konfiguration bearbeiten
sudo nano /etc/fail2ban/jail.local

[DEFAULT]
# Verbannung auf 1 Stunde (3600 Sekunden)
bantime = 3600
# Maximale Fehlversuche
maxretry = 5
# Zeitfenster für Fehlversuche
findtime = 600

[sshd]
enabled = true
port = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
```
- für Node-Server:
```
[DEFAULT]
# Verbannung auf 1 Stunde (3600 Sekunden)
bantime = 3600
# Maximale Fehlversuche
maxretry = 5
# Zeitfenster für Fehlversuche
findtime = 600

[sshd]
enabled = true
port = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
```
- für eigenen Server:
```
sudo nano /etc/fail2ban/filter.d/my-http.conf

[Definition]
failregex = ^<HOST> -.*"(GET|POST|HEAD).*" (404|403|500) .*$
ignoreregex = 
```
- wichtige fail2ban Befehlte:
```
# Status von fail2ban prüfen
sudo fail2ban-client status
# Alle aktiven Jails sehen
sudo fail2ban-client status sshd
# Spezifische Jail prüfen

# Manuell IPs verbannen/freigeben
sudo fail2ban-client set sshd banip 123.456.789.0
sudo fail2ban-client set sshd unbanip 123.456.789.0

# Logs von fail2ban
sudo journalctl -u fail2ban -f
```
- Außerdem solltest du auf folgenden SSH-Configurations beachten:
```
sudo nano /etc/ssh/sshd_config

# Wichtige Änderungen:
Port 2222  # Standard-Port ändern
PermitRootLogin no
PasswordAuthentication no  # Nur SSH-Keys!
PubkeyAuthentication yes
AllowUsers dein_benutzername
MaxAuthTries 3
```

# WLAN

- [ ] DHCP pool
	- [ ] leases
	- [ ] range 192.168.100-200
- [ ] Link Rate
- [ ] data transfer TX/RX rates
- [ ] double NAT issues when connecting to a hotspot that passes wifi connection
	- [ ] repeater?
	- [ ] can you get information about how the hotspot is configured?
- [ ] channel width 
	- [ ] 20Mhz vs 40Mhz vs. mixed auto
- [ ] security WPA2-PSK (AES) vs. WPA3 vs. mixed
- [ ] WifiMode
	- [ ] setting laptop router to `802.11g only` isntead of `mixed`
	- [ ] "802.11b/g/n/ac/ax mixed" to **"802.11b/g mixed"** or **"802.11g only"**
	- [ ] 2.4GHz band vs 5GHz band
- [ ] setting laptop router channel 6 vs 1 vs 11
- [ ] wifi WMM (WiFi-Multimedia/QoS)
- [ ] SGI - Short Guard Interval
- [ ] Mac Adress Filtering
- [ ] SSID vs BSID
- [ ] MIMO Power Save
- [ ] A-MPDU Aggregation

## tools

- [ ] nmap
- [ ] sudo journalctl -u NetworkManager
	- [ ] as a debugging tool for seeing what is happening with devices

# downloading stuff


## Diffie-Hellman

## AES

## SHA

## TLS


# Cryptography

- https://www.youtube.com/playlist?list=PLLFcS83smQEzbPU-ftHRGQELoSijqEImI

