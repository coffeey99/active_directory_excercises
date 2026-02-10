# iSCSI-Server einrichten
# *Set up iSCSI server*


iSCSI steht für Internet Small Computer System Interface.
Es ist ein Protokoll, mit dem du Speicher über ein Netzwerk so nutzen kannst, als wäre er direkt an deinem Rechner angeschlossen.

## Wozu man iSCSI braucht

### 1. Zentrale Speicherung
- Mehrere Server greifen auf denselben Speicher zu (SAN – Storage Area Network)
- Keine lokale Festplatte erweitern, sondern Storage im Netzwerk nutzen

### 2. Flexibilität
- Volumes (LUNs) können dynamisch zugewiesen oder vergrößert werden
- Einfaches Backup, Snapshots oder Migration

### 3. Kosteneffizient
- Nutzung von normalem TCP/IP statt teurem Fibre Channel
- LAN/WAN kann für Storage-Verbindungen verwendet werden

### 4. Virtualisierung
- Hyper-V- oder VMware-Hosts speichern VHDX/VMFS direkt auf iSCSI-LUNs
- Hochverfügbarkeit durch Clustering

---

## Vereinfachtes Beispiel

- **Server 1** → iSCSI-Initiator  
- **Storage** → iSCSI-Target (enthält LUNs)  
- **Server 1** bindet **LUN 1** → erscheint als normale Festplatte  
- **Server 2** bindet **LUN 2** → erscheint als normale Festplatte  

Der Zugriff erfolgt über das Netzwerk, für den Server wirkt der Speicher jedoch wie **lokaler Blockspeicher** und nicht wie ein klassischer Netzwerkshare.  

## *What is iSCSI*

*iSCSI stands for **Internet Small Computer System Interface**.*  
*It is a protocol that allows you to use storage over a network as if it were directly connected to your computer.*

---

## *Why you need iSCSI*

### *1. Centralized storage*
*- Multiple servers access the same storage (SAN – Storage Area Network)*  
*- No need to expand local disks; storage is provided over the network*

### *2. Flexibility*
*- Volumes (LUNs) can be dynamically allocated or expanded*  
*- Easy backups, snapshots, and migrations*

### *3. Cost-effective*
*- Uses standard TCP/IP instead of expensive Fibre Channel*  
*- Storage can be connected over LAN or WAN*

### *4. Virtualization*
*- Hyper-V or VMware hosts can store VHDX/VMFS directly on iSCSI LUNs*  
*- High availability through clustering*

---

## *Simplified example*

*- **Server 1** → iSCSI initiator*  
*- **Storage** → iSCSI target (contains LUNs)*  
*- **Server 1** binds **LUN 1** → appears as a normal hard drive*  
*- **Server 2** binds **LUN 2** → appears as a hard drive for Server 2*

*All communication runs over the network, but the server sees the storage as **local block storage**, not just a network share.*

<br>
<br>

„Server2“ als iSCSI-Zielserver einrichten und füge zur VM „Server2“  und „Server3“ eine weitere Netzwerkkarte hinzu.  
*Set up “Server2” as an iSCSI target server and add another network card to the ‘Server2’ and “Server3” VMs.*

### Methode 1:  
### *Methode 1:*

<img width="449" height="547" alt="image" src="https://github.com/user-attachments/assets/42da9397-9128-453c-b881-010184906306" />

<img width="532" height="522" alt="image" src="https://github.com/user-attachments/assets/313b46ca-0137-4e0e-bb89-b20403ab847a" />

Anwenden + OK  
*Apply + OK*

<img width="380" height="323" alt="image" src="https://github.com/user-attachments/assets/a6904bb9-e075-4564-b52d-260a691c9ec6" />

<img width="889" height="397" alt="image" src="https://github.com/user-attachments/assets/fbf9b03b-506c-4165-a4df-a2b68b3b1def" />

<img width="540" height="423" alt="image" src="https://github.com/user-attachments/assets/a066f464-a1c6-4f47-87c6-531f4dfa73cf" />

Anwenden + OK  
*Apply + OK*

Konfiguriere auf der neuen Netzwerkkarte „Ethernet2“ Folgendes:  
*Configure the following on the new network card “Ethernet2”:*

o IP-Adresse 172.16.1.2
o Maske 255.255.255.

<img width="889" height="453" alt="image" src="https://github.com/user-attachments/assets/80b2d8fc-bc90-45fe-8ee3-c12162d69a21" />

<img width="468" height="361" alt="image" src="https://github.com/user-attachments/assets/241c749f-b720-4b64-87f4-642e6e7d2532" />

<img width="446" height="581" alt="image" src="https://github.com/user-attachments/assets/0b0560ad-c99d-48ff-b985-432f43b32758" />

<img width="490" height="563" alt="image" src="https://github.com/user-attachments/assets/ef66863d-c7ec-4355-83e8-94036d02f541" />

### Methode 2:  
### *Methode 2:*

Öffne auf dem Hyper-V-Host die Powershell-ISE als Administrator:  
*Open Powershell ISE as an administrator on the Hyper-V host:*

<img width="771" height="344" alt="image" src="https://github.com/user-attachments/assets/e2c7afd1-3b4b-4c0b-8dea-52c8e2859a4b" />

```powershell
# Server3 wird ein zusätzlicher Netzwerkadapter hinzugefügt.
Add-VMNetworkAdapter -VMName "Server3" -SwitchName "Hamburg"
# verschlüsseltes Speichern des Klartext-Passwortes
$AdminPwd = ConvertTo-SecureString "Kennw0rt!" -AsPlainText -Force
# Speichern des Anmeldekontos
$Konto = "gfnlab\administrator"
# Definition des gleich zu erstellenden Objekttyps "PSCredential"
$ObjectTypPSCredential = "System.Management.Automation.PSCredential"
# Erstellen des PSCredential Objects für die spätere Anmeldung
$AdminCred = New-Object $ObjectTypPSCredential ($Konto, $AdminPwd)
# Netzwerkkonfiguration von Server 3 via PowershellDirect
Invoke-Command -VMName "Server3" `
 -ScriptBlock `
 {New-NetIPAddress `
 -InterfaceAlias "Ethernet 2" `
 -IPAddress "172.16.1.3" `
 -PrefixLength 24 `
 } `
 -Credential $AdminCred
```

Skript ausführen (F5)  
*Run script*

<img width="580" height="864" alt="image" src="https://github.com/user-attachments/assets/04bdaea8-83f6-4b30-b588-f87d2ff839df" />

<img width="608" height="870" alt="image" src="https://github.com/user-attachments/assets/90e89449-340f-424e-8928-430f87427f6d" />

<img width="767" height="389" alt="image" src="https://github.com/user-attachments/assets/dbfc3a30-04b4-4b0f-906a-e0b11f3f474f" />

Wechsele zu Server2 --> Verwalten --> Rollen und Features hinzufügen --> Serverrollen auswählen --> Datei-/Speicherdienste --> Datei- und iSCSI-Dienste" --> iSCSI-Zielserver  
*Switch to Server2 --> Manage --> Add Roles and Features --> Select Server Roles --> File/Storage Services --> File and iSCSI Services --> iSCSI Target Server*

<img width="735" height="699" alt="image" src="https://github.com/user-attachments/assets/9dc4c022-e6d2-4a52-821b-3d8c42dd13e5" />

<img width="517" height="542" alt="image" src="https://github.com/user-attachments/assets/e04b2cca-c746-4625-b54c-710dff205487" />

Installieren + Schließen  
*Install + Close*

Installation des iSCSI-Targets  
*Installing the iSCSI target*

<img width="1283" height="407" alt="image" src="https://github.com/user-attachments/assets/4d2817b1-4e61-47d5-bd03-d4d312ce2507" />

<img width="938" height="692" alt="image" src="https://github.com/user-attachments/assets/c6b9cfe0-3abe-4687-8c5f-95bb8ca4a43b" />

<img width="938" height="692" alt="image" src="https://github.com/user-attachments/assets/80cc9bfb-5b4b-4dbd-8bc5-f41c8a848a51" />

<img width="938" height="690" alt="image" src="https://github.com/user-attachments/assets/7ecdace7-21fa-43a5-a30c-21cb197f8996" />

<img width="939" height="692" alt="image" src="https://github.com/user-attachments/assets/df7b6225-4d28-44d1-b132-0b1ae3803f7f" />

<img width="940" height="695" alt="image" src="https://github.com/user-attachments/assets/41644c25-3c62-428d-b956-5c4fc18d89e4" />

<img width="711" height="554" alt="image" src="https://github.com/user-attachments/assets/b370087a-885b-40b0-a229-f8533b45e8f7" />

<img width="622" height="648" alt="image" src="https://github.com/user-attachments/assets/6667e0d8-1608-4200-9c56-4cf7df3288e3" />

Nochmal Hinzufügen --> IP-Adresse: 192.168.1.3  
*Add again --> IP address: 192.168.1.3*

<img width="733" height="696" alt="image" src="https://github.com/user-attachments/assets/331fa035-559b-496d-b39f-df685bf33ca5" />

<img width="733" height="696" alt="image" src="https://github.com/user-attachments/assets/3288540d-1da7-48d6-bb6e-73c3210eae90" />

<img width="943" height="694" alt="image" src="https://github.com/user-attachments/assets/cb779278-f794-4cf4-9a16-f93275793d1e" />

Und dann Schließen  
*Close*

<img width="950" height="696" alt="image" src="https://github.com/user-attachments/assets/50db9b29-13de-4092-8c3b-6f8a429ff171" />

Verbinden des ISCI-Targets mit Server3 als SCSI-Initiator  
*Connecting the ISCI target to Server3 as a SCSI initiator*

Server3 --> Tools  --> ISCI-Initiator

<img width="503" height="227" alt="image" src="https://github.com/user-attachments/assets/614e9a65-b1c0-417f-8e02-59bb309f5050" />

<img width="590" height="321" alt="image" src="https://github.com/user-attachments/assets/7cc06155-5098-4394-8113-c3e4a4e83a62" />

<img width="543" height="636" alt="image" src="https://github.com/user-attachments/assets/94f2d32c-eaf5-4db9-b50a-b8c9256e162e" />

<img width="592" height="684" alt="image" src="https://github.com/user-attachments/assets/04e9cebc-425d-486d-bea6-379d83035614" />

<img width="589" height="865" alt="image" src="https://github.com/user-attachments/assets/6869ca7d-6e34-4bee-924d-207898c81d44" />

Server3 --> Datenträgerverwaltung  
*Server3 --> data carrier management*

<img width="426" height="559" alt="image" src="https://github.com/user-attachments/assets/d57daac5-ea9b-4e0a-9857-17bb111e4947" />

<img width="748" height="553" alt="image" src="https://github.com/user-attachments/assets/abbdcc83-aab6-45f7-88f4-59c37dd07112" />

<img width="748" height="553" alt="image" src="https://github.com/user-attachments/assets/f82fa0d4-9531-4faa-9733-b5f28c8dc882" />

<img width="742" height="551" alt="image" src="https://github.com/user-attachments/assets/d459cddb-e374-4ba4-9b0d-9828b088994f" />

<img width="555" height="290" alt="image" src="https://github.com/user-attachments/assets/64a177c0-70a1-454e-b4d8-c0a8d9e08d2d" />

<img width="612" height="480" alt="image" src="https://github.com/user-attachments/assets/3a948370-879a-42f8-acf5-9b4e741ed439" />

Fertig stellen  
*Finish*

Erstelle im Fileexplorer auf Laufwerk „I“ ein großes jpeg-Bild.  
*Create a large jpeg image on drive “I” in File Explorer.*

Fertig stellen  
*Finish*

Erstelle im Fileexplorer auf Laufwerk „I“ ein großes jpeg-Bild.  
*Create a large jpeg image on drive “I” in File Explorer.*

Installiere MPIO auf „Server3“  
*Install MPIO on “Server3.”*

MPIO bedeutet, dass mehrere physische Datenpfade zwischen Server und Storage genutzt werden.

Wozu?
- **Ausfallsicherheit:** Fällt ein Pfad (Kabel, Switch, HBA) aus, bleibt der Zugriff bestehen
- **Performance:** Je nach Modus kann der Traffic auf mehrere Pfade verteilt werden

Typische Einsatzfälle
- SAN-Umgebungen (iSCSI, Fibre Channel)
- Datenbanken
- Virtuelle Maschinen

Beispiel  
Ein Server hat **2 Netzwerkkarten → 2 Switches → 2 Storage-Controller**.  
MPIO sorgt dafür, dass der Storage auch bei einem Ausfall weiterhin erreichbar bleibt.


*Multipath I/O (MPIO)*

*MPIO means that multiple physical data paths are used between the server and storage.*

*What for?*
- **Reliability:** If one path (cable, switch, HBA) fails, access continues
- **Performance:** Depending on the mode, traffic can be distributed across multiple paths

*Typical use cases*
- SAN environments (iSCSI, Fibre Channel)
- Databases
- Virtual machines

*Example*  
*A server has **2 network cards → 2 switches → 2 storage controllers**.*  
*MPIO ensures that the storage remains accessible despite a failure.*

<br>
<br>
Server3 --> Verwalten --> Rollen und Features hinzufügen --> Weiter --> Weiter --> Weiter --> Weiter  
*Server3 --> Manage --> Add roles and features --> Next --> Next --> Next --> Next*

<img width="960" height="695" alt="image" src="https://github.com/user-attachments/assets/ee777448-9e01-46f5-ba44-7cfbaaa9fe8c" />

Installieren +  Schließen  
*install + close*

Server3 --> Tools --> iSCSI-Initiator

<img width="587" height="596" alt="image" src="https://github.com/user-attachments/assets/72449b07-798f-4d12-933e-5d52cc1e1822" />

<img width="359" height="177" alt="image" src="https://github.com/user-attachments/assets/2bfb41bb-6d5b-4792-923b-b8217a1fbf8b" />

<img width="586" height="384" alt="image" src="https://github.com/user-attachments/assets/8a526e3d-4719-43c2-b383-8b55190f5c4d" />

<img width="500" height="190" alt="image" src="https://github.com/user-attachments/assets/828eb0c1-0473-428b-926a-0ca8df1e4a09" />

<img width="592" height="660" alt="image" src="https://github.com/user-attachments/assets/db683f9c-1309-4e71-b789-f4791108e0d1" />

Datenträgerverwaltung öffnen --> Datenträger1 wird nicht mehr angezeigt.  
*Open Disk Management --> Disk1 is no longer displayed.*

Server3 --> Tools --> MPIO

<img width="490" height="376" alt="image" src="https://github.com/user-attachments/assets/68c566e4-8ab1-4347-a5c1-289cc630b397" />

<img width="372" height="188" alt="image" src="https://github.com/user-attachments/assets/a102a776-bdf5-4e4d-980c-27a589621077" />

OK + Server3 neu starten  
*OK + Restart Server3*

Konfigurieren von MPIO für das vorhandene Ziel  
*Configuring MPIO for the existing target*

Server3 --> Tools --> iSCSI-Initiator

<img width="588" height="371" alt="image" src="https://github.com/user-attachments/assets/63b36518-ec16-44bb-8f44-13d3b351e888" />

Fertig  
*finish*

<img width="588" height="371" alt="image" src="https://github.com/user-attachments/assets/6214fe91-32f6-418f-bf9d-369b34e8c714" />

<img width="583" height="653" alt="image" src="https://github.com/user-attachments/assets/f9363e79-6160-4050-a71d-75f0f6317558" />

<img width="504" height="313" alt="image" src="https://github.com/user-attachments/assets/4cbde2c9-70fa-486c-8587-6a1bcb61bfd4" />

<img width="510" height="266" alt="image" src="https://github.com/user-attachments/assets/77456b93-45cd-45c6-b71e-bb5af9bb143e" />

<img width="644" height="801" alt="image" src="https://github.com/user-attachments/assets/534c5474-8a5f-40e3-a39b-72630eecb5e7" />

OK

Nochmal  
*again*

<img width="510" height="316" alt="image" src="https://github.com/user-attachments/assets/e88fc96b-c691-43c7-b1c9-5d1b6da2b9ef" />

<img width="645" height="258" alt="image" src="https://github.com/user-attachments/assets/71537ad0-4a07-49b2-b002-86cd89804aa6" />

OK + OK

<img width="645" height="258" alt="image" src="https://github.com/user-attachments/assets/955da20e-f70e-4f65-b92f-c48945822bf4" />

ID Nummer können variieren  
*ID numbers may vary*

Öffne auf Server3 die Powershell-ISE als Administrator:  
*Open Powershell ISE as an administrator on Server3:*

<img width="749" height="380" alt="image" src="https://github.com/user-attachments/assets/82c07f76-08ca-4a0f-9f87-570077d6bad6" />

```powershell
Get-IscsiSession |
Format-List -Property InitiatorPortalAddress,SessionIdentifier
```

iSCSI läuft über die NICs mit diesen IPs  
*iSCSI runs via the NICs with these Ips*

Prüfe, ob auf Laufwerk „I“ das Bild vorhanden ist.  
*Check whether the image is available on drive “I”.*

Prüfen der Redundanz:  
*Checking redundancy:*

Server3 --> Server-Manager --> Lokale Server
Deaktiviere den Adapter "Ethernet", aber lass das Fenster geöffnet.
Prüfe, ob auf Laufwerk „I“ das Bild vorhanden ist.
Aktiviere den Adapter erneut und deaktiviere nun den anderen Adapter (Ethernet 2).
(Du solltest immer noch Zugriff darauf haben und die Verbindung wird gehalten.)  

*Server3 --> Server Manager --> Local Servers
Disable the “Ethernet” adapter, but leave the window open.
Check whether the image is available on drive “I”.
Re-enable the adapter and now disable the other adapter (Ethernet 2).
(You should still have access to it and the connection will be maintained.)*

<img width="676" height="490" alt="image" src="https://github.com/user-attachments/assets/89627170-f472-4da6-ac93-d9fc2604a660" />

<img width="533" height="445" alt="image" src="https://github.com/user-attachments/assets/e5e3dcf9-0439-4390-ae8b-1a6e92178160" />

<img width="548" height="352" alt="image" src="https://github.com/user-attachments/assets/87f6ebda-4bb2-4075-91a4-2bec7e7c52c1" />

<img width="548" height="352" alt="image" src="https://github.com/user-attachments/assets/091df0bc-40fd-4cda-865a-d9bb238a655f" />

„Deaktiviere“ nun auch die andere Netzwerkkarte, sodass beide Netzwerkkarte deaktiviert sind, 
aber lass das Fenster geöffnet. --> Das Laufwerk „i“ wird im Fileexplorer nicht mehr angezeigt. Beide Verbindungen sind getrennt.  
*Now “disable” the other network card as well, so that both network cards are disabled, 
but leave the window open. --> The “i” drive is no longer displayed in File Explorer. Both connections are disconnected.*

<img width="709" height="298" alt="image" src="https://github.com/user-attachments/assets/7f232f2e-d226-4af0-b7df-2c8ca14c479d" />

<img width="908" height="355" alt="image" src="https://github.com/user-attachments/assets/ae411236-73a8-4195-a404-7face2c6c420" />

Aktiviere beide Netzwerkkarten wieder.  
*Reactivate both network cards.
