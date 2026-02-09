# Datendepublizierung

Einrichtung einer zusätzlichen Festplatte (Server2) + Datendeduplizierung für allgemeinen Dateiserver + Hintergrundoptimierung aktivieren  
Set up an additional hard drive (Server2) + enable data deduplication for general file server + enable background optimization

Datendeduplizierung ist ein Verfahren, bei dem doppelte Daten erkannt und nur einmal gespeichert werden, um Speicherplatz zu sparen.  
*Data deduplication is a technique that detects duplicate data and stores it only once to save storage space.*

### Laufwerk einfügen  
### *Insert drive*

Wechseln Sie auf Ihre Hostmaschine --> Öffnen Sie den Hyper-V-Manager --> Klicken Sie im mittleren Fenster mit der rechten Maustaste auf die virtuelle Maschine „Server2“  
*Switch to your host machine --> Open Hyper-V Manager --> Right-click on the “Server2” virtual machine in the middle window.*

<img width="592" height="605" alt="image" src="https://github.com/user-attachments/assets/cecc0d93-d279-4b74-ac9b-845297a02481" />

<img width="890" height="424" alt="image" src="https://github.com/user-attachments/assets/fa2a4451-654e-41fe-9b4f-b901ce037cd8" />

<img width="884" height="497" alt="image" src="https://github.com/user-attachments/assets/0cc43cd0-c414-4bee-ab2c-ce6ae519519f" />

<img width="879" height="662" alt="image" src="https://github.com/user-attachments/assets/e38e8e1f-790f-48a4-8060-c5d0256f0abb" />

<img width="873" height="658" alt="image" src="https://github.com/user-attachments/assets/51751507-78a8-4282-b9a3-a882863bbd82" />

<img width="866" height="660" alt="image" src="https://github.com/user-attachments/assets/5fa41af0-107d-4628-b82c-2786837bf148" />

<img width="866" height="660" alt="image" src="https://github.com/user-attachments/assets/0f865cc8-b897-4240-83f0-4aeda072d46e" />

<img width="867" height="658" alt="image" src="https://github.com/user-attachments/assets/71e41a00-78b6-4379-ad09-a84c90812d6c" />

<img width="888" height="560" alt="image" src="https://github.com/user-attachments/assets/76238caa-61a9-4e82-a94f-1783ae0841bb" />

Anwenden + OK  
*Apply + OK*

<img width="888" height="560" alt="image" src="https://github.com/user-attachments/assets/d56e3bfe-85bf-4271-af27-20ed04a93dae" />

### Einrichten des Laufwerks im System  
**Setting up the drive in the system**

Server2  

<img width="456" height="923" alt="image" src="https://github.com/user-attachments/assets/f3223c22-70be-43d0-9b96-3581fc8b84a7" />

<img width="931" height="740" alt="image" src="https://github.com/user-attachments/assets/f68983c5-0933-447c-a5f2-699ec2b55965" />

<img width="576" height="385" alt="image" src="https://github.com/user-attachments/assets/14c22333-6fc7-4ef2-ae2d-c14afeade088" />

<img width="485" height="384" alt="image" src="https://github.com/user-attachments/assets/53d1e740-faf5-4e74-a55d-c4d8765598ad" />

<img width="665" height="300" alt="image" src="https://github.com/user-attachments/assets/998cccc6-9de0-4939-bdd9-abfbc8a3ec0a" />

Weiter  
*Continue*

<img width="615" height="485" alt="image" src="https://github.com/user-attachments/assets/3ad5c476-e119-47a9-a6b3-5ef1963dee29" />

<img width="613" height="483" alt="image" src="https://github.com/user-attachments/assets/24227dd0-870b-48ee-84b6-fa93b60d7c29" />

<img width="611" height="485" alt="image" src="https://github.com/user-attachments/assets/b4e82ec7-0df3-48c8-9e9b-899f3acc11a1" />

<img width="607" height="483" alt="image" src="https://github.com/user-attachments/assets/caae75fa-0832-49bd-9f5e-2ddf10cc0ee3" />

### Rolle installieren  
### *Install roll*

<img width="891" height="369" alt="image" src="https://github.com/user-attachments/assets/00e551f6-61bd-4e82-9886-ee6048b4f536" />

<img width="967" height="694" alt="image" src="https://github.com/user-attachments/assets/ec2d2d72-d79a-4388-a090-ff3f9fe0e70d" />

<img width="971" height="690" alt="image" src="https://github.com/user-attachments/assets/a30e1324-b216-47bc-a2b1-85c125ab77de" />

<img width="968" height="691" alt="image" src="https://github.com/user-attachments/assets/c3ce5a1e-0cc3-4445-823d-42c5588a9053" />

<img width="970" height="686" alt="image" src="https://github.com/user-attachments/assets/d6594a36-d61d-497a-9078-13933f5d5b36" />

<img width="981" height="697" alt="image" src="https://github.com/user-attachments/assets/24dc103e-e8b6-464c-95a6-d0a6a2167689" />

<img width="965" height="695" alt="image" src="https://github.com/user-attachments/assets/20967e44-88bb-42ee-b085-89a3a3a72123" />

### Einrichten der Datendeduplizierung auf Laufwerk E:\  
### *Setting up data deduplication on drive E:\*

<img width="807" height="357" alt="image" src="https://github.com/user-attachments/assets/c9c1ca80-40b5-4b8c-83f1-8c687f3c29f4" />

<img width="985" height="609" alt="image" src="https://github.com/user-attachments/assets/0eca82f7-027d-404e-951b-64038d4cc57c" />

<img width="810" height="683" alt="image" src="https://github.com/user-attachments/assets/4960fdf2-9ffb-457b-b72b-37c7e4e9b2b8" />

Anwenden + OK   
*Apply + OK*

<img width="768" height="831" alt="image" src="https://github.com/user-attachments/assets/26ad3f1d-8450-4c29-8984-2471b499299e" />

Anwenden + OK   
*Apply + OK*

<img width="797" height="680" alt="image" src="https://github.com/user-attachments/assets/f262723f-b747-408d-99b2-abdef03492c9" />

Betrachten Sie die eingerichtete Datendeduplizierung mithilfe der gelernten PowerShell Cmdlets.  
*Review the configured data deduplication using the PowerShell cmdlets you have learned.*

<img width="439" height="936" alt="image" src="https://github.com/user-attachments/assets/a8dc7ee0-be0e-4955-a3c9-0eaaeb18e34b" />

Get-DedupStatus

Get-DedupVolumes

<img width="1063" height="392" alt="image" src="https://github.com/user-attachments/assets/8f25894a-5b7b-4930-8d05-dbf2c5ea9360" />

Get-DedupMetadata

<img width="904" height="576" alt="image" src="https://github.com/user-attachments/assets/15488c90-a8c8-433c-911a-d9cc308f9031" />

Get-DedupSchedule

<img width="993" height="196" alt="image" src="https://github.com/user-attachments/assets/6d562b24-6022-4b1d-8fb9-160809a09182" />





