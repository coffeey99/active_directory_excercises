# Die Serverumgebung betriebsbereit halten  
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

