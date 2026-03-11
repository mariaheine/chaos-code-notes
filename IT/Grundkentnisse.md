
# System

## Grundlage
---

***Fragen***:
- [x] Was ist ein Betriebssystem?
- [x] Was ist ein Prozess?

---

***Was ist ein Betriebssystem?***
- Ein *Betriebssystem (OS)* ist die Software, die die *Hardware des Computers steuert* und es anderen Programmen *erlaubt, zu laufen*.

***Was ist ein Prozess?***
- Ein Prozess ist ein Programm oder ein Teil eines Programms, 
	- das im Hintergrund läuft 
	- und bestimmte Funktionen bereitstellt.

## Linux
---

***Fragen***:
- [ ] Was ist Linux?
- [ ] Warum benutzt du Linux?
- [ ] Was ist Shell?
	- [x] Was ist Bash?

---


- **Was ist Linux?**
	- Linux ist eine Famile *von* Open Source *unix-like* *Betriebssystemen*, die auf dem Linux-kernel *basieren*. 
	- Es gibt viele verschiedene Distributionen, die für *unterschiedliche* Aufgaben geeignet sind. 
	- Zum beispiel Debian, Red Hat und Ubuntu *gut für den Einsatz als Server* geeignet, *während* Kali Linux *vor allem* für IT-Sicherheit und Penetrationstests *eingesetzt* *wird*.

Was ist Shell?

***Was ist Bash?***
- Eine Shell, über die man Befehle ausführt und Automatisierungen über Skripte erstellen kann.


- **Was ist Kommandozeile?**
	- Kommandozeile ist eine textbasierte Art, den Computer zu steuern.

- **Warum nutzt du Linux?**
	- Ich arbeite täglich mit Linux, weil es flexibel und *Open Source* ist. Außerdem kann ich viele Aufgaben über die *Kommandozeile* automatisieren, zum Beispiel welche Prozesse *beim Start des Computers* automatisch *ausgeführt* werden sollen.


- **Was ist der Unterschied zwischen Linux, Windows und macOS?**
	- Alle drei sind Betriebssysteme, *unterschieden sich jedoch in* Linzenz, Aufbau und Einsatzbereich.
	- Linux ist ein unix-like Betriebssystem, es Open Source, flexibel und stabil ist und häufig *auf Servern* und *in der Cloud* eingesetzt wird. Viele Menschen - auch ich - nutzen Linux jedoch täglich auch als *persönliches* Desktop-Betriebssystem.
	- Windows ist proprietär, *weit verbreitet* im Desktop-Bereich und in *Unternehmensumgebungen*, vor allem *in Kombination mit* Microsoft-Produkten wie Microsoft Office.
		- Windows wird mit Windows Server auch im Serverbereich eingesetzt, aber da es proprietär ist, gibt es weniger Transparenz als bei Open-Source-Systemen.
	- macOS ist ebenfalls unix-like, jedoch proprietär und *läuft ausschließlich auf* Apple-Hardware, was macht es fast nutzlos im Server-Bereich.
- Was ist Unix?
- **Was ist Open Source?**
	- Open Source bedeutet, dass der *Quellcode* einer Software *offen zugänglich* ist und *gemeinschaftlich weiterentwickelt* werden kann.

# Netzwerk

## Grundlage
---

***Fragen***:
- [x] Was ist ein Netzwerk?
- [x] Was ist ein Server?
- [x] Was ist der Unterschied zwischen einem Client und einem Server?
- [x] Was ist ein Mainframe?
- [ ] Was ist ein Router?
	- [ ] Was ist ein Switch?
- [ ] Was ist LAN?
	- [ ] Was ist WLAN?
	- [ ] Was ist WiFi?
	- [ ] Was ist Bluetooth?
	- [ ] Was ist Funk-Signal?
	- [ ] Was ist Ethernet?
- [ ] Was ist eine URL?
- [x] Was ist eine IP-Adresse?
	- [ ] Was ist der Unterschied zwischen IPv4 und IPv6?
	- [ ] Wie wird eine öffentliche IP-Adresse in lokale IP-Adresse übersetzt?
- [x] Was ist eine MAC-Adresse?
	- [x] IP vs. MAC
	- [ ] Was ist ARP?
- [ ] Was ist Virtualisierung?

---

***Was ist ein Netzwerk?***
- Ein Netzwerk ist *eine Verbindung von mehreren* Computern oder Geräten, die miteinander kommunizieren und Daten austauschen.

***Was ist ein Server?***
- Ein Server ist ein Computer oder *ein Softwaredienst*, der *über ein Netzwerk* Ressourcen, Daten oder Funktionen für andere Computer – ***sogenannte*** Clients – *bereitstellt*.
- Ein Webserver stellt zum Beispiel Webseiten bereit, die *vom Browser angefragt und dargestellt* werden, während ein Datenbankserver *strukturierte Daten speichert* und diese ***auf Anfrage*** *an Anwendungen* *zurückgibt*.
- Server verwenden ***meist*** ein Linux-basiertes Betriebssystem.

***Was ist der Unterschied zwischen einem Client und einem Server?***
- Ein Client fragt Daten oder Dienste an, und ein Server stellt diese zur Verfügung.

***Was ist ein Mainframe?***
- Er ist ein *besonders leistungsstarker* und zuverlässiger Computer.
- Mainframes werden von großen Organisationen für ***(unternehmens)kritische*** Aufgaben verwendet.

### LAN

***Was ist LAN?*** – _Local **Area** Network_
- Ein *lokales Netzwerk* in einem *begrenzten Bereich*, z. B. Zuhause oder im Büro.
- Solche netzwerk funktioniert durch Ethernet und WLAN.

***Was ist WLAN?*** – _Wireless Local Area Network_
- Ein *drahtloses LAN*, das Geräte *per Funk* verbindet.

### IP & MAC

***Was ist eine IP-Adresse?***
- Eine IP-Adresse ist eine *eindeutige Adresse*, die auf dem *Internet Protocol* die basiert. 
- Sie *identifiziert* jeden Computer, *netzwerkverbundene* Gerät oder Server in einem Netzwerk.
- Wir können von lokalen und öffentlichen IP-Adressen spechen.
- Außerdem gibt es ein Unterschied zwischen IPv4 und IPv6.

***Was ist eine MAC-Adresse?***
- MAC-Adresse, ***kurz für Media Access Control***, ist ein 48-bit-*langer*, 
- *global eindeutiger* ***Identifier***,
- der jeder Netzwerkkarte (NIC) ***zugewissen*** ist.

***IP vs. MAC***
- IP-Adressen werden auch im lokalen Netzwerk genutzt, 
	- weil Anwendungen und Protokolle auf IP basieren, 
- während MAC-Adressen *nur für die technische Zustellung* ***innerhalb*** eines *einzelnen Netzwerksegments* verwendet werden.
	- Meist nach einer ARP-Auflösung.

Was ist Ethernet?
Was ist LAN?
Was ist der Unterschied zwischen Server und Backend?
Was passiert, wenn ich eine Webseite aufrufe?

Was ist ein Cookie?
„Wie kommunizieren Client und Server?“

## Fortgeschrittene
---

***Fragen***:

- [ ] Was passiert, wenn du eine *URL in den Browser eingibst*?
      Was passiert, wenn man *eine Webseite aufruft*.
- [ ] Was ist HTTP?
	- [ ] HTTP vs. HTTPS?
- [ ] Was ist ein Datenpaket?
- [ ] Was ist TCP/IP?
- [ ] Was ist OSI?
- [ ] Was ist TCP?
	- [ ] TCP vs. UDP?
- [ ] Was ist eine Domain?
	- [ ] Was ist DNS?
	- [ ] Wie ist ein DNS-Resolver?
- Was ist ein VPN?
Was ist ein Reverse Proxy? Was ist Load Balancer?
3️⃣ **Was passiert technisch beim Login auf einer Webseite**


---

*Was passiert, wenn du* ***eine URL in den Browser eingibst***?
	*Was passiert, wenn man **eine Website aufruft**?*
1. Der Browser fragt **über DNS die IP-Adresse** der Domain ab.
2. Es wird eine **TCP-Verbindung** zum Server aufgebaut (meist über Port 80/443).
3. Der Browser sendet eine **HTTP-Anfrage** (GET oder POST).
4. Der Server antwortet mit **Daten (HTML, CSS, JS, Bilder …)**.
5. Der Browser **zeigt die Webseite an**.

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

***Was ist ein DNS-Resolver?***
- *Was?* 
	- Ein DNS-Resolver ist *ein Dienst oder ein Server*, der ***Namensauflösung*** (DNS Resolution) *durchführt*.
- *Hauptaufgabe?* 
	- Seine *Hauptaufgabe* ist die ***rekursive Abfrage***, ***bei der er*** eine *angefragte* Domain in die *zugehörige* IP-Adresse übersetzt.
- *Wo?* 
	- Er kann selbst gehostet sein, aber meistens benutzen wir entweder den von unserem ISP oder öffentlichen Resolver wie den von Cloudflare (`1.1.1.1`).



- **Warum Linux für Server?**
	- Linux ist häufig für Server eingesetzt, weil es sehr *stabil, sicher und resourceschonend* ist.
	- Außerdem kann es lange *ohne Neustart* laufen. Selbst 24/7-Betrieb ist möglich.
	- *Es lässt sich* gut automatisieren und administrieren *über* die Kommandozeile.
	- Es ist sehr *anpassbar* und wird von vielen Server- und Cloud-Technologien *unterstützt.*

## Weiterreichend
---

***Fragen***:

- How does Diffie-Hellman work?

---

### NAT

Was ist NAT?
Warum funktionieren private IP-Adressen dank NAT?
- „Warum braucht IPv6 kein NAT?“
- „Was ist Port Forwarding?“
- „Unterschied NAT vs Firewall?“
- „Warum sind eingehende Verbindungen schwierig bei NAT?“
- „Was passiert bei Peer-to-Peer hinter NAT?“
# Computersprache TODO


# SORT basic internet knowledge

List of questions:
- [x] Was ist ein Netzwerk?
- [x] Was ist ein Server?
- [x] Was ist ein Mainframe?
- [x] Was ist ein Protokoll?
- [ ] Was ist ein Router?
- [ ] Was ist ein Switch?
- [ ] Was ist eine Netzwerkkarte?
- [ ] Was ist ein Firewall?
- [ ] Was bedeutet LAN?
	- [ ] Was bedeutet WLAN?
- [ ] Was bedeutet Virtualisierung?





***Was ist ein Protokoll?***
- Ein *Protokoll* ist eine *Regelsammlung*, die beschreibt wie Daten zwischen Computern *ausgetauscht* werden sollen.
- Beispiele: HTTP, TCP, UDP, FTP, SMTP

***Was ist eine Netzwerkkarte?***
- *Auch genannt Netzwerkadapter* oder NIC für englisch Network Interface Controller.
- Ihre *primäre Aufgabe* ist die *Herstellung* einer ***physikalischen Verbindung*** zum Netzwerk.
- Jede hat ihren eigenen eindeutigen MAC-Identifier.


- Was ist ***IEEE 802.11***? – _Institute of Electrical and Electronics Engineers, Standard 802.11_
	- Der **IEEE-Standard** für WLAN (z. B. 802.11n, ac, ax).

- What is bandwidth?
	- **Maximale Datenmenge**, die pro Zeit übertragen werden kann
	- Einheit: **bit/s (z. B. Mbit/s)**
	- Vergleich: _Breite einer Autobahn_
- What is latency? die Latenz
	- **Verzögerung**, bis ein Paket von Sender zu Empfänger kommt
	- Einheit: **Millisekunden (ms)**
	- Vergleich: _Fahrzeit auf der Autobahn_


- Was ist SMTP? – _Simple Mail Transfer Protocol_
	- Ein **Protokoll zum Versenden von E-Mails**.
- Was ist HTTP? – _Hypertext Transfer Protocol_
	- Ein **Protokoll zur Übertragung von Webseiten**.
- Was ist HTTPS? – _Hypertext Transfer Protocol Secure_
	- **HTTP mit Verschlüsselung** durch TLS.
- Was ist TLS? – _Transport Layer Security_
	- Ein **Verschlüsselungsprotokoll** für sichere Datenübertragung.
- Was ist FTP? – _File Transfer Protocol_
	- Ein **Protokoll zur Dateiübertragung** zwischen Client und Server.
- Was ist VPN? – _Virtual Private Network_
	- Ein **virtuelles privates Netzwerk**, das eine **verschlüsselte Verbindung** über ein unsicheres Netz herstellt.
- Was ist MAC? – _Media Access Control_
	- Eine **eindeutige Hardware-Adresse** einer Netzwerkschnittstelle.

- What is packet switching?
	- Daten werden in **kleine Pakete** zerlegt
	- Pakete können **unterschiedliche Wege** nehmen
	- Am Ziel werden sie **wieder zusammengesetzt**
- What does “connection-oriented” mean?
	- Es wird vor der Datenübertragung eine Verbindung aufgebaut
	- Die Verbindung wird verwaltet (Zustand, Reihenfolge, Bestätigungen)
	- Beispiel: TCP
- What is a certificate?
	- Ein **digitales Zertifikat** bestätigt die **Identität eines Servers**
	- Enthält u. a.:
	    - öffentlichen Schlüssel
	    - Servername
	    - Signatur einer **Zertifizierungsstelle (CA)**
	- Wird bei **HTTPS/TLS** verwendet
- What is TLS/SSL?
	- **TLS (Transport Layer Security)** = Verschlüsselungsprotokoll
	- **SSL** ist der **veraltete Vorgänger**
	- Sorgt für:
	    - **Verschlüsselung**
	    - **Integrität**
	    - **Authentizität**
	- Wird z. B. bei **HTTPS** genutzt

- What is throughput? - *Durchsatz*
	- **Tatsächlich übertragene Datenmenge pro Zeit**
	- Meist **kleiner als die Bandbreite**
	- Vergleich: _Wie viele Autos wirklich ankommen_
- What is packet loss?
	- **Pakete gehen unterwegs verloren**
	- Der Empfänger erhält sie **nicht**
	- Kann zu:
	    - Ruckeln
	    - Verbindungsabbrüchen
	    - Qualitätsverlust führen
- What is jitter?
	- **Schwankende Verzögerung** zwischen Paketen
	- Besonders problematisch bei:
	    - VoIP
	    - Video-Calls
	- Vergleich: _Unregelmäßige Ankunftszeiten_




***Was ist ein Cookie?***
- **Kleine Textdatei**, die vom Server im Browser gespeichert wird
- Speichert **Informationen über den Nutzer**
	- z. B. Login-Status oder Einstellungen
- Wird *bei jeder Anfrage* an denselben Server *zurückgeschickt*.

