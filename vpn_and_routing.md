# VPN und Routing auf dem Server#
# *VPN and routing on the server*

### DHCP-Rolle auf DC installieren  
### *Install DHCP role on DC*

```markdown

- **Privaten virtuellen Switch erstellen**  
  Erstelle mit PowerShell einen neuen privaten virtuellen Switch mit Namen `VPN`.

- **Zweite Netzwerkkarte auf Server2 einrichten**  
  Richte auf `Server2` mit PowerShell eine zweite Netzwerkkarte ein und verbinde diese mit dem neuen virtuellen Switch `VPN`.

- **Netzwerkkarte konfigurieren**  
  - IP-Adresse: `172.16.0.1`  
  - Subnetzmaske: `255.255.0.0`  
  - DNS-Server: `192.168.1.200`

- **Routing und RAS installieren**  
  Richte auf `Server2` die Rolle „Routing und RAS“ für VPN und einen internen Router ein.

- **Internen Router aktivieren**  
  Aktiviere einen internen Router auf `Server2`.
```

Host-PC --> Powershell-ISE Administrator:  

```powershell
# Erstellen des neuen virtuellen Switches
aNew-VMSwitch -Name VPN -SwitchType Private
```

### Einrichten der Netzwerkkarte  
### *Setting up the network card*

```powershell
# Einrichten der Netzwerkkarte
Add-VMNetworkAdapter -VMName "Server2" -SwitchName "VPN" -Name "VPN-Adapter"
```

Hinweis: Damit wird außerdem der Netzwerkadapter im Hyper-V Manager mit dem Namen „VPN-Adapter“ versehen. Das geht nur mit PowerShell  
*Note: This also assigns the name “VPN adapter” to the network adapter in Hyper-V Manager. This can only be done with PowerShell.*

<img width="711" height="337" alt="image" src="https://github.com/user-attachments/assets/751d4faf-b547-400f-afde-cca5d46cc197" />

<img width="449" height="579" alt="image" src="https://github.com/user-attachments/assets/90d0884b-b20e-44b7-bd31-7fd7af50b3fb" />

<img width="497" height="564" alt="image" src="https://github.com/user-attachments/assets/76e744d7-a4a9-40dd-93c9-9efb15dff1d7" />

### Installation der Rolle  
### *Installation of the roller*

Server2 --> Server-Manager --> Rollen und Features hinzufügen --> Weiter --> Weiter --> Weiter  
*Server2 --> Server Manager --> Add Roles and Features --> Next --> Next --> Next*

<img width="721" height="695" alt="image" src="https://github.com/user-attachments/assets/72c4e353-5296-4bac-9615-35e9cc7defc4" />

<img width="749" height="694" alt="image" src="https://github.com/user-attachments/assets/a522e429-27b4-4892-ad8a-871e218db06a" />

Weiter --> Weiter --> Installieren --> Abschließen  
*Next --> Next --> Install --> Finish*

<img width="993" height="363" alt="image" src="https://github.com/user-attachments/assets/61ea7d5a-f830-427e-9c2a-a95ef4bb9754" />

Alternative: Server-Manager --> Tools --> Routing und RAS  
*Alternative: Server Manager --> Tools --> Routing and RAS*

<img width="548" height="596" alt="image" src="https://github.com/user-attachments/assets/cfcdf1bf-8d30-4c12-aaf8-d74864c345f6" />

<img width="590" height="682" alt="image" src="https://github.com/user-attachments/assets/5b736c19-8eb1-4436-b990-f6b8ad071b35" />

<img width="521" height="528" alt="image" src="https://github.com/user-attachments/assets/7cf35d8d-5685-472d-ad5f-43563382d8dd" />

<img width="617" height="527" alt="image" src="https://github.com/user-attachments/assets/4d977da3-1ebc-4e3a-bfde-a40d578ce124" />

<img width="509" height="258" alt="image" src="https://github.com/user-attachments/assets/12a6b3b8-2753-42ff-9182-32a833ba724d" />

```markdown

- Betrachte die Routingtabelle des erstellten Routers in der Routing- und RAS-Konsole.

- Ermittle die Routen mit dem Befehl `route` in der Eingabeaufforderung.

- Ermittle die Routen mit PowerShell `Get-NetRoute`.

- Teste mit „W11“ die Routingfunktion zum „DC“. Füge im Hyper-V Manager eine zweite Netzwerkkarte zu „W11“ hinzu und verbinde diese mit dem Switch `VPN`.

- Konfiguriere den neuen Adapter (Ethernet 2) auf „W11“:
  - Adresse: `172.16.0.2`
  - Subnetzmaske: `255.255.0.0`
  - Standardgateway: `172.16.0.1`
  - DNS: `192.168.1.200`

- Verbinde die ursprüngliche Netzwerkkarte mit Switch `Nicht verbunden`.

- Stelle auf dem „DC“ als Standardgateway die Adresse `192.168.1.2` ein.

- Teste einen Ping von „W11“ zu „DC“.
```

```markdown
*- View the routing table of the created router in the Routing and Remote Access console.*

*- Determine the routes using the `route` command in the Command Prompt.*

*- Determine the routes using PowerShell with `Get-NetRoute`.*

*- Test the routing functionality from `W11` to `DC`. In Hyper-V Manager, add a second network adapter to `W11` and connect it to the `VPN` switch.*

*- Configure the new adapter (Ethernet 2) on `W11`:  
  - Address: `172.16.0.2`  
  - Subnet mask: `255.255.0.0`  
  - Default gateway: `172.16.0.1`  
  - DNS: `192.168.1.200`*

*- Connect the original network adapter to the `Not connected` switch.*

*- Set the default gateway on `DC` to `192.168.1.2`.*

*- Test a ping from `W11` to `DC`.*
```

### Betrachten der Routingtabelle in „Routing uns RAS“ Konsole  
### *Viewing the routing table in the Routing and RAS console*

Routingtabelle  
*routing table*

<img width="1035" height="614" alt="image" src="https://github.com/user-attachments/assets/074a69ef-56fa-4f81-8670-14e9e9010fb4" />


In CMD:
```powershell
route print
route print -4
```

<img width="794" height="786" alt="image" src="https://github.com/user-attachments/assets/30dc0f67-322f-4a9f-a0f0-42158d35274f" />

In Powershell:

```powershell
Get-NetRoute
Get-NetRoute -AddressFamily IPv4
```

<img width="1209" height="657" alt="image" src="https://github.com/user-attachments/assets/d13867a9-a0b5-4c93-af62-ea97512666f6" />

<img width="1262" height="429" alt="image" src="https://github.com/user-attachments/assets/8998d668-a5fb-4ce4-b301-ba90c49972f3" />

### Testen der Routingfunktion  
### *Testing the routing function*

Host-PC --> Hyper-V-Manager --> VM W11 auswählen --> Hardware hinzufügen --> Netzwerkkarte --> VPN:  
*Host PC --> Hyper-V Manager --> Select VM W11 --> Add Hardware --> Network Adapter --> VPN:*

<img width="651" height="957" alt="image" src="https://github.com/user-attachments/assets/00cc2d61-0f40-4ca2-8698-eb2248dc1d50" />

### Konfiguration des neuen Netzwerkadapters auf „W11“  
### *Configuration of the new network adapter on “W11”*

<img width="506" height="691" alt="image" src="https://github.com/user-attachments/assets/384b1974-358e-422e-9214-9cd52ac8ba7f" />

<img width="798" height="461" alt="image" src="https://github.com/user-attachments/assets/ea9f4d34-31b0-4fa6-a9f3-4be8dac3bab9" />

<img width="498" height="571" alt="image" src="https://github.com/user-attachments/assets/78c1cf00-4b91-4310-ba00-cdcf6aef0013" />

<img width="509" height="212" alt="image" src="https://github.com/user-attachments/assets/232c4bf1-0811-46ad-a028-5091e4456e2d" />

In DC:

<img width="531" height="659" alt="image" src="https://github.com/user-attachments/assets/c3830149-5603-4fc6-b2e8-ebd8d3afd2df" />

In W11:

<img width="806" height="666" alt="image" src="https://github.com/user-attachments/assets/396a8b79-23f1-476a-a87c-5d14f0402599" />

Man kann DC anpingen, aber Server1 nicht. Warum nicht? Weil Server1 kein Standardgateway hat und kann daher nicht antworten.  
*You can ping DC, but not Server1. Why not? Because Server1 does not have a default gateway and therefore cannot respond.*

### Bereinigen der Aufgabe für weitere Übungen:  
### *Clean up the task for further exercises:*

In Host-PC --> Hyper-V --> W11 --> Netzwerkkarte Privat wieder aktivieren:  
*In Host PC --> Hyper-V --> W11 --> Reactivate Private Network Card:*

<img width="769" height="933" alt="image" src="https://github.com/user-attachments/assets/996867bd-d7cc-431a-9409-7a2c233d8346" />

Anwenden + OK  
*Apply + OK*

```markdown
- Erstelle einen „NAT-Router“ mit dem Server `Server2`.

- Verbinde die Netzwerkkarte, die mit dem VPN-Switch verbunden ist, mit dem externen Switch und stelle in `Server2` die Schnittstelle auf DHCP um.

- Aktiviere das Routingprotokoll „NAT“ und wähle die Schnittstelle, die mit dem externen Switch verbunden ist, als Schnittstelle, auf der NAT ausgeführt werden soll.

- Teste den Zugriff auf das Internet vom `DC` aus.
```

```markdown
*- Create a “NAT router” using the server `Server2`.*

*- Connect the network adapter that is linked to the VPN switch to the external switch and configure the interface on `Server2` to use DHCP.*

*- Enable the “NAT” routing protocol and select the interface connected to the external switch as the interface on which NAT should run.*

*- Test internet access from `DC`.*
```

### Konfiguration des NAT-Routers  
### *Configuring the NAT router*

Host-PC --> Hyper-V

<img width="784" height="961" alt="image" src="https://github.com/user-attachments/assets/246634d4-206e-4905-b93c-e6b737f183ac" />

Anwenden + OK
*Apply + OK*

Server2:

<img width="558" height="866" alt="image" src="https://github.com/user-attachments/assets/b46aafff-619d-4593-8252-5dacbad09ba3" />

<img width="555" height="739" alt="image" src="https://github.com/user-attachments/assets/b1a52050-ac82-423d-b7e1-6e5055a721a7" />

<img width="469" height="485" alt="image" src="https://github.com/user-attachments/assets/3b584e34-3820-424d-ae34-2c1d9121df21" />

<img width="496" height="617" alt="image" src="https://github.com/user-attachments/assets/ea8c5949-2d98-4b71-85d3-bdfd89c39135" />

Übernehmen + OK  
*Apply + OK*

<img width="524" height="727" alt="image" src="https://github.com/user-attachments/assets/2889bcfa-1dca-4656-9f79-1d47c07145d6" />

<img width="478" height="479" alt="image" src="https://github.com/user-attachments/assets/670cce95-a588-4a61-a92a-f080acd984ae" />

<img width="496" height="612" alt="image" src="https://github.com/user-attachments/assets/624c7c5c-2f4b-43ea-8f6e-b87c5e9ad601" />

<img width="840" height="511" alt="image" src="https://github.com/user-attachments/assets/6d310389-0252-4411-b847-7da23ee9449b" />

Vergewissere dich, dass die Schnittstelle „VPN“ eine automatische IP-Adresse erhalten hat (in unserem Beispiel: von GFN-DHCP).  
*Ensure that the “VPN” interface has been assigned an automatic IP address (in our example: from GFN-DHCP).*

### Testen der NAT-Funktion  
### *Testing the NAT function*

<img width="777" height="712" alt="image" src="https://github.com/user-attachments/assets/9db98837-e5da-4849-9fa2-716b754e0154" />

Anmerkung: Die Namensauflösung für das Internet kann auf „DC“ fehlschlagen, da oftmals fehlerhafte Weiterleitung-Server eingetragen sind. 
Diese Einträge müssen gelöscht werden. Außerdem kann es sein, dass die Liste der Stammserver nur sehr wenige IPv4-Adressen umfasst. 
Mit der Option, die Stammserverinformationen von einem Server zu kopieren, kann unter Angabe einer der vorhandenen IPv4-Stammserveradressen 
die Liste ergänzt werden. Dann funktioniert auch der Internetzugriff von „DC“ aus.  
*Note: Name resolution for the Internet may fail for “DC” because incorrect forwarding servers are often entered. These entries must be deleted. 
In addition, the list of root servers may contain very few IPv4 addresses. With the option to copy the root server information from a server, 
the list can be supplemented by specifying one of the existing IPv4 root server addresses. Then Internet access from “DC” will also work.*

<img width="1283" height="799" alt="image" src="https://github.com/user-attachments/assets/f84d66ff-c582-4bbc-8ea0-db9a7bdfeb40" />

Hinweis: Bei NAT in Routing und RAS zeigen die Zuordnungen (NAT Mappings), welche interne IP-Adresse und Portnummer einer externen IP-Adresse und Portnummer zugewiesen wurden. 
Diese Tabelle dokumentiert die aktiven Verbindungen und sorgt dafür, dass Antworten vom Internet korrekt an den internen Client zurückgeleitet werden.  
*Note: With NAT in Routing and RAS, the mappings show which internal IP address and port number have been assigned to an external IP address and port number. 
This table documents the active connections and ensures that responses from the Internet are correctly forwarded back to the internal client.*

### Vorbereitung auf die nächste Übung: Einrichten des VPN-Servers  
### *Preparing for the next exercise: Setting up the VPN server*

<img width="558" height="571" alt="image" src="https://github.com/user-attachments/assets/784892d2-2791-42d6-8e29-0a8f13990128" />

<img width="507" height="213" alt="image" src="https://github.com/user-attachments/assets/7f1db569-6fbf-46c2-9146-08717b0eb9d2" />

<img width="769" height="804" alt="image" src="https://github.com/user-attachments/assets/36969412-434b-44b3-ae17-319e1d55dd09" />

Anwenden + OK  
*Apply + OK*

<img width="802" height="535" alt="image" src="https://github.com/user-attachments/assets/2c640484-fce6-4866-ad92-4fd6699f2d32" />

<img width="493" height="561" alt="image" src="https://github.com/user-attachments/assets/15360a7e-6d43-4dd2-95da-cc3f8d99de13" />

<img width="521" height="540" alt="image" src="https://github.com/user-attachments/assets/8998fbad-6645-41cf-b920-5dfac3a1d617" />

<img width="616" height="526" alt="image" src="https://github.com/user-attachments/assets/7f22fa20-89d9-4311-81d1-8e6ae1c823e8" />

<img width="618" height="524" alt="image" src="https://github.com/user-attachments/assets/9a9c7af6-fa27-435f-8c4e-41743971c3ae" />

<img width="626" height="522" alt="image" src="https://github.com/user-attachments/assets/ad3a2337-920c-4e8a-8f9b-e705001041c6" />

<img width="616" height="522" alt="image" src="https://github.com/user-attachments/assets/f6063f4e-b3f9-4934-b677-a35118fd02a4" />

<img width="622" height="522" alt="image" src="https://github.com/user-attachments/assets/1027a555-9d3b-40f4-bd26-8da35b7d58fa" />

<img width="467" height="293" alt="image" src="https://github.com/user-attachments/assets/7dc14c93-e85d-49d6-8a2e-dc997b4f49d6" />

OK + Weiter  
*OK + Next*

<img width="615" height="522" alt="image" src="https://github.com/user-attachments/assets/b8daa2af-e296-4c1b-98b2-5e02f526f1bc" />

Fertigstellen  
*Finish*

<img width="676" height="193" alt="image" src="https://github.com/user-attachments/assets/3498bd75-f222-4ae9-98e6-e5d1af6d6262" />

<img width="834" height="780" alt="image" src="https://github.com/user-attachments/assets/600c32cc-c5c7-4ddb-b804-9406842e40b7" />

Hinweis: Du siehst die möglichen VPN-Protokolle und wie viele Geräte parallel mit dem jeweiligen Protokoll eine Verbindung aufbauen können.
- SSTP (Secure Socket Tunneling Protocol) ist ein VPN-Protokoll von Microsoft, das HTTPS (Port 443) nutzt und besonders gut Firewalls und Proxy-Server durchdringen kann – ideal für Verbindungen aus restriktiven Netzwerken.
- IKEv2 (Internet Key Exchange Version 2) ist ein modernes VPN-Protokoll, das schnell, stabil und sicher ist, vor allem auf mobilen Geräten. Es unterstützt Verbindungswiederherstellung bei Netzwechseln sehr gut.
- L2TP (Layer 2 Tunneling Protocol) wird oft mit IPsec kombiniert (L2TP/IPsec) für sichere VPN Verbindungen. Es bietet selbst keine Verschlüsselung, sondern nutzt IPsec für Datenschutz.
- PPTP (Point-to-Point Tunneling Protocol) ist ein älteres VPN-Protokoll, das einfach zu konfigurieren, aber unsicher ist und heute kaum noch empfohlen wird.
- PPPoE (Point-to-Point Protocol over Ethernet) wird häufig von DSL-Anbietern verwendet, um Internetzugang über Ethernet herzustellen – es kombiniert PPP-Funktionen mit Ethernet-Übertragung.
- GRE (Generic Routing Encapsulation) ist ein Tunneling-Protokoll, das beliebige Netzwerkprotokolle über IP transportiert, es bietet keine Verschlüsselung, wird aber oft für Site-to-Site-Tunnel oder spezielle Routing Szenarien genutzt.  

*Note: You can see the available VPN protocols and how many devices can connect to each protocol at the same time.
- SSTP (Secure Socket Tunneling Protocol) is a VPN protocol from Microsoft that uses HTTPS (port 443) and is particularly good at penetrating firewalls and proxy servers – ideal for connections from restrictive networks.
- IKEv2 (Internet Key Exchange Version 2) is a modern VPN protocol that is fast, stable, and secure, especially on mobile devices. It supports connection restoration very well when switching networks.
- L2TP (Layer 2 Tunneling Protocol) is often combined with IPsec (L2TP/IPsec) for secure VPN connections. It does not provide encryption itself, but uses IPsec for data protection.
- PPTP (Point-to-Point Tunneling Protocol) is an older VPN protocol that is easy to configure but insecure and is rarely recommended today.
- PPPoE (Point-to-Point Protocol over Ethernet) is often used by DSL providers to establish Internet access via Ethernet – it combines PPP functions with Ethernet transmission.
- GRE (Generic Routing Encapsulation) is a tunneling protocol that transports any network protocols over IP. It does not offer encryption, but is often used for site-to-site tunnels or special routing scenarios.*

```markdown
Du möchtest mit dem Client `W11` remote auf den RAS-Server `Server2` zugreifen.  
Verwende dazu den lokalen Network Policy Server auf `Server2`.

- Binde den Netzwerkadapter, der derzeit mit dem Switch `Nicht verbunden` verbunden ist, an den virtuellen Switch `VPN` und den anderen Adapter an den Switch `Nicht verbunden`.

- Der Adapter wurde in Übung 31.1.2 bereits mit der richtigen IP-Konfiguration versehen.

- Erstelle auf `W11` eine VPN-Verbindung mit dem Namen `vpn01`, die als Zielserver die externe Schnittstelle des VPN-Servers `172.16.0.1` verwendet.

- Umgehe zunächst die VPN-Richtlinien, indem im Konto von `Karl Klammer` die Option **Zugriff gestatten** eingerichtet wird.

- Teste den VPN-Zugriff vom Client aus.

- Stelle das Konto anschließend wieder auf **Zugriff über NPS-Netzwerkrichtlinien steuern**.

- Teste den Zugriff erneut.

- Ändere die Zugriffsrichtlinie **Verbindung mit dem Microsoft Routing- und RAS-Server** auf **Zugriff gestatten**.

- Teste den VPN-Zugriff vom Client aus.

- Ändere die Zugriffsrichtlinie **Verbindung mit dem Microsoft Routing- und RAS-Server** wieder zurück auf **Zugriff verweigern**.

- Erstelle eine neue Zugriffsrichtlinie mit dem Namen `Nur Admins`, in der nur die Gruppe `Domänen-Administratoren` VPN-Zugriff erhält.

- Verschiebe diese Richtlinie an die erste Stelle.

- Teste abschließend den VPN-Zugriff vom Client aus.
```

```markdown
*You want to access the RAS server `Server2` remotely from the client `W11`.  
Use the local Network Policy Server on `Server2`.*

*- Connect the network adapter that is currently attached to the `Not connected` switch to the virtual switch `VPN`, and connect the other adapter to the `Not connected` switch.*

*- The adapter was already configured with the correct IP settings in Exercise 31.1.2.*

*- Create a VPN connection on `W11` named `vpn01` that uses the external interface of the VPN server `172.16.0.1` as the destination server.*

*- First, bypass the VPN policies by setting the account of `Karl Klammer` to **Allow access**.*

*- Test the VPN connection from the client.*

*- Then set the account back to **Control access through NPS Network Policy**.*

*- Test the access again.*

*- Change the access policy **Connections to Microsoft Routing and Remote Access server** to **Grant access**.*

*- Test the VPN connection from the client.*

*- Change the access policy **Connections to Microsoft Routing and Remote Access server** back to **Deny access**.*

*- Create a new access policy named `Admins Only` that allows VPN access only for the group `Domain Admins`.*

*- Move this policy to the top of the list.*

*- Test the VPN connection from the client again.*
```

### Binden des Adapters an die virtuelle Schnittstelle „VPN“  
### *connecting the adapter to the virtual interface “VPN”*

<img width="897" height="857" alt="image" src="https://github.com/user-attachments/assets/e75be653-b16e-4eaf-8280-45ec5fc22996" />

W11 --> als Karl Klammer (oder domain mitglied) anmelden --> Start Rechte Maustate --> Netzwerkverbindungen:  
*W11 --> Log in as Karl Klammer (or domain member) --> Right-click --> Network Connections:*

<img width="991" height="790" alt="image" src="https://github.com/user-attachments/assets/426d1edf-12ad-4928-bd96-36ea84083aba" />

<img width="995" height="785" alt="image" src="https://github.com/user-attachments/assets/ba740388-3154-4353-9b27-6eb8f6e9c293" />

<img width="475" height="507" alt="image" src="https://github.com/user-attachments/assets/4d774a73-6d48-4dc0-ab48-8b1bdb73af14" />

<img width="484" height="502" alt="image" src="https://github.com/user-attachments/assets/26b7b5b6-05d6-4628-ba42-7128a17ba1c3" />

In DC --> Server-Manager --> Tools --> Active Directory-Benutzer und –Computer:  
*In DC --> Server Manager --> Tools --> Active Directory Users and Computers:*

<img width="900" height="780" alt="image" src="https://github.com/user-attachments/assets/5ab8d324-576e-443d-a3c8-5c6a6ec526d0" />

<img width="513" height="669" alt="image" src="https://github.com/user-attachments/assets/799b047f-ed03-4782-9f7f-6fad901c4098" />

Übernehmen + OK  
*Apply + OK*

In W11:

<img width="991" height="343" alt="image" src="https://github.com/user-attachments/assets/2cb8bd15-6c45-4b34-a6aa-11bb7c1e2402" />

<img width="981" height="552" alt="image" src="https://github.com/user-attachments/assets/9523c824-2761-436f-a150-de5446f3aca1" />

Die Verbindung findet statt.  
*The connection is being established.*

<img width="820" height="674" alt="image" src="https://github.com/user-attachments/assets/462656c6-0440-430c-ade3-43fb20bc1175" />

<img width="980" height="310" alt="image" src="https://github.com/user-attachments/assets/5079ec2c-1ab1-4540-b4c6-311f4941619d" />

<img width="488" height="317" alt="image" src="https://github.com/user-attachments/assets/11977b4f-29a6-4471-950e-506ef47d630d" />

<img width="447" height="592" alt="image" src="https://github.com/user-attachments/assets/16fdf200-06e7-469f-827a-179142b63a07" />

```markdown
Hinweis:

Unsere VPN-Verbindung wurde mit dem unsicheren Protokoll PPTP aufgebaut, obwohl dies nicht explizit eingestellt wurde.

Grund:  
Bei der Einstellung **VPN-Typ: Automatisch** wählt Windows das „beste verfügbare“ Protokoll. Wenn jedoch die Voraussetzungen für sicherere Protokolle (IKEv2, SSTP, L2TP/IPsec) nicht erfüllt sind, greift Windows auf PPTP zurück.

Warum wird trotzdem PPTP gewählt?

- Der VPN-Server unterstützt IKEv2 oder SSTP nicht (z. B. wegen fehlender Zertifikate oder blockierter Ports).
- Firewall oder Router lassen die benötigten Ports (z. B. UDP 500 für IKEv2) nicht durch.
- Windows priorisiert PPTP, wenn kein anderes Protokoll antwortet – obwohl es veraltet und unsicher ist.
```

```markdown
*Note:*

*Our VPN connection was established using the insecure PPTP protocol, even though this was not explicitly configured.*

*Reason:*  
*With the setting **VPN type: Automatic**, Windows selects the “best available” protocol. However, it falls back to PPTP if the requirements for more secure protocols (IKEv2, SSTP, L2TP/IPsec) are not met.*

*Why is PPTP still selected?*

*- The VPN server does not support IKEv2 or SSTP (e.g., due to missing certificates or blocked ports).  
- Firewall or router rules do not allow the required ports (e.g., UDP 500 for IKEv2).  
- Windows prioritizes PPTP if no other protocol responds — even though it is outdated and insecure.*
```

Server2 --> Routing and RAS --> RAS-Clients:

<img width="845" height="408" alt="image" src="https://github.com/user-attachments/assets/e1b4f983-ed91-4c68-8748-52ba30853b17" />

<img width="995" height="339" alt="image" src="https://github.com/user-attachments/assets/9b32a954-ccbb-465c-b52b-f0782807d459" />

<img width="860" height="423" alt="image" src="https://github.com/user-attachments/assets/ead0ea79-b36a-4327-ba25-05c68b7028f1" />

### Ändern der Kontoeinstellungen  
### *Changing account settings*

DC --> Server-Manager --> Tools --> Active Directory-Benutzer und -Computer:  
*DC --> Server Manager --> Tools --> Active Directory Users and Computers:*

<img width="508" height="668" alt="image" src="https://github.com/user-attachments/assets/4c973e96-7e6b-48a3-9c00-af211317848e" />

Übernehmen + OK  
*Apply + OK*

<img width="979" height="543" alt="image" src="https://github.com/user-attachments/assets/f98efdbc-308f-45ad-9bcf-05d9d02a0dc1" />

### Ändern der Zugriffsrichtlinie „Verbindung mit dem Microsoft Routing- und RAS-Server“  
### *Changing the “Connect to Microsoft Routing and RAS Server” access policy*

<img width="405" height="525" alt="image" src="https://github.com/user-attachments/assets/13a82936-b98b-4f2c-94d0-abad3921caef" />

<img width="904" height="504" alt="image" src="https://github.com/user-attachments/assets/11d758ad-92d3-4de6-8298-98b8ca8c3651" />

<img width="884" height="704" alt="image" src="https://github.com/user-attachments/assets/1fcc9c99-f85e-449b-a9dc-41656fa81341" />

<img width="907" height="753" alt="image" src="https://github.com/user-attachments/assets/f034790c-ff30-4498-bedd-862d0d106c30" />

Übernehmen + OK  
*Apply + OK*


Hinweis: Die NPS-Richtlinie „Verbindungen mit Microsoft-Routing- und Remotezugriffsserver“ wird automatisch für RAS-Verbindungen genutzt;
die Bedingung MS-RAS-Hersteller-ID = ^311$ steht für Microsoft RAS-Clients, also Geräte, die sich per VPN oder Einwahl über RRAS mit 
dem Server verbinden (z. B. via SSTP, L2TP oder PPTP).


*Note: The NPS policy “Connections with Microsoft Routing and Remote Access Server” is automatically applied for RAS connections; the condition*  
MS-RAS-Manufacturer-ID = ^311$ *identifies Microsoft RAS clients, i.e., devices connecting to the server via VPN or dial-up through 
RRAS (e.g., SSTP, L2TP, or PPTP).*


### Testen des Zugriffs  
### *Testing access*

<img width="1006" height="342" alt="image" src="https://github.com/user-attachments/assets/4fc82535-105f-44df-866a-5a9d45f60b2c" />

<img width="984" height="577" alt="image" src="https://github.com/user-attachments/assets/c2b7af81-7483-49a4-acb7-09b7b08f9151" />

<img width="708" height="606" alt="image" src="https://github.com/user-attachments/assets/14d70c6b-53f9-4240-a744-ab2dc05389a1" />

### Erstellen der neuen Zugriffsrichtlinie  
### *Creating the new access policy*

<img width="1130" height="630" alt="image" src="https://github.com/user-attachments/assets/fcf402ac-3288-4a88-9766-7b7e332897aa" />

<img width="898" height="755" alt="image" src="https://github.com/user-attachments/assets/5652390a-597e-47f1-ba32-7a28850bc725" />

Übernehmen + OK  
*Apply + OK*

<img width="425" height="581" alt="image" src="https://github.com/user-attachments/assets/8ebdbb3c-01fe-4832-99f3-f610ebe5f8ba" />

<img width="673" height="763" alt="image" src="https://github.com/user-attachments/assets/1bd31962-f0cb-47cc-a002-de4519704f53" />

<img width="844" height="663" alt="image" src="https://github.com/user-attachments/assets/76d57b5f-5fb6-4dc5-88d1-635b752fb994" />

<img width="839" height="601" alt="image" src="https://github.com/user-attachments/assets/4a8c7e78-0f05-4db4-b402-3f1a05e74e0a" />

<img width="499" height="379" alt="image" src="https://github.com/user-attachments/assets/ae1a4786-d756-446d-a04e-6500c88b7e54" />

<img width="578" height="354" alt="image" src="https://github.com/user-attachments/assets/d49e9640-1c63-4bcf-a443-e2662f91b94c" />

OK + OK

<img width="843" height="760" alt="image" src="https://github.com/user-attachments/assets/8ee9fa79-bc92-436f-acf3-82e1b7e4b3d6" />

<img width="845" height="759" alt="image" src="https://github.com/user-attachments/assets/cb065ec6-af84-4229-88d8-69673b73bb24" />

<img width="598" height="431" alt="image" src="https://github.com/user-attachments/assets/f34aec35-4c1d-4905-b2b7-ab1d4bb8c8d9" />

<img width="400" height="286" alt="image" src="https://github.com/user-attachments/assets/3d7c095d-ceab-4c08-86f0-158abc5216ed" />

<img width="402" height="286" alt="image" src="https://github.com/user-attachments/assets/641d3ff2-c100-4132-885d-800db4f803cc" />

<img width="663" height="767" alt="image" src="https://github.com/user-attachments/assets/c8416f1b-9998-4970-9220-3c329b193274" />

<img width="648" height="762" alt="image" src="https://github.com/user-attachments/assets/0f3695a5-7e72-4a48-a32c-90cfddd13236" />

<img width="657" height="757" alt="image" src="https://github.com/user-attachments/assets/25e23565-6009-437c-9905-f82e29cbf0e9" />

<img width="844" height="763" alt="image" src="https://github.com/user-attachments/assets/b3a422ad-f7ec-4fda-bed1-0749bd361bc1" />

Fertigstellen  
*Finish*

### Reihenfolge der Netzwerkrichtlinien ändern, falls nötig  
### *Change the order of network policies if necessary*

<img width="1065" height="425" alt="image" src="https://github.com/user-attachments/assets/1373d71f-ac92-4d88-bcd4-0347513c758a" />

### Testen des Zugriffs  
### *Testing access*

W11:

<img width="995" height="342" alt="image" src="https://github.com/user-attachments/assets/e00ea05e-0b5b-4d07-a24b-1e2392077a28" />

<img width="979" height="546" alt="image" src="https://github.com/user-attachments/assets/fa9ebdd8-9202-4861-b844-7448bad9d171" />

Die Verbindung wird verweigert (Karl Klammer ist kein Domänen-Admin).  
#The connection is denied (Karl Klammer is not a domain administrator).*

<img width="1004" height="228" alt="image" src="https://github.com/user-attachments/assets/6f1884e1-ee24-4bb4-a024-a0782977b9aa" />

<img width="468" height="602" alt="image" src="https://github.com/user-attachments/assets/adad1619-e833-4dce-812a-d2e38bb22861" />

<img width="476" height="500" alt="image" src="https://github.com/user-attachments/assets/adf0447b-7d2d-4734-be9d-27f635adc210" />

<img width="991" height="425" alt="image" src="https://github.com/user-attachments/assets/40c18903-fe4a-4217-b62f-4d69b63f340d" />

<img width="974" height="650" alt="image" src="https://github.com/user-attachments/assets/b924c39e-bd8e-4e46-a720-65e6b3ba5f8c" />

<img width="686" height="346" alt="image" src="https://github.com/user-attachments/assets/1542286c-f660-4d93-8825-e42e053ed45f" />

Trenne AdminVPN wieder.  
*Disconnect AdminVPN again.*



