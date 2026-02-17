# Fernzugriff auf die Firma mit Always On VPN erlernen  
# *Learn how to access your company remotely with Always On VPN*

**Always On VPN** ist eine VPN-Technologie von Microsoft, die eine dauerhafte und automatische VPN-Verbindung zwischen einem Gerät und dem Firmennetzwerk herstellt.

Im Gegensatz zu klassischen VPN-Lösungen, die man manuell starten muss, wird die Verbindung automatisch aufgebaut, im Hintergrund aufrechterhalten und bei einer Unterbrechung selbstständig wiederhergestellt. Der Benutzer muss nichts aktiv einschalten.

Always On VPN wird vor allem in Unternehmen eingesetzt, um einen sicheren Remote-Zugriff auf interne Ressourcen zu ermöglichen, Homeoffice- oder Außendienstgeräte dauerhaft abzusichern und Sicherheitsrichtlinien zentral durchzusetzen.

Typische Merkmale sind die Integration in Windows 10 und Windows 11, die Unterstützung moderner Sicherheitsprotokolle wie IKEv2, sowie die Möglichkeit zur geräte- oder benutzerbasierten Authentifizierung. Die Verwaltung erfolgt meist über MDM-Lösungen oder Gruppenrichtlinien.

Kurz gesagt sorgt Always On VPN dafür, dass ein Unternehmensgerät automatisch und dauerhaft sicher mit dem Firmennetzwerk verbunden ist.  

*Always On VPN is a VPN technology from Microsoft that establishes a permanent and automatic VPN connection between a device and a corporate network.*

*Unlike traditional VPN solutions that must be started manually, the connection is built automatically, maintained in the background, and re-established automatically if it drops. The user does not need to activate anything.*

*Always On VPN is primarily used in enterprise environments to provide secure remote access to internal resources, protect remote and home office devices, and centrally enforce security policies.*

*Typical features include integration into Windows 10 and Windows 11, support for modern security protocols such as IKEv2, and the ability to use device-based or user-based authentication. It is commonly managed through MDM solutions or group policies.*

*In short, Always On VPN ensures that a corporate device is automatically and continuously connected securely to the company network.*

---

**PPTP (Point-to-Point Tunneling Protocol)**  
Ein älteres VPN-Protokoll, das einfach einzurichten ist, aber als unsicher gilt. Verwendet TCP-Port 1723 und GRE für die Tunnelung.

**IKEv2 (Internet Key Exchange v2)**  
Modernes VPN-Protokoll, besonders stabil bei Verbindungswechseln (z. B. zwischen WLAN und Mobilfunk). Unterstützt starke Verschlüsselung und wird oft mit IPsec kombiniert.

**SSTP (Secure Socket Tunneling Protocol)**  
VPN-Protokoll von Microsoft, das SSL/TLS über TCP-Port 443 verwendet. Vorteil: funktioniert auch durch Firewalls, die andere VPN-Protokolle blockieren.

**GRE (Generic Routing Encapsulation)**  
Tunnelprotokoll, das Pakete kapselt, um sie über ein IP-Netz zu transportieren. Wird z. B. von PPTP verwendet, selbst aber nicht verschlüsselt.  


*PPTP (Point-to-Point Tunneling Protocol)*  
*An older VPN protocol that is easy to set up but considered insecure. Uses TCP port 1723 and GRE for tunneling.*

*IKEv2 (Internet Key Exchange v2)*  
*A modern VPN protocol, particularly stable during connection changes (e.g., switching between Wi-Fi and mobile networks). Supports strong encryption and is often paired with IPsec.*

*SSTP (Secure Socket Tunneling Protocol)*  
*A Microsoft VPN protocol that uses SSL/TLS over TCP port 443. Advantage: works through firewalls that block other VPN protocols.*

*GRE (Generic Routing Encapsulation)*  
*A tunneling protocol that encapsulates packets for transport over an IP network. Used by PPTP, but does not provide encryption by itself.*

```markdown
In der heutigen Übungen arbeiten mit dem VMs, die wir in der vorherigen Übung vpn_and_routing erstellt und konfiguriert haben  

*In today's exercises, we will work with the VMs that we created and configured in the previous exercise, vpn_and_routing.*
```

### Einrichtung einer IKEv2-Infrastruktur  
### *Setting up an IKEv2 infrastructure*

- Der „DC“ wird Zertifizierungsstelle.  
- „Server1“ wird Netzwerkrichtlinien-Server.  
- „Server2“ wird als VPN-Server konfiguriert.  
- „Server2“ enthält zusätzlich eine Website für AIA- und CDP-Informationen der Zertifizierungsstelle.  
- „Server3“ hat einen DNS-Server, der den Namensraum „gfnlab.test“ im fiktiven Internet bereitstellt.  
- W11 (Client) stellt eine VPN-Verbindung (basierend auf IKEv2) zum Firmennetz her.  

- *The "DC" will act as a certification authority.*  
- *"Server1" will serve as the network policy server.*  
- *"Server2" will be configured as a VPN server.*  
- *"Server2" will also host a website for the certification authority's AIA and CDP information.*  
- *"Server3" has a DNS server that provides the "gfnlab.test" namespace in the fictional Internet.*  
- *W11 (Client) establishes a VPN connection (based on IKEv2) to the corporate network.*

```markdown
- Erstelle die Gruppen „VPNUser“, „NPS-Server“ und „VPN-Server“ in einer neuen OU „VPN“.  
- Nimm Karl Klammer in die Gruppe „VPNUser“ auf, „Server2“ in die Gruppe „VPN-Server“ und „Server1“ in die Gruppe „NPS-Server“ auf.  
- Konfiguriere „Server2“ mit einer Website „Certs“ mit dem Pfad „C:\Certs“, die unter Port 8088 erreicht werden soll. Lass das „Double Escaping“ auf der Webseite zu.  
- Gebe den Ordner „C:\Certs“ frei und berechtige die Domänen-Admins und den „DC“ mit Vollzugriff.  
- Erstelle eine Firewall-Regel „CDP-AIA-Webfreigabe“, die TCP-Port 8088 eingehend in allen Profilen zulässt.  
- Erstelle in Routing und RAS einen eingehenden Paketfilter, der auf der Schnittstelle mit der Adresse 172.16.0.1 den TCP-Port 8088 für den Zugriff von jedem beliebigen Port und jeder beliebigen IP-Adresse auf die Adresse 172.16.0.1 zulässt.  
- Erstelle eine Organisations-Stammzertifizierungsstelle „GfnLab-Enterprise-Root-CA“ mit einer Lebensdauer von 10 Jahren, 4096 Byte Schlüssel und Hashalgorithmus SHA256.  
- Füge als AIA und CDP-Verteilungspunkte die URL http://server2.gfnlab.test/certs entsprechend hinzu.  
- Starte die Zertifizierungsstelle neu und erstelle eine Sperrliste.  
- Kopiere die Sperrlisten (.crl) und das Zertifizierungsstellenzertifikat (.crt) in die Freigabe Certs auf „Server2“.  
- Aktiviere die automatische Zertifikatsregistrierung für Computer und Benutzer auf der Domänenebene mit einem neuen Gruppenrichtlinienobjekt „Automatische Zertifikatsregistrierung“.  
- Erstelle das Gruppenrichtlinienobjekt „Clientcomputer IPSEC Zertifikate“ zur automatischen Registrierung eines IPSEC-Zertifikats.  
- Erstelle die Vorlagen für die benötigten Zertifikate für die „VPN-User“, „VPN-Server“ und „NPS-Server“, sowie für die Clientrechner und registriere die Zertifikate für den NPS- und den VPN-Server.  
- Konfiguriere den NPS-Server für die RADIUS-Authentifizierung der „VPN-User“ mit dem EAP-Protokoll PEAP, sodass Smartcard- oder anderes Zertifikat verwendet werden kann. Entferne alle anderen Methoden.  
- Trage „Server2“ als „RADIUS-Client“ ein.  
- Konfiguriere den VPN-Server („Server2“) so, dass er „Server1“ als „RADIUS-Server“ verwendet.  
- Erlaube als Authentifizierungsmethode die Computerzertifikatsauthentifizierung für IKEv2.  
- Verbinde „W11“ mit dem Switch „VPN“ und stelle die IP 172.16.0.2/24 ein. Ändere die DNS-Server-Adresse auf 172.16.0.3.  
- Erstelle und teste die VPN-Verbindung.
```

```markdown
- *Create the groups "VPNUser", "NPS-Server", and "VPN-Server" in a new OU "VPN".*  
- *Add Karl Klammer to the group "VPNUser", "Server2" to the group "VPN-Server", and "Server1" to the group "NPS-Server".*  
- *Configure "Server2" with a website "Certs" at the path "C:\Certs", accessible on port 8088. Enable "Double Escaping" on the website.*  
- *Share the folder "C:\Certs" and grant full control to Domain Admins and the "DC".*  
- *Create a firewall rule "CDP-AIA-Webfreigabe" to allow incoming TCP port 8088 on all profiles.*  
- *In Routing and RAS, create an inbound packet filter on the interface with address 172.16.0.1 allowing TCP port 8088 access from any port and any IP address to 172.16.0.1.*  
- *Create an Enterprise Root Certification Authority "GfnLab-Enterprise-Root-CA" with a 10-year validity, 4096-bit key, and SHA256 hash algorithm.*  
- *Add the URL http://server2.gfnlab.test/certs as AIA and CDP distribution points.*  
- *Restart the certification authority and create a certificate revocation list.*  
- *Copy the CRLs (.crl) and the CA certificate (.crt) to the Certs share on "Server2".*  
- *Enable automatic certificate enrollment for computers and users at the domain level using a new Group Policy Object "Automatic Certificate Enrollment".*  
- *Create the Group Policy Object "Client Computers IPSEC Certificates" for automatic IPSEC certificate enrollment.*  
- *Create certificate templates for "VPN-User", "VPN-Server", and "NPS-Server", as well as for client computers, and enroll certificates for the NPS and VPN servers.*  
- *Configure the NPS server for RADIUS authentication of "VPN-User" using the EAP-PEAP protocol, allowing smart card or other certificate authentication and removing all other methods.*  
- *Add "Server2" as a RADIUS client.*  
- *Configure the VPN server ("Server2") to use "Server1" as the RADIUS server.*  
- *Allow computer certificate authentication for IKEv2.*  
- *Connect "W11" to the "VPN" switch and set IP 172.16.0.2/24. Change the DNS server address to 172.16.0.3.*  
- *Create and test the VPN connection.*
```

### Erstellen von OU und Gruppen  
### *Creating OUs and groups*

In DC

- Wechsle auf „DC“.  
- Öffne „Active Directory Benutzer und Computer“.  
- Erstelle eine OU mit dem Namen „VPN“.  
- Erstelle drei globale Sicherheitsgruppen in der OU:  
  - „VPN-User“ mit „Karl Klammer“ als Mitglied  
  - „VPN-Server“ mit „Server2“ als Mitglied  
  - „NPS-Server“ mit „Server1“ als Mitglied

- *Switch to "DC".*  
- *Open "Active Directory Users and Computers".*  
- *Create an OU named "VPN".*  
- *Create three global security groups in the OU:*  
  - *"VPN-User" with "Karl Klammer" as a member*  
  - *"VPN-Server" with "Server2" as a member*  
  - *"NPS-Server" with "Server1" as a member*

<img width="1215" height="294" alt="image" src="https://github.com/user-attachments/assets/8cb3651f-4975-4b3e-9251-6cbeca950904" />

<img width="820" height="656" alt="image" src="https://github.com/user-attachments/assets/29973221-5b94-45bb-963e-7808732ff5fe" />

<img width="539" height="470" alt="image" src="https://github.com/user-attachments/assets/10cd62cc-80e1-40fc-8bf0-5e8c0783acea" />

<img width="929" height="550" alt="image" src="https://github.com/user-attachments/assets/8de49c6b-4f43-4332-8103-3102f827d33e" />

<img width="544" height="472" alt="image" src="https://github.com/user-attachments/assets/eebbd0c7-ea57-443c-ab55-e798720e3786" />

Füge zwei weitere Gruppen hinzu: VPN-Server und NPS-Server  
*Add two more groups: VPN servers and NPS servers*

<img width="776" height="391" alt="image" src="https://github.com/user-attachments/assets/1233aaa5-2f64-4714-a2b2-7835e1350e11" />

Mitglieder hinzufügen:  
*add member:*

<img width="766" height="495" alt="image" src="https://github.com/user-attachments/assets/c91bd459-f314-4f68-ae1c-87dbc0e092e0" />

<img width="496" height="569" alt="image" src="https://github.com/user-attachments/assets/a9b09f57-3369-42a4-8566-71974b321c9f" />

<img width="567" height="310" alt="image" src="https://github.com/user-attachments/assets/dcaf8ce1-06c6-4ffa-971b-bc1d1b8e9b5e" />

OK + Übernehmen + OK  
*OK + Apply + OK*

<img width="593" height="570" alt="image" src="https://github.com/user-attachments/assets/55089db1-b154-45eb-831a-af13b4ba1db0" />

<img width="616" height="360" alt="image" src="https://github.com/user-attachments/assets/1e7f3994-898c-4073-9cde-b81b538397a4" />

<img width="567" height="314" alt="image" src="https://github.com/user-attachments/assets/42550479-5fd7-4968-b776-b9e83611c34e" />

OK + Übernehmen + OK  
*OK + Apply + OK*

<img width="650" height="563" alt="image" src="https://github.com/user-attachments/assets/bf6d9588-68e1-4dc8-8ec4-fb8cb3ba33d1" />

<img width="570" height="311" alt="image" src="https://github.com/user-attachments/assets/4f626eb5-f92f-40d4-b842-84068d3b006f" />

OK + Übernehmen + OK  
*OK + Apply + OK*

### Konfiguration des VPN-Servers als CDP- und AIA-Verteilungspunkt  
### *Configuring the VPN server as a CDP and AIA distribution point*

Server2 --> Servermanager --> Tools --> Internetinformationsdienste (IIS)Manager:  
*Server2 --> Server Manager --> Tools --> Internet Information Services (IIS) Manager:*

<img width="802" height="440" alt="image" src="https://github.com/user-attachments/assets/cfe3acab-9e6d-4455-9a87-5d821cc2de03" />

<img width="609" height="649" alt="image" src="https://github.com/user-attachments/assets/9a40cd3a-e44f-4455-b2de-2abf62ca58c5" />

<img width="915" height="1046" alt="image" src="https://github.com/user-attachments/assets/0dff6aab-9140-400e-bed4-3f595e3490cd" />

<img width="572" height="764" alt="image" src="https://github.com/user-attachments/assets/bd285582-d178-4881-a095-65891ae1c63e" />

<img width="600" height="575" alt="image" src="https://github.com/user-attachments/assets/5544e8e5-94eb-4b45-b22d-946c5d4bce89" />

<img width="1022" height="449" alt="image" src="https://github.com/user-attachments/assets/c56908af-255c-4731-a18a-21ee26390713" />

Hinweis: In bestimmten Szenarien, z. B. bei der Verwendung von Zertifikatsdaten in URLs, müssen diese 
manchmal doppelt maskiert (escaped) übertragen werden, weshalb in IIS die Option „allowDoubleEscaping“ aktiviert werden muss, 
damit solche URLs nicht aus Sicherheitsgründen blockiert werden.  

*Note: In certain scenarios, for example when using certificate data in URLs, these sometimes need to be transmitted with double escaping. Therefore, in IIS the option "allowDoubleEscaping" must be enabled so that such URLs are not blocked for security reasons.*

### Konfiguration der Freigabe Certs  
### *Configuring the release certs*

Der DC bekommt Vollzugriff, damit er die CRLs automatisch speichern kann.

Hinweis: Eine CRL (Certificate Revocation List) ist eine von der Zertifizierungsstelle veröffentlichte Liste widerrufener Zertifikate (Sperrliste), um deren Gültigkeit zu prüfen.  

*The DC is given full access so that it can automatically store the CRLs.

Note: A CRL (Certificate Revocation List) is a list of revoked certificates (block list) published by the certification authority to check their validity.*

- Du bist weiterhin auf „Server2“.  
- Wechsle auf Laufwerk C: und rufe die Eigenschaften des Ordners „Certs“ auf.  
- Wechsle auf die Registerkarte **Freigabe** und dann auf **Erweiterte Freigabe**.  
- Gib den Ordner als „Certs“ frei (Haken setzen) und rufe anschließend die Berechtigungen auf.  
- Füge den „DC“ hinzu, wobei du unter „Objekttypen“ zuvor „Computer“ auswählen musst.  
- Erteile dem „DC“ die Berechtigung „Vollzugriff“.  
- Bestätige mit „OK“.  
- Füge die „Domänen-Admins“ mit „Vollzugriff“ hinzu.  

- *You are still on "Server2".*  
- *Switch to drive C: and open the properties of the "Certs" folder.*  
- *Go to the "Sharing" tab and then to "Advanced Sharing".*  
- *Share the folder as "Certs" (check the box) and then open the permissions.*  
- *Add the "DC", making sure to select "Computer" under "Object Types" first.*  
- *Grant the "DC" "Full Control" permissions.*  
- *Confirm with "OK".*  
- *Add the "Domain Admins" group with "Full Control".*

<img width="534" height="575" alt="image" src="https://github.com/user-attachments/assets/17dcb526-4752-42a9-addc-c95ce5b20e36" />

<img width="447" height="437" alt="image" src="https://github.com/user-attachments/assets/05290d86-0bb9-4d12-89c9-411916688657" />

<img width="440" height="403" alt="image" src="https://github.com/user-attachments/assets/7f120173-f82b-4411-848d-5de9b6573e7a" />

<img width="443" height="313" alt="image" src="https://github.com/user-attachments/assets/3bff8a12-5e2e-4cfd-9a85-72edcaba04c9" />

<img width="567" height="314" alt="image" src="https://github.com/user-attachments/assets/265165a7-55f2-4028-b606-06699867cd8c" />

<img width="619" height="357" alt="image" src="https://github.com/user-attachments/assets/060268aa-e1f9-4f66-8af1-f86319b317bc" />

<img width="561" height="311" alt="image" src="https://github.com/user-attachments/assets/63aca3b3-2bcf-41f0-9352-80dc3ff56082" />

<img width="447" height="579" alt="image" src="https://github.com/user-attachments/assets/394139c8-b6a1-4f9f-ad77-aabfd0f389e7" />

<img width="561" height="312" alt="image" src="https://github.com/user-attachments/assets/a574654c-06dd-4ad4-aa04-fe8403a72fe9" />

<img width="447" height="579" alt="image" src="https://github.com/user-attachments/assets/5a53bf33-b2d4-4724-8712-6c73ad360980" />

<img width="448" height="574" alt="image" src="https://github.com/user-attachments/assets/f1c4bb14-acfe-4bcf-83e9-00711a4f3102" />

Übernehmen + OK + Übernehmen + OK 
*Apply + OK + Apply + OK*

<img width="451" height="597" alt="image" src="https://github.com/user-attachments/assets/830ec239-4b76-4ca9-8e32-307dfd406b28" />

<img width="449" height="334" alt="image" src="https://github.com/user-attachments/assets/fde51d0c-3e1d-4988-ab7c-f42c918ece91" />

<img width="670" height="409" alt="image" src="https://github.com/user-attachments/assets/5449a312-a982-4e81-a07d-cbee4d2e87b5" />

<img width="569" height="317" alt="image" src="https://github.com/user-attachments/assets/08aabd97-3ff8-45f8-bef3-068c7d8cb7b3" />

<img width="446" height="520" alt="image" src="https://github.com/user-attachments/assets/21d76761-8aec-47b9-9bb0-b52757d75b73" />

Übernehmen + OK  
*Apply + OK*

### Konfiguration der Firewall auf „Server2“  
### *Configuring the firewall on “Server2”*

Server2 --> Windows Defender Firewall mit erweiterter Sicherheit --> Eingehende Regeln:  
*Server2 --> Windows Defender Firewall with Advanced Security --> Inbound Rules:*

<img width="420" height="850" alt="image" src="https://github.com/user-attachments/assets/23c69cba-fe59-402b-bc40-6014e8048e08" />

<img width="1060" height="287" alt="image" src="https://github.com/user-attachments/assets/f5867f6c-943a-40d5-a21c-313016e5fa48" />

<img width="761" height="728" alt="image" src="https://github.com/user-attachments/assets/cfc24c81-ba0d-4fbf-9037-51ba81768152" />

<img width="765" height="718" alt="image" src="https://github.com/user-attachments/assets/1c57cc0e-d0bc-415d-a470-077a539ddd5a" />

<img width="889" height="722" alt="image" src="https://github.com/user-attachments/assets/50ee7567-81bc-4052-94bd-cae2dfb62e13" />

<img width="888" height="722" alt="image" src="https://github.com/user-attachments/assets/3503ce2a-a402-442e-ad59-eb45fd31ae33" />

<img width="888" height="722" alt="image" src="https://github.com/user-attachments/assets/8f361530-4c38-41bc-b4da-12cc344cb4dd" />

<img width="644" height="247" alt="image" src="https://github.com/user-attachments/assets/e27f7017-055d-4e46-a9f8-0a050fb87595" />

- Hinweis: CDP (Certificate Distribution Point) und AIA (Authority Information Access) sind Komponenten der Public Key Infrastructure (PKI):  
 - CDP: Gibt den Speicherort der Zertifikatsperrliste (CRL) an, die widerrufene Zertifikate auflistet.  
 - AIA: Enthält Informationen über die Zertifizierungsstelle (CA), die das Zertifikat ausgestellt hat, und bietet oft einen Pfad zum Abrufen des CA-Zertifikats.  

- *Note: CDP (Certificate Distribution Point) and AIA (Authority Information Access) are components of the Public Key Infrastructure (PKI):*  
- *CDP: Specifies the location of the certificate revocation list (CRL), which lists revoked certificates.*  
- *AIA: Contains information about the certification authority (CA) that issued the certificate and often provides a path to retrieve the CA certificate.*

### Konfiguration des Paketfilters auf „Server2“  
### *Configuring the packet filter on “Server2”*

Server2 --> Server-Manager --> Tools --> Routing und RAS:  
*Server2 --> Server Manager --> Tools --> Routing and RAS:*

<img width="751" height="632" alt="image" src="https://github.com/user-attachments/assets/199fb4d2-5b38-43b4-b23a-eb23932a4af4" />

<img width="497" height="588" alt="image" src="https://github.com/user-attachments/assets/7f2cf66b-7391-413a-9e1c-c4deadb429bb" />

<img width="590" height="424" alt="image" src="https://github.com/user-attachments/assets/ffb29051-21cb-450f-877f-aeced4e0e6c8" />

<img width="471" height="484" alt="image" src="https://github.com/user-attachments/assets/6a735c8f-5120-4677-aa8e-f5e2a858d168" />

OK + OK + Übernehmen + OK  
*OK + OK + Apply + OK*

### Konfiguration der Zertifizierungsstelle auf „DC“  
### *Configuration of the certification authority on “DC”*

DC --> Powershell-ISE Administrator:

<img width="820" height="260" alt="image" src="https://github.com/user-attachments/assets/59477cb8-1363-41f1-b8fd-c666f7109c8d" />

```powershell
Install-WindowsFeature -Name "ADCS-Cert-Authority" -IncludeManagementTools

$params = @{
 CAType = "EnterpriseRootCA"
 ValidityPeriod = "Years"
 ValidityPeriodUnits = "10"
 CACommonName = "GfnLab-Enterprise-Root-CA"
 CADistinguishedNameSuffix = "DC=gfnlab,DC=test"
 KeyLength = "4096"
 HashAlgorithmName = "SHA256"
 }
 Install-AdcsCertificationAuthority @params
```
Hinweis: Das Skript installiert und konfiguriert eine Enterprise-Root-Zertifizierungsstelle (CA) in einer Active-Directory-Umgebung mit den angegebenen Einstellungen.  
*Note: The script installs and configures an enterprise root certification authority (CA) in an Active Directory environment with the specified settings.*

Bestätige mit „Ja, alle“ die Abfrage  
*Confirm the query with “Yes, all.”*

<img width="577" height="624" alt="image" src="https://github.com/user-attachments/assets/32e45cb1-0d92-4fb1-aa6f-f9d49eebd0bf" />

```powershell
# Erstellt neue CDP und AIA-Erweiterungen für Server2
# Wegen der Länge wird die URI jeweils mit Variablen erstellt
# Sonst gäbe es hier im Dokument Zeilenumbrüche
# Ein ähnliches Setup habe ich bereits grafisch (GUI) am Tag 24 durchgeführt

$FilePath = "file://Server2.gfnlab.test/certs/"
$File = "<CAName><CRLNameSuffix><DeltaCRLAllowed>.crl"
$URI = $FilePath + $File
$p1 = @{
    URI = $URI
    AddToCertificateCdp = $true
    PublishToServer = $true
    PublishDeltaToServer = $true
    AddToFreshestCrl = $true
}
Add-CACrlDistributionPoint @P1 -Confirm:$false

$webpath = "http://Server2.gfnlab.test:8088/Certs/"
$file = "<CAName><CRLNameSuffix><DeltaCRLAllowed>.crl"
$uri = $webpath + $file
$P2 = @{
    URI = $uri
    AddToCertificateCdp = $true
    AddToFreshestCrl = $true
}
Add-CACrlDistributionPoint @P2 -Confirm:$false

$Filepath = "file://Server2.gfnlab.test/certs/"
$file = "<ServerDNSName>_<CAName><CertificateName>.crt"
$Uri = $Filepath + $File
$P3 = @{
    URI = $Uri
    AddToCertificateAia = $true
}
Add-CAAuthorityInformationAccess @P3 -Confirm:$false

$webpath = "http://Server2.gfnlab.test:8088/Certs/"
$file = "<ServerDNSName>_<CAName><CertificateName>.crt"
$uri = $webpath + $file
$P4 = @{
    URI = $uri
    AddToCertificateAia = $true
}
Add-CAAuthorityInformationAccess @P4 -Confirm:$false
```

<img width="391" height="129" alt="image" src="https://github.com/user-attachments/assets/a8831532-8e5d-456c-bf22-4a7e4b6a2c8a" />

Server-Manager --> Tools  --> Zertifizierungsstelle:  
*Server Manager --> Tools --> Certification Authority:*

<img width="730" height="372" alt="image" src="https://github.com/user-attachments/assets/513affc4-dc7f-4172-9c79-41442d8a9247" />

<img width="773" height="361" alt="image" src="https://github.com/user-attachments/assets/b6bc2c7d-af83-4f09-a8af-50305809dc99" />

<img width="777" height="394" alt="image" src="https://github.com/user-attachments/assets/3b2ab374-62ad-4866-8ea3-25d9f5bc3914" />

<img width="563" height="342" alt="image" src="https://github.com/user-attachments/assets/c8130a50-9af9-430b-92ae-07f8504e53ae" />

Es wird eine neue Sperrliste angelegt in:  
*A new block list is created in:*

<img width="877" height="274" alt="image" src="https://github.com/user-attachments/assets/83a1c2f6-9c3a-4ecc-a29c-a44b4081c8cf" />

### Konfiguration der Gruppenrichtlinie zur automatischen Zertifikatsanforderung  
### *Configuring the group policy for automatic certificate requests*

DC --> Server-Manager --> Tools --> Gruppenrichtlinienverwaltung:  
*DC --> Server Manager --> Tools --> Group Policy Management:*

<img width="617" height="745" alt="image" src="https://github.com/user-attachments/assets/8901d293-d2ee-4e4d-9b03-724701ca5c9e" />

<img width="481" height="220" alt="image" src="https://github.com/user-attachments/assets/b1a4959e-71a1-4c31-98aa-fcd99b866b3e" />
<br>
<img width="481" height="185" alt="image" src="https://github.com/user-attachments/assets/20a53fac-a881-42f9-b168-d5f06d783c8a" />

<img width="647" height="557" alt="image" src="https://github.com/user-attachments/assets/bdbe1c1b-9226-4dbe-8494-14f783851f1e" />

<img width="1046" height="698" alt="image" src="https://github.com/user-attachments/assets/6e2b4126-cfb6-45e5-9048-82b49fe5c505" />

<img width="511" height="660" alt="image" src="https://github.com/user-attachments/assets/ebb1b677-9e97-44c3-879e-2e8675a4b29b" />

Übernehmen + OK  
*Apply + OK*

<img width="1049" height="703" alt="image" src="https://github.com/user-attachments/assets/36ee3767-3526-4c39-8681-d2dd9a9698e0" />

<img width="511" height="664" alt="image" src="https://github.com/user-attachments/assets/ef7b6644-e380-48d3-aefb-f81b355c2f85" />

Übernehmen + OK  
*Apply + OK*

### Konfiguration der Zertifikate - Konfiguration für Clientcomputer IPSEC-Zertifikate  
### *Configuring Certificates - Configuration for Client Computers IPSEC Certificates*

DC --> Server-Manager --> Tools --> Gruppenrichtlinienverwaltung:  
*DC --> Server Manager --> Tools --> Group Policy Management:*

<img width="797" height="532" alt="image" src="https://github.com/user-attachments/assets/b42667f6-3abe-4d86-b13e-1f6015a7d6db" />

<img width="476" height="222" alt="image" src="https://github.com/user-attachments/assets/01727212-d4f4-4507-ad12-670d019b1e31" />

<img width="1274" height="633" alt="image" src="https://github.com/user-attachments/assets/00d16983-04b8-4588-9aca-dde1f4548f0d" />

<img width="705" height="778" alt="image" src="https://github.com/user-attachments/assets/7153c765-01e3-4e9c-a8be-db912e8b0bf9" />

<img width="1034" height="702" alt="image" src="https://github.com/user-attachments/assets/3a55c84d-5ec4-43f1-aab9-bd0638f30be8" />

<img width="617" height="487" alt="image" src="https://github.com/user-attachments/assets/fd53a685-c228-48c0-965f-53b5b6bdf789" />

<img width="613" height="485" alt="image" src="https://github.com/user-attachments/assets/527ff901-6ddd-4855-8040-c87c9159c916" />

<img width="619" height="481" alt="image" src="https://github.com/user-attachments/assets/50607755-8f80-457f-8b12-a9efc7393bba" />

<img width="677" height="666" alt="image" src="https://github.com/user-attachments/assets/c3337266-3d94-468b-941a-d3e2b03c5cee" />

<img width="587" height="432" alt="image" src="https://github.com/user-attachments/assets/f3fab67b-5927-49a6-966b-fb6e377dd42b" />

<img width="481" height="349" alt="image" src="https://github.com/user-attachments/assets/5d7ae9c8-94e7-446e-9991-90fb31f0e24d" />

```powershell
Select * from Win32_OperatingSystem where ProductType = '1'
```

<img width="585" height="430" alt="image" src="https://github.com/user-attachments/assets/2d789986-57aa-48b2-a575-f84256f81a24" />

<img width="949" height="877" alt="image" src="https://github.com/user-attachments/assets/cb39909b-1fe7-4b15-aa42-1e61e899bfe2" />

