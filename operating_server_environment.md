<img width="690" height="406" alt="image" src="https://github.com/user-attachments/assets/8a10524e-ba30-45d2-bd2f-32066fc8ed9e" /># Die Serverumgebung betriebsbereit halten  
# *Keep the server environment operational*

• Konfiguriere für „Server2“ eine zusätzliche Netzwerkkarte am externen Switch.
• Erweitere die Anzahl der CPU-Kerne auf 4 und den RAM auf 4096 MB.
• Füge eine zusätzliche Festplatte von 200 GB an.
• Konfiguriere die Festplatte als einzelne Partition als Laufwerk W mit NTFS.
• Installiere auf „Server2“ die Rolle für den WSUS-Server.
• Es sollen nur Updates für Windows 11 geladen und manuell synchronisiert werden.
• Für die Erstsynchronisation deaktiviere die Netzwerkkarte „Ethernet“.
• Es soll eine Gruppe „W11Clients“ auf dem „WSUS“ vorhanden sein.
• Installiere den SQL-Reportviewer 2012.  

- *Configure an additional network card on the external switch for "Server2".*
- *Increase the number of CPU cores to 4 and the RAM to 4096 MB.*
- *Add an additional 200 GB hard drive.*
- *Configure the hard drive as a single partition as drive W with NTFS.*
- *Install the WSUS server role on "Server2".*
- *Ensure that only updates for Windows 11 are downloaded and that synchronization is performed manually.*
- *Deactivate the "Ethernet" network card for the initial synchronization.*
- *Ensure that a group named "W11Clients" is available on the WSUS.*
- *Install SQL Report Viewer 2012.*

### Vorbereiten von „Server2“  
### *Preparing Server2*

```markdown
- Öffne den Hyper-V-Manager auf dem GFN-RLAB.
- Fahre „Server2“ herunter und wechsle in die „Einstellungen“.
- Markiere „Prozessor“ und stelle rechts die Anzahl der Prozessoren auf 4.
- Markiere „Arbeitsspeicher“ und stelle rechts den Wert auf 4096 MB.
- Markiere oben „Hardware hinzufügen“ und wähle rechts „Netzwerkkarte“.
- Klicke auf „Hinzufügen“.
- Wähle unter „Virtueller Switch“ den „External Network“ Switch aus.
- Markiere den „SCSI-Controller“, wähle rechts „Festplatte“ und klicke auf „Hinzufügen“.
- Klicke auf „Neu“.
  - Dynamisch erweiterbar
  - WSUS.vhdx im Ordner „H:\VM\Server2“
  - (Verwende ggf. den passenden Ordnerpfad deiner Hyper-V Installation.)
  - 200 GB
- Klicke zum Abschluss auf „Fertigstellen“.
- Klicke zum Abschluss der Einstellungen von „Server2“ auf „OK“.
```

```markdown
- *Open the Hyper-V Manager on the GFN-RLAB.*
- *Shut down "Server2" and open "Settings".*
- *Select "Processor" and set the number of virtual processors to 4.*
- *Select "Memory" and set the value to 4096 MB.*
- *Click on "Add Hardware" and choose "Network Adapter".*
- *Click "Add".*
- *Under "Virtual switch", select the "External Network" switch.*
- *Select the "SCSI Controller", choose "Hard Drive", and click "Add".*
- *Click "New".*
  - *Dynamically expanding*
  - *WSUS.vhdx in the folder "H:\VM\Server2"*
  - *(Use the appropriate folder path of your Hyper-V installation if necessary.)*
  - *200 GB*
- *Click "Finish".*
- *Click "OK" to complete the configuration of "Server2".*
```

<img width="843" height="834" alt="image" src="https://github.com/user-attachments/assets/4f9c1ac6-bd9c-476b-ae43-dd7d43f06910" />

<img width="832" height="682" alt="image" src="https://github.com/user-attachments/assets/b339cfd6-8f41-46d9-a1fa-4355d2cd1a7d" />

<img width="838" height="402" alt="image" src="https://github.com/user-attachments/assets/95140463-04c3-4654-acd9-45203a188e64" />

<img width="834" height="466" alt="image" src="https://github.com/user-attachments/assets/6edd1850-4a09-4189-a6d6-87041510413c" />

<img width="837" height="448" alt="image" src="https://github.com/user-attachments/assets/6b59461f-45ce-4a44-ac56-e6b57a335394" />

<img width="834" height="435" alt="image" src="https://github.com/user-attachments/assets/3e6fd616-c941-4943-bf37-04e438206b23" />

<img width="829" height="645" alt="image" src="https://github.com/user-attachments/assets/24f9659e-654f-4a80-b36d-7f43409c700f" />

<img width="813" height="643" alt="image" src="https://github.com/user-attachments/assets/7d79716d-329f-4bfa-b481-1f02b1e13f41" />

<img width="824" height="648" alt="image" src="https://github.com/user-attachments/assets/6bf09666-0abf-46b3-ba2c-c30002a00103" />

<img width="818" height="646" alt="image" src="https://github.com/user-attachments/assets/ebef1e52-2980-487f-bd15-d8ef07d3091e" />

Fertigstellen  
*Finish*

Anwenden + OK  
*Apply + OK*

Server2 starten  
*Start Server2*

Server2 --> Server-Manager --> Start-Symbol --> Rechte-Maustaste: Datenträgerverwaltung  
*Server2 --> Server Manager --> Start icon --> Right mouse button: Disk Management*

<img width="401" height="753" alt="image" src="https://github.com/user-attachments/assets/1c0a1484-7b59-4c77-89de-d041369e844d" />

<img width="932" height="174" alt="image" src="https://github.com/user-attachments/assets/68a2d9c6-1f5d-4435-af11-0a8269613c10" />

<img width="939" height="207" alt="image" src="https://github.com/user-attachments/assets/de21906f-93eb-4828-9bec-3405bc0c6551" />

<img width="493" height="381" alt="image" src="https://github.com/user-attachments/assets/f1fd15bf-2745-42fe-b2dc-1206479c7252" />

<img width="915" height="279" alt="image" src="https://github.com/user-attachments/assets/4d377100-c775-4f94-b1d3-c80c789a5677" />

<img width="620" height="488" alt="image" src="https://github.com/user-attachments/assets/b2518ce9-20fd-4c6d-b291-b518c38bc9df" />

<img width="614" height="483" alt="image" src="https://github.com/user-attachments/assets/1d04bbe0-5fd6-445d-9988-2933fe3c458a" />

<img width="608" height="484" alt="image" src="https://github.com/user-attachments/assets/582b86a7-4b28-449a-9370-3b4080f31581" />

<img width="617" height="486" alt="image" src="https://github.com/user-attachments/assets/a5388baf-d93d-49ac-b17a-ecc62cade6e9" />

Fertigstellen  
*Finish*

Server2 --> Server-Manager --> Verwalten --> Rolle und Features hinzufügen --> Weiter --> Weiter --> Weiter  
*Server2 --> Server Manager --> Manage --> Add Roles and Features --> Next --> Next --> Next*

<img width="739" height="693" alt="image" src="https://github.com/user-attachments/assets/4e46c23c-434a-4e58-b287-711a0296fb3d" />

<img width="510" height="558" alt="image" src="https://github.com/user-attachments/assets/e2838cfb-5b86-4e61-819f-6d6b31a25a46" />

Weiter --> Weiter --> Weiter  
*Next --> Next --> Next*

<img width="755" height="699" alt="image" src="https://github.com/user-attachments/assets/08340acc-f600-49c1-a4d7-b216806fe9d7" />

<img width="743" height="697" alt="image" src="https://github.com/user-attachments/assets/7e3682ae-9e26-4df7-ac51-847a9b12d7e6" />

Weiter --> Weiter --> Installieren --> Fertigstellen  
*Next --> Next --> Install --> Finish*


WICHTIG!
- Wegen eines Fehlers in der Installationsquelle muss eine Datei gepatcht werden.
- Wechsle dazu im Windows Explorer zu „C:\Programme\Update Services\Database“.
- Klicke mit der rechten Maustaste auf „Versioncheck.sql“.
- Klicke auf „Eigenschaften“, dann „Sicherheit“ und anschließend auf „Erweitert“.
- Wähle bei „Besitzer“ den Link „Ändern“.
- Wähle „Domänen-Admins“ aus und klicke auf „OK“.
- Klicke auf „OK“ und anschließend auf „Bearbeiten“.
- Klicke auf „Hinzufügen“, gib „Domänen-Admins“ ein und bestätige mit „OK“.
- Markiere „Domänen-Admins“ und aktiviere „Vollzugriff“.
- Bestätige zweimal mit „OK“.
- „Domänen-Admins“ sind nun Besitzer der Datei und haben Vollzugriff.
- Klicke mit der rechten Maustaste auf „Versioncheck.sql“ und wähle „Öffnen mit“.
- Wähle „Editor“ und klicke auf „OK“.

*IMPORTANT!*
- *Due to an error in the installation source, a file must be patched.*
- *Navigate in Windows Explorer to "C:\Program Files\Update Services\Database".*
- *Right-click on "Versioncheck.sql".*
- *Click "Properties", then "Security", and then "Advanced".*
- *Next to "Owner", click the link "Change".*
- *Select "Domain Admins" and click "OK".*
- *Click "OK", then click "Edit".*
- *Click "Add", enter "Domain Admins", and confirm with "OK".*
- *Select "Domain Admins" and enable "Full control".*
- *Confirm twice with "OK".*
- *"Domain Admins" are now the owner of the file and have full control.*
- *Right-click on "Versioncheck.sql" and select "Open with".*
- *Choose "Notepad" and click "OK".*

<img width="778" height="646" alt="image" src="https://github.com/user-attachments/assets/022f352c-fb31-4a1c-87a2-2adce7c8ad24" />

<img width="448" height="585" alt="image" src="https://github.com/user-attachments/assets/a3fb44c9-454c-4bbe-80fe-e7c3655af540" />

<img width="697" height="500" alt="image" src="https://github.com/user-attachments/assets/cc0c5c20-55b8-4771-8baa-8680293d4c30" />

<img width="567" height="309" alt="image" src="https://github.com/user-attachments/assets/a1d46565-dbac-413b-af13-9e40be83667e" />

Übernehmen  
*Apply*

<img width="508" height="213" alt="image" src="https://github.com/user-attachments/assets/bab66ade-c936-4f5d-a272-1063a5b26481" />

OK --> wieder auf Erweitert klicken  
*OK --> click on Advanced again*

<img width="690" height="550" alt="image" src="https://github.com/user-attachments/assets/1eec5ba5-c6bc-4729-b628-0d1bac4f0864" />

<img width="706" height="554" alt="image" src="https://github.com/user-attachments/assets/16a997c7-b0fa-4b8e-9a3d-4cd9cbbfecca" />

<img width="671" height="390" alt="image" src="https://github.com/user-attachments/assets/a435c381-b5c9-4d0f-a377-f07ec3628947" />

<img width="562" height="314" alt="image" src="https://github.com/user-attachments/assets/f950a2aa-5960-4aef-8a88-10375c00a9bf" />

<img width="777" height="373" alt="image" src="https://github.com/user-attachments/assets/cd92cc8a-3d0f-41b3-a6e4-e5e8072a9421" />

OK

<img width="931" height="554" alt="image" src="https://github.com/user-attachments/assets/3d444f0c-7d7a-45a0-bc25-f0933bc12e41" />

Übernehmen + OK  + OK  
*Apply + OK + OK*

<img width="619" height="531" alt="image" src="https://github.com/user-attachments/assets/3c5a2dc2-5339-4442-926f-f26d2efbe763" />

<img width="489" height="403" alt="image" src="https://github.com/user-attachments/assets/dc404632-b230-4788-8a3b-e9811062a8b6" />

<img width="849" height="286" alt="image" src="https://github.com/user-attachments/assets/a9f2a08f-7ac2-4704-bf66-65bcd4964047" />

Speichern + Beenden  
*Save + Close*

Server2 --> Netzwerkverbindungen  
*Server2 --> Network Connections*

<img width="491" height="480" alt="image" src="https://github.com/user-attachments/assets/3dd6e387-6866-47df-8240-0b231f52dc9e" />

<img width="995" height="538" alt="image" src="https://github.com/user-attachments/assets/70be246c-78ee-4e8c-8f10-0fedfab88f24" />

<img width="458" height="203" alt="image" src="https://github.com/user-attachments/assets/e10c8c2a-d842-4d6d-8d8f-10dababbd47a" />

<br>

Warte, bis die Aufgabe abgeschlossen ist.  
*Wait until the task is complete.*

Server2 --> Server-Manager

<img width="469" height="747" alt="image" src="https://github.com/user-attachments/assets/b98e94f5-669d-4916-8ba7-bef27d481757" />

<img width="927" height="676" alt="image" src="https://github.com/user-attachments/assets/949e39bd-a71f-413d-831b-1b3397025ec8" />

<img width="922" height="677" alt="image" src="https://github.com/user-attachments/assets/5d9a35a6-156f-4e4a-985f-ea2aa35ff264" />

<img width="917" height="672" alt="image" src="https://github.com/user-attachments/assets/d3539257-a7ad-4dd9-aa86-8d32ac89c836" />

<img width="918" height="674" alt="image" src="https://github.com/user-attachments/assets/0767861e-8d19-4b99-8ecc-ed4737dad8d8" />

<img width="924" height="485" alt="image" src="https://github.com/user-attachments/assets/1b39b1ed-57b9-47aa-ae9c-f7debe59b2f2" />

 Verbindungsaufbau kann lange dauern (ca. 20 Minuten)  
*Connection establishment may take a long time (approx. 20 minutes)*

<img width="920" height="677" alt="image" src="https://github.com/user-attachments/assets/96ab9b16-5cfd-4be1-995f-73d005785a76" />

<img width="911" height="677" alt="image" src="https://github.com/user-attachments/assets/575eac06-d760-4862-99ac-2e184a7c7788" />

<img width="925" height="676" alt="image" src="https://github.com/user-attachments/assets/efd998a7-f307-4fa8-a126-aef952b754e0" />

<img width="919" height="674" alt="image" src="https://github.com/user-attachments/assets/e0487afe-24db-46e6-9829-0078361d8327" />

<img width="921" height="674" alt="image" src="https://github.com/user-attachments/assets/a2f39098-44b2-4357-8cda-ed0a69210203" />

<img width="919" height="674" alt="image" src="https://github.com/user-attachments/assets/4884263e-e3f1-4a5a-8d18-30a90a7cac47" />

<img width="920" height="674" alt="image" src="https://github.com/user-attachments/assets/7f2b2f6d-9744-4ee2-a3ec-b6b99299e70b" />

<img width="946" height="514" alt="image" src="https://github.com/user-attachments/assets/2a425d8d-e232-4931-bd7d-3a31a4d8c8ba" />

Beobachte den Verlauf der Synchronisation. (Es wird lange dauern, sehr lange, ca. 40 bis 60 Minuten.)  
*Watch the synchronization process. (It will take a long time, very long, approx. 40 to 60 minutes.)*

Nach erfolgreicher Synchronisation wechsle links zu „Computer“ und erweitere den Knoten.  
*After successful synchronization, switch to “Computer” on the left and expand the node.*

<img width="522" height="535" alt="image" src="https://github.com/user-attachments/assets/89b24c20-fc46-442a-80f3-50c921630269" />

<img width="621" height="244" alt="image" src="https://github.com/user-attachments/assets/e409bd42-6689-4be0-957d-ce98641f4b6f" />

### Installation des Reportviewers  
### *Installing the Report Viewer*

Standardmäßig wird „ReportViewer“ nicht mit installiert:  
*By default, “ReportViewer” is not installed:*

Doppelklick auf ein Update (egal welche)  
*Double-click on any update*

<img width="1110" height="603" alt="image" src="https://github.com/user-attachments/assets/18a1f3e7-811c-43e4-a0bd-f6ddab6ecd20" />

Wechsel zu Host-PC  
*Switch to host PC*

<img width="996" height="312" alt="image" src="https://github.com/user-attachments/assets/393010a3-5252-49fc-be35-3937f5e13a3a" />

Kopiere die beiden vorhandenen Dateien auf den Desktop von Server2 (auf dem wir WSUS konfigurieren).  
Alternativ: herunterladen von https://www.microsoft.com/de-DE/download/details.aspx?id=100451 und https://www.microsoft.com/de-de/download/details.aspx?id=35747&quot (Links können veraltet sein)  
*Copy the two existing files to the desktop of Server2 (on which we are configuring WSUS).  
Alternatively: download from https://www.microsoft.com/de-DE/download/details.aspx?id=100451 and https://www.microsoft.com/de-de/download/details.aspx?id=35747&quot (links may be outdated).*

<img width="701" height="387" alt="image" src="https://github.com/user-attachments/assets/e8ac6fb3-1eb2-4857-9d31-83fba26740e2" />

Installiere zunächst die Datei „SQLSysClrTypes.msi“:  
*First, install the file “SQLSysClrTypes.msi”:*

<img width="614" height="475" alt="image" src="https://github.com/user-attachments/assets/c8b0fd68-3bfa-42ea-b172-1805a9786a75" />

<img width="619" height="472" alt="image" src="https://github.com/user-attachments/assets/be066ed5-e573-4ffc-9fec-b2d351b350d2" />

<img width="614" height="471" alt="image" src="https://github.com/user-attachments/assets/8bc1b5f5-9ef1-4503-9ee9-c2315b7097ad" />

Anschließend installiere die Datei „ReportViewer.msi“:  
*Then install the file “ReportViewer.msi”:*

<img width="619" height="470" alt="image" src="https://github.com/user-attachments/assets/2412f69f-1896-4924-8359-2242ead79d84" />

<img width="622" height="468" alt="image" src="https://github.com/user-attachments/assets/e4facf5b-f6d9-427f-94b4-adca21740892" />

<img width="613" height="471" alt="image" src="https://github.com/user-attachments/assets/40d36006-6ee9-461a-aef8-b00b3263cd63" />

- Schließe die WSUS-Konsole und rufe sie erneut auf.
- Gehe auf „Alle Updates“ und wähle oben bei Status „Alle“ und dann „Aktualisieren“.
- Wähle ein beliebiges Update und klicke es doppelt an.
- Ein Bericht wird erstellt  

- *Close the WSUS console and reopen it.*
- *Go to “All Updates” and select “All” at the top under Status, then click “Update”.*
- *Select any update and double-click it.*
- *A report will be generated.*

<img width="915" height="639" alt="image" src="https://github.com/user-attachments/assets/dc9e1782-cc8f-4af9-8161-eef102d1c1ad" />

Dank der Installation von ReportViewer, kann der Report nun angezeigt und erstellt werden (Vorsicht End of Life! Eventuell ein Sicherheitsproblem!)  
*Thanks to the installation of ReportViewer, the report can now be displayed and created (Caution: End of Life! Possible security issue!)*

Aktiviere nun die Netzwerkkarte „Ethernet“ wieder die du vor der letzten Übung deaktiviert hast.  
*Now reactivate the “Ethernet” network card that you deactivated before the last exercise.*

<img width="1135" height="113" alt="image" src="https://github.com/user-attachments/assets/9df3af68-a162-4166-ac46-cf8551cc3771" />

- Konfiguriere eine Gruppenrichtlinie mit dem Namen „Windows Updates für Windowsclients“.
- Stelle darin ein, dass Updates automatisch heruntergeladen und wöchentlich installiert werden.
- Konfiguriere als internen Updatepfad „http://Server2.gfnlab.test:8530“.
- Verwende einen WMI-Filter, der ausschließlich Clientrechner erfasst.
- Verknüpfe die GPO mit dem WMI-Filter und anschließend mit der Domäne.
- Wende die GPO auf „W11“ an.
- Prüfe, ob sich „W11“ beim „WSUS“ gemeldet hat.
- Verschiebe „W11“ in die Gruppe „W11Clients“.
- Genehmige ein Update für „W11Clients“.


- *Configure a Group Policy named "Windows Updates for Windows Clients".*
- *Set it so that updates are automatically downloaded and installed weekly.*
- *Configure the internal update path as "http://Server2.gfnlab.test:8530".*
- *Use a WMI filter that targets only client computers.*
- *Link the GPO to the WMI filter and then to the domain.*
- *Apply the GPO to "W11".*
- *Check if "W11" has reported to the WSUS server.*
- *Move "W11" into the "W11Clients" group.*
- *Approve an update for "W11Clients".*

### Erstellen und konfigurieren der Gruppenrichtlinie  
### *Creating and configuring the group policy*

DC --> Server-Manager --> Tools --> Gruppenrichtlinienverwaltung  
*DC --> Server Manager --> Tools --> Group Policy Management*

<img width="639" height="657" alt="image" src="https://github.com/user-attachments/assets/f41d2b7b-f404-48e2-8af3-23f087f12681" />

<img width="480" height="222" alt="image" src="https://github.com/user-attachments/assets/39667265-116e-4d25-bbe9-bae345b795b0" />

<br>

<img width="620" height="483" alt="image" src="https://github.com/user-attachments/assets/c5d7aaa1-e393-4441-a87a-d2cc203d0584" />

Computerkonfiguration --> Richtlinien -> Administrative Vorlagen --> Windows Komponenten --> Windows Update  
*Computer Configuration --> Policies --> Administrative Templates --> Windows Components --> Windows Update*

<img width="1197" height="724" alt="image" src="https://github.com/user-attachments/assets/23e8a437-e3db-4a2c-9da6-5086d259efbb" />

<img width="855" height="793" alt="image" src="https://github.com/user-attachments/assets/05766209-a035-46e4-aea2-7a2a5fe30ae4" />

Übernehmen + OK  
*Apply + OK*

<img width="635" height="440" alt="image" src="https://github.com/user-attachments/assets/d48c0c9f-72b7-429b-9656-d5192072daef" />

<img width="854" height="789" alt="image" src="https://github.com/user-attachments/assets/303a9d03-c32b-4ed8-b087-7d5508be0f6e" />

Übernehmen + OK  
*Apply + OK*

<img width="556" height="622" alt="image" src="https://github.com/user-attachments/assets/69a7e8dd-a049-486b-842a-0385009bcb15" />

<img width="585" height="429" alt="image" src="https://github.com/user-attachments/assets/1a172cf0-6e58-4bdc-b0e3-00674530a067" />

<img width="469" height="346" alt="image" src="https://github.com/user-attachments/assets/ef8ccf30-b142-4052-bc3a-b232b2109133" />

Speichern  
*Save*

(Select * from Win32_OperatingSystem: Dieser Teil der Abfrage wählt alle Eigenschaften aus der Win32_OperatingSystem WMI-Klasse aus. Diese Klasse enthält viele Informationen über das Betriebssystem, wie z. B. den Namen, die Version, den Hersteller, die Installationsdatum usw.
where ProductType = "1": Dieser Teil der Abfrage filtert die Ergebnisse auf Betriebssysteme, deren ProductType Eigenschaft den Wert “1” hat. Laut der Microsoft-Dokumentation bedeutet ein ProductType von „1“, dass das Betriebssystem eine Workstation ist und kein Domain Controller.)  
*(Select * from Win32_OperatingSystem: This part of the query selects all properties from the Win32_OperatingSystem WMI class. This class contains a lot of information about the operating system, such as the name, version, manufacturer, installation date, etc.
where ProductType = “1”: This part of the query filters the results to operating systems whose ProductType property has the value ‘1’. According to Microsoft documentation, a ProductType of “1” means that the operating system is a workstation and not a domain controller.)*

<img width="795" height="631" alt="image" src="https://github.com/user-attachments/assets/4c0e2115-f845-498b-b64e-feb811933aaa" />

<img width="513" height="183" alt="image" src="https://github.com/user-attachments/assets/20ea258a-6844-4ad1-8e5f-c5af0ef5028b" />
<br>
<img width="650" height="552" alt="image" src="https://github.com/user-attachments/assets/cf034e5c-9ebd-4dc4-91bf-5a146600fda1" />

<img width="547" height="504" alt="image" src="https://github.com/user-attachments/assets/e8372019-1b56-4bfa-9abe-2e3f2c411867" />

### Anwenden der Richtlinie auf „W11“  
*### Applying the policy to “W11”*

W11 --> Melde dich als Domain-Administrator an (in unserem Fall gfnlab\administrator) --> Powershell als Administrator ausführen:  
*W11 --> Log in as domain administrator (in our case, gfnlab\administrator) --> Run Powershell as administrator:*

<img width="554" height="706" alt="image" src="https://github.com/user-attachments/assets/93f8340c-b6b5-4e92-ab90-07809ef611da" />

```powershell
gpupdate /force
```

<img width="779" height="241" alt="image" src="https://github.com/user-attachments/assets/06db7e43-e736-40bd-86e6-52b1ed5c4aea" />

Danach

```powershell
gpresult /r
```

<img width="852" height="753" alt="image" src="https://github.com/user-attachments/assets/1f3b3fee-a6e8-45d5-aeef-90e7493acea8" />

<img width="825" height="253" alt="image" src="https://github.com/user-attachments/assets/c2ec36c5-bbfe-44a8-82e1-bdd2ad6ceeca" />

<img width="795" height="756" alt="image" src="https://github.com/user-attachments/assets/ff1c5eb8-8060-46dd-a2db-806a71c9d827" />

### Prüfen, ob „W11“ beim WSUS gemeldet ist (siehe Report: Computereinstellungen --> Angewendete Gruppenrichtlinienobjekte)  
### *Check whether “W11” is reported to WSUS (see Report: Computer Settings --> Applied Group Policy Objects).*

Server2 --> Server-Manager --> Tools --> WSUS:  

Falls „W11“ in WSUS:Computer nicht aufgelistet wird, kannst du auf dem Client folgende Befehle versuchen:  
*If “W11” is not listed in WSUS:Computer, you can try the following commands on the client:*

```powershell
# Group Policy aktualisieren
gpupdate /force
# Windows Update Dienst neu starten
Restart-Service wuauserv
# Manuelle Überprüfung auf Updates und Synchronisation mit WSUS
usoclient startscan
# Alternative Befehle (falls erforderlich)
# wuauclt /detectnow
# wuauclt /reportnow
# Überprüfen der Windows Update Logs
Get-WindowsUpdateLog
```

(Sollten diese Befehle nicht unmittelbar zum Erfolg führen, kann es dennoch sein, dass du Geduld haben musst und „W11“ erst später (10–60 Min.) in der WSUS-Konsole auftaucht. Fahre in der Zwischenzeit mit nächsten Übung fort und kehre anschließend hierher zurück.)  
*(If these commands do not immediately lead to success, you may still need to be patient and wait for “W11” to appear in the WSUS console later (10–60 minutes). In the meantime, continue with the next exercise and then return here.)*

### Verschieben von „W11“ in die „W11Client“-Gruppe  
### *Moving “W11” to the “W11Client” group*

<img width="917" height="465" alt="image" src="https://github.com/user-attachments/assets/81a0fee7-6208-4137-8c02-94c3ae2f40e6" />

<img width="497" height="503" alt="image" src="https://github.com/user-attachments/assets/cea8fa60-6526-4f70-bdae-82fea89ef812" />

<img width="804" height="362" alt="image" src="https://github.com/user-attachments/assets/531226c8-38c0-446a-b5f6-ba79bf11f5e6" />

### Genehmigen eines Updates  
### *### Approving an update*

<img width="1181" height="419" alt="image" src="https://github.com/user-attachments/assets/881a7d48-3f02-4c4e-8b99-c89f85ff7746" />

<img width="505" height="428" alt="image" src="https://github.com/user-attachments/assets/15a494f8-a998-4e9d-9248-6d8a4d6355ce" />

<img width="712" height="274" alt="image" src="https://github.com/user-attachments/assets/55719a2a-3485-45f5-b6e8-1255d8f1d9c6" />

<img width="826" height="447" alt="image" src="https://github.com/user-attachments/assets/b04bd86e-f85c-4ba6-be8b-f6b84697707d" />

<img width="747" height="467" alt="image" src="https://github.com/user-attachments/assets/a4d8e312-0ceb-44b4-99c0-50999805a9c3" />

OK + Schließen  
*OK + Close*

<img width="1213" height="519" alt="image" src="https://github.com/user-attachments/assets/234def58-b4f4-4440-a053-d4081efa0a73" />

Warte, bis die Synchronisation abgeschlossen ist (kann bis zu 20-30 Minuten dauern).  
*Wait until synchronization is complete (may take up to 20-30 minutes)*

(Auf Laufwerk W: finde unter dem Ordner „wsus“ den Ordner „WsusContent“. Dort befindet sich ein neuer Ordner mit dem aktuellen Datum. Je nach Update können es auch mehrere Ordner sein. Darin befinden sich nun die Updatedaten.)  
*(On drive W:, locate the “WsusContent” folder in the “wsus” folder. There you will find a new folder with the current date. Depending on the update, there may be several folders. The update data is now stored in these folders.)*

<img width="1275" height="538" alt="image" src="https://github.com/user-attachments/assets/dd984e5e-f644-4019-bb66-c27eda144691" />















## Übung 2 - Taskmanager  
## *Excercise 2 - Task manager*

- Benutze auf „Server2“ den Task-Manager, um die aktuell laufenden Prozesse zu beobachten.
- Untersuche die Leistung von CPU, RAM und Netzwerk und beurteile diese.
- Setze die Priorität des Prozesses „spoolsv.exe“ auf „Höher als normal“.
- Die Einstellung „Höher als normal“ bedeutet, dass der Prozess mehr CPU-Ressourcen erhält als Prozesse mit normaler Priorität, was die Ausführung beschleunigen kann, aber andere Prozesse möglicherweise langsamer macht.  

- *Use the Task Manager on "Server2" to monitor the currently running processes.*
- *Examine the performance of CPU, RAM, and network, and evaluate it.*
- *Set the priority of the process "spoolsv.exe" to "Above Normal".*
- *The "Above Normal" setting means the process receives more CPU resources than processes with normal priority, which can speed up its execution but may slow down other processes.*

Server2 --> Rechte Maustaste auf Taskleiste: Task-Manager --> Mehr Details:  
*Server2 --> Right-click on taskbar: Task Manager --> More details:*

- Untersuche, welche Apps, Hintergrundprozesse und Windows-Prozesse laufen.
- Wechsle auf die Registerkarte „Leistung“.
- Sieh dir an, welche Leistungsobjekte angezeigt werden.
- Schließe den „Task-Manager“.  

- *Examine which apps, background processes, and Windows processes are running.*
- *Switch to the "Performance" tab.*
- *Observe which performance metrics are displayed.*
- *Close the Task Manager.*









