# Erstellung eines DFS-Namespaces mit dem Namen „Firmendaten“ auf Server2
# Create a DFS namespace named “Company Data” on Server2

## Das Prinzip von DFS (Distributed File System)

## Distributed File System (DFS)

### Grundidee
Ein Distributed File System speichert Dateien verteilt auf mehreren Rechnern, lässt sie für Nutzer und Programme aber wie ein einziges, normales Dateisystem aussehen.

### Wie es funktioniert
- Dateien werden in Blöcke zerlegt
- Blöcke liegen auf verschiedenen Servern (Nodes)
- Ein zentrales oder verteiltes Metadaten-System weiß:
  - wo welcher Block liegt
  - wem die Datei gehört
  - welche Zugriffsrechte gelten
- Clients greifen transparent darauf zu, als wäre es lokal

### Zentrale Eigenschaften
- **Skalierbar:** Speicher wächst mit neuen Nodes
- **Fehlertolerant:** Replikation der Daten
- **Parallelzugriffe möglich**
- **Ortstransparenz:** User muss Speicherort nicht kennen

### Typische Architekturen
- **Master–Worker**
  - Master: Metadaten
  - Worker: eigentliche Daten
  - Beispiel: HDFS
- **Voll verteilt (Peer-to-Peer)**
  - kein zentraler Master
  - Beispiel: Ceph

### Beispiele
- HDFS (Hadoop Distributed File System)
- NFS (Network File System)
- Ceph FS
- GlusterFS

### Vorteile
- Hohe Verfügbarkeit
- Große Datenmengen handhabbar
- Gute Performance bei parallelem Lesen

### Nachteile
- Komplexe Verwaltung
- Netzwerkabhängig
- Konsistenz schwieriger als lokal

### Typische Einsatzgebiete
- Big-Data-Analyse
- Cloud-Speicher
- Unternehmensnetzwerke
- Hochverfügbare Systeme

## Installation DFS-Rolle auf einem beliebigen Server (z.B. Server2)  
*Installing the DFS role on a server (e.g., Server2)*

<img width="1058" height="829" alt="image" src="https://github.com/user-attachments/assets/63b82717-b45c-4fb2-8c98-08bb7eee66bd" />

Auf Weiter klicken  
*Continue*

<img width="983" height="701" alt="image" src="https://github.com/user-attachments/assets/dbdec601-e6ce-44e1-96e8-66bb2b75ce3f" />

<img width="807" height="574" alt="image" src="https://github.com/user-attachments/assets/b9ee70b8-8f5e-4ff3-9fbb-c7643ab726e9" />

<img width="977" height="694" alt="image" src="https://github.com/user-attachments/assets/4f68a871-8923-475f-a992-2e7b242d4d0e" />

<img width="517" height="541" alt="image" src="https://github.com/user-attachments/assets/484fbeed-e7ae-44c2-8794-910c978ae46d" />

<img width="976" height="696" alt="image" src="https://github.com/user-attachments/assets/e22dba80-c733-4fd0-9121-ceb1ebdcc395" />

Bei Features --> weiter  
*In features --> continue*

<img width="801" height="573" alt="image" src="https://github.com/user-attachments/assets/5ad195cb-0295-4191-ba2f-f3e3af2a3463" />

Installieren:

<img width="803" height="575" alt="image" src="https://github.com/user-attachments/assets/0c6674e2-0e8d-4612-a7d1-8b1ef0d82085" />

## Einrichten des Namespaces  
*Setting up the namespace*

Tools --> DFS-Verwaltung  
*Tools --> DFS Management*

<img width="410" height="602" alt="image" src="https://github.com/user-attachments/assets/f4556344-2e88-4255-80fd-97d01c41b15f" />

Klicken Sie mit der rechten Maustaste auf „Namespaces“  
*Right-click on “Namespaces.”*

<img width="1031" height="730" alt="image" src="https://github.com/user-attachments/assets/2cbb8005-1909-4f07-8555-865e7f2d3333" />

<img width="886" height="720" alt="image" src="https://github.com/user-attachments/assets/3a64b422-fbaf-49f6-aa3b-fd1e48a2a79b" />

<img width="881" height="721" alt="image" src="https://github.com/user-attachments/assets/d82656e4-9430-412e-a07e-4ee205a787c6" />

Name: Firmendaten --> auf "Einstellung bearbeiten" klicken  
*click on Edit company data*

<img width="733" height="601" alt="image" src="https://github.com/user-attachments/assets/3124cfec-3a57-4d37-93a7-54878d2988cc" />

Betrachten, aber keine Änderungen vornehmen, das sind die Rechte auf den DFS-Stamm, nicht auf die Ordner!  
*View, but do not make changes—these are the rights to the DFS root, not to the folders!*

Weiter  
*Continue*

<img width="890" height="722" alt="image" src="https://github.com/user-attachments/assets/d70d7d40-65b5-4864-89b1-58a2e48e8466" />

Domänenbasierter Namespace --> Haken bei „Windows Server 2008-Modus aktivieren“ --> Weiter  
*Domain-based namespace --> Check “Enable Windows Server 2008 mode” --> Next*

<img width="882" height="720" alt="image" src="https://github.com/user-attachments/assets/28bfb449-d682-47ff-addd-1f15746eb250" />

Erstellen  
*Create*

<img width="889" height="721" alt="image" src="https://github.com/user-attachments/assets/c5945197-acb2-41f0-af2c-78619b2c6595" />

Schließen  
*Close*

<img width="885" height="721" alt="image" src="https://github.com/user-attachments/assets/fa35cd06-e0fd-481f-8a9f-dd23c6195567" />

<img width="313" height="442" alt="image" src="https://github.com/user-attachments/assets/6f6ac95f-d975-441e-88f5-9bc9f0eccca4" />

## Erstellen und Freigeben des Ordners  
*Creating and sharing the folder*

Öffnen Sie auf Server2 den Windows-Explorer und legen Sie im Laufwerk „C:\“ einen Ordner mit Namen „Briefe“ an  
*Open Windows Explorer on Server2 and create a folder named “Briefe” in the “C:\” drive.*

<img width="1020" height="784" alt="image" src="https://github.com/user-attachments/assets/5415b812-9cd6-4116-a08d-b81b4e5c66d2" />

Freigabe ändern  
*Change release settings*

<img width="1014" height="771" alt="image" src="https://github.com/user-attachments/assets/19ea43ec-6486-450f-b790-f2ffab85c077" />

<img width="501" height="521" alt="image" src="https://github.com/user-attachments/assets/066f1a5c-98c1-4c95-87de-58f02e4dc795" />

Haken setzen vor „Diesen Ordner freigeben“ --> Im unteren Bereich Klicken auf „Berechtigungen“  
*Check the box next to “Share this folder.” --> In the lower section, click on “Permissions.*

<img width="436" height="455" alt="image" src="https://github.com/user-attachments/assets/1cd892a5-fe6f-45ef-a001-ffaf530ee81d" />

Hinzufügen  
*Add*

<img width="449" height="574" alt="image" src="https://github.com/user-attachments/assets/faaf3582-ac94-49c3-8de9-442d8edf56d0" />

Auswahl „Authentifizierten Benutzer“  
*Select “Authenticated User”*

<img width="565" height="309" alt="image" src="https://github.com/user-attachments/assets/accef41f-63de-47fa-a0d0-137017c3cba7" />

Vollzugriff  
*full access*

<img width="445" height="577" alt="image" src="https://github.com/user-attachments/assets/5ce3ef99-7741-4d81-962e-0cb9a2aaded0" />

Übernehmen + OK  
*Apply + OK*


## Hinzufügen des Ordnerziels  
*Adding the folder destination*

Server Manager --> Tools --> DFS-Verwaltung  
*Server Manager --> Tools --> DFS Management*

<img width="642" height="722" alt="image" src="https://github.com/user-attachments/assets/524bc38b-28ee-4677-967e-e030b94153db" />

Name: Briefe  --> Auf Hinzufügen klicken  
*Name: Briefe --> Click Add*

<img width="487" height="513" alt="image" src="https://github.com/user-attachments/assets/64366cc7-ca4e-4eca-a6bc-a6aa2a74755c" />

Netzwerkadresse der oben freigegeben Ordner eintragen + OK  
*Enter the network address of the shared folder above + OK*

\\Server2\Briefe

<img width="493" height="218" alt="image" src="https://github.com/user-attachments/assets/1fef6459-7ed1-45bf-8743-6895222c2332" />

OK

<img width="487" height="515" alt="image" src="https://github.com/user-attachments/assets/74978ffa-6266-4feb-9458-8f0fcf55dba5" />

## Erstellung eines replizierenden Ordnerziels für den Ordner „Briefe“ im DFS-Namespace „Firmendaten“ auf Server2.  
*Create a replicating folder target for the “Briefe” folder in the “Company Data” DFS namespace on Server2.*

Replikation bedeutet, dass Ordnerinhalte automatisch zwischen mehreren Servern synchron gehalten werden.
In dieser Anleitung wird die Replikation der Daten von Server2 auf Server3 initiiert, sodass Änderungen auf Server2 automatisch auf Server3 übernommen werden.  

*Replication means that folder contents are automatically kept in sync between multiple servers.
In this guide, replication from Server2 to Server3 is initiated so that changes made on Server2 are automatically transferred to Server3.*


Öffnen Sie auf Server3 den Windows-Explorer und legen Sie im Laufwerk „C:\“ einen Ordner mit Namen „Briefe_Server3“ an  
*Open Windows Explorer on Server3 and create a folder named “Letters_Server3” in the “C:\” drive.*

<img width="979" height="777" alt="image" src="https://github.com/user-attachments/assets/b2b1c9f5-dce4-4d15-a2f8-de623d9a2153" />

<img width="481" height="466" alt="image" src="https://github.com/user-attachments/assets/03c4bd90-ec8f-4a0d-94f9-c4ae8c38cf8b" />

Auf Berechtigungen klicken  
*Click on Permissions*

<img width="437" height="452" alt="image" src="https://github.com/user-attachments/assets/beb4de2d-f65b-4619-89b7-032cf4aaae76" />

"Authentifizierten Benutzer" hinzufügen + Vollzugriff gewähren + Übernehmen + OK  
*Add “Authenticated User” + Grant Full Access + Apply + OK*

## DFS-Replikation auf Server3 installieren  
*Install DFS replication on Server3*

<img width="1271" height="541" alt="image" src="https://github.com/user-attachments/assets/a0a1cc7c-9ba1-455f-b312-d4b714621aaa" />

Weiter  
*Continue*

<img width="973" height="698" alt="image" src="https://github.com/user-attachments/assets/e465f3bf-c7f0-40d3-a1b2-ed054b94f6a9" />

<img width="975" height="694" alt="image" src="https://github.com/user-attachments/assets/8f4c4d6e-32a3-4656-b15f-08df32c9fede" />

<img width="975" height="691" alt="image" src="https://github.com/user-attachments/assets/204a59ae-f55c-4226-b20a-af7d1216d052" />

Serverrollen auswählen --> Erweitern Sie „Datei- und Speicherdienste“ --> Datei- und iSCSI-Dienste --> DFS-Replikation + Weiter  
*Select server roles --> Expand “File and Storage Services” --> File and iSCSI Services --> DFS Replication + Continue*

<img width="684" height="564" alt="image" src="https://github.com/user-attachments/assets/585d2986-48b1-4b97-ab94-31ae2ceb541c" />

<img width="513" height="534" alt="image" src="https://github.com/user-attachments/assets/b68f72d0-dd06-401b-8f37-40081cc240d2" />

Weiter  
*Continue*

<img width="971" height="695" alt="image" src="https://github.com/user-attachments/assets/c20cedcf-a0da-484d-b9ef-737d5e7e0023" />

Installieren  
*Install*

<img width="976" height="692" alt="image" src="https://github.com/user-attachments/assets/636d4b05-ec82-49a9-9969-92f41421b33d" />

### Einrichten des replizierenden Ordnerziels  
*Setting up the replicating folder destination*

Wechseln Sie auf „Server2“ --> Server-Manager --> Tools --> DFS-Verwaltung<br>
Erweitern Sie DFS-Verwaltung --> Namespaces --> xxx\Firmendaten  

*Switch to “Server2” --> Server Manager --> Tools --> DFS Management<br>
Expand DFS Management --> Namespaces --> xxx\CompanyData*

<img width="701" height="542" alt="image" src="https://github.com/user-attachments/assets/226ddc68-9f42-4ad2-ae72-d4ad0833f19f" />

Pfad zum Ordnerziel: \\Server3\Briefe_Server3  
*Path to the destination folder: \\Server3\Briefe_Server3*

<img width="565" height="282" alt="image" src="https://github.com/user-attachments/assets/4769d3fd-1ead-4b0d-b2c1-6f2c1a0913de" />

OK

<img width="489" height="213" alt="image" src="https://github.com/user-attachments/assets/45a9a1ac-b4e4-4a76-baf0-9b8a842bc8d4" />

<img width="566" height="139" alt="image" src="https://github.com/user-attachments/assets/b6a522e2-315d-4968-becf-a351a9f24781" />

Weiter  
*Continue*

<img width="883" height="721" alt="image" src="https://github.com/user-attachments/assets/552b18f5-9326-4146-9d19-3635e030cdf6" />

<img width="886" height="721" alt="image" src="https://github.com/user-attachments/assets/30909dd0-d0bf-4137-a034-0625fc286a41" />

Primäres Mitglied: Server2  
*rimary member: Server2*

<img width="884" height="722" alt="image" src="https://github.com/user-attachments/assets/bde541f5-ccd0-4545-bd7e-183c4511a0af" />

Topologieauswahl: vollständig vermaschtes Netz + Weiter  
*Topology selection: fully meshed network + Continue*

<img width="883" height="721" alt="image" src="https://github.com/user-attachments/assets/88c55f14-bde2-43b3-953b-61fe3263fda7" />

Weiter  
*Continue*

<img width="882" height="722" alt="image" src="https://github.com/user-attachments/assets/ca2a6752-b1cb-4ecd-81ee-e56091c440f4" />

Erstellen  
*Create*

<img width="886" height="722" alt="image" src="https://github.com/user-attachments/assets/c1b98757-f6b9-4fe6-bc47-5499264fca39" />


Schließen  
*Close*

<img width="888" height="718" alt="image" src="https://github.com/user-attachments/assets/c41273fa-7e7c-4f30-b8ed-5aeaabdc7f82" />

OK

<img width="584" height="207" alt="image" src="https://github.com/user-attachments/assets/36e4ecdc-8dbf-4b7a-a048-15d49f4339ee" />


## Erstellung eines Stammreplikats auf Server3 für den DFS-Namespace „Firmendaten“ auf Server2.  
*Creation of a master replica on Server3 for the DFS namespace “Company Data” on Server2.*

Server3 --> Service-Manager --> Verwalten --> Rollen und Featues hinzufügen  
*Server3 --> Service Manager --> Manage --> Add roles and features*

<img width="438" height="499" alt="image" src="https://github.com/user-attachments/assets/d51443f9-e332-4e30-aacf-fc2fa7fa71d0" />

Installieren  
*Install*

<img width="977" height="695" alt="image" src="https://github.com/user-attachments/assets/24923fb6-f174-451b-951a-c0d18bf230bf" />

### Einrichten des Stammreplikats  
*Setting up the master replica*

Wechseln Sie auf „Server2“ --> Wählen Sie im Server-Manager --> Tools --> DFS-Verwaltung  
*Switch to “Server2” --> Select Server Manager --> Tools --> DFS Management*

Namespaceserver hinzufügen  
*Add Namespace Server*

<img width="877" height="703" alt="image" src="https://github.com/user-attachments/assets/bfa5d6a6-a448-4658-8afe-5ad0c3ee377f" />

<img width="487" height="397" alt="image" src="https://github.com/user-attachments/assets/ae13d754-6437-46b0-95b8-3c684019f269" />

<img width="556" height="137" alt="image" src="https://github.com/user-attachments/assets/6c064e35-c0a0-41a2-af87-e8551eccd6e6" />

Beide Namespaceserver sind nun in der Konsole im mittleren Fenster unter „Namespaceserver“ zu finden.  
*Both namespace servers can now be found in the console in the middle window under “Namespace Server.”*

<img width="696" height="342" alt="image" src="https://github.com/user-attachments/assets/8e2866a5-8cf8-4d1a-a2a7-58231f60a88d" />

## Aktivierung der zugriffsbasierten Aufzählung im DFS-Namespace „Firmendaten“ und Änderung der Dauer der Zwischenspeicherung des Ordnerziels im Client-Cache auf 360 Sekunden.  
*Enabling access-based enumeration in the “Company Data” DFS namespace and changing the duration of the folder destination cache to 360 seconds.*

Server2 --> Tools --> DFS-Verwaltung  
*Server2 --> Tools --> DFS Management*


